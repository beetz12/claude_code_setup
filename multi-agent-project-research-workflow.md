# Multi-Agent Project Research Workflow (with Intelligent Gap Analysis & Requirements Generation)

## Overview

This document describes a comprehensive workflow for conducting thorough project research using multiple specialized agents, intelligent gap analysis, and automated requirements document generation. The workflow coordinates agents using Claude Code MCP (`mcp__ccm__claude_code`) with real-time chat communication and leverages Perplexity Deep for web research.

**Target Audience**: This document serves as a guide for Claude Code to execute multi-agent project research for websites, mobile apps, and cross-platform applications.

**Key Features**:
- **Intelligent Gap Analysis**: Identifies missing information based on project type
- **Perplexity Deep Integration**: Leverages advanced web research capabilities
- **Confidence Scoring**: Transparent assessment of research completeness
- **User Checkpoints**: Validates research direction at critical points
- **Adaptive Research**: Adjusts research depth based on provided information
- **Requirements Generation**: Produces comprehensive, actionable documentation

## Workflow Components

### 1. Project Classifier Agent
- **Role**: Analyze project type and determine research strategy
- **Model**: Haiku (for speed)
- **Deliverable**: Project classification and research roadmap
- **Chat Name**: ClassifierAgent

### 2. Gap Analysis Agent
- **Role**: Identify missing information based on project type
- **Model**: Sonnet
- **Deliverables**: 
  - `project-gap-analysis.md`
  - Research priority matrix
  - Confidence scores for existing info
- **Chat Name**: GapAnalysisAgent

### 3. Market Research Agent
- **Role**: Research market trends, competitors, and user expectations
- **Model**: Sonnet
- **Tools**: Perplexity Deep MCP
- **Deliverables**: 
  - `market-research-report.md`
  - Competitor analysis
  - Market positioning insights
- **Chat Name**: MarketResearchAgent

### 4. Technical Research Agent
- **Role**: Research technical solutions, frameworks, and best practices
- **Model**: Sonnet
- **Tools**: Perplexity Deep MCP
- **Deliverables**: 
  - `technical-research-report.md`
  - Technology recommendations
  - Architecture proposals
- **Chat Name**: TechResearchAgent

### 5. UX Research Agent
- **Role**: Research UX patterns, accessibility standards, and design trends
- **Model**: Sonnet
- **Tools**: Perplexity Deep MCP
- **Deliverables**: 
  - `ux-research-report.md`
  - Design pattern recommendations
  - Accessibility guidelines
- **Chat Name**: UXResearchAgent

### 6. Compliance Research Agent
- **Role**: Research legal, regulatory, and security requirements
- **Model**: Sonnet
- **Tools**: Perplexity Deep MCP
- **Deliverables**: 
  - `compliance-research-report.md`
  - Security requirements
  - Legal considerations
- **Chat Name**: ComplianceAgent

### 7. Integration Specialist Agent
- **Role**: Consolidate all research findings
- **Model**: Opus
- **Deliverable**: `integrated-research-findings.md`
- **Chat Name**: IntegrationAgent

### 8. Requirements Generator Agent
- **Role**: Generate comprehensive requirements document
- **Model**: Opus
- **Deliverable**: `comprehensive-requirements-document.md`
- **Chat Name**: RequirementsAgent

### 9. Review Agent
- **Role**: Review and validate requirements document
- **Model**: Opus
- **Deliverable**: `requirements-review-report.md`
- **Chat Name**: ReviewAgent

### 10. Coordinator (Main Agent)
- **Role**: Monitor chat room, coordinate agents, handle user interactions
- **Chat Name**: Coordinator

## Project Classification System

### Project Types and Research Focus

```yaml
project_types:
  website:
    subtypes: ["e-commerce", "corporate", "blog", "portfolio", "saas", "marketplace"]
    research_focus: ["seo", "performance", "conversion", "content_strategy"]
    
  mobile_app:
    subtypes: ["ios", "android", "cross-platform", "pwa"]
    research_focus: ["app_store_optimization", "platform_guidelines", "offline_capability"]
    
  web_app:
    subtypes: ["dashboard", "collaboration", "productivity", "social", "enterprise"]
    research_focus: ["real-time_features", "scalability", "data_visualization"]
    
  hybrid:
    subtypes: ["web_and_mobile", "multi_platform", "progressive_web_app"]
    research_focus: ["responsive_design", "platform_consistency", "shared_codebase"]
```

