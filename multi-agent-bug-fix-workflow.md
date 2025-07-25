# Multi-Agent Bug Fix Workflow

## Overview

This document describes a workflow for efficiently fixing bugs by coordinating multiple specialized agents using Claude Code MCP (`mcp__ccm__claude_code`).

**Target Audience**: This document serves as a guide for Claude Code to execute multi-agent bug fixes.

## Workflow Components

### 1. Investigation Agent
- **Role**: Investigate bug causes and propose solutions
- **Model**: Opus
- **Deliverable**: `bug-investigation-report.md`

### 2. Implementation Agent
- **Role**: Implement fixes based on investigation results
- **Model**: sonnet
- **Deliverable**: Actual code fixes

### 3. Review Agent
- **Role**: Code review of implemented fixes
- **Model**: Opus
- **Deliverable**: `code-review-report.md`

### 4. Debug Agent
- **Role**: Troubleshooting when issues occur
- **Model**: sonnet (prioritizing cost efficiency)
- **Launch Condition**: Only launched when errors occur or verification fails
- **Task Examples**:
  - Check environment state (process status, file system)
  - Detailed investigation of error logs
  - Identify execution environment issues
  - Propose simple solutions

## Detailed Workflow

### Phase 0: Cleanup

Clean up deliverable files generated from previous executions before starting the workflow.

```bash
# Create backup directory (if it doesn't exist)
mkdir -p ./backup

# Check existing deliverable files
ls -la | grep -E "(bug-investigation-report|code-review-report)\.md"

# Backup existing files if present
if [ -f "bug-investigation-report.md" ]; then
  # Backup to ./backup directory with timestamp
  mv bug-investigation-report.md "./backup/bug-investigation-report_$(date +%Y%m%d_%H%M%S).md"
  echo "Backed up bug-investigation-report.md"
fi

if [ -f "code-review-report.md" ]; then
  # Backup to ./backup directory with timestamp
  mv code-review-report.md "./backup/code-review-report_$(date +%Y%m%d_%H%M%S).md"
  echo "Backed up code-review-report.md"
fi

# Automatic deletion of old backups (files older than 30 days)
# find ./backup -name "*_report_*.md" -mtime +30 -delete
```

### Phase 0.5: Foundation Session Setup

Build a foundation session for efficient multi-agent execution:

**Creating Foundation Session**:
```bash
# 1. Create foundation session for project understanding
mcp__ccm__claude_code [
  model: "sonnet",
  prompt: "Project structure analysis:
  1. Read CLAUDE.md to understand project overview
  2. Check technology stack with package.json
  3. Investigate main directory structure
  4. Organize foundational knowledge needed for bug fixes
  Report 'BASE_CONTEXT_READY' upon completion and provide session ID"
]
```

**Foundation Session Benefits**:
- **Prompt Cache**: Large content like CLAUDE.md automatically cached
- **Context Sharing**: All agents get common understanding foundation
- **Cost Reduction**: 60-90% token cost reduction for subsequent agents
- **Execution Time Reduction**: Fast startup through cache utilization

**Important**: Save foundation session ID (utilized by all agents)

### Phase 1: Bug Investigation

Launch Investigation Agent (Opus) **from foundation session** to analyze bug causes:

1. **Use mcp__ccm__claude_code** tool
2. **session_id**: Specify foundation session ID (context inheritance)
3. **model**: Specify "opus"
4. **Include in prompt**:
   - Bug details provided by user
   - Investigation procedure (code reading, cause identification, impact analysis, fix proposal)
   - Output instruction to bug-investigation-report.md

**Specify output format**:
```
# Bug Investigation Report
## Problem Overview
## Detailed Cause  
## Affected Files and Functions
## Recommended Fix Approach
## Specific Fix Locations and Code Examples
```

Save the returned process ID (for progress monitoring).

### Phase 2: Progress Monitoring

1. **Wait 5 minutes** (execute `sleep 300` with Bash tool)
2. **Check process status** with mcp__ccm__get_claude_result
3. If not complete, **wait another 2 minutes** and check again
4. **Verify bug-investigation-report.md** creation (check with Read tool)

### Phase 3: Fix Implementation

Launch Implementation Agent (Sonnet) **from foundation session** to implement fixes:

1. **Use mcp__ccm__claude_code** tool
2. **session_id**: Specify foundation session ID (inherit project understanding)
3. **model**: Specify "sonnet"
4. **Include in prompt**:
   - Instruction to read bug-investigation-report.md
   - Specific implementation instructions
   - Build command execution instruction
   - Implementation completion report format

**Important**: Save the returned **new session ID** (Claude Code automatically generates new session and inherits foundation context).

### Phase 3.5: Verification and Debugging (Optional)

After implementing fixes, conduct verification as needed. If errors occur, launch Debug Agent:

1. **Conduct Verification** (executed by main agent)
   - Basic functional test of fixed features
   - Record details if errors occur

2. **Debug Agent Launch Conditions**:
   - Error occurs during verification
   - Build succeeds but runtime error occurs
   - Environment-related issues suspected

3. **Launch Debug Agent** (only when conditions are met):
   ```
   - model: Specify "sonnet" (prioritize cost efficiency)
   - Example prompt:
     "The following error occurred. Check environment state and identify the cause:
     [Error details]
     Check items:
     - Running process state
     - Verify existence of related files
     - Log file contents
     - Propose simple solutions"
   ```

4. **Utilize Debug Results**:
   - Environment issues: Main agent handles
   - Code issues: Request Implementation Agent for fixes

### Phase 4: Code Review

1. **Wait 3 minutes** (wait for implementation completion)
2. Launch Review Agent (Opus) **from foundation session**:
   - **Use mcp__ccm__claude_code** tool
   - **session_id**: Specify foundation session ID (inherit project understanding)
   - **model**: Specify "opus"
   - **Clearly specify output format**:
     - Line 1: COMPLETED or INCOMPLETE
     - Line 2 onwards: Specific issues only if INCOMPLETE

**Important**: Save the returned **new session ID** (for continuing review context).

### Phase 5: Review Fix and Re-review Cycle

1. **Wait 2 minutes** (wait for review completion)
2. **Check first line of code-review-report.md**
3. **If INCOMPLETE** (repeat up to 5 times):
   - **Resume** Implementation Agent session (use session ID generated in Phase 3)
   - Instruct to fix issues
   - Wait 2 minutes
   - **Resume** Review Agent session (use session ID generated in Phase 4)
   - Instruct re-review
   - Wait 2 minutes
   - Check review result again

**Important**: 
- Each agent continues work in independent session after inheriting context from foundation session
- Prompt cache effect maintained during session resumption, enabling fast execution
- File conflicts automatically avoided (each agent writes to independent files)

### Completion Confirmation

**If COMPLETED**:
- Bug fix is complete
- Confirm deliverables:
  - bug-investigation-report.md (investigation report)
  - code-review-report.md (review results)
  - Fixed source code

**If still INCOMPLETE after 5 iterations**:
- Report that manual intervention is needed

## Best Practices for Task Management

Recommend using TodoWrite tool to manage progress:

1. **Confirm bug investigation agent completion and obtain investigation report**
2. **Fix by implementation agent**
3. **Post-fix verification and testing**
4. **Code review by review agent**
5. **Fix review issues** (as needed)
6. **Re-review after fixes** (as needed)

Update status upon completion of each task to visualize overall progress.

## Inter-Agent Coordination

### Optimized Session Management Strategy

**Efficiency through Foundation Session Utilization**:

1. **Foundation Session**: Build project-wide understanding (Phase 0.5)
2. **Investigation Agent**: Launch from foundation session → Automatically generate new session
3. **Implementation Agent**: Launch from foundation session → Generate new session (reuse in fix cycle)
4. **Review Agent**: Launch from foundation session → Generate new session (reuse for re-review)

**Claude Code's Actual Behavior**:
- When launched with `-r <session_id>`: Load specified session context
- **Automatically generate new session ID**: Avoid conflicts by writing to new files
- **Inherit prompt cache**: Utilize foundation session cache
- **Cross-model sharing**: Cache effect maintained even from Sonnet→Opus

**Demonstrated Cost Reduction Effect**:
```json
// Example of agent launch from foundation session
"usage": {
  "input_tokens": 56,                    // New input (minimal)
  "cache_creation_input_tokens": 43370,  // Cache creation
  "cache_read_input_tokens": 390302,     // Massive cache utilization
  "output_tokens": 6111
}
// Result: Over 90% token cost reduction
```

**Session Management Best Practices**:
- Reliably save foundation session ID and utilize across all agents
- Save each agent's new session ID for continued work
- Safe parallel execution through session duplication
- Maximize prompt cache effect

