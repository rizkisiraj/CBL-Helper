# CBL Session: Database Stress-Testing & Monitoring System

**Started:** 2026-06-21
**Mode:** Learn
**Status:** Investigate Phase - In Progress
**Wiki:** cbl-wiki/

## Engage Phase

### Big Idea
Scalability

### Essential Question
"How do I know when my current database design will break, and what do I do before it does?"

### Challenge
Design a database stress-testing and monitoring system that detects bottlenecks before they cause failures in production — and use each bottleneck as a lens into the broader system design patterns (caching, load balancing, queues) that solve it at scale.

---

## Investigate Phase

**Status:** In Progress
**Started:** 2026-06-21
**Familiarity Level:** Total beginner

### Guiding Questions

#### Knowledge — Foundations (start here)
- [x] What is a database, and how is it different from just storing data in a file?
- [x] What is SQL, and what are the basic operations (SELECT, INSERT, UPDATE, DELETE)?
- [x] What is a table, a row, and a column — how is data organized?
- [x] What are primary keys and foreign keys — how do tables relate to each other?
- [ ] What is a transaction, and what does "ACID" mean?
- [ ] What is PostgreSQL, and how is it different from MySQL, SQLite, or MongoDB?

#### Knowledge — What even is a bottleneck?
- [x] What is a database bottleneck, and why does it matter?
- [x] What's the difference between a stress test, a load test, and a benchmark?
- [ ] What is a query, and how does the database decide how to run it?
- [x] What is an index, and why does it make queries faster?
- [ ] What is a connection pool, and why can't I just open a new connection every time?

#### Knowledge — How databases fail under load
- [ ] What actually happens when too many users hit a database at once?
- [ ] What is "lock contention" and how does it slow things down?
- [ ] What is a deadlock, and how does the database handle it?
- [ ] What is the N+1 query problem, and why is it a trap?
- [ ] What does "disk I/O" mean, and why is reading from disk slow?

#### Knowledge — Metrics and monitoring
- [x] What is TPS (transactions per second) and why does it matter?
- [x] What is p95/p99 latency, and why is it better than looking at averages?
- [x] What is cache hit ratio, and what should it be?
- [x] What is a slow query log?

#### Knowledge — System design bridges
- [ ] When a database bottleneck can't be fixed at the DB level, what sits in front of it to help? (caching, load balancing, queues)
- [x] What is a cache (e.g., Redis), and when does it make more sense than optimizing the query?
- [x] What is a message queue, and how does it relieve write pressure on a database?
- [x] What is horizontal scaling vs. vertical scaling — when do you hit the limits of "just get a bigger server"?
- [ ] What is the CAP theorem in simple terms — what tradeoffs exist when scaling a database?

#### Skills — Running a stress test
- [ ] How do I set up a local PostgreSQL database with Docker?
- [ ] How do I seed a database with enough fake data to make tests meaningful?
- [ ] How do I use `pgbench` to run a basic stress test?
- [ ] How do I read `pgbench` output — what do TPS and latency numbers actually tell me?

#### Skills — Diagnosing bottlenecks
- [ ] How do I use `EXPLAIN ANALYZE` to read a query's execution plan?
- [ ] How do I spot a "Seq Scan" (full table scan) and know if it's a problem?
- [ ] How do I add an index and verify it actually helped?
- [ ] How do I check how many connections are active vs. the maximum?

#### Skills — Monitoring setup
- [ ] How do I enable `pg_stat_statements` to see which queries are slowest?
- [ ] How do I read `pg_stat_activity` to see what the database is doing right now?

#### Context — Tools and ecosystem
- [x] What tools exist for stress testing, and which should a beginner start with?
- [x] What is Prometheus, and how does it collect database metrics?
- [x] What is Grafana, and what does a database monitoring dashboard look like?
- [x] What is PgBouncer, and when would I need it?

#### Impact — How will I know it works?
- [ ] How do I establish a "baseline" to compare before vs. after?
- [ ] What numbers prove that a fix actually improved things?
- [x] When is performance "good enough" — what are realistic targets?
- [ ] Can I draw a basic architecture diagram showing where each scalability pattern fits?

