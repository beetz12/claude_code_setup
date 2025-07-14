# Multi-Agent Custom Workflow Generator (with Interactive Requirements Gathering & Confidence Scoring)

## Overview

This document describes a comprehensive workflow for generating custom multi-agent workflows through systematic requirements gathering, research, and collaborative workflow design. The workflow uses confidence scoring to ensure thorough understanding before proceeding with workflow creation.

**Target Audience**: This document serves as a guide for Claude Code to execute an interactive workflow generation process with user collaboration at every step.

**Key Features**:
- **Interactive Requirements Gathering**: Open-ended questioning with confidence tracking
- **Perplexity Integration**: Research-driven workflow design
- **User Checkpoints**: Validation at critical decision points
- **Workflow Naming**: User-approved naming before file creation
- **Adaptive Questioning**: Continues until 80% confidence threshold
- **Multi-Agent Collaboration**: When complexity demands it

## Workflow Components

### 1. Requirements Gatherer Agent
- **Role**: Collect and clarify user requirements interactively
- **Model**: Opus (for nuanced understanding)
- **Deliverables**: 
  - `workflow-requirements.md`
  - Confidence score tracking
- **Chat Name**: RequirementsGatherer

### 2. Context Analyzer Agent
- **Role**: Analyze gathered information and identify gaps
- **Model**: Sonnet
- **Deliverables**: 
  - `context-analysis.md`
  - Missing information list
  - Question priorities
- **Chat Name**: ContextAnalyzer

### 3. Research Agent
- **Role**: Research similar workflows and best practices
- **Model**: Sonnet
- **Tools**: Perplexity Search & Deep MCP
- **Deliverables**: 
  - `workflow-research-report.md`
  - Pattern recommendations
- **Chat Name**: ResearchAgent

### 4. Workflow Designer Agent
- **Role**: Design the workflow structure and components
- **Model**: Opus
- **Deliverables**: 
  - `workflow-design-plan.md`
  - Agent architecture
  - Process flow
- **Chat Name**: WorkflowDesigner

### 5. Workflow Writer Agent
- **Role**: Write the actual workflow markdown file
- **Model**: Opus
- **Deliverable**: The custom workflow file
- **Chat Name**: WorkflowWriter

### 6. Review Agent
- **Role**: Review workflow for completeness and quality
- **Model**: Opus
- **Deliverable**: `workflow-review-report.md`
- **Chat Name**: ReviewAgent

### 7. Coordinator (Main Agent)
- **Role**: Facilitate user interaction and coordinate agents
- **Chat Name**: Coordinator

## Confidence Scoring System

### Information Completeness Calculation

```python
def calculate_requirements_confidence(info):
    """
    Calculate confidence in understanding user requirements
    Returns: float between 0.0 and 1.0
    """
    weights = {
        'problem_clarity': 0.25,         # How well we understand the problem
        'desired_outcome': 0.20,         # Clear understanding of end goal
        'context_completeness': 0.15,    # Domain/project context
        'technical_requirements': 0.15,  # Tools, MCPs, constraints
        'workflow_complexity': 0.15,     # Understanding of steps needed
        'success_criteria': 0.10         # How to measure success
    }
    
    score = sum(info[key] * weights[key] for key in weights)
    return round(score, 2)
```

### Confidence Thresholds

- **High Confidence (>= 0.80)**: Ready to create workflow
- **Medium Confidence (0.60-0.79)**: Need specific clarifications
- **Low Confidence (< 0.60)**: Need substantial information

## Question Templates for Requirements Gathering

### Initial Open-Ended Questions

