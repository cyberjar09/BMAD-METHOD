# BI Analyst User Guide

This guide provides step-by-step instructions for using the BMAD BI Analyst effectively.

## Getting Started Checklist

### Before First Use

- [ ] Install BI expansion pack: `npx bmad-method install --expansion=bi-analyst`
- [ ] Set required environment variables (see Environment Setup below)
- [ ] Test warehouse connection
- [ ] Optional: Configure BI tool API access

### Environment Setup

**Required Environment Variables:**
```bash
# Data warehouse connection (required)
export WAREHOUSE_CONNECTION="your-connection-string"

# Example connection strings:
# Snowflake: "snowflake://user:pass@account.region/database/schema"
# Databricks: "databricks://token:@host:443/database"
# PostgreSQL: "postgresql://user:pass@host:5432/database"
```

**Optional Environment Variables:**
```bash
# BI Tool Integration
export METABASE_HOST="https://metabase.company.com"
export METABASE_API_KEY="your-metabase-api-key"

# Environment Selection
export BMAD_BI_ENV="development"  # or "production"
```

**Validate Setup:**
```bash
# Check required variables are set
echo $WAREHOUSE_CONNECTION

# Test BI analyst activation
/BMad:agents:bi-analyst
> *help
```

## First Time Usage

### 1. Initial BI Stack Discovery

```bash
# Activate BI analyst
/BMad:agents:bi-analyst

# Discover your BI environment
*document-bi-integration
```

**What happens:**
- Detects dbt project (if present)
- Tests warehouse connectivity  
- Discovers available BI tools
- Creates initial configuration files

**Expected output:**
```
✓ Warehouse connection successful
✓ Found dbt project at ./dbt_project.yml
✓ Metabase API connection successful
✓ Created .bmad-bi/ configuration structure
```

### 2. First Business Question

Ask a simple business question to start learning:

```bash
> "How many customers do we have?"

# First time - agent will ask for help
> I need to learn about your data structure.
> Which table contains customer information?
< customers

> Which column identifies unique customers?
< customer_id

> Generating SQL...
SELECT COUNT(DISTINCT customer_id) as total_customers FROM customers;

> This is a SAFE query. Execute? (y/n): y
> Result: 12,543 customers

> Should I save this pattern for future customer count questions? (y/n): y
> ✓ Pattern saved to domain knowledge
```

### 3. Building Domain Knowledge

Continue asking related questions to build knowledge:

```bash
> "Show me customer signups by month"

# Agent uses learned table but asks for new column
> I know customers are in the 'customers' table.
> Which column contains the signup date?
< created_date

> Generating SQL...
SELECT DATE_TRUNC('month', created_date) as month, 
       COUNT(*) as signups
FROM customers 
GROUP BY month 
ORDER BY month DESC;

> Execute? (y): y
> ✓ Pattern saved for monthly trend analysis
```

## Everyday Usage Patterns

### Asking Business Questions

**Effective Question Format:**
- ✅ "Show me revenue by product category"
- ✅ "Which customers have the highest lifetime value?"
- ✅ "What's our monthly churn rate?"

**Less Effective:**
- ❌ "Run this SQL: SELECT * FROM table"
- ❌ "Give me a report" (too vague)
- ❌ "Complex join with 5 tables" (start simple)

### Understanding Agent Responses

**When Agent Knows the Answer:**
```bash
> "Show me top customers by revenue"
> Using saved pattern from customer-revenue analysis...
> [SQL generated automatically]
```

**When Agent Needs to Learn:**
```bash
> "Show me product performance"
> I haven't learned about products yet.
> Which table contains product information?
> Which metrics indicate product performance?
```

**When SQL Requires Approval:**
```bash
⚠️  WARNING: This SQL will modify data!
SQL: UPDATE customers SET status = 'inactive' WHERE last_login < '2024-01-01'
Do you want to proceed? Type 'YES' to confirm:
```

### Working with Different SQL Types

**SAFE Queries (Auto-executed):**
- SELECT statements
- DESCRIBE table commands
- EXPLAIN query plans
- SHOW commands

**MUTATION Queries (Require approval):**
- INSERT, UPDATE, DELETE
- CREATE, ALTER, DROP
- Data modification operations

**BLOCKED Queries (Never allowed):**
- GRANT/REVOKE permissions
- System configuration changes
- Administrative commands

## Advanced Usage

### Working with dbt Projects

**If you have a dbt project:**
```bash
# Agent automatically discovers and uses dbt models
*document-bi-integration
> Found dbt project
> Compiling models...
> ✓ 47 models discovered

# Ask questions using dbt model names
> "Show me data from dim_customers model"
> Using dbt model 'dim_customers' → customers table
```

**If dbt compilation fails:**
```bash
> dbt compilation failed - check ~/.dbt/profiles.yml
> Common solutions:
> 1. Run 'dbt debug' to test connection
> 2. Verify warehouse credentials in profiles.yml
> 3. Check model dependencies are available
> Would you like me to help troubleshoot? (y/n)
```

