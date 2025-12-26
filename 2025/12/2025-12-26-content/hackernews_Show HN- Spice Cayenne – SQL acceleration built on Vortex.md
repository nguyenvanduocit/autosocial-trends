---
source: hackernews
title: Show HN: Spice Cayenne – SQL acceleration built on Vortex
url: https://spice.ai/blog/introducing-spice-cayenne-data-accelerator
date: 2025-12-26
---

Product

Platform

[SQL Federation & Acceleration

Query across operational and analytical data sources with local acceleration](/platform/sql-federation-acceleration)[Hybrid SQL Search

Combine vector similarity, full-text, and keyword search in a single SQL query](/platform/hybrid-sql-search)[LLM Inference

Call local or hosted LLMs from the Spice query engine](/platform/llm-inference)

Features

[Secure AI Sandboxing

Safely connect AI to enterprise data](/feature/secure-ai-sandboxing)[AI Model Serving

Serve, evaluate, and ground AI models in the context of your data](/feature/ai-model-serving)[Edge to Cloud Deployments

Deploy Spice anywhere](/feature/edge-to-cloud-deployments)[Real-Time Change Data Capture

Sync accelerated datasets with real-time changes](/feature/real-time-change-data-capture)[Distributed Query

Scale beyond single-node limits](/feature/distributed-query)[MCP Server & Gateway

Run MCP servers locally or over SSE](/feature/mcp-server-gateway)

Solutions

Use cases

[Application Search

Context-aware, hybrid search for apps](/use-case/application-search)[Datalake Accelerator

Up to 100x faster queries](/use-case/datalake-accelerator)[Operational Data Lakehouse

Power operational and analytical workloads from your datalake](/use-case/operational-data-lakehouse)[Secure AI Agents

Deploy secure, scalable AI agents](/use-case/secure-ai-agents)[Retrieval-Augmented Generation

Build more accurate and trustworthy RAG systems](/use-case/retrieval-augmented-generation)

Industries

[Cybersecurity

Data and AI foundation for modern security solutions](/industry/cybersecurity)[Financial Services

Unify real-time, governed financial data](/industry/financial-services)[SaaS

Deliver responsive, personalized, and secure SaaS apps](/industry/saas)

[Pricing](/pricing)

Resources