```yaml
opening_questions:
  - "What problem are you trying to solve with this workflow?"
  - "What should the end result look like when the workflow completes?"
  - "Are there specific tools or MCP services that must be used?"
  - "What type of project is this for? (website, mobile app, data processing, etc.)"
  - "Are there any similar workflows you've seen that inspire this?"

context_questions:
  - "What triggers this workflow? (user request, scheduled task, event, etc.)"
  - "Who will be using this workflow? (developers, end users, automated systems)"
  - "What's the typical scale? (one-time use, daily, hundreds of times)"
  - "Are there any constraints? (time limits, budget, technical limitations)"
  - "What could go wrong that the workflow should handle?"

technical_questions:
  - "What files or data does the workflow need to access?"
  - "Should this be multi-agent? What specialized roles do you envision?"
  - "What deliverables should the workflow produce?"
  - "Are there approval checkpoints needed?"
  - "What level of automation vs user interaction is desired?"

success_questions:
  - "How will you know if the workflow succeeded?"
  - "What metrics matter? (speed, accuracy, completeness)"
  - "What's the minimum acceptable outcome?"
  - "Are there any deal-breakers or must-haves?"
```

### Adaptive Follow-Up Questions

```yaml
clarification_patterns:
  vague_problem:
    - "Can you give me a specific example of [vague term]?"
    - "When you say [phrase], what does that mean in your context?"
    - "What currently happens without this workflow?"
    
  missing_technical:
    - "What format should [deliverable] be in?"
    - "Which MCP tools do you have access to?"
    - "What programming languages/frameworks are involved?"
    
  unclear_scope:
    - "Is this a one-time solution or reusable?"
    - "Should this handle edge cases like [example]?"
    - "What's in scope vs out of scope?"
    
  insufficient_detail:
    - "Walk me through the ideal step-by-step process"
    - "What does each agent/step need to accomplish?"
    - "What information flows between steps?"
```

## Detailed Workflow

### Phase 0: Setup and Initialization

```bash
# Create tracking structure
mkdir -p ./workflow-generation/{requirements,research,design}
GENERATION_ID="WF-GEN-$(date +%Y%m%d-%H%M%S)"

# Clean up any existing files
for file in workflow-*.md; do
  [ -f "$file" ] && mv "$file" "./workflow-generation/archive/${file%.md}_$(date +%Y%m%d_%H%M%S).md"
done

# Initialize chat room
ROOM_NAME="workflow-gen-$GENERATION_ID"

# Message format standards
[GATHERING] Collecting requirement: [topic]
[CONFIDENCE] Current understanding: [score]%
[RESEARCH] Investigating [topic]
[CHECKPOINT] User confirmation needed
[DESIGN] Creating workflow structure
[NAMING] Proposing workflow name
[COMPLETE] Workflow ready
```

### Phase 1: Interactive Requirements Gathering

#### Step 1.1: Initial Information Collection

```
Welcome! I'll help you create a custom multi-agent workflow.

To build the most effective workflow for your needs, I'll need to understand:
- What problem you're trying to solve
- What the workflow should accomplish
- Any specific requirements or constraints

Please share whatever information you have about the workflow you need. 
Feel free to be as detailed or high-level as you'd like - I'll ask 
clarifying questions to fill in any gaps.

What workflow would you like to create today?
```

#### Step 1.2: Launch Requirements Gatherer

```
mcp__ccm__claude_code:
  model: "opus"
  prompt: |
    You are the Requirements Gatherer for custom workflow generation.
    
    Join chat "$ROOM_NAME" as "RequirementsGatherer"
    
    1. Analyze user's initial response
    2. Calculate confidence score for each understanding area:
       - Problem clarity (0-1)
       - Desired outcome (0-1)
       - Context completeness (0-1)
       - Technical requirements (0-1)
       - Workflow complexity (0-1)
       - Success criteria (0-1)
    
    3. Create workflow-requirements.md with:
       - What we know
       - Confidence scores
       - What we need to clarify
    
    4. Report: "[CONFIDENCE] Current understanding: [overall]%"
    
    IMPORTANT: Continue gathering until confidence >= 80%
```

#### Step 1.3: Adaptive Questioning Loop

```
While confidence < 80%:

Based on what you've shared, I understand:
✓ [Thing we're confident about] (90% confident)
✓ [Another clear thing] (85% confident)

To create the most effective workflow, I need clarification on:

? [Most important missing piece]
  Why this matters: [explanation]

? [Second priority question]
  This will help me: [reason]

[If confidence < 60%: Show 3-4 questions]
[If confidence 60-79%: Show 1-2 specific questions]

Current understanding: [X]%

Please help me understand these aspects better:
```

