# BMAD Business Intelligence Analyst

The BI Analyst expansion pack extends BMAD-METHOD with specialized Business Intelligence capabilities, enabling SQL analysis, data warehouse exploration, and domain knowledge accumulation through a safe, user-guided workflow.

## Quick Start

### Installation

**Option 1: NPM Installation (Recommended)**
```bash
npx bmad-method install --expansion=bi-analyst
```

**Option 2: Development Installation**
```bash
git submodule add https://github.com/bmadcode/BMAD-METHOD.git bmad-framework
ln -s bmad-framework/expansion-packs/bmad-bi-analyst .bmad-bi-agent
mkdir -p .bmad-bi/{config,domain-knowledge,audit}
```

### Prerequisites

- **Runtime:** Node.js (Latest LTS), bash, curl, jq
- **Data Warehouse:** Access credentials to your data warehouse
- **Optional:** dbt CLI (for enhanced model discovery)
- **Optional:** BI tool API access (Metabase, Looker, etc.)

### Environment Setup

Create environment variables for your data connections:

```bash
# Required
export WAREHOUSE_CONNECTION="your-warehouse-connection-string"

# Optional BI Tool Integration
export METABASE_HOST="https://metabase.company.com"
export METABASE_API_KEY="your-api-key"

# Optional Environment
export BMAD_BI_ENV="development"  # or "production"
```

## Using the BI Analyst

### Activation

Start the BI analyst agent:
```bash
/BMad:agents:bi-analyst
```

The BI analyst will greet you and provide available commands.

### Core Workflow

The BI analyst follows the standard BMAD workflow:
**PM → Architect → BI Analyst** (instead of Developer)

1. **Story Consumption:** BI analyst reads stories created by PM/Architect
2. **Tool Discovery:** Identifies your BI stack (warehouses, dbt, BI tools)
3. **Domain Learning:** Accumulates business knowledge through user interaction
4. **SQL Generation:** Creates safe SQL queries using learned domain knowledge
5. **Analysis Execution:** Executes SQL with safety validation and user approval

### Key Commands

- `*execute-safe-sql` - Execute SQL with safety classification and user approval
- `*document-bi-integration` - Discover and document your BI tool stack
- `*accumulate-domain-knowledge` - Interactive domain knowledge building
- `*generate-analysis` - Generate SQL analysis using domain knowledge
- `*help` - Show all available commands

### Safety Features

**SQL Safety Classification:**
- ✅ **SAFE:** SELECT, DESCRIBE, EXPLAIN, SHOW queries (auto-executed)
- ⚠️ **MUTATION:** INSERT, UPDATE, DELETE, CREATE, DROP (requires user approval)
- 🚫 **BLOCKED:** GRANT, REVOKE, SET system commands (never executed)

**User Approval Workflow:**
```
⚠️  WARNING: This SQL will modify data!
SQL: UPDATE customers SET status = 'active' WHERE id = 123
Do you want to proceed? Type 'YES' to confirm:
```

## Domain Knowledge Learning

The BI analyst learns your business domain through interaction:

### Business Term Mapping
When you ask "Show me customer lifetime value", the agent:
1. **Asks for mapping:** "Which table contains customer lifetime value?"
2. **Learns pattern:** Maps "customer lifetime value" → `customers.clv_calculated`
3. **Saves knowledge:** Stores mapping for future use
4. **Reuses pattern:** Next time generates SQL automatically

### Pattern Recognition
Successful SQL patterns are saved and reused:
- **Monthly trends:** Learns DATE_TRUNC patterns
- **Cohort analysis:** Remembers user retention calculations  
- **Revenue metrics:** Maps business terms to database columns

### Knowledge Files
Domain knowledge is stored in git-friendly files:
```
.bmad-bi/
├── domain-knowledge/
│   ├── business-terms/
│   │   ├── customer-lifecycle.md
│   │   └── revenue-metrics.md
│   └── analysis-patterns/
│       ├── cohort-analysis.md
│       └── funnel-analysis.md
```

## Tool Integration

### dbt Integration

**If dbt project exists:**
```bash
# Agent automatically discovers dbt models
*document-bi-integration
```

The agent will:
1. Compile dbt models: `dbt compile --profiles-dir ~/.dbt`
2. Extract model metadata from `target/manifest.json`
3. Map dbt model names to actual warehouse tables

**If dbt compilation fails:**
The agent suggests common solutions:
- Check `~/.dbt/profiles.yml` configuration
- Verify warehouse connection credentials  
- Ensure model dependencies are available

**Fallback without dbt:**
Agent discovers pure SQL files and warehouse schemas directly.

### BI Tool Integration

