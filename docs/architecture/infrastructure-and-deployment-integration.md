# Infrastructure and Deployment Integration

**Basic Deployment Strategy:**

The BI expansion pack integrates with existing BMAD-METHOD deployment patterns using simple, proven approaches. During active development, both npm and subrepo installation methods are supported, with subrepo method planned for removal once stable.

**Existing Infrastructure Integration:**

**Current BMAD-METHOD Deployment:**
- **Distribution**: npm package with semantic versioning
- **Installation**: CLI-based installation via `npx bmad-method install`
- **Configuration**: File-based configuration with environment variables

**BI Enhancement Deployment:**

**Installation Methods (Temporary Dual Support):**

**Method 1: NPM Installation (Production Ready)**
```bash