# 🧊🍔 iceberger — Filesystem Iceberg Table Inspector (v0.1.0)

iceberger is a lightweight Rust-based CLI tool that inspects Apache Iceberg tables directly from the filesystem,
without requiring any catalog or external service.

It reads the table’s metadata files (metadata/v*.metadata.json) to display core information such as schema, snapshots,
and data file summaries — useful for quick inspection or debugging.

🚀 Quick Start
# Install from source
cargo install --path .

# Inspect a local Iceberg table directory
iceberger info --table-path ./warehouse/sales/orders
iceberger schema --table-path ./warehouse/sales/orders
iceberger snapshots --table-path ./warehouse/sales/orders
iceberger files --table-path ./warehouse/sales/orders --limit 5

📂 Table Layout Assumption

The CLI expects a standard Iceberg directory structure:

/warehouse/sales/orders/
├── data/
│   ├── 00000-0-abc.parquet
│   ├── 00001-0-def.parquet
│   └── ...
└── metadata/
    ├── v1.metadata.json
    ├── v2.metadata.json
    └── version-hint.txt   # optional


If metadata/version-hint.txt exists, its version number determines the current metadata file.

Otherwise, the CLI uses the highest vN.metadata.json file.

No catalog connection (REST, Hive, Glue) is required — everything is read from local files.

⚙️ CLI Overview
Common Options
Option	Description	Required
--table-path <PATH>	Path to the root directory of the Iceberg table	✅
--at <SNAPSHOT_ID or TIMESTAMP>	Read metadata at a specific snapshot or time	❌
--json	Output results in JSON format	❌
--limit <N>	Limit the number of rows/files displayed	❌
🧩 Commands
info

Display high-level table metadata.

iceberger info --table-path ./warehouse/sales/orders


Example (table format):

Table: sales.orders
Format Version : 2
Current Snapshot : 982734982734
Snapshots Count : 17
Schema Version : 3
Partition Specs : 2 (country STRING, dt DATE)
Sort Order : dt ASC, order_id ASC


Example (JSON):

{
  "table": "sales.orders",
  "format_version": 2,
  "current_snapshot_id": "982734982734",
  "snapshot_count": 17,
  "schema_version": 3,
  "partitions": [
    {"name": "country", "type": "string"},
    {"name": "dt", "type": "date"}
  ],
  "sort_order": ["dt ASC", "order_id ASC"]
}

schema

Show the Iceberg table’s current schema.

iceberger schema --table-path ./warehouse/sales/orders


Example (table format):

ID | Name         | Type     | Required
---+--------------+----------+----------
1  | order_id     | long     | yes
2  | customer_id  | string   | no
3  | order_amount | double   | no
4  | dt           | date     | yes

snapshots

List available snapshots and their summary info.

iceberger snapshots --table-path ./warehouse/sales/orders


Example (table format):

Snapshot ID       | Timestamp           | Operation | Added | Removed
------------------ +--------------------+------------+--------+---------
982734982734      | 2025-09-30T23:59Z  | append     | 42     | 0
982734982733      | 2025-09-29T23:59Z  | append     | 37     | 0

files

Display data file summaries for the current (or a specific) snapshot.

iceberger files --table-path ./warehouse/sales/orders --limit 5


Example (table format):

File Path                              | Records | Size(MB) | Partition
---------------------------------------+----------+----------+-------------------------------
data/00000-0-abc.parquet               | 104,231  | 12.4     | dt=2025-10-01, country=KR
data/00001-0-def.parquet               | 95,847   | 11.3     | dt=2025-10-01, country=US

🧮 Internal Behavior (Simplified)

Locate the metadata/ directory under --table-path.

Read version-hint.txt (if present) or pick the latest vN.metadata.json.

Load the metadata using the Rust Iceberg library (iceberg crate with fs feature).

Extract table, schema, snapshot, and manifest information.

Render in either human-readable (tabled) or JSON (serde_json) output.

🚦 Error Handling
Code	Cause	Message Example
2	Invalid path	Invalid table path: ./warehouse/foo
3	Missing metadata	No metadata files found under metadata/
4	Parse error	Failed to parse v2.metadata.json
5	Snapshot not found	Snapshot 12345 not found in table metadata
🧱 Project Structure
iceberger/
 ├─ crates/
 │   ├─ iceberger-core/   # Reads metadata, provides structs & logic
 │   └─ iceberger-cli/    # clap-based command-line interface
 ├─ Cargo.toml
 └─ README.md

📜 License

Dual-licensed under Apache-2.0 and MIT.

✨ Summary

iceberger (v0.1.0) focuses on a single responsibility —
inspect Iceberg table metadata directly from the filesystem with zero dependencies.

No catalog, no network, no configuration —
just point it at a table path and get instant insight into its structure.