#### Example Interaction Flow

```
User: "I need a workflow to analyze customer feedback from multiple sources and generate insights"

System: Current understanding: 45%

Based on what you've shared, I understand:
✓ You want to analyze customer feedback (85% confident)
✓ Multiple data sources are involved (80% confident)
✓ The output should be insights (70% confident)

To create the most effective workflow, I need clarification on:

? What specific sources of feedback do you have?
  Why this matters: Different sources (surveys, reviews, support tickets) 
  require different collection methods and MCPs

? What kind of insights are most valuable?
  This will help me: Design the right analysis agents and outputs

? How often will this workflow run?
  This will help me: Optimize for one-time vs recurring use

? Do you have access to any specific MCP tools for data analysis?

Current understanding: 45%
```

### Phase 2: Research and Context Building

#### Step 2.1: Launch Context Analyzer

```
mcp__ccm__claude_code:
  model: "sonnet"
  prompt: |
    You are the Context Analyzer. Review gathered requirements.
    
    Join chat "$ROOM_NAME" as "ContextAnalyzer"
    
    1. Read workflow-requirements.md
    2. Identify:
       - Workflow category (data processing, development, analysis, etc.)
       - Complexity level (simple, moderate, complex)
       - Similar existing workflows in this repo
       - Potential challenges
    3. Create context-analysis.md
    4. Report: "[ANALYSIS] Category: [type], Complexity: [level]"
```

#### Step 2.2: Launch Research Agent

```
mcp__ccm__claude_code:
  model: "sonnet"
  prompt: |
    You are the Research Agent. Use Perplexity to research best practices.
    
    Join chat "$ROOM_NAME" as "ResearchAgent"
    Report: "[RESEARCH] Starting research phase"
    
    Based on the workflow type: $WORKFLOW_TYPE
    
    Use mcp__perplexity-search__perplexity_search_web for:
    1. "[workflow type] automation best practices 2024"
    2. "multi-agent [domain] workflow patterns"
    3. "[specific tool/MCP] integration examples"
    
    If complex domain, use mcp__perplexity-deep__perplexity_search_web for:
    1. Deep dive into technical implementation patterns
    2. Industry-specific workflow standards
    
    Create workflow-research-report.md with:
    - Relevant patterns found
    - Best practices for this domain
    - Common pitfalls to avoid
    - Tool-specific insights
    
    Report: "[RESEARCH] Complete, found [X] relevant patterns"
```

### Phase 2.3: User Checkpoint #1 - Requirements Confirmation

```
Requirements Summary - Please Confirm

Based on our discussion, here's my understanding of your workflow needs:

PROBLEM STATEMENT:
[Clear problem description]

WORKFLOW OBJECTIVES:
1. [Primary objective]
2. [Secondary objective]
3. [Additional goals]

KEY REQUIREMENTS:
• Input: [What triggers/feeds the workflow]
• Process: [Key steps identified]
• Output: [Expected deliverables]
• Tools: [MCP tools to use]

WORKFLOW CHARACTERISTICS:
• Type: [Category]
• Complexity: [Simple/Moderate/Complex]
• Frequency: [One-time/Recurring]
• User Interaction: [Automated/Interactive]

SUCCESS CRITERIA:
✓ [Measure 1]
✓ [Measure 2]
✓ [Measure 3]

Have I understood your requirements correctly? 
(yes to proceed / no to clarify further)
```

### Phase 3: Workflow Design

#### Step 3.1: Launch Workflow Designer

```
mcp__ccm__claude_code:
  model: "opus"
  prompt: |
    You are the Workflow Designer. Create comprehensive workflow architecture.
    
    Join chat "$ROOM_NAME" as "WorkflowDesigner"
    
    Using requirements and research:
    1. Design agent architecture:
       - Number of agents needed
       - Specialization for each
       - Coordination pattern
    2. Design process flow:
       - Linear vs parallel execution
       - Checkpoints needed
       - Error handling
    3. Design deliverables:
       - Output formats
       - File structures
       - Success metrics
    
    Create workflow-design-plan.md with:
    - Agent descriptions
    - Process diagram (text-based)
    - Data flow between agents
    - User interaction points
    - Estimated execution time
    
    Report: "[DESIGN] Architecture complete, [N] agents designed"
```