#### Correlations / Differences
- [ ] What's the difference between a missing index and connection pool exhaustion — how do symptoms differ?
- [x] How does cache hit ratio relate to disk I/O — what's the connection?
- [x] What's the difference between latency and throughput, and can you have good throughput but bad latency?
- [x] How does `EXPLAIN` differ from `EXPLAIN ANALYZE` — when do I use each?
- [ ] When should I fix the database vs. add a cache vs. add a queue — how do I decide?

#### Limitations
- [ ] Will my local Docker results actually predict what happens in production?
- [x] Can pgbench's default workload represent a real app's behavior?
- [x] What can't I monitor if I use a managed database (RDS, Cloud SQL) instead of a self-hosted one?
- [x] Is there a universal formula for the "right" connection pool size?

### Guiding Activities

1. **Learn the basics** — Follow a beginner SQL tutorial. Create a database, write basic SELECT/INSERT/JOIN queries. Understand tables, keys, and relationships before anything else. *(answers: foundations)*
2. **Set up a playground** — Install Docker, spin up PostgreSQL, create a sample schema with `users` and `orders` tables. Seed with 1M+ rows using a script. *(answers: Docker setup, seeding)*
3. **Run your first stress test** — Use `pgbench` with default TPC-B workload, then a custom script. Record baseline TPS and latency. *(answers: pgbench usage, TPS, latency, baseline)*
4. **Break it on purpose** — Remove indexes, flood connections, write N+1 queries. Observe what happens to TPS and latency each time. *(answers: bottleneck types, what fails under load, N+1)*
5. **Diagnose with EXPLAIN ANALYZE** — Take your slowest queries and read their execution plans. Spot Seq Scans, add indexes, re-run. *(answers: query plans, indexes, before/after proof)*
6. **Monitor with pg_stat_statements** — Enable the extension, run a mixed workload, then query it to find your top 5 slowest queries. *(answers: monitoring, slow query identification)*
7. **Connect bottlenecks to system design patterns** — For each bottleneck you found, research the "next level" solution (cache, queue, horizontal scaling). Write a short note explaining when you'd reach for each. *(answers: system design bridges)*
8. **Draw an architecture diagram** — Sketch a simple system: app → DB, then add the layers you learned about (cache, queue, load balancer) and label when each kicks in. *(answers: impact, big picture understanding)*

### Guiding Resources

- **SQLBolt** (sqlbolt.com): Interactive SQL tutorial — best starting point for the foundations
- **PostgreSQL docs**: `EXPLAIN`, `pg_stat_statements`, `pgbench` — the authoritative reference
- **Docker Getting Started**: official Docker docs for running PostgreSQL containers
- **"Use The Index, Luke"** (use-the-index-luke.com): visual, beginner-friendly guide to database indexing
- **pgbench tutorial**: PostgreSQL wiki + blog posts on Medium/DEV Community
- **ByteByteGo** (blog + YouTube): system design concepts explained visually — great for the "bridge" questions
- **"Designing Data-Intensive Applications" by Martin Kleppmann**: the go-to book for understanding databases and distributed systems (read selectively)
- **Hussein Nasser** (YouTube): deep dives on database internals, connection pooling, indexing — beginner-friendly

### Findings & Analysis

#### 2026-06-21 — Collaborative Research: Claude's 40% (21 questions)
**Activity:** Web research across 21 factual/definitional guiding questions — the simpler lookup questions in the 60/40 split
**Questions Addressed:** 21 questions checked off (foundations ×4, bottleneck definitions ×2, metrics ×4, system design definitions ×2, tools/ecosystem ×4, correlations ×2, limitations ×3)
**Understanding Level:** Working
**New Questions:** None — these were "what is X" questions. The deeper "how" and "why" questions remain for the user.

---

##### Foundations

