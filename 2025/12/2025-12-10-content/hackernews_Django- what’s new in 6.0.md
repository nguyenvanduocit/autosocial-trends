---
source: hackernews
title: Django: what’s new in 6.0
url: https://adamj.eu/tech/2025/12/03/django-whats-new-6.0/
date: 2025-12-10
---

[Adam Johnson](/)

[Home](/) | [Blog](/tech/) | [Books](/books/) | [Projects](/projects/) | [Consulting](/consulting/) | [Contact](/contact/) | [Colophon](/colophon/)

# Django: what’s new in 6.0

2025-12-03![Django 6.0: codename “mosaic”](/tech/assets/2025-12-03-django-mosaic.webp "Django 6.0: codename “mosaic”")

Django 6.0 was [released today](https://www.djangoproject.com/weblog/2025/dec/03/django-60-released/), starting another release cycle for the loved and long-lived Python web framework (now 20 years old!). It comes with a *mosaic* of new features, contributed to by many, some of which I am happy to have helped with. Below is my pick of highlights from [the release notes](https://docs.djangoproject.com/en/stable/releases/6.0/).

## Upgrade with help from django-upgrade

If you’re upgrading a project from Django 5.2 or earlier, please try my tool [django-upgrade](https://django-upgrade.readthedocs.io/en/latest/). It will automatically update old Django code to use new features, fixing some deprecation warnings for you, including five fixers for Django 6.0. (One day, I’ll propose django-upgrade to become an official Django project, when energy and time permit…)

## Template partials

There are four headline features in Django 6.0, which we’ll cover before other notable changes, starting with this one:
> The Django Template Language now supports [template partials](https://docs.djangoproject.com/en/stable/ref/templates/language/#template-partials), making it easier to encapsulate and reuse small named fragments within a template file.

Partials are sections of a template marked by the new `{% partialdef %}` and `{% endpartialdef %}` tags. They can be reused within the same template or rendered in isolation. Let’s look at examples for each use case in turn.

### Reuse partials within the same template

The below template reuses a partial called `filter_controls` within the same template. It’s defined once at the top of the template, then used twice later on. Using a partial allows the template avoid repetition without pushing the content into a separate include file.

```
<section id=videos>
  {% partialdef filter_controls %}
    <form>
      {{ filter_form }}
    </form>
  {% endpartialdef %}

  {% partial filter_controls %}

  <ul>
    {% for video in videos %}
      <li>
        <h2>{{ video.title }}</h2>
        ...
      </li>
    {% endfor %}
  </ul>

  {% partial filter_controls %}
</section>
```

Actually, we can simplify this pattern further, by using the `inline` option on the `partialdef` tag, which causes the definition to also render in place:

```
<section id=videos>
  {% partialdef filter_controls inline %}
    <form>
      {{ filter_form }}
    </form>
  {% endpartialdef %}

  <ul>
    {% for video in videos %}
      <li>
        <h2>{{ video.title }}</h2>
        ...
      </li>
    {% endfor %}
  </ul>

  {% partial filter_controls %}
</section>
```

Reach for this pattern any time you find yourself repeating template code within the same template. Because partials can use variables, you can also use them to de-duplicate when rendering similar controls with different data.

### Render partials in isolation

The below template defines a `view_count` partial that’s intended to be re-rendered in isolation. It uses the `inline` option, so when the whole template is rendered, the partial is included.

The page uses [htmx](https://htmx.org/), via my [django-htmx package](https://django-htmx.readthedocs.io/en/latest/), to periodically refresh the view count, through the `hx-*` attributes. The request from htmx goes to a dedicated view that re-renders the `view_count` partial.

```
{% load django_htmx %}
<!doctype html>
<html>
  <body>
    <h1>{{ video.title }}</h1>
    <video width=1280 height=720 controls>
      <source src="{{ video.file.url }}" type="video/mp4">
      Your browser does not support the video tag.
    </video>

    {% partialdef view_count inline %}
    <section
      class=view-count
      hx-trigger="every 1s"
      hx-swap=outerHTML
      hx-get="{% url 'video-view-count' video.id %}"
    >
      {{ video.view_count }} views
    </section>
    {% endpartialdef %}

    {% htmx_script %}
  </body>
</html>
```

The relevant code for the two views could look like this:

```
from django.shortcuts import render

def video(request, video_id):
    ...
    return render(request, "video.html", {"video": video})

def video_view_count(request, video_id):
    ...
    return render(request, "video.html#view_count", {"video": video})
```

The initial `video` view renders the full template `video.html`. The `video_view_count` view renders just the `view_count` partial, by appending `#view_count` to the template name. This syntax is similar to how you’d reference an HTML fragment by its ID in a URL.

### History

htmx was the main motivation for this feature, as promoted by htmx creator Carson Gross in [a cross-framework review post](https://htmx.org/essays/template-fragments/). Using partials definitely helps maintain “Locality of behaviour” within your templates, easing authoring, debugging, and maintenance by avoiding template file sprawl.

Django’s support for template partials was initially developed by Carlton Gibson in the [django-template-partials package](https://pypi.org/project/django-template-partials/), which remains available for older Django versions. The integration into Django itself was done in a Google Summer of Code project this year, worked on by student Farhan Ali and mentored by Carlton, in [Ticket #36410](https://code.djangoproject.com/ticket/36410). You can read more about the development process in [Farhan’s retrospective blog post](https://www.farhana.li/blog/my-gsoc25-journey-django). Many thanks to Farhan for authoring, Carlton for mentoring, and Natalia Bidart, Nick Pope, and Sarah Boyce for reviewing!

## Tasks framework

The next headline feature we’re covering:
> Django now includes a built-in Tasks framework for running code outside the HTTP request–response cycle. This enables offloading work, such as sending emails or processing data, to background workers.

Basically, there’s a new API for defining and enqueuing background tasks—very cool!

Background tasks are a way of running code outside of the request-response cycle. They’re a common requirement in web applications, used for sending emails, processing images, generating reports, and more.

Historically, Django has not provided any system for background tasks, and kind of ignored the problem space altogether. Developers have instead relied on third-party packages like [Celery](https://docs.celeryq.dev/en/stable/) or [Django Q2](https://django-q2.readthedocs.io/en/master/). While these systems are fine, they can be complex to set up and maintain, and often don’t “go with the grain” of Django.

The new Tasks framework fills this gap by providing an interface to define background tasks, which task runner packages can then integrate with. This common ground allows third-party Django packages to define tasks in a standard way, assuming you’ll be using a compatible task runner to execute them.

Define tasks with the new [`@task`](https://docs.djangoproject.com/en/stable/ref/tasks/#django.tasks.task) decorator:

```
from django.tasks import task

@task
def resize_video(video_id): ...
```

…and enqueue them for background execution with the [`Task.enqueue()`](https://docs.djangoproject.com/en/stable/ref/tasks/#django.tasks.Task.enqueue) method:

```
from example.tasks import resize_video

def upload_video(request):
    ...
    resize_video.enqueue(video.id)
    ...
```

### Execute tasks

At this time, Django does not include a production-ready task backend, only two that are suitable for development and testing:

* [`ImmediateBackend`](https://docs.djangoproject.com/en/stable/topics/tasks/#immediate-execution) - runs tasks synchronously, blocking until they complete.* [`DummyBackend`](https://docs.djangoproject.com/en/stable/topics/tasks/#dummy-backend) - does nothing when tasks are enqueued, but allows them to be inspected later. Useful for tests, where you can assert that tasks were enqueued without actually running them.

For production use, you’ll need to use a third-party package that implements one, for which [django-tasks](https://pypi.org/project/django-tasks/), the reference implementation, is the primary option. It provides `DatabaseBackend` for storing tasks in your SQL database, a fine solution for many projects, avoiding extra infrastructure and allowing atomic task enqueuing within database transactions. We may see this backend merged into Django in due course, or at least become an official package, to help make Django “batteries included” for background tasks.

To use django-tasks’ `DatabaseBackend` today, first install the package:

```
uv add django-tasks
```

**Second,** add these two apps to your `INSTALLED_APPS` setting:

```
INSTALLED_APPS = [
    # ...
    "django_tasks",
    "django_tasks.backends.database",
    # ...
]
```

**Third,** configure `DatabaseBackend` as your tasks backend in the [new `TASKS` setting](https://docs.djangoproject.com/en/stable/ref/settings/#tasks):

```
TASKS = {
    "default": {
        "BACKEND": "django_tasks.backends.database.DatabaseBackend",
    },
}
```

**Fourth,** run migrations to create the necessary database tables:

```
$ ./manage.py migrate
```

**Finally,** to run the task worker process, use the package’s `db_worker` management command:

```
$ ./manage.py db_worker
Starting worker worker_id=jWLMLrms3C2NcUODYeatsqCFvd5rK6DM queues=default
```

This process runs indefinitely, polling for tasks and executing them, logging events as it goes:

```
Task id=10b794ed-9b64-4eed-950c-fcc92cd6784b path=example.tasks.echo state=RUNNING
Hello from test task!
Task id=10b794ed-9b64-4eed-950c-fcc92cd6784b path=example.tasks.echo state=SUCCEEDED
```

You’ll want to run `db_worker` in production, and also in development if you want to test background task execution.

### History

It’s been a long path to get the Tasks framework into Django, and I’m super excited to see it finally available in Django 6.0. Jake Howard started on the idea for Wagtail, a Django-powered CMS, back in 2021, as they have a need for common task definitions across their package ecosystem. He upgraded the idea to target Django itself in 2024, when he proposed [DEP 0014](https://github.com/django/deps/blob/main/accepted/0014-background-workers.rst). As a member of the Steering Council at the time, I had the pleasure of helping review and accept the DEP.

Since then, Jake has been leading the implementation effort, building pieces first in the separate [django-tasks package](https://pypi.org/project/django-tasks/) before preparing them for inclusion in Django itself. This step was done under [Ticket #35859](https://code.djangoproject.com/ticket/35859), with [a pull request](https://github.com/django/django/pull/18627) that took nearly a year to review and land. Thanks to Jake for his perseverance here, and to all reviewers: Andreas Nüßlein, Dave Gaeddert, Eric Holscher, Jacob Walls, Jake Howard, Kamal Mustafa, @rtr1, @tcely, Oliver Haas, Ran Benita, Raphael Gaschignard, and Sarah Boyce.

Read more about this feature and story in [Jake’s post celebrating when it was merged](https://theorangeone.net/posts/django-dot-tasks-exists/).

## Content Security Policy support

Our third headline feature:
> Built-in support for the [Content Security Policy (CSP)](https://docs.djangoproject.com/en/stable/topics/security/#security-csp) standard is now available, making it easier to protect web applications against content injection attacks such as cross-site scripting (XSS). CSP allows declaring trusted sources of content by giving browsers strict rules about which scripts, styles, images, or other resources can be loaded.

I’m really excited about this, because I’m a bit of a security nerd who’s been deploying CSP for client projects for years.

[CSP](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CSP) is a security standard that can protect your site from cross-site scripting (XSS) and other code injection attacks. You set a `content-security-policy` header to declare which content sources are trusted for your site, and then browsers will block content from other sources. For example, you might declare that only scripts your domain are allowed, so an attacker who manages to inject a `<script>` tag pointing to evil.com would be thwarted, as the browser would refuse to load it.

Previously, Django had no built-in support for CSP, and developers had to rely on building their own, or using a third-party package like the very popular [django-csp](https://django-csp.readthedocs.io/en/latest/). But this was a little bit inconvenient, as it meant that other third-party packages couldn’t reliably integrate with CSP, as there was no common API to do so.

The new CSP support provides all the core features that django-csp did, with a slightly tidier and more Djangoey API. To get started, **first** add [`ContentSecurityPolicyMiddleware`](https://docs.djangoproject.com/en/stable/ref/middleware/#django.middleware.csp.ContentSecurityPolicyMiddleware) to your `MIDDLEWARE` setting:

```
MIDDLEWARE = [
    # ...
    "django.middleware.csp.ContentSecurityPolicyMiddleware",
    # ...
]
```

Place it next to [`SecurityMiddleware`](https://docs.djangoproject.com/en/stable/ref/middleware/#django.middleware.security.SecurityMiddleware), as it similarly adds security-related headers to all responses. (You *do* have `SecurityMiddleware` enabled, right?)

**Second,** configure your CSP policy using the new settings:

* [`SECURE_CSP`](https://docs.djangoproject.com/en/stable/ref/settings/#secure-csp) to configure the `content-security-policy` header, which is your actively enforced policy.* [`SECURE_CSP_REPORT_ONLY`](https://docs.djangoproject.com/en/stable/ref/settings/#secure-csp-report-only) to configure the `content-security-policy-report-only` header, which sets a non-enforced policy for which browsers report violations to a specified endpoint. This option is useful for testing and monitoring a policy before enforcing it.

For example, to adopt the nonce-based strict CSP [recommended by web.dev](https://web.dev/articles/strict-csp), you could start with the following setting:

```
from django.utils.csp import CSP

SECURE_CSP_REPORT_ONLY = {
    "script-src": [CSP.NONCE, CSP.STRICT_DYNAMIC],
    "object-src": [CSP.NONE],
    "base-uri": [CSP.NONE],
}
```

The [`CSP`](https://docs.djangoproject.com/en/stable/ref/csp/#django.utils.csp.CSP) enum used above provides constants for CSP directives, to help avoid typos.

This policy is quite restrictive and will break most existing sites if deployed as-is, because it requires nonces, as covered next. That’s why the example shows starting with the report-only mode header, to help track down places that need fixing before enforcing the policy. You’d later change to setting the `SECURE_CSP` setting to enforce the policy.

Anyway, those are the two basic steps to set up the new CSP support!

### Nonce generation

A key part of the new feature is that **nonce generation** is now built-in to Django, when using the CSP middleware. Nonces are a security feature in CSP that allow you to mark specific `<script>` and `<style>` tags as trusted with a `nonce` attribute:

```
<script src=/static/app.js type=module nonce=55vsH4w7ATHB85C3MbPr_g></script>
```

The nonce value is randomly generated per-request, and included in the CSP header. An attacker performing content injection couldn’t guess the nonce, so browsers can trust only those tags that include the correct nonce. Because nonce generation is now part of Django, third-party packages can depend on it for their `<script>` and `<style>` tags and they’ll continue to work if you adopt CSP with nonces.

Nonces are the recommended way to use CSP today, avoiding problems with previous allow-list based approaches. That’s why the above recommended policy enables them. To adopt a nonce-based policy, you’ll need to annotate your `<script>` and `<style>` tags with the nonce value through the following steps.

**First,** add the new [`csp`](https://docs.djangoproject.com/en/stable/ref/templates/api/#django.template.context_processors.csp) template context processor to your `TEMPLATES` setting:

```
TEMPLATES = [
    {
        "BACKEND": "django.template.backends.django.DjangoTemplates",
        "OPTIONS": {
            "context_processors": [
                # ...
                "django.template.context_processors.csp",
            ],
        },
    },
]
```

**Second,** annotate your `<script>` and `<style>` tags with `nonce="{{ csp_nonce }}"`:

```
-   <script src="{% static 'app.js' %}" type="module"></script>
+   <script src="{% static 'app.js' %}" type="module" nonce="{{ csp_nonce }}"></script>
```

This can be tedious and error-prone, hence using the report-only mode first to monitor violations might be useful, especially on larger projects.

Anyway, deploying CSP right would be another post in itself, or even a book chapter, so we’ll stop here for now. For more info, check out [that web.dev article](https://web.dev/articles/strict-csp) and [the MDN CSP guide](https://developer.mozilla.org/en-US/docs/Web/HTTP/CSP).

### History

CSP itself was proposed for browsers way back in 2004, and was first implemented in Mozilla Firefox version 4, released 2011. That same year, [Django Ticket #15727](https://code.djangoproject.com/ticket/15727) was opened, proposing adding CSP support to Django. Mozilla created [django-csp](https://django-csp.readthedocs.io/en/latest/) from 2010, before the first public availability of CSP, using it on their own Django-powered sites. The first comment on Ticket #15727 pointed to django-csp, and the community basically rolled with it as the de facto solution.

Over the years, CSP itself evolved, as did django-csp, with [Rob Hudson](https://rob.cogit8.org/) ending up as its maintainer. Focusing on the package motivated to finally get CSP into Django itself. He made a draft PR and posted on Ticket #15727 in 2024, which I enjoyed helping review. He iterated on the PR over the next 13 months until it was finally merged for Django 6.0. Thanks to Rob for his heroic dedication here, and to all reviewers: Benjamin Balder Bach, Carlton Gibson, Collin Anderson, David Sanders, David Smith, Florian Apolloner, Harro van der Klauw, Jake Howard, Natalia Bidart, Paolo Melchiorre, Sarah Boyce, and Sébastien Corbin.

## Email API updates

The fourth and final headline feature:
> Email handling in Django now uses Python’s modern email API, introduced in Python 3.6. This API, centered around the [`email.message.EmailMessage`](https://docs.python.org/3/library/email.message.html#email.message.EmailMessage) class, offers a cleaner and Unicode-friendly interface for composing and sending emails.

This is a major change, but it’s unlikely to affect projects using basic email features. You can still use Django’s [`send_mail()` function](https://docs.djangoproject.com/en/stable/topics/email/#django.core.mail.send_mail) and [`EmailMessage` class](https://docs.djangoproject.com/en/stable/topics/email/#django.core.mail.EmailMessage) as before, like:

```
from django.core.mail import EmailMessage

email = EmailMessage(
    subject="🐼 Need more bamboo",
    body="We are desperately low, please restock before the pandas find out!",
    from_email="zookeeper@example.com",
    to=["supplies@example.com"],
)
email.attach_file("/media/bamboo_cupboard.jpg")
email.send()
```

The key change is that, under-the-hood, when you call `send()` on a Django `EmailMessage` object, it now translates itself into a Python’s newer `email.message.EmailMessage` type before sending.

Modernizing provides these benefits:

1. **Fewer bugs** - many edge case bugs in Python’s old email API have been fixed in the new one.- **Django is less hacky** - a bunch of workarounds and security fixes in Django‘s email code have been removed.- **More convenient API** - the new API supports some niceties, like the below inline attachment example.

### Easier inline attachments with `MIMEPart`

Django’s [`EmailMessage.attach()`](https://docs.djangoproject.com/en/stable/topics/email/#django.core.mail.EmailMessage) method allows you to attach a file as an attachment. Emails support images as **inline attachments**, which can be displayed within the HTML email body.

While you could previously use `EmailMessage.attach()` to add inline attachments, it was a bit fiddly, using a legacy class. Now, you can call the method with a Python [`email.message.MIMEPart`](https://docs.python.org/3/library/email.message.html#email.message.MIMEPart) object to add an inline attachment in a few steps:

```
import email.utils
from email.message import MIMEPart
from django.core.mail import EmailMultiAlternatives

message = EmailMultiAlternatives(
    subject="Cute Panda Alert",
    body="Here's a cute panda picture for you!",
    from_email="cute@example.com",
    to=["fans@example.com"],
)
with open("panda.jpg", "rb") as f:
    panda_jpeg = f.read()

cid = email.utils.make_msgid()
inline_image = MIMEPart()
inline_image.set_content(
    panda_jpeg,
    maintype="image",
    subtype="jpeg",
    disposition="inline",
    cid=cid,
)
message.attach(inline_image)
message.attach_alternative(
    f'<h1>Cute panda baby alert!</h1><img src="cid:{cid[1:-1]}">',
    "text/html",
)
```

It’s not the simplest API, but it does expose all the power of the underlying email system, and it’s better than the past situation.

### History

The new email API was added to Python as provisional [in version 3.4 (2014)](https://docs.python.org/3/whatsnew/3.4.html#email), and made stable [in version 3.6 (2016)](https://docs.python.org/3/whatsnew/3.6.html#email). The legacy API, however, was never planned for deprecation, so there was never any deadline to upgrade Django’s email handling.

In 2024, Mike Edmunds [posted on the (old) django-developers mailing list](https://groups.google.com/g/django-developers/c/2zf9GQtjdIk), proposing the upgrade with strong reasoning and planning. This conversation led to [Ticket #35581](https://code.djangoproject.com/ticket/35581), which he worked on for eight months until it was merged. Many thanks to Mike for leading this effort, and to Sarah Boyce for reviewing! Email is not a glamorous feature, but it’s a critical communication channel for nearly every Django project, so props for this.

## Positional arguments in `django.core.mail` APIs

We’re now out of the headline features and onto the “minor” changes, starting with this deprecation related to the above email changes:
> [`django.core.mail`](https://docs.djangoproject.com/en/stable/topics/email/#module-django.core.mail) APIs now require keyword arguments for less commonly used parameters. Using positional arguments for these now emits a deprecation warning and will raise a `TypeError` when the deprecation period ends:
>
> * All optional parameters (`fail_silently` and later) must be passed as keyword arguments to `get_connection()`, `mail_admins()`, `mail_managers()`, `send_mail()`, and `send_mass_mail()`.* All parameters must be passed as keyword arguments when creating an `EmailMessage` or `EmailMultiAlternatives` instance, except for the first four (`subject`, `body`, `from_email`, and `to`), which may still be passed either as positional or keyword arguments.

Previously, Django would let you pass all parameters positionally, which gets a bit silly and hard to read with long parameter lists, like:

```
from django.core.mail import send_mail

send_mail(
    "🐼 Panda of the week",
    "This week’s panda is Po Ping, sha-sha booey!",
    "updates@example.com",
    ["adam@example.com"],
    True,
)
```

The final `True` doesn’t provide any clue what it means without looking up the function signature. Now, using positional arguments for those less-commonly-used parameters raises a deprecation warning, nudging you to write:

```
from django.core.mail import send_mail

send_mail(
    subject="🐼 Panda of the week",
    body="This week’s panda is Po Ping, sha-sha booey!",
    from_email="updates@example.com",
    ["adam@example.com"],
    fail_silently=True,
)
```

This change is appreciated for API clarity, and Django is generally moving towards using keyword-only arguments more often. django-upgrade can automatically fix this one for you, via its [`mail_api_kwargs` fixer](https://django-upgrade.readthedocs.io/en/latest/fixers.html#mail-api-kwargs).

Thanks to Mike Edmunds, again, for making this improvement in [Ticket #36163](https://code.djangoproject.com/ticket/36163).

## Extended automatic `shell` imports

Next up:
> Common utilities, such as django.conf.settings, are now automatically imported to the shell by default.

One of the headline features back in Django 5.2 was [automatic model imports in the shell](/tech/2025/04/07/django-whats-new-5.2/#automatic-model-imports-in-the-shell), making `./manage.py shell` import all of your models automatically. Building on that DX boost, Django 6.0 now also imports other common utilities, for which we can find the full list by running `./manage.py shell` with `-v 2`:

```
$ ./manage.py shell -v 2
6 objects imported automatically:

  from django.conf import settings
  from django.db import connection, models, reset_queries
  from django.db.models import functions
  from django.utils import timezone

...
```

(This is from a project without any models, so only the utilities are listed.)

So that’s:

* [`settings`](https://docs.djangoproject.com/en/stable/topics/settings/#using-settings-in-python-code), useful for checking your runtime configuration:

  ```
  In [1]: settings.DEBUG
  Out[1]: False
  ```

  * [`connection`](https://docs.djangoproject.com/en/5.2/topics/db/sql/#executing-custom-sql-directly) and `reset_queries()`, great for [checking the executed queries](https://docs.djangoproject.com/en/stable/faq/models/#how-can-i-see-the-raw-sql-queries-django-is-running):

    ```
    In [1]: Book.objects.select_related('author')
    Out[1]: <QuerySet []>

    In [2]: connection.queries
    Out[2]:
    [{'sql': 'SELECT "example_book"."id", "example_book"."title", "example_book"."author_id", "example_author"."id", "example_author"."name" FROM "example_book" INNER JOIN "example_author" ON ("example_book"."author_id" = "example_author"."id") LIMIT 21',
      'time': '0.000'}]
    ```

    * [`models`](https://docs.djangoproject.com/en/stable/topics/db/models/#defining-models) and [`functions`](https://docs.djangoproject.com/en/stable/ref/models/database-functions/#module-django.db.models.functions), useful for advanced ORM work:

      ```
      In [1]: Book.objects.annotate(
         ...:   title_lower=functions.Lower("title")
         ...: ).filter(
         ...:   title_lower__startswith="a"
         ...: ).count()
      Out[1]: 71
      ```

      * [`timezone`](https://docs.djangoproject.com/en/stable/ref/utils/#module-django.utils.timezone), useful for using Django’s timezone-aware date and time utilities:

        ```
        In [1]: timezone.now()
        Out[1]: datetime.datetime(2025, 12, 1, 23, 42, 22, 558418, tzinfo=datetime.timezone.utc)
        ```

It remains possible to extend the automatic imports with whatever you’d like, as documented in [How to customize the `shell` command](https://docs.djangoproject.com/en/stable/howto/custom-shell/) documentation page.

Salvo Polizzi contributed the original automatic shell imports feature in Django 5.2. He’s then returned to offer these extra imports for Django 6.0, in [Ticket #35680](https://code.djangoproject.com/ticket/35680). Thanks to everyone that contributed to the forum discussion agreeing on which imports to add, and to Natalia Bidart and Sarah Boyce for reviewing!

## Dynamic field refresh on `save()`

Now let’s discuss a series of ORM improvements, starting with this big one:
> `GeneratedField`s and fields assigned expressions are now refreshed from the database after `save()` on backends that support the `RETURNING` clause (SQLite, PostgreSQL, and Oracle). On backends that don’t support it (MySQL and MariaDB), the fields are marked as deferred to trigger a refresh on subsequent accesses.

Django models support having the database generate field values for you in three cases:

1. The [`db_default`](https://docs.djangoproject.com/en/stable/ref/models/fields/#django.db.models.Field.db_default) field option, which lets the database generate the default value when creating an instance:

   ```
   from django.db import models
   from django.db.models.functions import Now

   class Video(models.Model):
       ...
       created = models.DateTimeField(db_default=Now())
   ```

   - The [`GeneratedField`](https://docs.djangoproject.com/en/stable/ref/models/fields/#django.db.models.GeneratedField) field type, which is always computed by the database based on other fields in the same instance:

     ```
     from django.db import models
     from django.db.models.functions import Concat

     class Video(models.Model):
         ...
         full_title = models.GeneratedField(
             models.TextField(),
             expression=Concat(
                 "title",
                 models.Value(" - "),
                 "subtitle",
             ),
         )
     ```

     - Assigning expression values to fields before saving:

       ```
       from django.db import models
       from django.db.models.functions import Now

       class Video(models.Model):
           ...
           last_updated = models.DateTimeField()

       video = Video.objects.get(id=1)
       ...
       video.last_updated = Now()
       video.save()
       ```

Previously, only the first method, using `db_default`, would refresh the field value from the database after saving. The other two methods would leave you with only the old value or the expression object, meaning you’d need to call [`Model.refresh_from_db()`](https://docs.djangoproject.com/en/stable/ref/models/instances/#django.db.models.Model.refresh_from_db) to get any updated value if necessary. This was hard to remember and it costs an extra database query.

Now Django takes advantage of the `RETURNING` SQL clause to save the model instance and fetch updated dynamic field values in a single query, on backends that support it (SQLite, PostgreSQL, and Oracle). A `save()` call may now issue a query like:

```
UPDATE "example_video"
SET "last_updated" = NOW()
WHERE "example_video"."id" = 1
RETURNING "example_video"."last_updated"
```

Django puts the return value into the model field, so you can read it immediately after saving:

```
video = Video.objects.get(id=1)
...
video.last_updated = Now()
video.save()
print(video.last_updated)  # Updated value from the database
```

On backends that don’t support `RETURNING` (MySQL and MariaDB), Django now marks the dynamic fields as deferred after saving. That way, the later access, as in the above example, will automatically call `Model.refresh_from_db()`. This ensures that you always read the updated value, even if it costs an extra query.

### History

This feature was proposed in [Ticket #27222](https://code.djangoproject.com/ticket/27222) way back in 2016, by Anssi Kääriäinen. It sat dormant for most of the nine years since, but ORM boss Simon Charette picked it up earlier this year, found an implementation, and pushed it through to completion. Thanks to Simon for continuing to push the ORM forward, and to all reviewers: David Sanders, Jacob Walls, Mariusz Felisiak, nessita, Paolo Melchiorre, Simon Charette, and Tim Graham.

## Universal `StringAgg` aggregate

The next ORM change:
> The new [`StringAgg`](https://docs.djangoproject.com/en/stable/ref/models/aggregates/#django.db.models.StringAgg) aggregate returns the input values concatenated into a string, separated by the `delimiter` string. This aggregate was previously supported only for PostgreSQL.

This aggregate is often used for making comma-separated lists of related items, among other things. Previously, it was only supported on PostgreSQL, as part of [`django.contrib.postgres`](https://docs.djangoproject.com/en/stable/ref/contrib/postgres/):

```
from django.contrib.postgres.aggregates import StringAgg
from example.models import Video

videos = Video.objects.annotate(
    chapter_ids=StringAgg("chapter", delimiter=","),
)

for video in videos:
    print(f"Video {video.id} has chapters: {video.chapter_ids}")
```

…which might give you output like:

```
Video 104 has chapters: 71,72,74
Video 107 has chapters: 88,89,138,90,91,93
```

Now this aggregate is available on all database backends supported by Django, imported from `django.db.models`:

```
from django.db.models import StringAgg, Value
from example.models import Video

videos = Video.objects.annotate(
    chapter_ids=StringAgg("chapter", delimiter=Value(",")),
)

for video in videos:
    print(f"Video {video.id} has chapters: {video.chapter_ids}")
```

Note the `delimiter` argument now requires a [`Value()`](https://docs.djangoproject.com/en/stable/ref/models/expressions/#django.db) expression wrapper for literal strings, as above. This change allows you to use database functions or fields as the delimiter if desired.

While most Django projects stick to PostgreSQL, having this aggregate available on all backends is a nice improvement for cross-database compatibility, and it means third-party packages can use it without affecting their database support.

### History

The PostgreSQL-specific `StringAgg` was added way back in Django 1.9 (2015) by Andriy Sokolovskiy, in [Ticket #24301](https://code.djangoproject.com/ticket/24301). In [Ticket #35444](https://code.djangoproject.com/ticket/35444), Chris Muthig proposed adding the `Aggregate.order_by` option, something used by `StringAgg` to specify the ordering of concatenated elements, and as a side effect this made it possible to generalize `StringAgg` to all backends.

Thanks to Chris for proposing and implementing this change, and to all reviewers: Paolo Melchiorre, Sarah Boyce, and Simon Charette.

## `BigAutoField` as the default primary key type

Next up:
> `DEFAULT_AUTO_FIELD` setting now defaults to `BigAutoField`

This important change helps lock in scalable larger primary keys.

Django 3.2 (2021) introduced [the `DEFAULT_AUTO_FIELD` setting](https://docs.djangoproject.com/en/stable/ref/settings/#default-auto-field) for changing the default primary key type used in models. Django uses this setting to add a primary key field called `id` to models that don’t explicitly define a primary key field. For example, if you define a model like this:

```
from django.db import models

class Video(models.Model):
    title = models.TextField()
```

…then it will have two fields: `id` and `title`, where `id` uses the type defined by `DEFAULT_AUTO_FIELD`.

The setting can also be overridden on a per-app basis by defining [`AppConfig.default_auto_field`](https://docs.djangoproject.com/en/stable/ref/applications/#django.apps.AppConfig.default_auto_field) in the app’s `apps.py` file:

```
from django.apps import AppConfig

class ChannelConfig(AppConfig):
    name = "channel"
    default_auto_field = "django.db.models.BigAutoField"
```

A key motivation for adding the setting was to allow projects to switch from `AutoField` (a 32-bit integer) to `BigAutoField` (a 64-bit integer) for primary keys, without needing changes to every model. `AutoField` can store values up to about 2.1 billion, which sounds large but it becomes easy to hit at scale. `BigAutoField` can store values up to about 9.2 quintillion, which is “more than enough” for every practical purpose.

If a model using `AutoField` hits its maximum value, it can no longer accept new rows, a problem known as **primary key exhaustion**. The table is effectively blocked, requiring an urgent fix to switch the model from `AutoField` to `BigAutoField` via a locking database migration on a large table. For a great watch on how Kraken is fixing this problem, see [Tim Bell’s DjangoCon Europe 2025 talk](https://www.youtube.com/watch?v=kZ4q0k_FNhw), detailing some clever techniques to proactively migrate large tables with minimal downtime.

To stop this problem arising for new projects, Django 3.2 made new projects created with `startproject` set `DEFAULT_AUTO_FIELD` to `BigAutoField`, and new apps created with `startapp` set their `AppConfig.default_auto_field` to `BigAutoField`. It also added a system check to ensure that projects set `DEFAULT_AUTO_FIELD` explicitly, to ensure users were aware of the feature and could make an informed choice.

Now Django 6.0 changes the actual default values of the setting and app config attribute to `BigAutoField`. Projects using `BigAutoField` can remove the setting:

```
-DEFAULT_AUTO_FIELD = "django.db.models.BigAutoField"
```

…and app config attribute:

```
from django.apps import AppConfig

 class ChannelConfig(AppConfig):
     name = "channel"
-    default_auto_field = "django.db.models.BigAutoField"
```

The default `startproject` and `startapp` templates also no longer set these values. This change reduces the amount of boilerplate in new projects, and the problem of primary key exhaustion can fade into history, becoming something that most Django users no longer need to think about.

### History

The addition of `DEFAULT_AUTO_FIELD` in Django 3.2 was proposed by Caio Ariede and implemented by Tom Forbes, in [Ticket #31007](https://code.djangoproject.com/ticket/31007). This new change in Django 6.0 was proposed and implemented by ex-Fellow Tim Graham, in [Ticket #36564](https://code.djangoproject.com/ticket/36564). Thanks to Tim for spotting that this cleanup was now possible, and to Jacob Walls and Clifford Gama for reviewing!

## Template variable `forloop.length`

Moving on to templates, let’s start with this nice little addition:
> The new variable forloop.length is now available within a for loop.

This small extension makes it possible to write a template loop like this:

```
<ul>
  {% for goose in geese %}
    <li>
      <strong>{{ forloop.counter }}/{{ forloop.length }}</strong>: {{ goose.name }}
    </li>
  {% endfor %}
</ul>
```

Previously, you’d need to refer to the length in an another way, like `{{ geese|length }}`, which is a bit less flexible.

Thanks to Jonathan Ströbele for contributing this idea and implementation in [Ticket #36186](https://code.djangoproject.com/ticket/36186), and to David Smith, Paolo Melchiorre, and Sarah Boyce for reviewing.

## `querystring` template tag enhancements

There are two extensions to [the `querystring` template tag](https://docs.djangoproject.com/en/stable/ref/templates/builtins/#django-template), which was added in Django 5.1 to help with building links that modify the current request’s query parameters.

1. Release note:

   > The `querystring` template tag now consistently prefixes the returned query string with a `?`, ensuring reliable link generation behavior.

   This small change improves how the tag behaves when an empty mapping of query parameters are provided. Say you had a template like this:

   ```
   <a href="{% querystring params %}">Reset search</a>
   ```

   …where `params` is a dictionary that may sometimes be empty. Previously, if `params` was empty, the output would be:

   ```
   <a href="">Reset search</a>
   ```

   Browsers treat this as a link to the same URL *including the query parameters*, so it would not clear the query parameters as intended. Now, with this change, the output will be:

   ```
   <a href="?">Reset search</a>
   ```

   Browsers treat `?` as a link to the same URL *without any query parameters*, clearing them as the user would expect.

   Thanks to Django Fellow Sarah Boyce for spotting this improvement and implementing the fix in [Ticket #36268](https://code.djangoproject.com/ticket/36268), and for Django Fellow Natalia Bidart for reviewing!

   - Release note:

     > The `querystring` template tag now accepts multiple positional arguments, which must be mappings, such as `QueryDict` or `dict`.

     This enhancement allows the tag to merge multiple sources of query parameters when building the output. For example, you might have a template like this:

     ```
     <a href="{% querystring request.GET super_search_params %}">Super search</a>
     ```

     …where `super_search_params` is a dictionary of extra parameters to add to make the current search “super”. The tag merges the two mappings, with later mappings taking precedence for duplicate keys.

     Thanks again to Sarah Boyce for proposing this improvement in [Ticket #35529](https://code.djangoproject.com/ticket/35529), to Giannis Terzopoulos for implementing it, and to Natalia Bidart, Sarah Boyce, and Tom Carrick for reviewing!

## Fin

That’s a wrap! Thank you for reading my highlights. There are plenty more changes to read about in [the release notes](https://docs.djangoproject.com/en/stable/releases/6.0/).

Also, there are always many more behind-the-scenes improvements and bug fixes that don’t make it into the release notes. Optimizations and micro-improvements get merged all the time, so don’t delay, upgrade today!

Thank you to **all 174 people** who contributed to Django 6.0, as counted in [this list](https://gist.github.com/felixxm/99501cdbf6ed5a69295b4cb3f8c21d80) by Mariusz Felisiak.

May your upgrade be swift, smooth, safe, and secure,

—Adam

---

😸😸😸 Check out my new book on using GitHub effectively, **[Boost Your GitHub DX](/tech/2025/11/11/boost-your-github-dx-out-now/)**! 😸😸😸

---

**Subscribe via [RSS](/tech/atom.xml), [Twitter](https://twitter.com/adamchainz), [Mastodon](https://fosstodon.org/%40adamchainz), or email:**

Your email address:

One summary email a week, no spam, I pinky promise.

**Related posts:**

* [Django: what’s new in 5.2](https://adamj.eu/tech/2025/04/07/django-whats-new-5.2/)

**Tags:** [django](https://adamj.eu/tech/tag/django/)© 2025 All rights reserved. Code samples are public domain unless otherwise noted.