## Research Templates

### Gap Analysis Template
```json
{
  "projectId": "PROJ-2024-XXX",
  "projectType": "website|mobile_app|web_app|hybrid",
  "providedInformation": {
    "businessGoals": {"present": true, "completeness": 0.8},
    "targetAudience": {"present": true, "completeness": 0.6},
    "features": {"present": true, "completeness": 0.7},
    "technicalRequirements": {"present": false, "completeness": 0.0},
    "budget": {"present": true, "completeness": 1.0},
    "timeline": {"present": true, "completeness": 0.9}
  },
  "missingCritical": [
    "technical_stack_preferences",
    "integration_requirements",
    "security_requirements"
  ],
  "missingOptional": [
    "branding_guidelines",
    "content_strategy",
    "analytics_requirements"
  ],
  "researchPriorities": [
    {"area": "technical_architecture", "priority": "high", "reason": "No tech stack specified"},
    {"area": "security_compliance", "priority": "high", "reason": "Financial data handling"},
    {"area": "ux_patterns", "priority": "medium", "reason": "Target audience needs clarity"}
  ]
}
```

### Research Query Templates for Perplexity Deep

```yaml
market_research_queries:
  competitors:
    - "top [industry] [project_type] competitors 2024 features comparison"
    - "[industry] [project_type] market leaders user experience analysis"
    - "emerging trends [industry] [project_type] 2024-2025"
    
  user_expectations:
    - "[target_audience] expectations [project_type] [industry] 2024"
    - "user pain points [industry] [project_type] solutions"
    - "[project_type] user retention strategies [industry]"

technical_research_queries:
  tech_stack:
    - "best tech stack [project_type] [scale] [industry] 2024"
    - "[specific_feature] implementation [project_type] frameworks comparison"
    - "performance optimization [project_type] [tech_stack] best practices"
    
  integrations:
    - "[third_party_service] API integration [project_type] guide"
    - "payment gateway integration [project_type] security best practices"
    - "authentication solutions [project_type] [scale] comparison"

ux_research_queries:
  design_patterns:
    - "[project_type] UX patterns [industry] best practices 2024"
    - "accessibility standards [project_type] WCAG compliance"
    - "mobile-first design [project_type] [industry] examples"
    
  user_flows:
    - "[specific_feature] user flow best practices [project_type]"
    - "onboarding experience [project_type] [industry] optimization"
    - "conversion optimization [project_type] UX strategies"
```

## Confidence Scoring System

### Research Completeness Calculation

```python
def calculate_research_confidence(factors):
    """
    Calculate confidence score for research completeness
    Returns: float between 0.0 and 1.0
    """
    weights = {
        'critical_info_coverage': 0.35,      # Coverage of critical gaps
        'market_research_depth': 0.20,       # Competitor/market analysis
        'technical_feasibility': 0.20,       # Technical solution confidence
        'ux_research_quality': 0.15,         # UX pattern research
        'compliance_coverage': 0.10          # Legal/security research
    }
    
    score = sum(factors[key] * weights[key] for key in weights)
    return round(score, 2)
```

### Confidence Thresholds

- **High Confidence (>= 0.85)**: Ready for requirements generation
- **Medium Confidence (0.70-0.84)**: May need targeted additional research
- **Low Confidence (< 0.70)**: Requires user input or expanded research

## Detailed Workflow

### Phase 0: Setup and Initialization

```bash
# Create directory structure
mkdir -p ./project-research/{reports,evidence,requirements}
mkdir -p ./project-history

# Initialize project tracking
PROJECT_ID="PROJ-$(date +%Y%m%d-%H%M%S)"
echo "{\"projectId\": \"$PROJECT_ID\", \"status\": \"initiated\"}" > ./project-history/$PROJECT_ID.json

# Clean up any existing reports
for file in *-report.md requirements-document.md; do
  [ -f "$file" ] && mv "$file" "./project-research/archive/${file%.md}_$(date +%Y%m%d_%H%M%S).md"
done
```

**Chat Room Setup**:
```bash
ROOM_NAME="project-research-$PROJECT_ID"

# Message format standards
[CLASSIFICATION] Project type: [type], Subtype: [subtype]
[GAP_ANALYSIS] Critical gaps: [count], Research priorities identified
[RESEARCH] [Agent] research complete, Confidence: [score]
[CHECKPOINT] User input requested for [topic]
[INTEGRATION] All research consolidated, Overall confidence: [score]
[REQUIREMENTS] Document generation complete
```