[Blog

Product updates, customer stories, and technical guides](/blog)[Connectors

Integrations with databases, data warehouses, data lakes, and more](/integrations)[Quick Starts

Get started in minutes](https://spiceai.org/docs/getting-started)[Cookbooks

Find ready-to-use examples](https://spiceai.org/cookbook)[Cloud Docs

Documentation for the Spice Cloud Platform](https://docs.spice.ai/)[Open Source Docs

Documentation for Spice OSS](https://spiceai.org/docs)

Company

[About Us

Powering the next generation of applications](/about-us)[Careers

Build the future of data and AI](/careers)[Integrations

Connect to 30+ data sources](/integrations)[Security

Security you can trust](/security)[Contact Us

Get in touch with the Spice AI team](/contact)

## Introducing Spice Cayenne: The Next-Generation Data Accelerator Built on Vortex for Performance and Scale

Luke Kim

December 17, 2025

![Introducing Spice Cayenne: The Next-Generation Data Accelerator Built on Vortex for Performance and Scale](/vc-ap-d1befa/_next/image?url=https%3A%2F%2Fspiceai.wpenginepowered.com%2Fwp-content%2Fuploads%2F2025%2F12%2FSpice-Cayenne-Blog-Header.png&w=3840&q=75)

#### TLDR

Spice Cayenne is the next-generation Spice.ai data accelerator built for high-scale and low latency data lake workloads. It combines the [Vortex columnar format](https://github.com/vortex-data) with an embedded metadata engine to deliver faster queries and significantly lower memory usage than existing Spice data accelerators, including DuckDB and SQLite. [Watch the demo](https://www.youtube.com/watch?v=HTdv6-cxKV4) for an overview of Spice Cayenne and Vortex.

![](https://spiceai.wpenginepowered.com/wp-content/uploads/2025/12/Spice-Cayenne-Architecture-1-1024x692.png)

#### Introduction

Spice.ai is a modern, [open-source](https://spiceai.org/) SQL query engine that enables development teams to federate, accelerate, search, and integrate AI across distributed data sources. It’s designed for enterprises building data-intensive applications and AI agents across disparate, tiered data infrastructure. Data acceleration of disparate and disaggregated data sources is foundational across [many vertical use cases](https://spiceai.org/docs/use-cases) the Spice platform enables.

Spice leans into the [industry shift to object storage](https://spice.ai/blog/making-object-storage-operational) as the primary source of truth for applications. These object store workloads are often multi-terabyte datasets using open data lake formats like Parquet, Iceberg, or Delta that must serve data and search queries for customer-facing applications with sub-second performance. [Spice data acceleration](https://spiceai.org/docs/features/data-acceleration), which transparently materializes working sets of data in embedded databases like DuckDB and SQLite, is the core technology that makes these applications built on object storage functional. [Embedded data accelerators](https://spiceai.org/docs/components/data-accelerators) are fast and simple for datasets up to 1TB, however for multi-terabyte workloads, a new class of accelerator is required.

So we built [Spice Cayenne](https://spiceai.org/docs/components/data-accelerators/cayenne), the next-generation data accelerator for high volume and latency-sensitive applications.

Spice Cayenne combines [Vortex](https://github.com/vortex-data), the next-generation columnar file format from the Linux Foundation, with a simple, embedded metadata layer. This separation of concerns ensures that both the storage and metadata layers are fully optimized for what each does best. Cayenne delivers better performance and lower memory consumption than the existing DuckDB, Arrow, SQLite, and PostgreSQL data accelerators.

This post explains why we built Spice Cayenne, how it works, when it makes sense to use instead of existing acceleration options, and how to get started.

#### How data acceleration works in Spice

Spice accelerates datasets by materializing them in local compute engines; which can be ApacheDataFusion + Apache Arrow, SQLite, or DuckDB, in-memory or on-disk. This provides applications with high-performance, low-latency queries and dynamic compute flexibility beyond static materialization. It also reduces network I/O, avoids repeated round-trips to downstream data sources, and as a result, accommodates applications that need to access disparate data, join that data, and make it really fast. By bringing frequently accessed working sets of data closer to the application, Spice delivers sub-second, often single-digit millisecond queries without requiring additional clusters, ingestion pipelines, or ETL.

![](https://spiceai.wpenginepowered.com/wp-content/uploads/2025/12/Spice-Acceleration-Image-1-1024x671.png)

To support the wide range of enterprise workloads run on Spice, the platform includes multiple acceleration engines suited to different data shapes, query patterns, and performance needs. The Spice ethos is to offer optionality: development teams can choose the engine that best fits their requirements. These are currently [the following acceleration engines](https://spiceai.org/docs/components/data-accelerators):

* **PostgreSQL:** PostgreSQL is great for row-oriented workloads, but is not optimized for high-volume columnar analytics.
* **Arrow (in-memory):** Arrow is ideal for workloads that need very fast in-memory access and low-latency scans. The tradeoff is that data isn’t persisted to disk and more sophisticated operations like indexes aren’t supported.
* **DuckDB:** DuckDB offers excellent all-around performance for medium-sized datasets and analytical queries. Single file limits and memory usage, however, can become a constraint as data volume grows beyond a terabyte.
* **SQLite:** SQLite is a lightweight option that excels for smaller tables and row-based lookups. SQLite’s single-writer model, file single limits, and limited parallelism make it less ideal for larger or analytical workflows.

#### Why we built Spice Cayenne

Enterprise workloads on multi-terabyte datasets stored in object storage share a common set of pressure points; the volume of data continues to increase, more applications and services are querying the same accelerated tables at once, and teams need consistently fast performance without having to manage extra infrastructure.

Existing accelerators perform well at smaller scale but run into challenges at different inflection points:

* Single-file architectures create bottlenecks for concurrency and updates.
* Memory usage of embedded databases like DuckDB can be prohibitive.
* Database and search index creation and storage can be prohibitive.

These constraints inspired us to develop the next-generation accelerator for petabyte-scale, that keeps metadata operations lightweight, and maintains low-latency, high-performance queries even as dataset sizes and concurrency increase. It also was critically important the underlying technologies aligned with the Spice philosophy of open-source with strong community support and governance.

***Spice Cayenne addresses these requirements by separating metadata and data storage into two complementary layers: the Vortex columnar format and an embedded metadata engine.***

#### Spice Cayenne architecture

![](https://spiceai.wpenginepowered.com/wp-content/uploads/2025/12/Spice-Cayenne-Architecture-1024x692.png)

Cayenne is built with two core concepts:

##### 1. Data: Vortex Columnar Format

Data is stored in [Vortex](https://github.com/vortex-data/vortex), the next-generation open-source, Apache-licensed format under the Linux Foundation.

Compared with Apache Parquet, Vortex provides:

* **100x faster** random access
* **10–20x faster** full scans
* **5x faster** writes
* Zero-copy compatibility with Apache Arrow
* Pluggable compression, encoding, and layout strategies

*Source:* [*Vortex Github*](https://github.com/vortex-data/vortex)

Vortex has a clean separation of logical schema and physical layout, which Cayenne leverages to support efficient segment-level access, minimize memory pressure, and extend functionality without breaking compatibility. It draws on [years of academic and systems research](https://github.com/vortex-data/vortex/blob/develop/README.md) including innovations from projects like YouTube's Procella, FSST, FastLanes, ALP/G-ALP, and MonetDB/X100 to push the boundaries of what’s possible in open-source analytics.

Extensible and community-driven, Vortex is already integrated with tools like Apache Arrow, DataFusion, and DuckDB, and is designed to support Apache Iceberg in future releases. It’s also the foundation of commercial offerings from [SpiralDB](https://spiraldb.com/) and [PolarSignals](https://www.polarsignals.com/blog/posts/2025/11/25/interface-parquet-vortex). Since version 0.36.0, Vortex guarantees backward compatibility of the file format.

##### 2. Metadata Layer

Cayenne stores metadata in an embedded database. SQLite is supported today, but aligned with the Spice philosophy of optionality, the design is extensive for pluggable metadata backends in the future. Cayenne’s metadata layer was intentionally designed as simple as possible, optimizing for maximum ACID performance.

The metadata layer includes:

* Schemas
* Snapshots
* File tracking
* Statistics
* Refreshes

All metadata access is done through standard SQL transactions. This provides:

* A single, local source of truth
* Fast metadata reads
* Consistent ACID semantics
* No external catalog servers
* No scattered metadata files

A single SQL query retrieves all metadata needed for query planning. This eliminates round-trip calls to object storage, supports file-pruning, and reduces sensitivity to storage throttling.

Together, the metadata engine and Vortex format enable Cayenne to scale beyond the limits of single-file engines while keeping acceleration operationally simple.

#### Benchmarks

So, how does Spice Cayenne stack up to the other accelerators?

We benchmarked Cayenne against DuckDB v1.4.2 using industry standard benchmarks (TPC-H SF100 and ClickBench), comparing both query performance and memory efficiency. All tests ran on a 16 vCPU / 64 GiB RAM instance (AWS c6i.8xlarge equivalent) with local NVMe storage. Cayenne was tested with [Spice v1.9.0](https://spiceai.org/blog/releases/v1.9.0).

![](https://spiceai.wpenginepowered.com/wp-content/uploads/2025/12/TPCH-Benchmark-1024x1024.png)

***Cayenne accelerated TPC-H queries 1.4x faster than DuckDB (file mode)*** *and used* ***nearly 3x less memory****.*

![](https://spiceai.wpenginepowered.com/wp-content/uploads/2025/12/Clickbench-Benchmark-1024x1024.png)

***Cayenne was 14% faster than DuckDB file mode, and used 3.4x less memory.***

Spice Cayenne achieves faster query times and drastically lower memory usage by pairing a purpose-built execution engine with the Vortex columnar format. Unlike DuckDB, Cayenne avoids monolithic file dependencies and high memory spikes, making it ideal for production-grade acceleration at scale.

#### Getting started with Spice Cayenne

Use Cayenne by specifying `engine: cayenne` in the Spicepod.yml (dataset configuration).

Following are a few example configurations.

Basic:

```
datasets:
  - from: spice.ai:path.to.my_dataset
    name: my_dataset
    acceleration:
      engine: cayenne
      mode: file
```

Full configuration:

```
version: v1
kind: Spicepod
name: cayenne-example

datasets:
  -  from: s3://my-bucket/data/
     name: analytics_data
     params:
        file_format: parquet
     acceleration:
        engine: cayenne
        enabled: true
        refresh_mode: full
        refresh_check_interval: 1h
```

##### Memory

Memory usage depends on dataset size, query patterns, and caching configuration. Vortex’s design reduces memory overhead by using selective segment reads and zero-copy access.

##### Storage

Disk space is required for:

* Vortex columnar data
* Temporary files during query execution
* Metadata tables

Provision storage according to dataset size and refresh patterns.

#### Roadmap

Spice Cayenne is in beta and still evolving. We encourage users to test Cayenne in development environments before deploying to production.

Upcoming improvements include:

* Index support
* Improved snapshot bootstrapping
* Additional metadata backends
* Advanced compression and encoding strategies
* Expanded data type coverage

The goal for Spice Cayenne stable is for Cayenne to be the fastest, most efficient accelerator across the full range of analytical and operational data and AI workloads at terabyte & petabyte-scale.

#### Conclusion

Spice Cayenne represents a step function improvement in Spice data acceleration, designed to serve multi-terabyte, high concurrency, and low-latency workflows with predictable operations. By pairing an embedded metadata engine with Vortex’s high-performance format, Cayenne offers a scalable alternative to single-file accelerators while keeping configuration simple.

Spice Cayenne is available in beta. We welcome feedback on the road to its stable release.

‍

Share

[![twitter logo](/vc-ap-d1befa/_next/static/media/s-twitter.0ca41e11.svg)](https://twitter.com/intent/tweet?url=)[![linkedin logo](/vc-ap-d1befa/_next/static/media/s-linkedin.61353419.svg)](https://www.linkedin.com/sharing/share-offsite/?url=)![mailto logo](/vc-ap-d1befa/_next/static/media/s-mail.26b136a7.svg)

![copy link logo](/vc-ap-d1befa/_next/static/media/s-copy.be9179b9.svg)

##### Get the latest insights

New releases, tutorials, platform updates, and more.

### See Spice in action

Get a guided walkthrough of how development teams use Spice to query, accelerate, and integrate AI for mission-critical workloads.

[Get a demo](https://meetings.hubspot.com/lukekim/talk-to-sales)

![content stat graphic](/vc-ap-d1befa/_next/static/media/cs-graphic-1.1f2239ad.svg)![content stat graphic](/vc-ap-d1befa/_next/static/media/cs-graphic-2.621a3def.svg)![content stat orb](/vc-ap-d1befa/_next/image?url=%2Fvc-ap-d1befa%2F_next%2Fstatic%2Fmedia%2Fcs-orb.850622d3.png&w=3840&q=75)

Sign up for the latest news from Spice AI

Platform[SQL Federation & Acceleration](/platform/sql-federation-acceleration)[Hybrid SQL Search](/platform/hybrid-sql-search)[LLM Inference](/platform/llm-inference)[Pricing](/pricing)

Features[Secure AI Sandboxing](/feature/secure-ai-sandboxing)[AI Model Serving](/feature/ai-model-serving)[Edge to Cloud Deployments](/feature/edge-to-cloud-deployments)[Real-Time Change Data Capture](/feature/real-time-change-data-capture)[Distributed Query](/feature/distributed-query)[MCP Server and Gateway](/feature/mcp-server-gateway)

Use Cases[Application Search](/use-case/application-search)[Datalake Accelerator](/use-case/datalake-accelerator)[Operational Data Lakehouse](/use-case/operational-data-lakehouse)[Secure AI Agents](/use-case/secure-ai-agents)[Retrieval-Augmented Generation](/use-case/retrieval-augmented-generation)

Industries[Cybersecurity](/industry/cybersecurity)[Financial Services](/industry/financial-services)[SaaS](/industry/saas)

Resources[Blog](/blog)[Connectors](https://spiceai.org/docs/components/data-connectors)[Quick Starts](https://spiceai.org/docs/getting-started)[Cookbooks](https://spiceai.org/cookbook)[Cloud Docs](https://docs.spice.ai/)[Open Source Docs](https://spiceai.org/docs)

Company[About Us](/about-us)[Careers](/careers)[Integrations](/integrations)[Security](/security)[Contact Us](/contact)

[![github logo](/vc-ap-d1befa/_next/static/media/github.eb3a52eb.svg)](https://github.com/spiceai/spiceai)[![twitter logo](/vc-ap-d1befa/_next/static/media/twitter.1c2e8938.svg)](https://x.com/spice_ai)[![linkedin logo](/vc-ap-d1befa/_next/static/media/linkedin.fb0379dc.svg)](https://linkedin.com/company/spice-ai)[![slack logo](/vc-ap-d1befa/_next/static/media/slack.3062e8c3.svg)](https://spiceai.org/slack)

![partner-logo](/vc-ap-d1befa/_next/image?url=https%3A%2F%2Fspiceai.wpenginepowered.com%2Fwp-content%2Fuploads%2F2025%2F11%2FLot_Network_Logo_600_162_2.png&w=256&q=75)

![partner-logo](https://spiceai.wpenginepowered.com/wp-content/uploads/2025/11/67a3c37c74d2dbbec1914c65_nvidia-badge.svg)

![partner-logo](/vc-ap-d1befa/_next/image?url=https%3A%2F%2Fspiceai.wpenginepowered.com%2Fwp-content%2Fuploads%2F2025%2F11%2F65d7d2394b746ff6bf2a70b3_SOC2.png&w=256&q=75)

![partner-logo](/vc-ap-d1befa/_next/image?url=https%3A%2F%2Fspiceai.wpenginepowered.com%2Fwp-content%2Fuploads%2F2025%2F11%2F68928a821a1f274c39e9f993_databricks-validated-partner.png&w=256&q=75)

[Privacy Policy](https://docs.spice.ai/legal/privacy)[Terms of Service](https://docs.spice.ai/legal/terms)

©2025 Spice AI, Inc. All rights reserved.
