# API Design and Integration

**CLI-Native API Integration Strategy:**

The BI expansion pack implements **bash/curl-based API integration** following established BMAD-METHOD patterns. All external API interactions use standard command-line tools available on all systems, maintaining consistency with existing agent architectures.

**Authentication Integration:**
- **Environment Variable Pattern**: API keys and tokens accessed via `$VARIABLE_NAME` in bash commands
- **Configuration File Integration**: Connection templates reference environment variables, never store secrets
- **Multi-Environment Support**: Environment-specific variable naming (e.g., `$DEV_METABASE_API_KEY`, `$PROD_METABASE_API_KEY`)

**Versioning Approach:**
- **API Version Headers**: curl commands include explicit API version headers where supported
- **Graceful Degradation**: Tasks include fallback commands for older API versions
- **Version Detection**: Initial API discovery includes version detection via curl

**External API Integration Specifications:**

## Metabase API Integration
- **Purpose:** Validate connectivity, discover databases, extract metadata
- **Documentation:** https://www.metabase.com/docs/latest/api-documentation
- **Implementation:** Direct curl commands in agent tasks
- **Authentication:** API Key via X-Metabase-Session header

**Connectivity Validation:**
```bash