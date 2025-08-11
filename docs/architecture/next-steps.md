# Next Steps

Based on the completed brownfield architecture and checklist validation, here are the recommended next steps for implementing the BI expansion pack with clear handoff guidance for Story Manager and Developer agents.

**Implementation Priority Sequence:**

## Phase 1: Foundation (Week 1-2)

**Story Manager Handoff:**

Create stories for the following implementation sequence based on this validated architecture:

1. **Story: BI Expansion Pack Foundation Setup**
   - Reference: This architecture document sections 7 (Source Tree Integration) and 8 (Infrastructure Integration)
   - Key Requirements: Directory structure, basic configuration files, installation methods
   - Integration Checkpoints: Verify zero bmad-core modifications, validate expansion pack structure
   - Success Criteria: `npx bmad-method install --expansion=bi-analyst` creates proper `.bmad-bi/` structure

2. **Story: SQL Safety Engine Implementation**
   - Reference: Architecture sections 5 (Component Architecture) and 11 (Security Integration)
   - Key Requirements: SQL classification (SAFE/MUTATION/BLOCKED), user approval workflow
   - Integration Requirements: Must integrate with existing bash command patterns
   - Success Criteria: All SQL operations properly classified before execution

3. **Story: Basic BI Agent Definition**
   - Reference: Existing agent patterns from bmad-core, architecture section 5 (Component Architecture)
   - Key Requirements: Agent persona, commands (*execute-safe-sql, *document-bi-integration)
   - Integration Requirements: Follow existing BMAD agent command structure exactly
   - Success Criteria: BI agent activates and responds to basic commands

## Phase 2: Core Functionality (Week 3-4)

**Developer Handoff:**

For developers implementing the architecture:

**Core Implementation Guidance:**
- **Reference Document:** This architecture document with focus on sections 4 (Data Models), 5 (Component Architecture), 9 (Coding Standards)
- **Technical Constraints:** Use existing BMAD standards, bash/curl for API integration, YAML/Markdown for data storage
- **Key Integration Points:** Must consume existing architecture.md files (read-only), integrate with story workflow
- **Critical Safety Requirements:** All SQL must pass through safety classification before execution

**Implementation Sequence:**
1. **Domain Knowledge Manager:** File-based YAML/Markdown storage with simple sharding
2. **Tool Integration Manager:** dbt project analysis, BI tool discovery via curl/API calls
3. **SQL Generation Engine:** Generate SQL using domain knowledge, validate against dbt model resolution

**Developer-Specific Integration Requirements:**
- **Existing Codebase Compatibility:** Zero modifications to bmad-core files or existing agents
- **Error Handling Standards:** Simple bash error handling, user-friendly messages
- **Testing Approach:** Task-based manual testing, integration with existing `npm run validate`
- **Documentation Requirements:** All functionality documented in expansion pack README

## Phase 3: Learning and Intelligence (Week 5-6)

4. **Story: Error Learning and Pattern Recognition**
   - Reference: Architecture section 10 (Testing Strategy - Enhanced with Error Learning)
   - Key Requirements: User-guided pattern saving, successful/failed pattern storage
   - Integration Requirements: Simple user elicitation, no complex automation
   - Success Criteria: System learns from successes and failures through user validation

5. **Story: dbt-Aware SQL Generation**
   - Reference: Architecture sections 6 (API Design) and enhanced testing strategy
   - Key Requirements: dbt model compilation, schema resolution, SQL pattern generation
   - Integration Requirements: Graceful fallback when dbt unavailable, clear error messages
   - Success Criteria: Generate SQL using actual warehouse table names, not dbt model references

## Critical Implementation Checkpoints

**Architecture Integrity Validation:**
- [ ] Zero files modified in bmad-core directory
- [ ] All BI functionality contained within expansion pack structure
- [ ] Existing agents and workflows unaffected by BI installation
- [ ] Standard BMAD installation process handles BI expansion pack

**SQL Safety Validation:**
- [ ] All SQL operations classified before execution
- [ ] MUTATION operations require explicit user approval
- [ ] BLOCKED operations prevented completely
- [ ] Audit logging captures all SQL activity without exposing sensitive data

**Integration Validation:**
- [ ] BI agent consumes existing architecture.md documents without modification
- [ ] Story workflow integration works through existing PM→Architect flow
- [ ] Environment variable patterns match existing BMAD security practices
- [ ] Template system processes BI-specific templates correctly

## Risk Mitigation During Implementation

**Address Checklist-Identified Risks:**

1. **dbt Compilation Failures:** Include comprehensive troubleshooting in first development iteration
2. **Knowledge File Growth:** Implement sharding automation early to prevent file size issues
3. **Git Conflict Resolution:** Document conflict resolution workflow before multi-user testing

**Development Environment Setup:**
- Validate both npm and subrepo installation methods work correctly
- Test environment variable configuration across macOS/Unix environments
- Ensure all required tools (curl, jq, dbt CLI) available and documented

**Testing Integration:**
- Implement SQL safety classification testing first (highest risk)
- Validate BI tool API connectivity with real development instances
- Test domain knowledge accumulation with actual business questions

## Success Metrics and Validation

**Phase 1 Success Criteria:**
- BI expansion pack installs without errors
- Basic agent commands respond correctly
- SQL safety engine prevents dangerous operations
- Zero impact on existing BMAD functionality

**Phase 2 Success Criteria:**
- Complete BI workflow (discovery → knowledge → SQL generation) works end-to-end
- dbt integration resolves model references to actual warehouse tables
- Tool discovery successfully identifies and configures BI stack components

**Phase 3 Success Criteria:**
- System learns from user interactions and becomes more autonomous over time
- Error patterns prevent repetition of failed approaches
- Successful patterns enable rapid SQL generation for similar business questions

**Final Implementation Readiness:**
This architecture provides a solid foundation for BI expansion pack development. The brownfield approach ensures safe integration while the simplified design patterns make implementation tractable for AI agents. Focus on core SQL safety and domain knowledge functionality first, then expand tool integration capabilities based on actual usage patterns.