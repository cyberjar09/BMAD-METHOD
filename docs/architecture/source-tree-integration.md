# Source Tree Integration

**Git-Native File-Based Architecture:**

The BI expansion pack implements a **completely git-friendly architecture** using YAML and Markdown files with intelligent sharding patterns. All data storage follows existing BMAD-METHOD patterns, ensuring seamless git operations and collaboration.

**Existing Project Structure Analysis:**

```
BMAD-METHOD/
├── bmad-core/                    # Core framework (NO MODIFICATIONS)
├── expansion-packs/              # Domain-specific extensions
│   ├── bmad-infrastructure-devops/
│   └── bmad-bi-analyst/         # NEW - BI functionality
└── tools/                        # Build and installation utilities
```

**BI Expansion Pack Structure (File-Based):**

```
BMAD-METHOD/expansion-packs/bmad-bi-analyst/
├── agents/
│   └── bi-analyst.md            # Primary BI agent definition
├── tasks/
│   ├── document-bi-integration.md
│   ├── execute-safe-sql.md
│   ├── accumulate-domain-knowledge.md
│   ├── shard-domain-knowledge.md    # Knowledge file sharding task
│   ├── validate-tool-connections.md
│   └── generate-bi-analysis.md
├── templates/
│   ├── bi-analysis-report-tmpl.yaml
│   ├── business-term-tmpl.md        # Template for domain knowledge files
│   └── analysis-pattern-tmpl.md     # Template for SQL pattern files
├── checklists/
│   └── bi-analyst-checklist.md
├── data/
│   ├── sql-safety-rules.md
│   └── sharding-guidelines.md       # File size and organization rules
└── config.yaml
```

**User Project BI Data Structure (Git-Friendly):**

```
project-root/.bmad-bi/
├── config/
│   ├── environments.yaml           # Environment-specific settings
│   ├── warehouses.yaml            # Warehouse connection templates
│   └── bi-tools.yaml              # BI tool discovery configurations
├── domain-knowledge/
│   ├── _index.yaml                 # Knowledge catalog and sharding metadata
│   ├── business-terms/             # Individual business concept files
│   │   ├── customer-lifecycle.md   # < 8KB, single business concept
│   │   ├── revenue-metrics.md      # < 8KB, revenue-related terms
│   │   └── product-analytics.md    # < 8KB, product metrics
│   └── analysis-patterns/          # Successful SQL pattern library
│       ├── cohort-analysis.md      # < 8KB, cohort analysis patterns
│       ├── funnel-analysis.md      # < 8KB, conversion funnel patterns
│       └── retention-analysis.md   # < 8KB, retention calculation patterns
├── tool-discovery/
│   ├── discovered-schemas.yaml     # Schema metadata from warehouse discovery
│   ├── dbt-models.yaml            # Compiled dbt model metadata
│   └── bi-tool-configs.yaml       # Live tool configurations and status
└── audit/
    ├── 2025-08/                    # Month-based organization
    │   ├── sql-executions.yaml     # SQL execution audit log
    │   └── domain-updates.yaml     # Knowledge learning events
    └── current/
        ├── recent-queries.yaml      # Last 50 queries for context
        └── active-patterns.yaml     # Currently validated analysis patterns
```

**Sharding Strategy Following BMAD Patterns:**

**Domain Knowledge Sharding Rules:**
```yaml