**Metabase Integration:**
```bash
# Test connectivity
curl -H "X-Metabase-Session: $METABASE_API_KEY" "$METABASE_HOST/api/user/current"

# Discover databases  
curl -H "X-Metabase-Session: $METABASE_API_KEY" "$METABASE_HOST/api/database"
```

**Other BI Tools:**
Similar curl-based integration patterns for:
- Looker API
- Tableau REST API
- Power BI REST API

## Example Usage Session

```bash
# Activate BI analyst
/BMad:agents:bi-analyst

# Discover BI stack
*document-bi-integration
> Found dbt project, compiling models...
> Discovered Snowflake warehouse
> Metabase connection successful

# Ask business question
> "Show me our top customers by revenue this month"

# First time - agent learns
> Which table contains customer revenue data?
< customer_metrics
> Which column represents monthly revenue?
< monthly_revenue
> Generating SQL...

SELECT customer_id, monthly_revenue 
FROM customer_metrics 
WHERE DATE_TRUNC('month', revenue_date) = DATE_TRUNC('month', CURRENT_DATE)
ORDER BY monthly_revenue DESC 
LIMIT 10;

> Execute this SQL? (y/n): y
> Results: [10 rows returned]
> Save this pattern for similar questions? (y): y
> Pattern saved!

# Next time - agent reuses pattern
> "Show me top customers by revenue last month"
> Using saved pattern...
> [Automatically generates similar SQL with date adjustment]
```

## File Structure

After installation, your project contains:
```
.bmad-bi/
├── config/
│   └── environments.yaml          # Environment configurations
├── domain-knowledge/
│   ├── business-terms/            # Business concept mappings
│   └── analysis-patterns/         # Successful SQL patterns
├── tool-discovery/
│   ├── discovered-schemas.yaml    # Warehouse schema metadata
│   └── dbt-models.yaml           # dbt model metadata
└── audit/
    └── recent-activity.yaml       # SQL execution audit log
```

## Troubleshooting

### Common Issues

**"WAREHOUSE_CONNECTION not set"**
```bash
export WAREHOUSE_CONNECTION="snowflake://user:pass@account/database/schema"
```

**"dbt compilation failed"**
- Check `~/.dbt/profiles.yml` exists and is configured
- Verify warehouse credentials are correct
- Run `dbt debug` to diagnose connection issues

**"API authentication failed"**  
- Verify API key is correct and has proper permissions
- Check API endpoint URL format
- Test connection manually with curl

**"Knowledge files growing too large"**
- Agent automatically shards files when they exceed 8KB
- Each business concept stored in separate file
- Use `*shard-domain-knowledge` if manual sharding needed

### Getting Help

1. **Agent Help:** Use `*help` command for available actions
2. **Debug Mode:** Check `.ai/debug-log.md` for detailed execution logs
3. **Audit Trail:** Review `.bmad-bi/audit/recent-activity.yaml` for SQL history
4. **GitHub Issues:** Report bugs at https://github.com/bmadcode/BMAD-METHOD/issues

## Advanced Usage

### Custom Environment Configuration

```yaml
# .bmad-bi/config/environments.yaml
development:
  warehouse:
    type: "snowflake"
    connection: "${DEV_WAREHOUSE_CONNECTION}"
  metabase:
    host: "${DEV_METABASE_HOST}"
    api_key: "${DEV_METABASE_API_KEY}"

production:
  warehouse:
    type: "snowflake" 
    connection: "${PROD_WAREHOUSE_CONNECTION}"
  metabase:
    host: "${PROD_METABASE_HOST}"
    api_key: "${PROD_METABASE_API_KEY}"
```

### Multiple Warehouse Support

```bash
# Primary warehouse
export WAREHOUSE_CONNECTION="snowflake://primary"

# Secondary warehouse
export SECONDARY_WAREHOUSE_CONNECTION="databricks://analytics"
```

### Pattern Export/Import

Domain knowledge files are portable between projects:
```bash
# Export patterns
cp -r .bmad-bi/domain-knowledge/ /path/to/shared/knowledge/

# Import patterns
cp -r /path/to/shared/knowledge/ .bmad-bi/domain-knowledge/
```

## Security Best Practices

1. **Read-Only Access:** Use read-only database roles when possible
2. **Environment Variables:** Never store credentials in configuration files
3. **API Key Rotation:** Regularly rotate BI tool API keys
4. **Audit Review:** Monitor `.bmad-bi/audit/` for unexpected SQL patterns
5. **User Approval:** Always review MUTATION SQL before approval

## Integration with BMAD Workflow

The BI analyst integrates seamlessly with existing BMAD workflows:

- **Stories:** Consumes PM/Architect stories with BI requirements
- **Templates:** Uses BMAD template system for analysis reports
- **Architecture:** Reads architecture documents for data context
- **Zero Impact:** Existing agents and workflows remain unchanged

For detailed workflow integration, see the main BMAD-METHOD documentation.