**Without dbt:**
```bash
# Agent falls back to direct table discovery
*document-bi-integration
> No dbt project found
> Discovering warehouse schema directly...
> ✓ Found 23 tables across 4 schemas
```

### Managing Domain Knowledge

**View Current Knowledge:**
```bash
# Check what the agent has learned
ls .bmad-bi/domain-knowledge/business-terms/
> customer-lifecycle.md
> revenue-metrics.md
> product-analytics.md
```

**Manual Knowledge Addition:**
```bash
# Add new business term mapping
*accumulate-domain-knowledge
> What business concept would you like to define?
< customer lifetime value
> Which table and column contains this data?
< customer_metrics.clv_calculated
> ✓ Knowledge added
```

**Knowledge File Structure:**
```markdown
# customer-lifetime-value.md
Business definition: Total revenue expected from customer relationship

**Data Source:** `customer_metrics.clv_calculated`

**SQL Pattern:**
```sql
SELECT customer_id, clv_calculated 
FROM customer_metrics 
WHERE clv_calculated > 1000
ORDER BY clv_calculated DESC;
```

## Troubleshooting Guide

### Connection Issues

**"WAREHOUSE_CONNECTION not set"**
```bash
# Set the connection string
export WAREHOUSE_CONNECTION="your-connection-string"

# Test the connection
*execute-safe-sql
> Test SQL: SELECT 1
```

**"Connection failed"**
- Verify connection string format for your warehouse type
- Check network access to warehouse host
- Validate credentials (username/password or token)
- Check warehouse/database permissions

### dbt Issues  

**"dbt command not found"**
```bash
# Install dbt
pip install dbt-core dbt-snowflake  # or your warehouse adapter

# Verify installation
dbt --version
```

**"dbt compilation failed"**
```bash
# Debug dbt configuration
dbt debug

# Common fixes:
# 1. Create ~/.dbt/profiles.yml
# 2. Update warehouse credentials
# 3. Fix model SQL syntax errors
# 4. Install missing dependencies
```

### BI Tool API Issues

**"Metabase API authentication failed"**
```bash
# Test API key manually
curl -H "X-Metabase-Session: $METABASE_API_KEY" \
     "$METABASE_HOST/api/user/current"

# Check permissions - key needs database access
```

**"API timeout"**
- Check network connectivity to BI tool
- Verify API endpoint URL is correct
- Check if BI tool is experiencing downtime

### Knowledge Management Issues

**"Knowledge file too large"**
```bash
# Agent automatically shards large files
# Manual sharding if needed:
*shard-domain-knowledge customer-metrics.md
```

**"Conflicting knowledge"**
```bash
# Agent detects conflicts and asks for resolution:
> Found conflicting mapping for 'revenue':
> 1. sales.total_amount (saved 2025-08-01)
> 2. revenue.monthly_total (saved 2025-08-05)
> Which should I use? (1/2):
```

### Performance Issues

**"Query taking too long"**
- Add LIMIT clauses to large result sets
- Use WHERE clauses to filter data
- Agent logs slow queries in audit files

**"Knowledge files growing"**
- Agent automatically manages file sizes
- Old patterns can be manually archived
- Use git to track knowledge evolution

## Best Practices

### Security

1. **Use read-only database roles** when possible
2. **Never store credentials** in configuration files
3. **Review MUTATION queries** carefully before approval
4. **Rotate API keys** regularly
5. **Monitor audit logs** for unexpected activity

### Domain Knowledge Management

1. **Start with simple questions** to build foundational knowledge
2. **Use consistent business terminology** when asking questions
3. **Validate agent mappings** - correct mistakes early
4. **Save successful patterns** when prompted
5. **Review knowledge files** periodically for accuracy

### SQL Generation

1. **Ask business questions** rather than technical queries
2. **Build complexity gradually** - start simple, add complexity
3. **Use agent's learned patterns** by asking similar questions
4. **Always review generated SQL** before execution
5. **Save time by reusing patterns** for similar analysis

### Collaboration

1. **Share knowledge files** with team members via git
2. **Document business context** in knowledge files  
3. **Use consistent naming** for business concepts
4. **Review team patterns** before adding conflicting mappings
5. **Export/import patterns** between similar projects

## Getting Help

### Built-in Help
- `*help` - Show available commands
- Agent provides context-specific guidance
- Check `.ai/debug-log.md` for detailed execution logs

### Documentation
- Main README.md for comprehensive reference  
- Architecture documentation for technical details
- BMAD-METHOD docs for workflow integration

### Community Support
- GitHub Issues: https://github.com/bmadcode/BMAD-METHOD/issues
- Include error messages, environment details, and steps to reproduce

### Professional Support
For enterprise deployments requiring custom integrations or advanced features, consider professional support through the BMAD-METHOD community.