**Database vs. file:** A database is a structured system for storing, organizing, and retrieving data efficiently. Unlike plain files, databases let multiple users read/write simultaneously without corruption, enforce data validity rules, and search millions of records in milliseconds. Think: throwing papers in a box (file) vs. a filing cabinet with labeled drawers and an index (database).
— Sources: [EDB: Data in the Database vs. the File System](https://www.enterprisedb.com/blog/data-database-vs-file-system), [GeeksforGeeks: File and Database Storage Systems](https://www.geeksforgeeks.org/system-design/file-and-database-storage-systems-in-system-design/)

**SQL basics:** SQL (Structured Query Language) is how you talk to a relational database. Four CRUD operations: **SELECT** (read), **INSERT** (add), **UPDATE** (change), **DELETE** (remove). Like giving precise instructions to a librarian: "find me all books by this author," "add this new book," etc.
— Sources: [DEV Community: SQL Basics Explained](https://dev.to/be11amer/sql-basics-select-insert-update-and-delete-explained-1cgf), [LearnSQL.com: SQL INSERT, UPDATE, DELETE](https://learnsql.com/blog/sql-insert-sql-update-sql-delete-oh-my/)

**Tables, rows, columns:** A **table** holds related data about one thing (like "Students"). **Columns** define attributes (name, email, age) — every entry shares the same data type. **Rows** are individual records — one student with all their info filled in. 500 students = 500 rows.
— Sources: [Cornell: Tables, Columns and Rows](https://cvw.cac.cornell.edu/RelationalDBs/intro/tables-columns-rows), [DEV Community: Key Concepts in Databases](https://dev.to/be11amer/key-concepts-in-databases-tables-rows-columns-and-keys-16c0)

**Primary & foreign keys:** A **primary key** uniquely identifies each row (like a student ID — no duplicates). A **foreign key** in one table points to a primary key in another, creating relationships. This prevents data duplication: reference by ID instead of repeating the full record everywhere. Foreign keys put the "relational" in "relational database."
— Sources: [GeeksforGeeks: Difference between Primary Key and Foreign Key](https://www.geeksforgeeks.org/dbms/difference-between-primary-key-and-foreign-key/), [Microsoft Learn: Primary and Foreign Key Constraints](https://learn.microsoft.com/en-us/sql/relational-databases/tables/primary-and-foreign-key-constraints?view=sql-server-ver17)

##### Bottleneck Basics

**What is a bottleneck:** The single point in your system that limits overall performance — the narrowest part of the pipe. Common causes: inefficient queries, missing indexes, too much data to scan, lock contention, or an overloaded server. Your app is only as fast as its slowest component — a 3-second DB query means users wait 3 seconds no matter how fast the web server is.
— Sources: [LoadView: What is Performance Bottleneck](https://www.loadview-testing.com/learn/performance-bottleneck-load-testing/), [JetRuby: 14 Performance Bottlenecks in Databases](https://jetruby.com/blog/performance-bottlenecks-in-databases-how-to-fix/)

**Stress vs load vs benchmark:** **Load test** = can it handle expected traffic? (1,000 req/sec because that's normal). **Stress test** = at what point does it break? (ramp to 10,000 req/sec until failure). **Benchmark** = standardized comparison between configurations. Analogy: daily traffic on a bridge / trucks until it collapses / comparing this bridge vs that bridge.
— Sources: [BrowserStack: Load Testing vs Stress Testing](https://www.browserstack.com/guide/load-testing-vs-stress-testing), [Gatling: Load testing vs stress testing](https://gatling.io/blog/load-testing-vs-stress-testing)

##### Metrics

**TPS (Transactions Per Second):** Measures how many complete database operations your system processes each second. If your app needs 500 actions/sec but the DB maxes at 300 TPS, users experience delays. It's the first number to watch when stress testing — if TPS drops as you add users, you've found a bottleneck. Note: TPS measures throughput, not latency — a system can have high TPS but poor latency.
— Sources: [phoenixNAP: Transaction per Second](https://phoenixnap.com/glossary/transaction-per-second), [Wikipedia: Transactions per second](https://en.wikipedia.org/wiki/Transactions_per_second)

**p95/p99 latency:** P95 means 95% of requests finished faster than this time; P99 means 99%. Averages lie: 99 requests at 10ms + 1 at 10,000ms = 110ms average (looks fine, but someone waited 10 seconds). Latency distributions are right-skewed — most requests are fast, a long tail is slow. In distributed systems this compounds: 5 parallel API calls each at 99% under 100ms = only 95% chance all five finish in time.
— Sources: [Zoyla: P95, P99, and Why Averages Lie](https://www.zoyla.app/resources/latency-percentiles-guide), [Aerospike: What Is P99 Latency?](https://aerospike.com/blog/what-is-p99-latency/), [Redis: P95 Latency](https://redis.io/blog/p95-latency/)

**Cache hit ratio:** Percentage of data reads served from fast RAM instead of slow disk. Formula: `hits / (hits + misses)`. Target: **99%+**. At 99%, 1 in 100 reads goes to disk. At 95%, 1 in 20 (5× more disk I/O). At 90%, 1 in 10 (10× more). If it drops below 90%, your working data likely exceeds available RAM. PostgreSQL default recommendation: set `shared_buffers` to 25% of system RAM.
— Sources: [Red-Gate: Understanding PostgreSQL's Cache Hit Ratio](https://www.red-gate.com/hub/product-learning/redgate-monitor/understanding-postgresqls-cache-hit-ratio/), [pganalyze: Buffer cache hit ratio in pg_stat_io](https://pganalyze.com/blog/5mins-postgres-16-buffer-cache-hit-ratio-io-times-pg-stat-io)

**Slow query log:** Built-in feature that records every query exceeding a time threshold (e.g., >500ms). In PostgreSQL, set `log_min_duration_statement = 500` in postgresql.conf. Captures query text, duration, rows examined. Like a security camera that only records suspicious activity. For aggregate analysis, use `pg_stat_statements` extension instead (tracks all queries with cumulative stats).
— Sources: [OneUptime: PostgreSQL Slow Query Logging](https://oneuptime.com/blog/post/2026-01-21-postgresql-slow-query-logging/view), [Crunchy Data: Logging Tips for Postgres](https://www.crunchydata.com/blog/logging-tips-for-postgres-featuring-your-slow-queries)

##### System Design Bridges

**Cache (Redis):** A fast in-memory storage layer between your app and database. Redis reads take <1ms vs 5-50ms for DB queries. Use caching when: data is read far more than written, queries are already optimized but still slow due to volume, or many users hit the same expensive computation. **Important caveat:** many teams add Redis to "fix" performance issues that actually need better indexing and query optimization — fix the query first, cache second.
— Sources: [Redis: Cache-Aside Pattern](https://redis.io/learn/howtos/solutions/microservices/caching), [Nordync: Redis vs PostgreSQL Caching 2026](https://www.nordync.com/blog/redis-vs-postgresql-caching-2026)

**Message queue:** A buffer (RabbitMQ, Kafka, SQS) between your app and database. App drops a message and responds immediately; a worker writes to DB at a controlled pace. Absorbs traffic spikes: 10,000 writes in one second → queue absorbs them → worker drains at 500/sec. Tradeoff: writes aren't instant ("eventual consistency"). RabbitMQ handles tens of thousands msg/sec; Kafka handles millions.
— Sources: [ByteByteGo: How to Choose a Message Queue](https://blog.bytebytego.com/p/how-to-choose-a-message-queue-kafka), [AWS: Difference between RabbitMQ and Kafka](https://aws.amazon.com/compare/the-difference-between-rabbitmq-and-kafka/)

##### Tools & Ecosystem

**Stress testing tools:** **pgbench** (built into PostgreSQL, zero setup — start here), sysbench, HammerDB, k6, JMeter. Beginner command: `pgbench -i mydb` (initialize), then `pgbench -c 10 -T 60 mydb` (10 clients, 60 seconds). Graduate to custom scripts (`-f` flag) for realistic workloads.
— Sources: [PostgreSQL Docs: pgbench](https://www.postgresql.org/docs/current/pgbench.html), [pgbench.com tutorials](https://pgbench.com/tutorials/)

**Prometheus:** Open-source monitoring that **pulls** metrics from HTTP endpoints every ~15 seconds. For PostgreSQL, install `postgres_exporter` — it connects to your DB, runs diagnostic queries (connections, transaction counts, table sizes), and exposes results at `http://localhost:9187/metrics`. Prometheus scrapes that endpoint and stores time-series data. Query with PromQL.
— Sources: [GitHub: postgres_exporter](https://github.com/prometheus-community/postgres_exporter), [Grafana: Postgres exporter](https://grafana.com/oss/prometheus/exporters/postgres-exporter/)

**Grafana:** Visualization tool that turns Prometheus data into interactive dashboards. A typical PostgreSQL dashboard shows: TPS over time, active connections gauge, cache hit ratio, p95/p99 latency, disk I/O, locks & blocking, vacuum activity, replication lag. Community has pre-built dashboards — import by ID (e.g., [#9628](https://grafana.com/grafana/dashboards/9628-postgresql-database/) or [#24298](https://grafana.com/grafana/dashboards/24298-postgresql-monitoring-dashboard/)).
— Sources: [Grafana: PostgreSQL monitoring](https://grafana.com/integrations/postgresql/monitor/), [Grafana: Understand PostgreSQL dashboards](https://grafana.com/docs/learning-paths/postgresql-integration/understand-dashboards/)

**PgBouncer:** Lightweight connection pooler between your app and PostgreSQL. Each PostgreSQL connection uses 5-10MB RAM, so 200 direct connections = 1-2GB just for connections. PgBouncer reuses a small pool of real connections. Need it when: >50 concurrent connections, serverless/Lambda environments, "too many connections" errors. Best mode for web apps: **transaction pooling** (connection assigned only during a transaction, then returned to pool).
— Sources: [PgBouncer.org](https://www.pgbouncer.org/), [Rivestack: PostgreSQL Connection Pooling with PgBouncer](https://rivestack.io/blog/postgresql-connection-pooling-pgbouncer)

##### Correlations

**Cache hit ratio ↔ disk I/O:** They're inversely related. High cache hit ratio (99%+) = almost all reads from RAM = low disk I/O. Dropping ratio = more cache misses falling through to disk = I/O spikes. At 99%, 1 in 100 goes to disk; at 95%, 1 in 20 (5× more I/O); at 90%, 1 in 10 (10× more). When disk I/O climbs on your dashboard, check cache hit ratio first. PostgreSQL check: `SELECT sum(heap_blks_hit) / (sum(heap_blks_hit) + sum(heap_blks_read)) FROM pg_statio_user_tables;`
— Sources: [Grokipedia: PostgreSQL Buffer Cache Hit Ratio](https://grokipedia.com/page/PostgreSQL_Buffer_Cache_Hit_Ratio), [Red-Gate: Understanding PostgreSQL's Cache Hit Ratio](https://www.red-gate.com/hub/product-learning/redgate-monitor/understanding-postgresqls-cache-hit-ratio/)

**EXPLAIN vs EXPLAIN ANALYZE:** `EXPLAIN` shows the planner's **plan** without running the query — fast, safe, but all numbers are estimates. `EXPLAIN ANALYZE` **actually executes** the query and reports real timing + row counts. Use EXPLAIN for quick checks and dangerous queries (DELETE/UPDATE). Use EXPLAIN ANALYZE for diagnosing actual slowness — but wrap destructive statements in `BEGIN; EXPLAIN ANALYZE ...; ROLLBACK;` to avoid changes.
— Sources: [PostgreSQL Docs: EXPLAIN](https://www.postgresql.org/docs/current/sql-explain.html), [Readyset: Demystifying EXPLAIN and EXPLAIN ANALYZE](https://readyset.io/blog/demystifying-explain-and-explain-analyze)

##### Limitations

**pgbench's default workload:** Not representative of real apps. It runs a TPC-B-like workload with uniform random access on small tables — real apps have complex joins, uneven data distribution, and bursty traffic. All sessions update the same row (global balance), which tests lock contention but not typical patterns. Use default for baseline comparisons between configurations; use custom scripts (`-f` flag) for realistic testing.
— Sources: [PostgreSQL Docs: pgbench](https://www.postgresql.org/docs/current/pgbench.html), [Yugabyte: Custom SQL Scripts in pgbench](https://www.yugabyte.com/blog/pgbench-custom-sql-scripts/)

**Managed DB monitoring gaps:** You lose OS-level visibility: no raw disk I/O details, can't run `top`/`htop`, no filesystem access, can't install arbitrary extensions or custom exporters on the host, no kernel tuning (`vm.swappiness`, I/O scheduler). AWS/GCP compensate with CloudWatch/Cloud Monitoring covering the key metrics (connections, CPU, IOPS, replication lag). Usually worth the tradeoff.
— Sources: [AWS: RDS vs Self-Managed Databases](https://www.rapydo.io/blog/aws-rds-vs-self-managed-databases-a-comprehensive-comparison), [Saras Analytics: Amazon RDS Pros and Cons](https://www.sarasanalytics.com/blog/amazon-rds-pros-and-cons)

**Connection pool size:** No universal formula, but a well-known starting point: `pool_size = (core_count × 2) + effective_spindle_count`. For a 4-core SSD server: ~9 connections. Counterintuitively, more connections ≠ more performance — beyond the optimal point, context switching overhead kills throughput. Core count should exclude hyperthreading threads. Practical rule: rarely exceed 2-3× CPU cores. Load test to find your sweet spot.
— Sources: [PostgreSQL Wiki: Number of Database Connections](https://wiki.postgresql.org/wiki/Number_Of_Database_Connections), [HikariCP Wiki: About Pool Sizing](https://github.com/brettwooldridge/HikariCP/wiki/About-Pool-Sizing)

#### 2026-06-23 — Horizontal vs. Vertical Scaling (Discord Case Study)
**Activity:** Read and ingested `raw/discord-database-article.md` — a Medium case study on Discord's database migrations (MongoDB → Cassandra → ScyllaDB)
**Finding:** Horizontal scaling means adding more equal-sized nodes to a cluster rather than upgrading one server. Cassandra does this via a distributed hash ring: each node holds ~2–4 TB; data is partitioned by key and replicated across nodes. Discord scaled from 12 Cassandra nodes (2017, billions of messages) to 177 nodes (2022, trillions of messages) before hitting toil limits (unpredictable latency, on-call constantly paged, maintenance ops too expensive). They moved to ScyllaDB — same architecture but written in C++ (no garbage collector) instead of Java, making it significantly faster. Discord also switched internal services from Go to Rust for further gains.
**Questions Addressed:** What is horizontal scaling vs. vertical scaling — when do you hit the limits of "just get a bigger server"?
**Understanding Level:** Working — can explain the model (nodes/rings/replication) and recognize the signals that a system has hit its limit (toil, unpredictable latency, expensive maintenance).
**New Questions:** None — this was a targeted lookup to close an open question.

---

#### 2026-06-23 — Performance, Indexes & Scaling (Designing Data-Intensive Applications)
**Activity:** Ingested `raw/Martin-Kleppmann---Designing-Data-Intensive-Applications…(2017).pdf` — compiled selectively against the challenge: Ch.1 *Describing Performance* & *Coping with Load*, Ch.3 *Storage & Retrieval*.
**Findings:**
- **Latency vs throughput.** Throughput = ops/sec (the metric for batch work; = TPS in DB land). Response time = what the client sees for one request = service time + network + queueing delay. Latency (strict sense) = just the time a request sits *latent*, waiting. **Yes — you can have high throughput but bad latency**: a system can push huge TPS while individual requests wait in long queues. Throughput counts completions; queueing inflates each request's response time, especially the tail.
- **Percentiles & SLOs.** Response time is a *distribution*, not a number; the mean misleads. Use p50/p95/p99/p999. High percentiles (tail latency) matter because the slowest requests hit the most valuable users (Amazon: 100ms slower → ~1% fewer sales). "Good enough" = meet an explicit **SLO/SLA** stated as percentiles + availability (e.g. p50<200ms, p99<1s, up 99.9%); deliberately *don't* chase p99.99 — diminishing returns dominated by random events.
- **Tail amplification.** Queueing dominates high percentiles; a few slow requests cause **head-of-line blocking**. One slow backend among parallel calls slows the whole end-user request. Measure client-side. Load generators must keep firing independently of responses or they artificially shorten queues and understate latency (relevant to pgbench).
- **Indexes.** An index is an additional structure *derived from* the primary data. Turns an O(n) scan into ~O(log n) lookup. Fundamental trade-off: **well-chosen indexes speed reads but every index slows writes** — so you choose indexes from query patterns, not index everything. **B-trees** are the standard relational index (PostgreSQL): balanced tree of fixed ~4kB pages, height O(log n), updated in place (vs append-only LSM-trees).
- **Scaling up vs out.** Vertical (bigger machine, simpler, has a ceiling) vs horizontal (shared-nothing, many machines). No "magic scaling sauce"; common wisdom — keep a stateful DB on a single node until cost/HA forces distribution. (Reinforces the Discord/Cassandra case study.)
**Questions Addressed:** What is an index, and why does it make queries faster? · When is performance "good enough" — realistic targets? · Latency vs throughput — can you have good throughput but bad latency?
**Understanding Level:** Working — can reason about the latency distribution, set percentile-based targets, and explain the index read/write trade-off.
**New Questions:** None new opened; the deeper DDIA chapters on replication, partitioning, transactions/ACID, and CAP map to still-open questions (`what-is-a-transaction-acid`, `what-is-a-deadlock`, `cap-theorem`, `lock-contention`) and could be ingested in a later pass.

---

## Act Phase
_Not started — run `/project:cbl:act` to begin_
