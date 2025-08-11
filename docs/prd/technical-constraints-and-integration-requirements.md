# Technical Constraints and Integration Requirements

## Existing Technology Stack

**Core BMAD-METHOD Stack**:
- **Languages**: Node.js (tooling), YAML (configuration), Markdown (templates/documentation)
- **Frameworks**: Natural language agent framework, YAML-driven template processing
- **Database**: File-based knowledge storage, Git-based version control
- **Infrastructure**: CLI-based installation, npm distribution, semantic release automation
- **External Dependencies**: Git, Node.js runtime, optional MCP server ecosystem

**BI Expansion Pack Additional Stack**:
- **Data Warehouses**: Databricks, Snowflake, BigQuery, Redshift (configurable)
- **BI Tools**: Metabase, Tableau, Looker, Power BI (extensible via MCP)
- **Data Transformation**: dbt (detection and integration)
- **Database Connectivity**: SQL Alchemy, JDBC drivers, cloud SDK libraries

## Integration Approach

**Database Integration Strategy**: 
- File-based domain knowledge storage following BMAD patterns (`.bmad-bi/domain-knowledge/`)
- Git-synchronized learning logs with timestamped entries
- Warehouse connections through environment variables and configuration files
- dbt integration through local dbt CLI for model compilation and metadata extraction

**API Integration Strategy**:
- BI tool APIs accessed through documented REST endpoints with API key authentication
- MCP server integration when available, direct API calls as fallback
- Warehouse APIs for metadata queries and connection testing
- Personal collection creation in BI tools for safe testing before user migration

**Frontend Integration Strategy**:
- CLI-based agent interaction maintaining existing BMAD patterns
- Markdown-based analysis report generation using template system
- Story-driven workflow execution through existing Scrum Master coordination
- **NO modifications to existing CLI or agent interfaces**

**Testing Integration Strategy**:
- SQL execution safety testing through operation classification validation
- BI tool integration testing in sandbox/staging environments
- Domain knowledge persistence testing through git commit verification
- MCP server integration testing with fallback validation

## Code Organization and Standards

**File Structure Approach**:
```
expansion-packs/bmad-bi-analyst/
├── agents/bi-analyst.md
├── tasks/document-bi-integration.md
├── tasks/execute-safe-sql.md
├── tasks/accumulate-domain-knowledge.md
├── templates/bi-analysis-report-tmpl.yaml
├── checklists/bi-analyst-checklist.md
├── data/sql-safety-rules.md
└── config.yaml
```

**Naming Conventions**: Follow existing BMAD expansion pack patterns with `bi-` prefix for BI-specific components

**Coding Standards**: Natural language instructions in Markdown, YAML configuration following existing bmad-core patterns, consistent template variable naming (`{{warehouse_type}}`, `{{bi_tool_config}}`)

**Documentation Standards**: Comprehensive task documentation with elicitation requirements, template usage examples, safety model documentation

**CRITICAL ISOLATION CONSTRAINT**: **All BI functionality must be contained within `expansion-packs/bmad-bi-analyst/` directory with zero files added, modified, or referenced in `bmad-core/`**

## Deployment and Operations

**Build Process Integration**: Leverages existing `npm run build` expansion pack bundling, web-builder.js integration for agent packaging **without modifying build scripts**

**Deployment Strategy**: 
- npm distribution following existing expansion pack model
- `npx bmad-method install --expansion=bi-analyst` installation command
- Git-based knowledge storage initialization on first installation
- Environment variable configuration for warehouse and BI tool connections

**Monitoring and Logging**: 
- SQL execution audit logging to `.bmad-bi/logs/sql-audit.log`
- Domain knowledge change tracking through git commit history
- BI tool API interaction logging for debugging and compliance
- Performance metrics for query execution times and knowledge lookup speeds

**Configuration Management**:
- Environment-based configuration for warehouse connections
- `.bmad-bi/config/bi-tools.yaml` for BI tool configurations
- Git-ignored secrets management for API keys and connection strings
- dbt profiles integration for target and connection configuration

## Risk Assessment and Mitigation

**Technical Risks**:
- **SQL Injection**: Mitigated through whitelist-based operation classification and parameterized query generation
- **Warehouse Cost Explosion**: Mitigated through query cost estimation and execution limits
- **Knowledge Consistency**: Mitigated through git-synchronized storage and conflict resolution workflows

**Integration Risks**:
- **Core Modification Temptation**: Mitigated through strict architectural constraints and expansion pack isolation
- **dbt Compilation Failures**: Mitigated through error handling and fallback to manual schema discovery
- **BI Tool API Changes**: Mitigated through version pinning and graceful degradation
- **MCP Server Unavailability**: Mitigated through direct API fallback implementation

**Deployment Risks**:
- **Environment Configuration**: Mitigated through comprehensive setup validation and clear error messaging
- **Permission Issues**: Mitigated through read-only database role requirements and connection testing
- **Version Conflicts**: Mitigated through semantic versioning and compatibility testing

**Mitigation Strategies**:
- Comprehensive testing suite for SQL safety classification
- Sandbox environment requirements for initial BI tool integration
- Rollback capabilities for domain knowledge changes
- Extensive documentation and troubleshooting guides
- **Strict architectural review ensuring zero bmad-core modifications**
