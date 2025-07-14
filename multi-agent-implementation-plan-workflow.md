# Multi-Agent Implementation Plan Workflow

## Overview

This workflow creates a comprehensive implementation plan after client approval, providing detailed technical specifications, API contracts, database schemas, UI/UX specifications, and development guidelines. It serves as the technical blueprint that transforms approved features into actionable development specifications.

**Target Audience**: This document guides Claude Code to execute multi-agent implementation planning using approved proposal and research outputs to create detailed technical specifications.

**Prerequisites**:
- Completed project research workflow outputs:
  - `comprehensive-requirements-document.md`
  - `integrated-research-findings.md`
  - Market, technical, UX, and compliance research reports
- **Approved client proposal** (`client-proposal.md`)
- Client confirmation on proposed features and timeline

**Key Outputs**: A complete technical blueprint ready for development teams to implement.

## Workflow Components

### 1. Requirements Analyzer Agent
- **Role**: Parse requirements and prioritize MVP features
- **Model**: Sonnet
- **Deliverables**: 
  - `mvp-feature-analysis.md`
  - Feature prioritization matrix
- **Chat Name**: RequirementsAnalyzer

### 2. API Designer Agent  
- **Role**: Design RESTful/GraphQL APIs and contracts
- **Model**: Opus
- **Deliverables**: 
  - `api-specification.yaml` (OpenAPI 3.0)
  - `api-documentation.md`
- **Chat Name**: APIDesigner

### 3. Database Architect Agent
- **Role**: Design database schema and data models
- **Model**: Opus  
- **Deliverables**: 
  - `database-schema.sql`
  - `data-models.md`
  - `migration-plan.md`
- **Chat Name**: DatabaseArchitect

### 4. Frontend Architect Agent
- **Role**: Design frontend architecture and component hierarchy
- **Model**: Sonnet
- **Deliverables**: 
  - `frontend-architecture.md`
  - `component-hierarchy.md`
  - `state-management-plan.md`
- **Chat Name**: FrontendArchitect

### 5. UI/UX Specification Agent
- **Role**: Create detailed design specifications
- **Model**: Sonnet
- **Deliverables**: 
  - `design-system.md`
  - `page-specifications.md`
  - `interaction-patterns.md`
- **Chat Name**: UIUXSpecAgent

### 6. DevOps Planning Agent
- **Role**: Plan deployment and infrastructure
- **Model**: Haiku
- **Deliverables**: 
  - `deployment-strategy.md`
  - `infrastructure-requirements.md`
- **Chat Name**: DevOpsPlanner

### 7. Integration Architect Agent
- **Role**: Consolidate all plans into cohesive implementation guide
- **Model**: Opus
- **Deliverable**: `comprehensive-implementation-plan.md`
- **Chat Name**: IntegrationArchitect

### 8. Technical Review Agent
- **Role**: Review implementation plan for consistency and feasibility
- **Model**: Opus
- **Deliverable**: `implementation-review-report.md`
- **Chat Name**: TechReviewAgent

### 9. Coordinator (Main Agent)
- **Role**: Orchestrate workflow and monitor progress
- **Chat Name**: PlanCoordinator

## MVP Prioritization Framework

```yaml
feature_prioritization:
  must_have: # Core MVP
    - User authentication & authorization
    - Core business logic
    - Essential CRUD operations
    - Basic UI/UX flow
    - Payment processing (if applicable)
    - Security essentials
    
  should_have: # Enhanced MVP
    - Advanced search/filtering
    - Notification system
    - Basic analytics
    - API rate limiting
    - Data export functionality
    
  nice_to_have: # Post-MVP
    - Advanced analytics dashboard
    - AI/ML features
    - Third-party integrations
    - Advanced customization
    - Multi-language support
```

## Detailed Workflow

### Phase 0: Setup and Initialization