### Phase 1: Project Classification and Strategy

#### Step 1.1: Launch Project Classifier
```
mcp__ccm__claude_code:
  model: "haiku"
  prompt: |
    You are the Project Classifier. Analyze this project description:
    "[user's project description]"
    
    1. Join chat "$ROOM_NAME" as "ClassifierAgent"
    2. Identify project type (website/mobile_app/web_app/hybrid)
    3. Determine project subtype and industry
    4. Extract all provided information:
       - Business goals
       - Target audience
       - Features mentioned
       - Technical requirements
       - Budget/timeline
       - Any other details
    5. Create initial classification report
    6. Report: "[CLASSIFICATION] Type: [type], Subtype: [subtype], Industry: [industry]"
```

### Phase 2: Gap Analysis

#### Step 2.1: Launch Gap Analysis Agent
```
mcp__ccm__claude_code:
  model: "sonnet"
  prompt: |
    You are the Gap Analysis Agent for project type: $PROJECT_TYPE
    
    Join chat "$ROOM_NAME" as "GapAnalysisAgent"
    
    1. Review classification and provided information
    2. Based on project type, identify:
       - Critical missing information
       - Optional but valuable information gaps
       - Research priorities
    3. Calculate completeness score for each area
    4. Create project-gap-analysis.md with:
       - Information inventory
       - Gap identification
       - Research priority matrix
       - Confidence scores
    5. Report: "[GAP_ANALYSIS] Critical gaps: [X], Priorities: [list]"
```

### Phase 2.2: User Checkpoint #1 - Research Direction

```
Research Planning Checkpoint

Project: $PROJECT_NAME
Type: $PROJECT_TYPE ($SUBTYPE)

Based on the information provided, I've identified the following research priorities:

Critical Information Gaps:
✗ [Missing critical item 1] - Required for [reason]
✗ [Missing critical item 2] - Required for [reason]

Research Plan:
1. Market Research: Focus on [specific areas]
2. Technical Research: Investigate [specific topics]
3. UX Research: Explore [specific patterns]
4. Compliance: Check [specific requirements]

Estimated research time: [X] minutes

Would you like to:
1. Proceed with this research plan
2. Provide additional information for any gaps
3. Adjust research priorities

Response (1/2/3):
```

### Phase 3: Parallel Research Execution

#### Step 3.1: Launch Research Agents Concurrently

**Market Research Agent**:
```
mcp__ccm__claude_code:
  model: "sonnet"
  prompt: |
    You are the Market Research Agent. Use mcp__perplexity-deep__perplexity_search_web
    
    Join chat "$ROOM_NAME" as "MarketResearchAgent"
    Report: "[PROGRESS] Starting market research"
    
    Based on gaps for $PROJECT_TYPE in $INDUSTRY:
    1. Research top 5-7 competitors
    2. Analyze market trends and user expectations
    3. Identify successful features and patterns
    4. Research pricing models and monetization
    
    Use these Perplexity queries:
    $MARKET_RESEARCH_QUERIES
    
    Create market-research-report.md with confidence scores
    Report: "[RESEARCH] Market research complete, Confidence: [score]"
```

**Technical Research Agent**:
```
mcp__ccm__claude_code:
  model: "sonnet"
  prompt: |
    You are the Technical Research Agent. Use mcp__perplexity-deep__perplexity_search_web
    
    Join chat "$ROOM_NAME" as "TechResearchAgent"
    
    Research based on project requirements:
    1. Recommended tech stacks for $PROJECT_TYPE
    2. Performance optimization strategies
    3. Scalability considerations
    4. Integration possibilities
    5. Security best practices
    
    Create technical-research-report.md
    Report: "[RESEARCH] Technical research complete, Confidence: [score]"
```

**UX Research Agent**:
```
mcp__ccm__claude_code:
  model: "sonnet"
  prompt: |
    You are the UX Research Agent. Use mcp__perplexity-deep__perplexity_search_web
    
    Join chat "$ROOM_NAME" as "UXResearchAgent"
    
    Research:
    1. Best UX patterns for $PROJECT_TYPE in $INDUSTRY
    2. Accessibility requirements (WCAG)
    3. Mobile/responsive considerations
    4. User onboarding best practices
    5. Design system recommendations
    
    Create ux-research-report.md
    Report: "[RESEARCH] UX research complete, Confidence: [score]"
```

