---
name: documentdb-indexing
description: Index-type selection and shape guidance for Azure DocumentDB — when to use single-field, compound (ESR), multikey, wildcard, hashed, 2dsphere, TTL, and vector indexes; query-pattern → index-shape cookbook; per-collection index budget; DocumentDB-specific preference for `textSearch` over community `$text`. Use when designing or reviewing indexes, choosing an index type for a query pattern, or deciding whether an additional index is worth the write cost.
license: MIT
---

# Indexing Strategies — Azure DocumentDB

Companion skill to `documentdb-query-optimizer`. That skill answers *"why is this query slow?"*; this one answers *"which index should I create, and what shape should it take?"*.

Azure DocumentDB supports the standard MongoDB index types. Only `_id` is created automatically — every other index must be created explicitly. Default limit: **64 single-field indexes per collection** (extendable to 300 on request).

The `_id` index is a **B-tree**, created automatically, and cannot be dropped. For sharded collections the `_id` key is composite — it includes a hash of the shard key. All other indexes created via `createIndex` are **RUM** indexes; the exception is geospatial indexes (`2dsphere`, `2d`), which are **GiST** indexes.

## Rules

- [index-single-field](index-single-field.md) — When a single-field index is enough; direction, options (`unique`, `sparse`, `partial`, collation).
- [index-compound-esr](index-compound-esr.md) — Compound index design via ESR (Equality → Sort → Range); prefer one compound over many singles.
- [index-multikey-arrays](index-multikey-arrays.md) — Indexing array fields; the one-array-per-compound (parallel-array) restriction; multikey can't cover queries.
- [index-text-prefer-textsearch](index-text-prefer-textsearch.md) — On Azure DocumentDB, prefer the `textSearch` index + `$search` over community `$text` indexes.
- [index-wildcard-dynamic-schemas](index-wildcard-dynamic-schemas.md) — Wildcard indexes for truly dynamic schemas; cost vs benefit; scope the prefix.
- [index-hashed-shard-keys](index-hashed-shard-keys.md) — Hashed indexes for even distribution; shard-key alignment; range-query caveats.
- [index-2dsphere-geospatial](index-2dsphere-geospatial.md) — GeoJSON types, `[longitude, latitude]` order, `$near` / `$geoWithin` / `$geoIntersects`.
- [index-ttl-expiry](index-ttl-expiry.md) — TTL indexes: `expireAfterSeconds` semantics, date-field requirement, monitoring.
- [index-count-budget](index-count-budget.md) — Keep 5–15 indexes per collection; review `$indexStats`; drop unused.
- [index-lifecycle-drop-hide](index-lifecycle-drop-hide.md) — Safe lifecycle: inventory → detect redundancy → `hideIndex` → `dropIndex`. The `_id` index cannot be dropped.
- [index-pattern-cookbook](index-pattern-cookbook.md) — Query-pattern → index-shape cookbook (equality+sort, multi-equality, range+sort, equality+range, hybrid).