```bash
# Create project structure
mkdir -p ./implementation-plan/{api,database,frontend,design,infrastructure}
PROJECT_ID="IMPL-$(date +%Y%m%d-%H%M%S)"

# Set up chat room
ROOM_NAME="implementation-plan-$PROJECT_ID"

# Chat message standards
[ANALYZING] Processing requirements for [component]
[DESIGNING] Creating [specification type]
[MVP] Feature classified as [must_have/should_have/nice_to_have]
[DEPENDENCY] Identified dependency: [description]
[COMPLETE] [Agent] finished [deliverable]
[REVIEW] Issue found: [description]
[APPROVED] Plan component approved
```

### Phase 1: Requirements Analysis and MVP Definition

#### Step 1.1: Launch Requirements Analyzer
```
mcp__ccm__claude_code:
  model: "sonnet"
  prompt: |
    You are the Requirements Analyzer for implementation planning.
    
    Join chat "$ROOM_NAME" as "RequirementsAnalyzer"
    
    1. Read client-proposal.md (approved proposal)
    2. Read comprehensive-requirements-document.md
    3. Read integrated-research-findings.md
    4. Extract approved features from proposal:
       - MVP features client has approved
       - Any adjustments made during proposal review
       - Confirmed timeline and budget
    5. Create mvp-feature-analysis.md with:
       - Approved feature list with technical specifications
       - Development effort estimates
       - Technical dependencies mapping
       - Implementation risk assessment
    6. Report: "[MVP] Processing X approved features for implementation"
```

### Phase 2: Parallel Technical Design

**User Checkpoint #1 - Technical Approach Confirmation**
```
Technical Planning Ready

Approved Features (from client proposal):
✓ [Feature 1] - Technical approach: [approach]
✓ [Feature 2] - Technical approach: [approach]
✓ [Feature 3] - Technical approach: [approach]

Proposed Technical Stack:
- Frontend: [Framework recommendation]
- Backend: [Framework recommendation]
- Database: [System recommendation]
- Hosting: [Platform recommendation]

Architecture Pattern: [e.g., RESTful API, Microservices]
Authentication: [e.g., JWT, OAuth2]

Development estimate aligns with approved timeline: 8 weeks

Proceed with this technical approach? (yes/adjust):
```

#### Step 2.1: Launch Design Agents Concurrently

**API Designer Agent**:
```
mcp__ccm__claude_code:
  model: "opus"
  prompt: |
    You are the API Designer. Use OpenAPI 3.0 specification.
    
    Join chat "$ROOM_NAME" as "APIDesigner"
    Report: "[DESIGNING] Starting API specification"
    
    Based on MVP features from mvp-feature-analysis.md:
    1. Design RESTful API endpoints
    2. Define request/response schemas
    3. Include authentication flows
    4. Error response standards
    5. Versioning strategy
    6. Rate limiting rules
    
    Create:
    - api-specification.yaml (OpenAPI 3.0)
    - api-documentation.md (developer guide)
    
    Include in specification:
    - Clear endpoint naming (/api/v1/resource)
    - Consistent HTTP methods usage
    - Status codes and error formats
    - Request validation rules
    - Response pagination
    
    Report: "[COMPLETE] API specification ready"
```

**Database Architect Agent**:
```
mcp__ccm__claude_code:
  model: "opus"
  prompt: |
    You are the Database Architect.
    
    Join chat "$ROOM_NAME" as "DatabaseArchitect"
    
    Design database schema for MVP features:
    1. Analyze data requirements from requirements
    2. Design normalized schema (3NF minimum)
    3. Plan for scalability
    4. Include indexes for performance
    5. Design migration strategy
    
    Create:
    - database-schema.sql (CREATE statements)
    - data-models.md (ER diagrams in mermaid)
    - migration-plan.md (versioned migrations)
    
    Consider:
    - User data privacy (encryption needs)
    - Audit trails
    - Soft deletes
    - Performance optimization
    - Backup strategy
    
    Report: "[COMPLETE] Database schema designed"
```

**Frontend Architect Agent**:
```
mcp__ccm__claude_code:
  model: "sonnet"
  prompt: |
    You are the Frontend Architect.
    
    Join chat "$ROOM_NAME" as "FrontendArchitect"
    
    Based on project type and requirements:
    1. Select appropriate framework (React/Vue/Angular)
    2. Design component architecture
    3. Plan state management (Redux/Zustand/Context)
    4. Routing structure
    5. Data fetching patterns
    
    Create:
    - frontend-architecture.md
    - component-hierarchy.md
    - state-management-plan.md
    
    Include:
    - Folder structure
    - Component naming conventions
    - Reusable component library
    - Performance optimization strategy
    - Testing approach
    
    Report: "[COMPLETE] Frontend architecture ready"
```