### Phase 3.2: User Checkpoint #2 - Design Approval

```
Workflow Design - Approval Required

WORKFLOW NAME: [Proposed Name]
ESTIMATED RUNTIME: [X] minutes

AGENT ARCHITECTURE:
I've designed a [N]-agent workflow:

1. [Agent Name] (Model: [Opus/Sonnet/Haiku])
   Role: [Description]
   Deliverable: [Output]

2. [Agent Name] (Model: [Model])
   Role: [Description]
   Tools: [MCP tools used]
   Deliverable: [Output]

[Continue for all agents]

PROCESS FLOW:
[ASCII diagram or structured description]

Step 1: [Description] → [Deliverable]
  ↓
Step 2: [Parallel branches if any]
  ↓
[Continue flow]

USER INTERACTIONS:
• Checkpoint 1: [When and why]
• Checkpoint 2: [When and why]
• Final approval: [What user reviews]

KEY FEATURES:
✓ [Feature 1 addressing requirement]
✓ [Feature 2 addressing requirement]
✓ [Feature 3 addressing requirement]

Does this design meet your needs? (yes/no/adjust)
```

### Phase 4: Workflow Creation

#### Step 4.1: Workflow Naming

```
Workflow File Naming

Based on your workflow's purpose, I suggest the filename:

    multi-agent-[primary-function]-workflow.md

For example:
    multi-agent-feedback-analysis-workflow.md

Press Enter to accept this name, or type a new filename:
> _
```

#### Step 4.2: Launch Workflow Writer

```
mcp__ccm__claude_code:
  model: "opus"
  prompt: |
    You are the Workflow Writer. Create the complete workflow file.
    
    Join chat "$ROOM_NAME" as "WorkflowWriter"
    
    Using the approved design and filename: $WORKFLOW_FILENAME
    
    Create a comprehensive workflow following these standards:
    1. Clear overview section
    2. Detailed agent descriptions
    3. Phase-by-phase execution steps
    4. Code examples for complex operations
    5. Error handling procedures
    6. Success metrics
    7. Example interaction flows
    
    Follow the structure of successful workflows like:
    - multi-agent-bug-fix-workflow-v3.md
    - multi-agent-project-research-workflow.md
    
    Include:
    - Confidence scoring where applicable
    - User checkpoints as designed
    - Chat room coordination
    - MCP tool usage examples
    - Detailed prompts for each agent
    
    Report: "[COMPLETE] Workflow file created: $WORKFLOW_FILENAME"
```

### Phase 5: Review and Finalization

#### Step 5.1: Launch Review Agent

```
mcp__ccm__claude_code:
  model: "opus"
  prompt: |
    You are the Review Agent. Ensure workflow quality.
    
    Join chat "$ROOM_NAME" as "ReviewAgent"
    
    Review $WORKFLOW_FILENAME for:
    1. Completeness (all requirements addressed)
    2. Clarity (easy to follow)
    3. Accuracy (correct MCP usage)
    4. Practicality (will actually work)
    5. Consistency (follows patterns)
    
    Create workflow-review-report.md
    Report: "[REVIEW] Quality score: [X]/10"
```

### Phase 5.2: Final Delivery

```
Custom Workflow Created Successfully!

FILE CREATED: [workflow-filename]
Quality Score: 9.5/10

WORKFLOW SUMMARY:
• Purpose: [One-line description]
• Agents: [N] specialized agents
• Runtime: ~[X] minutes
• Complexity: [Level]

KEY CAPABILITIES:
✓ [Capability addressing requirement 1]
✓ [Capability addressing requirement 2]
✓ [Capability addressing requirement 3]

USAGE:
To execute your new workflow:
@[workflow-filename] [Initial parameters]

NEXT STEPS:
1. Review the workflow file for any adjustments
2. Test with a sample scenario
3. Customize agent prompts if needed

Would you like me to:
1. Walk through the workflow step-by-step
2. Create a test scenario
3. Make any adjustments
4. Create another workflow

Response (1/2/3/4/done):
```

