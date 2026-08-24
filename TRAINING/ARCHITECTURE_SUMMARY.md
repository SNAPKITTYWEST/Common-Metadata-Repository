# Common-Metadata-Repository — Architecture Summary

## Domain
NASA Earth Science metadata catalog — ingests, stores, indexes, and serves metadata records for Earth observation datasets and associated services

## Architecture Type
Clojure microservices monorepo — eight independently deployable JVM services sharing a set of libraries; services communicate via HTTP (transmit-lib) and AWS SQS/SNS; persistent state split between Oracle (metadata-db) and Elasticsearch (indexer/search); Redis for distributed caching

## Primary Language
Clojure

## Major Components
- search-app — public REST/AQL/STAC search API backed by Elasticsearch
- ingest-app — provider-facing metadata ingestion; validates, stores, and triggers indexing
- indexer-app — consumes ingest events, transforms concepts, writes to Elasticsearch
- metadata-db-app — durable Oracle-backed concept store with CRUD and history
- access-control-app — ACL groups and permissions, backed by Elasticsearch
- bootstrap-app — bulk migration and reindex utility (core-async + SQS dispatch)
- virtual-product-app — derives virtual granules from real products
- mock-echo-app — stub ECHO/URS for local development and integration tests
- subscription — Python Lambda worker (SQS polling + SNS fanout for data change notifications)
- umm-spec-lib — Unified Metadata Model parsers, validators, and format converters
- elastic-utils-lib — Elasticsearch query construction, index management, doc-values field mapping
- message-queue-lib — AWS SQS/SNS abstraction with in-memory test double
- transmit-lib — typed HTTP stubs for all inter-service calls
- spatial-lib — geodetic and cartesian geometry, orbit intersection, shapefile handling
- acl-lib — ACL fetcher and enforcement helpers
- oracle-lib — Oracle JDBC connection pool and SQL utilities
- redis-utils-lib — Redis cache and hash-cache with embedded test server
- common-lib — cross-cutting: lifecycle, config, concepts, generics, caching, MIME types
- common-app-lib — shared Ring middleware, humanizers, search service protocol
- umm-lib — legacy UMM record definitions (pre-UMM-Spec)
- schemas — JSON Schema files for all generic document types (grid, visualization, order-option, etc.)
- dev-system — single-process dev harness that embeds all services plus Elasticsearch and Redis

## Data Stores
- Oracle (metadata-db-app primary persistence — collections, granules, services, tools, subscriptions, variables, tags, groups, ACLs, associations)
- Elasticsearch (indexer-app writes; search-app reads; access-control-app has own index)
- Redis (distributed caching for ACLs, humanizers, community usage metrics, granule counts, KMS keywords)

## External Interfaces
- AWS SQS — ingest events queue; subscription notification queue; dead-letter queues
- AWS SNS — subscription notification fan-out to end users
- ECHO / Launchpad — token authentication and system permission verification
- NASA URS (Earthdata Login) — user identity lookup
- NASA KMS (Keyword Management Service) — controlled science keyword vocabulary
- EDSC (Earthdata Search Client) — primary consumer of the search API
- Provider data systems — push metadata via ingest-app HTTP endpoints

## Evidence Files Read
- README.md
- project.clj
- search-app/project.clj
- ingest-app/project.clj
- indexer-app/project.clj
- search-app/src/cmr/search/routes.clj
- search-app/src/cmr/search/api/routes.clj
- search-app/src/cmr/search/services/query_service.clj
- search-app/src/cmr/search/services/query_execution.clj
- ingest-app/src/cmr/ingest/services/ingest_service.clj
- ingest-app/src/cmr/ingest/services/ingest_service/collection.clj
- ingest-app/src/cmr/ingest/api/routes.clj
- indexer-app/src/cmr/indexer/services/index_service.clj
- metadata-db-app/src/cmr/metadata_db/data/oracle/ (directory listing)
- umm-spec-lib/src/cmr/umm_spec/umm_spec_core.clj
- dev-system/src/cmr/dev_system/system.clj
- subscription/src/subscription_worker.py
- bootstrap-app/src/cmr/bootstrap/services/bootstrap_service.clj
- common-lib/src/cmr/common/concepts.clj
- common-lib/src/cmr/common/generics.clj
- Generics.md
- message-queue-lib/src/cmr/message_queue/queue/ (directory listing)
