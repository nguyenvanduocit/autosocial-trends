---
source: hackernews
title: Migrating my web analytics from Matomo to Umami
url: https://stanislas.blog/2025/12/migrating-matomo-to-umami-web-analytics/
date: 2025-12-27
---

[↓Skip to main content](#main-content)

[Stan’s blog](/)

[ ]

* [Posts](/posts/ "Posts")
* [About](/about/ "About")
* [Tags](/tags/ "tags")

* [Posts](/posts/ "Posts")
* [About](/about/ "About")
* [Tags](/tags/ "tags")

# Migrating my web analytics from Matomo to Umami

22 December 2025·714 words·4 mins

[free-software](https://stanislas.blog/tags/free-software/)
[privacy](https://stanislas.blog/tags/privacy/)
[sysadmin](https://stanislas.blog/tags/sysadmin/)
[devops](https://stanislas.blog/tags/devops/)

![](https://stanislas.blog/2025/12/migrating-matomo-to-umami-web-analytics/matomo-to-umami_hu_8ac2a4a949ea8c08.webp)

Table of Contents

* [Finding a modern alternative to Matomo](#finding-a-modern-alternative-to-matomo)
* [Building my own migration tool](#building-my-own-migration-tool)
  + [Testing the migration](#testing-the-migration)
  + [My migration](#my-migration)
    - [Port-forward MariaDB](#port-forward-mariadb)
    - [Dry-run migration (preview)](#dry-run-migration-preview)
    - [Generate migration SQL](#generate-migration-sql)
    - [Import to Umami](#import-to-umami)

Quickly after starting [French blog](https://angristan.fr/) in 2014, I switched from Google Analytics to Piwik for my web analytics. It’s been since [renamed Matomo in 2018](https://matomo.org/blog/2018/01/piwik-is-now-matomo/). It’s been working pretty well for more than 10 years, and I’m thankful for the creator and maintainers.

## Finding a modern alternative to Matomo [#](#finding-a-modern-alternative-to-matomo)

In 2022 I started using [Umami](https://github.com/umami-software/umami) as well, with the intention of replacing Matomo. Matomo has barely evolved in terms of UI, and it feels pretty dated now. Umami, in comparison, has a much more modern and clean UI.

Matomo also has a [**lot**](https://matomo.org/features/) of features, most of which I don’t use. It has a few gotchas like this weirdness around [updating it in its Docker image](https://github.com/matomo-org/docker/issues/248). It terms of stack, Matomo is PHP + MySQL, Umami is a NextJS app with PostgreSQL. Not fundamentally different, but Umami is just simpler to host: automatic database migrations, no plugins, reduced feature set, etc.

![Matomo dashboard showing visitor logs, real-time map, and various analytics widgets](./matomo.png)

Matomo dashboard. Very powerful and very busy

It’s been a few years now, and I’m happy with Umami! I’m feeling like it’s time to decommission my Matomo instance.

However Matomo holds my analytics data for `angristan.fr` and `stanislas.blog` for the past 10 years… I don’t want to lose that!

## Building my own migration tool [#](#building-my-own-migration-tool)

I tried searching for a way to export my data from Matomo and import it into Umami, but it doesn’t seem to exist. There is an open issue [on this subject](https://github.com/umami-software/umami/discussions/1085). It seems that their Cloud hosted version [has an import feature](https://umami.is/features), but it’s not open source. Fair!

I was able to create my own tool to do so, by studying the data models of both Matomo and Umami, and evaluating how to possibly [map every field](https://github.com/angristan/matomo-to-umami/blob/master/src/matomo_to_umami/mappings.py). It’s available as [`angristan/matomo-to-umami`](https://github.com/angristan/matomo-to-umami) on GitHub.

It’s a Python program to migrate analytics data from Matomo’s database (MySQL/MariaDB) to Umami’s database (PostgreSQL). It extracts visitor sessions and pageview events from a Matomo database and generates SQL INSERT statements compatible with Umami’s schema.

It [covers all the features](https://github.com/angristan/matomo-to-umami?tab=readme-ov-file#whats-covered) I need! I double-checked that the mappings were correct for browsers, countries, etc. And things such as outlinks and downloads, or return visits to make sure the bounce rate is the same.

I decided not to use APIs to export/import sessions to bypass any limitations and have full control. It’s also much faster to do raw SQL.

### Testing the migration [#](#testing-the-migration)

To sanity check my data in Umami, I set up a local [docker-compose environment](https://github.com/angristan/matomo-to-umami/blob/master/docker-compose.yml) in the repo which allows me to [import the generated dump into a local umami instance](https://github.com/angristan/matomo-to-umami?tab=readme-ov-file#5-import-into-local-umami-and-verify) to check if the numbers and values look good.

This was very useful and allowed me to catch bugs. I suggest that anyone following the migration step do a sanity check as well.

### My migration [#](#my-migration)

I run both Matomo and Umami on my [k8s node](https://stanislas.blog/2025/04/moving-to-k8s/). Matomo is backed by a MariaDB instance and Umami by a PostgreSQL instance.

I have two sites to migrate:

| Matomo ID | Umami UUID | Domain |
| --- | --- | --- |
| 1 | a5d41854-bde7-4416-819f-3923ea2b2706 | angristan.fr |
| 5 | 3824c584-bc9d-4a9b-aa35-9aa64f797c6f | stanislas.blog |

#### Port-forward MariaDB [#](#port-forward-mariadb)

```
kubectl -n matomo port-forward svc/mariadb 3306:3306 &
```

#### Dry-run migration (preview) [#](#dry-run-migration-preview)

```
➜  matomo-to-umami git:(master) uv run migrate \
 --mysql-host localhost --mysql-port 3307 \
 --mysql-user root --mysql-password password --mysql-database matomo \
 --site-mapping "1:a5d41854-bde7-4416-819f-3923ea2b2706:angristan.fr" \
 --site-mapping "5:3824c584-bc9d-4a9b-aa35-9aa64f797c6f:stanislas.blog" \
 --start-date 2015-01-01 --end-date 2022-07-10 \
 --dry-run -v
[22:13:22] INFO     Configured 2 site mapping(s)
           INFO     Connecting to MySQL at localhost:3307
[22:13:23] INFO     Successfully connected to MySQL database

Dry Run Mode - No SQL will be generated

            Migration Summary
┏━━━━━━━━━━━━━━━━━━┳━━━━━━━━━━━━━━━━━━━━━┓
┃ Metric           ┃ Value               ┃
┡━━━━━━━━━━━━━━━━━━╇━━━━━━━━━━━━━━━━━━━━━┩
│ Total Sessions   │ 1,352,812           │
│ Total Events     │ 1,999,709           │
│ Date Range Start │ 2015-01-18 10:45:32 │
│ Date Range End   │ 2022-07-09 23:58:58 │
└──────────────────┴─────────────────────┘
                 Per-Site Breakdown
┏━━━━━━━━━━━┳━━━━━━━━━━━━━━━━┳━━━━━━━━━━┳━━━━━━━━━━━┓
┃ Matomo ID ┃ Domain         ┃ Sessions ┃    Events ┃
┡━━━━━━━━━━━╇━━━━━━━━━━━━━━━━╇━━━━━━━━━━╇━━━━━━━━━━━┩
│ 1         │ angristan.fr   │  861,301 │ 1,332,275 │
│ 5         │ stanislas.blog │  491,511 │   667,434 │
└───────────┴────────────────┴──────────┴───────────┘

Ready to migrate. Run without --dry-run to generate SQL.
```

Result: 1,352,812 sessions, 1,999,709 events to migrate

#### Generate migration SQL [#](#generate-migration-sql)

![Terminal recording showing the matomo-to-umami migration tool generating SQL statements](./matomo-to-umami-migration.gif)

#### Import to Umami [#](#import-to-umami)

```
kubectl -n umami exec -i umami-cnpg-1 -- env PGPASSWORD=<password> \
 psql -h localhost -U app -d app < migration.sql
```

![Umami dashboard displaying pageviews and sessions over time with a clean, modern interface](./umami.png)

Now, I’m free to say goodbye to Matomo, and save resources.

Feel free to use [`matomo-to-umami`](https://github.com/angristan/matomo-to-umami), I hope it will be useful to someone else.

Thank you Matomo and Umami for being open source ❤️

![Stanislas](https://stanislas.blog/img/profile.jpg)

Author

Stanislas

I like building things with code and computers

---

[←→
How Claude Code is helping me as an open source maintainer
19 December 2025](https://stanislas.blog/2025/12/claude-code-opus-open-source-maintenance/)

---

## Related Posts

[![Automatically build and push Docker images using GitLab CI](/2018/09/build-push-docker-images-gitlab-ci/docker-build-push-gitlab-ci_hu_94b25c5d6f9fb1aa.png)

Automatically build and push Docker images using GitLab CI
7 September 2018](/2018/09/build-push-docker-images-gitlab-ci/?utm_source=related)[![How to setup a Telegram bot for your Drone CI/CD builds](/2018/08/setup-telegram-bot-for-drone-ci-cd-builds/drone-telegram_hu_c303b308211de4e2.png)

How to setup a Telegram bot for your Drone CI/CD builds
29 August 2018](/2018/08/setup-telegram-bot-for-drone-ci-cd-builds/?utm_source=related)[![Host your own CI/CD server with Drone](/2018/08/host-your-own-ci-cd-server-with-drone/drone_hu_a8768a5077d3a968.png)

Host your own CI/CD server with Drone
25 August 2018](/2018/08/host-your-own-ci-cd-server-with-drone/?utm_source=related)

---

Please enable JavaScript to view comments.

[↑](#the-top "Scroll to top")

Since 2014 |
[Say thanks 🙏](https://coindrop.to/stanislas) |
Content under the [CC BY-NC-SA 4.0 license.](https://creativecommons.org/licenses/by-nc-sa/4.0/)