**Compliance Research Agent**:
```
mcp__ccm__claude_code:
  model: "sonnet"
  prompt: |
    You are the Compliance Research Agent. Use mcp__perplexity-deep__perplexity_search_web
    
    Join chat "$ROOM_NAME" as "ComplianceAgent"
    
    Research:
    1. Industry-specific regulations
    2. Data privacy requirements (GDPR/CCPA)
    3. Security standards
    4. Accessibility compliance
    5. Platform-specific guidelines
    
    Create compliance-research-report.md
    Report: "[RESEARCH] Compliance research complete, Confidence: [score]"
```

### Phase 4: Research Integration

#### Step 4.1: Launch Integration Specialist
```
mcp__ccm__claude_code:
  model: "opus"
  prompt: |
    You are the Integration Specialist. 
    
    Join chat "$ROOM_NAME" as "IntegrationAgent"
    
    1. Read all research reports:
       - project-gap-analysis.md
       - market-research-report.md
       - technical-research-report.md
       - ux-research-report.md
       - compliance-research-report.md
    2. Identify conflicts or contradictions
    3. Synthesize findings into cohesive insights
    4. Calculate overall research confidence
    5. Create integrated-research-findings.md
    6. Report: "[INTEGRATION] Research consolidated, Confidence: [score]"
```

### Phase 4.2: User Checkpoint #2 - Research Review

```
Research Complete - Review Summary

Overall Research Confidence: 87%

Key Findings:

Market Insights:
• [Top competitor] uses [technology] achieving [metric]
• Users expect [feature] as standard in 2024
• Market gap identified: [opportunity]

Technical Recommendations:
• Recommended stack: [Frontend] + [Backend] + [Database]
• Critical integrations: [List]
• Performance target: [Metric]

UX/Design Direction:
• Design pattern: [Pattern] based on [research]
• Accessibility: WCAG 2.1 AA compliance required
• Mobile-first approach recommended

Compliance Requirements:
• [Regulation] compliance needed for [reason]
• Security standards: [List]

Would you like to:
1. Proceed to requirements generation
2. Request additional research in specific areas
3. Provide feedback on findings

Response (1/2/3):
```

### Phase 5: Requirements Generation

#### Step 5.1: Launch Requirements Generator
```
mcp__ccm__claude_code:
  model: "opus"
  prompt: |
    You are the Requirements Generator Agent.
    
    Join chat "$ROOM_NAME" as "RequirementsAgent"
    
    Using the integrated research findings and original project information,
    create a comprehensive requirements document following this structure:
    
    1. Executive Summary
    2. Project Overview
       - Goals and objectives (from user + research)
       - Target audience (refined from research)
       - Key features (prioritized)
    3. Technical Requirements
       - Tech stack (from research)
       - Architecture (based on scale/needs)
       - Integrations (researched APIs)
    4. UX/UI Requirements
       - Information architecture
       - User flows
       - Design system recommendations
       - Accessibility standards
    5. Functional Requirements
       - Feature specifications
       - User stories with acceptance criteria
    6. Non-Functional Requirements
       - Performance benchmarks
       - Security requirements
       - Scalability plan
    7. Data Management
       - Data models
       - Privacy compliance
    8. Integration Requirements
       - Third-party services
       - API specifications
    9. Testing Strategy
    10. Deployment Plan
    11. Timeline and Milestones
    12. Risk Analysis
    13. Success Metrics
    
    Create comprehensive-requirements-document.md
    Report: "[REQUIREMENTS] Document complete"
```

### Phase 6: Review and Finalization

#### Step 6.1: Launch Review Agent
```
mcp__ccm__claude_code:
  model: "opus"
  prompt: |
    You are the Review Agent.
    
    Join chat "$ROOM_NAME" as "ReviewAgent"
    
    Review the requirements document for:
    1. Completeness (all sections populated)
    2. Consistency (no contradictions)
    3. Feasibility (technical and timeline)
    4. Clarity (actionable requirements)
    5. Alignment (matches user goals)
    
    Create requirements-review-report.md
    Calculate final confidence score
    Report: "[REVIEW] Requirements validated, Confidence: [score]"
```

### Phase 7: Final Delivery