**UI/UX Specification Agent**:
```
mcp__ccm__claude_code:
  model: "sonnet"
  prompt: |
    You are the UI/UX Specification Agent.
    
    Join chat "$ROOM_NAME" as "UIUXSpecAgent"
    
    Using UX research findings:
    1. Define design system:
       - Color palette (primary, secondary, accent)
       - Typography (fonts, sizes, weights)
       - Spacing system (8px grid)
       - Component styles
    2. Specify each page/screen
    3. Define interaction patterns
    4. Accessibility guidelines
    5. Responsive breakpoints
    
    Create:
    - design-system.md (complete style guide)
    - page-specifications.md (wireframe descriptions)
    - interaction-patterns.md (user flows)
    
    Include specific values:
    - Primary color: #[hex]
    - Font family: [specific fonts]
    - Button styles, form elements
    - Loading states, error states
    
    Report: "[COMPLETE] Design specifications ready"
```

### Phase 3: Infrastructure and DevOps Planning

#### Step 3.1: Launch DevOps Planner
```
mcp__ccm__claude_code:
  model: "haiku"
  prompt: |
    You are the DevOps Planning Agent.
    
    Join chat "$ROOM_NAME" as "DevOpsPlanner"
    
    Plan deployment strategy for $20k budget:
    1. Choose hosting platform (AWS/GCP/Azure/Vercel)
    2. Estimate infrastructure costs
    3. CI/CD pipeline design
    4. Monitoring and logging
    5. Security considerations
    
    Create:
    - deployment-strategy.md
    - infrastructure-requirements.md
    
    Consider:
    - Auto-scaling needs
    - Database hosting
    - CDN requirements
    - SSL certificates
    - Backup automation
    
    Report: "[COMPLETE] Infrastructure plan ready"
```

### Phase 4: Integration and Consolidation

#### Step 4.1: Launch Integration Architect
```
mcp__ccm__claude_code:
  model: "opus"
  prompt: |
    You are the Integration Architect.
    
    Join chat "$ROOM_NAME" as "IntegrationArchitect"
    
    1. Read all technical specifications
    2. Identify integration points
    3. Resolve any conflicts
    4. Create implementation sequence
    5. Generate comprehensive-implementation-plan.md
    
    Structure:
    # Comprehensive Implementation Plan
    
    ## Executive Summary
    - Project overview
    - Technology stack
    - Timeline with milestones
    
    ## MVP Features (Phase 1)
    [Detailed feature list with priorities]
    
    ## Technical Architecture
    ### Frontend
    - Framework: [specific choice]
    - Key libraries
    - Build configuration
    
    ### Backend API
    - Framework: [specific choice]
    - Authentication method
    - API endpoints summary
    
    ### Database
    - System: [PostgreSQL/MySQL/MongoDB]
    - Schema overview
    - Migration strategy
    
    ## API Contracts
    [Summary with reference to api-specification.yaml]
    
    ## UI/UX Specifications
    - Design system summary
    - Page layouts
    - Component library
    
    ## Development Guidelines
    - Coding standards
    - Git workflow
    - Testing requirements
    - Documentation standards
    
    ## Infrastructure
    - Hosting platform
    - Deployment process
    - Monitoring setup
    
    ## Implementation Phases
    ### Week 1-2: Foundation
    ### Week 3-4: Core Features
    ### Week 5-6: Integration
    ### Week 7-8: Testing & Polish
    
    ## Risk Mitigation
    - Technical risks
    - Timeline risks
    - Mitigation strategies
    
    Report: "[COMPLETE] Comprehensive plan ready"
```

### Phase 5: Technical Review