## Estimated Wait Times

- **Investigation Agent**: 5 minutes (initial), 2 minutes (recheck)
- **Implementation Agent**: 3 minutes
- **Review Agent**: 2 minutes
- **Fix Cycle**: 2 minutes each

Use Bash tool's `sleep` command to wait (e.g., `sleep 300` waits 5 minutes).

## Success Case Study

### LockService Bug Fix Example (Optimized Version)

**Foundation Session Setup**:
- **Foundation Session**: Load project structure and CLAUDE.md (Session ID: base-001)
- **Cache Creation**: 18,621 tokens of project context

**Agent Execution**:
1. **Issue**: Directory not found error when creating lock file
2. **Investigation Agent**: Launch from foundation session → Generate new session (ID: inv-924fab52)
   - Cache utilization: 99,870 tokens read, new input: 245 tokens
   - **Investigation Result**: Missing directory creation in `acquireFileLock` method
3. **Implementation Agent**: Launch from foundation session → Generate new session (ID: impl-e8387697)
   - Cache utilization: 324,529 tokens read, new input: 186 tokens
   - **Implementation**: Parent directory creation with `fs.mkdir({ recursive: true })`
4. **Review Agent**: Launch from foundation session (Opus model) → Generate new session (ID: rev-a1b2c3d4)
   - **Cross-model cache sharing**: 390,302 tokens read, new input: 56 tokens
   - **Initial Review**: INCOMPLETE (with improvement suggestions)
5. **Fix Cycle**: Context continuation through session resumption for each agent
6. **Re-review**: COMPLETED

**Effect Measurement**:
- **Total Cost Reduction**: ~85% (through cache utilization)
- **Execution Time Reduction**: ~60% (acceleration through cache)
- **Quality Improvement**: Multi-perspective verification through model coordination

## Benefits

1. **Leveraging Specialization**: Each agent focuses on specific role
2. **Quality Assurance**: Verification from multiple perspectives
3. **Documentation**: Deliverables from each phase automatically documented
4. **Reproducibility**: Formalized workflow, reusable
5. **Parallel Processing**: Independent tasks can be executed concurrently

## Precautions

1. **Optimized Cost Management**: 
   - **Foundation Session Utilization**: 60-90% cost reduction effect
   - **Prompt Cache**: Token reduction through automatic activation
   - **Model Selection**: Strategic use of Sonnet (efficiency) → Opus (quality)
   - **Debug Agent**: Cost control with Sonnet
2. **Session Management Reality**:
   - **Foundation Session ID**: Reliably save as starting point for all agents
   - **Automatic New Session Generation**: Utilize Claude Code's safe design
   - **File Conflict Avoidance**: Automatically write to independent files
   - **Maintain Cache Effect**: Continued cost reduction through session inheritance
3. **Wait Times**: Time settings considering acceleration from cache effect
4. **Error Handling**: 
   - Alternative approach for session duplication failure
   - Handling when cache effect not obtained
   - Process management improvements
5. **Cleanup**: Properly handle previous deliverables (backup or delete)
6. **Utilizing Parallel Execution**:
   - Multiple agents can be launched simultaneously from foundation session
   - Risk avoidance through independence guarantee
   - Throughput improvement effect

## Extensibility

This workflow can be extended as follows:

- **Test Agent**: Automated test execution after fixes
- **Documentation Agent**: Automated documentation of change history
- **Deploy Agent**: Automated deployment of fixes
- **Monitoring Agent**: System monitoring after fixes

## Key Error Handling Points

### Basic Error Handling
- **File not found**: Properly handle Read tool errors and report
- **Timeout**: Check status with mcp__ccm__get_claude_result, retry as needed
- **Process abnormal termination**: Check status with mcp__ccm__list_claude_processes and clean up

### Session-Related Error Handling
- **Foundation session creation failure**: Fallback to traditional new session approach
- **No cache effect**: Continue execution (efficiency decreases but functionality unaffected)
- **Session inheritance failure**: Re-execute with new session

### Performance Monitoring
- **Check usage statistics**: Verify cache effect with cache_read_input_tokens
- **Cost anomaly**: Review foundation session strategy
- **Execution time monitoring**: Measure reduction from cache effect

This workflow enables systematic and efficient implementation of even complex bug fixes.