```
Project Research Complete!

Deliverables Created:
✓ Market Research Report
✓ Technical Research Report  
✓ UX Research Report
✓ Compliance Research Report
✓ Integrated Findings
✓ Comprehensive Requirements Document

Overall Confidence: 89%

The requirements document includes:
- Detailed technical architecture
- Complete feature specifications
- UX/UI guidelines
- Implementation roadmap
- Risk mitigation strategies

All research is based on current (2024) best practices and 
industry standards specific to your project type and goals.

Next Steps:
1. Review the comprehensive requirements document
2. Proceed to client proposal generation
3. Get client approval on proposed features
4. Then create detailed implementation plan

Workflow Sequence:
1. Project Research (THIS WORKFLOW) ✓
2. Client Proposal → Get approval
3. Implementation Plan → Technical specs
4. Project Scaffolding → Build project

Would you like to proceed with generating the client proposal?
```

## Adaptive Research Strategies

### Information Completeness Levels

```python
def determine_research_depth(provided_info):
    """Adjust research depth based on provided information"""
    
    if provided_info['completeness'] > 0.8:
        return {
            'strategy': 'targeted',
            'focus': 'validation and enhancement',
            'depth': 'specific gaps only'
        }
    elif provided_info['completeness'] > 0.5:
        return {
            'strategy': 'balanced',
            'focus': 'fill gaps and validate assumptions',
            'depth': 'moderate research all areas'
        }
    else:
        return {
            'strategy': 'comprehensive',
            'focus': 'full discovery',
            'depth': 'deep research all areas'
        }
```

## Success Metrics Dashboard

```
Project Research Summary for $PROJECT_NAME
=========================================
Project Type: $PROJECT_TYPE ($SUBTYPE)
Industry: $INDUSTRY

Research Coverage:
- Market Analysis:      92% ✓
- Technical Research:   88% ✓
- UX/Design Research:   85% ✓
- Compliance Coverage:  90% ✓

Information Completeness:
- Initial: 45%
- After Research: 89%
- Gap Closure Rate: 98%

Time Invested: 25 minutes
Research Queries: 47
Sources Analyzed: 132

Confidence Levels:
- Classification: 95% ✓
- Gap Analysis: 90% ✓
- Research Quality: 87% ✓
- Requirements: 89% ✓

Key Insights Discovered: 23
Actionable Recommendations: 41
Risk Factors Identified: 8
```

## Benefits of This Approach

1. **Intelligent Gap Analysis**: Focuses research on what's actually missing
2. **Parallel Research**: Multiple agents work simultaneously for speed
3. **Perplexity Deep Integration**: Current, comprehensive web research
4. **Confidence Transparency**: Clear metrics on research quality
5. **Adaptive Depth**: Adjusts research based on provided information
6. **Actionable Output**: Requirements document ready for development
7. **Domain Expertise**: Specialized agents for different research areas
8. **Quality Assurance**: Review process ensures completeness

## Example Interaction Flow

```
User: "I need to build an e-commerce website for selling handmade jewelry. 
       Budget is $15-20k, timeline 3 months. Target audience is women 25-45 
       interested in unique, artisanal pieces."

System: [CLASSIFICATION] Type: website, Subtype: e-commerce, Industry: jewelry/fashion
System: [GAP_ANALYSIS] Critical gaps: 3, Priorities: tech stack, payment processing, shipping

System: "I've identified some information gaps. Your budget and timeline are clear, 
         but I'll need to research:
         - Technical requirements (platform, integrations)
         - Market expectations for jewelry e-commerce
         - Compliance needs (payment, shipping)
         Proceed with research? (yes/customize)"

User: "Yes"

System: [RESEARCH] Market research complete, Confidence: 0.88
System: [RESEARCH] Technical research complete, Confidence: 0.85
System: [RESEARCH] UX research complete, Confidence: 0.90
System: [RESEARCH] Compliance research complete, Confidence: 0.92

System: "Research complete! Key findings:
         - Shopify Plus recommended for your scale
         - Instagram integration critical for your market  
         - Mobile-first design essential (67% of audience)
         Shall I generate the full requirements document?"

User: "Yes"

System: [REQUIREMENTS] Document complete
System: "Requirements document created with 89% confidence. 
         Includes technical architecture, feature specs, and implementation roadmap."
```

This workflow creates a systematic, intelligent approach to project research that adapts to the information provided and produces actionable requirements for any development project.