#### Step 5.1: Launch Technical Review Agent
```
mcp__ccm__claude_code:
  model: "opus"
  prompt: |
    You are the Technical Review Agent.
    
    Join chat "$ROOM_NAME" as "TechReviewAgent"
    
    Review the implementation plan for:
    1. Technical feasibility within budget/timeline
    2. Consistency across all components
    3. Security vulnerabilities
    4. Scalability concerns
    5. Missing technical details
    
    Create implementation-review-report.md:
    - Overall assessment
    - Risk areas
    - Recommendations
    - Approval status
    
    Report: "[APPROVED] or [REVIEW] with specific issues"
```

### Phase 6: Final Delivery Package

```
Implementation Plan Complete!

Deliverables Created:
✓ MVP Feature Analysis
✓ API Specification (OpenAPI 3.0)
✓ Database Schema & Models
✓ Frontend Architecture Plan  
✓ UI/UX Design System
✓ Infrastructure Strategy
✓ Comprehensive Implementation Plan

Technical Stack Selected:
- Frontend: [Framework]
- Backend: [Framework]
- Database: [System]
- Hosting: [Platform]

Development Timeline:
- Week 1-2: Setup & Foundation
- Week 3-4: Core Features
- Week 5-6: Integration & Testing
- Week 7-8: Polish & Deployment

The plan includes:
- Complete API contracts
- Database migrations
- Component specifications
- Deployment procedures
- Testing strategies

Ready for:
1. Client proposal generation
2. Project scaffolding
3. Development team handoff
```

## Plan Validation Checklist

### Technical Completeness
- [ ] All API endpoints documented
- [ ] Database schema covers all entities
- [ ] Frontend routes match features
- [ ] Authentication flow specified
- [ ] Error handling defined

### Design Specifications  
- [ ] Colors with hex values
- [ ] Typography with specific fonts
- [ ] Spacing system defined
- [ ] Component states specified
- [ ] Responsive breakpoints set

### Development Readiness
- [ ] File structure planned
- [ ] Dependencies listed
- [ ] Environment variables identified
- [ ] Testing approach defined
- [ ] Deployment steps clear

## Success Metrics

```yaml
plan_quality_metrics:
  completeness:
    api_coverage: 100%  # All features have endpoints
    ui_coverage: 100%   # All screens specified
    data_coverage: 100% # All data needs addressed
  
  clarity:
    ambiguous_requirements: 0
    missing_specifications: 0
    conflicting_details: 0
  
  feasibility:
    timeline_confidence: >85%
    budget_confidence: >90%
    technical_risk: <15%
```

## Integration Points

### Inputs From Research Workflow
- Requirements document
- Technical research
- UX patterns research
- Compliance requirements

### Outputs To Other Workflows
- **Client Proposal**: Feature list, timeline, technical approach
- **Project Scaffolding**: Technical specs, file structure, dependencies

## Common Patterns

### E-commerce Implementation
```yaml
must_have:
  - Product catalog
  - Shopping cart
  - Checkout flow
  - Payment processing
  - Order management
  - User accounts

api_patterns:
  - /api/v1/products
  - /api/v1/cart
  - /api/v1/orders
  - /api/v1/payments
  - /api/v1/users

database_entities:
  - users
  - products
  - categories
  - cart_items
  - orders
  - order_items
  - payments
```

### SaaS Dashboard Implementation
```yaml
must_have:
  - User authentication
  - Subscription management
  - Core functionality
  - Data visualization
  - User settings
  - Billing integration

frontend_architecture:
  - Auth layout
  - Dashboard layout
  - Settings layout
  - Public layout
  
state_management:
  - User state
  - Subscription state
  - App data state
  - UI state
```

## Workflow Sequence Note

This implementation plan workflow comes **after** client proposal approval. The correct sequence is:

1. **Project Research** → Gather requirements and insights
2. **Client Proposal** → Present features for client approval
3. **Implementation Plan** → Create technical specifications (THIS WORKFLOW)
4. **Project Scaffolding** → Build the actual project

This workflow transforms approved features from the client proposal into detailed technical specifications. It assumes the client has already reviewed and approved:
- The feature list and priorities
- The timeline and budget
- The phased development approach

The implementation plan focuses on **HOW** to build what was approved, not **WHAT** to build (which was decided in the proposal phase).