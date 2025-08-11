# Data Models and Schema Changes

**Hybrid Data Architecture Strategy with Operational Safeguards:**

The BI expansion pack implements a **git-friendly hybrid storage approach** with comprehensive operational safeguards: static configuration in files, dynamic operational data in file-based YAML/Markdown with intelligent sharding patterns.

**File-Based Knowledge Management:**

```
.bmad-bi/
├── config/
│   ├── environments.yaml        # Environment configurations
│   ├── warehouses.yaml         # Warehouse connection templates  
│   └── bi-tools.yaml           # BI tool configurations
├── domain-knowledge/
│   ├── _index.yaml             # Knowledge catalog and sharding info
│   ├── business-terms/         # Individual business concept files
│   │   ├── customer-lifecycle.md
│   │   ├── revenue-metrics.md
│   │   └── product-analytics.md
│   └── analysis-patterns/      # Successful SQL pattern library
│       ├── cohort-analysis.md
│       ├── funnel-analysis.md
│       └── retention-analysis.md
├── tool-discovery/
│   ├── discovered-schemas.yaml # Schema metadata from warehouse discovery
│   ├── dbt-models.yaml        # Compiled dbt model metadata
│   └── bi-tool-configs.yaml   # Live tool configurations
└── audit/
    ├── 2025-08/                # Month-based log organization
    │   ├── sql-executions.yaml
    │   └── domain-updates.yaml
    └── current/
        ├── recent-queries.yaml  # Last 100 queries for quick reference
        └── learning-events.yaml # Recent knowledge updates
```

**Sharding Strategy Using Existing BMAD Patterns:**

Following the `shard-doc.md` task pattern from bmad-core:

**Business Terms Sharding:**
```markdown
<!-- customer-lifecycle.md -->