## Multi-Agent Collaboration Patterns

### When to Use Multi-Agent

```python
def should_use_multi_agent(requirements):
    """Determine if multi-agent approach is beneficial"""
    
    multi_agent_indicators = {
        'parallel_tasks': requirements.has_parallel_work,
        'specialization': requirements.needs_different_expertise,
        'complexity': requirements.complexity_score > 0.7,
        'checkpoints': requirements.needs_review_cycles,
        'scale': requirements.data_volume == 'large'
    }
    
    score = sum(1 for indicator in multi_agent_indicators.values() if indicator)
    return score >= 2  # Use multi-agent if 2+ indicators present
```

### Single vs Multi-Agent Decision

```yaml
single_agent_suitable:
  - Simple linear process
  - Single expertise domain
  - Quick execution needed
  - No parallel work possible
  - Limited user interaction

multi_agent_beneficial:
  - Parallel processing opportunities
  - Multiple expertise areas
  - Complex decision trees
  - Review/approval cycles
  - Large-scale processing
  - Quality checkpoints needed
```

## Research Query Templates

### Workflow Pattern Research

```yaml
general_patterns:
  - "[domain] workflow automation best practices 2024"
  - "multi-agent [task type] patterns"
  - "[industry] process automation examples"

tool_specific:
  - "[MCP tool] integration patterns"
  - "[programming language] automation workflows"
  - "[framework] multi-agent examples"

optimization:
  - "[workflow type] performance optimization"
  - "parallel processing [domain] workflows"
  - "error handling [process type] best practices"
```

## Success Metrics

```
Workflow Generation Summary
==========================
Requirements Gathering:
- Questions asked: [N]
- Final confidence: [X]%
- Time to 80%: [Y] minutes

Research Phase:
- Patterns found: [N]
- Best practices identified: [M]
- Research confidence: [X]%

Design Phase:
- Agents designed: [N]
- Complexity score: [X]/10
- User approval: ✓

Creation Phase:
- Workflow length: [N] lines
- Examples included: [M]
- Quality score: [X]/10

Total Generation Time: [T] minutes
User Interactions: [N]
Ready for Use: ✓
```

## Example Interaction - Complete Flow

```
User: "I need a workflow to help me refactor legacy code safely"

System: Current understanding: 35%

To create an effective code refactoring workflow, I need to understand:

? What type of code are you refactoring? (language, framework, size)
  Why this matters: Different languages need different analysis tools

? What makes it "legacy"? (age, lack of tests, poor structure, outdated patterns)
  This helps me: Design appropriate safety measures

? What's your main refactoring goal? (performance, readability, modernization)
  This helps me: Focus the workflow on right outcomes

Current understanding: 35%

User: "It's a Python Django app, about 50k lines, no tests, using Django 1.8 
      patterns. Goal is to modernize to Django 4.x patterns and add tests."

System: Current understanding: 75%

Much clearer! I understand:
✓ Python Django application (95% confident)
✓ Large codebase - 50k lines (100% confident)  
✓ No test coverage currently (100% confident)
✓ Django 1.8 → 4.x upgrade (90% confident)

One more clarification:

? Do you have specific Django components to prioritize? (models, views, templates)
  This helps me: Sequence the refactoring phases effectively

Current understanding: 75%

User: "Start with models and views, templates can wait"

System: [CONFIDENCE] Current understanding: 85%

[Research phase begins]
[Design presented]
[User approves]

System: Based on your workflow's purpose, I suggest the filename:

    multi-agent-django-legacy-refactoring-workflow.md

Press Enter to accept this name, or type a new filename:

User: [Enters]

System: [Creates workflow]

Custom Workflow Created Successfully!

FILE CREATED: multi-agent-django-legacy-refactoring-workflow.md

The workflow includes:
• 6 specialized agents for safe refactoring
• Automated test generation
• Django version compatibility checking  
• Rollback capabilities
• Progress tracking

Ready to help modernize your Django application safely!
```

This workflow generator ensures thorough understanding through confidence-based questioning, leverages research for best practices, and creates workflows tailored exactly to user needs while maintaining the high quality standards of existing workflows in the repository.