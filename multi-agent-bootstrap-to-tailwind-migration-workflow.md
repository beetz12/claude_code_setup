# Multi-Agent Bootstrap to Tailwind Migration Workflow

## Overview

This workflow orchestrates 6 specialized agents to systematically migrate Laravel applications from Bootstrap 4 to Tailwind CSS. The workflow handles 10-20 pages with visual verification, organized screenshots for team review, and chat-based real-time coordination.

### Key Features

- **Visual-First Migration**: Before/after screenshots for every page
- **Page-by-Page Processing**: Systematic migration with verification
- **Laravel 10 Optimized**: Handles Blade templates, components, and Laravel Mix/Vite
- **Team Review Ready**: Organized screenshot folders for stakeholder approval
- **Real-Time Coordination**: Chat-based agent communication
- **User Checkpoints**: Strategic approval points for quality control
- **Regression Testing**: Visual comparison with configurable tolerance

### Agent Architecture

1. **Inventory Agent (Opus)**: Maps all pages, routes, and Bootstrap usage
2. **Screenshot Agent (Sonnet)**: Captures before/after screenshots
3. **Migration Agent (Sonnet)**: Converts Bootstrap to Tailwind
4. **Verification Agent (Opus)**: Visual regression testing
5. **Fix Agent (Sonnet)**: Resolves migration issues
6. **Review Agent (Opus)**: Final quality assessment

### Prerequisites

- MCP chat, ccm, and playwright installed
- Laravel 10 application with Bootstrap 4
- Tailwind CSS installed and configured
- Playwright for screenshot capture

## Phase 1: Project Inventory and Planning

### Step 1.1: Create Chat Room and Initialize Coordinator

```bash
# Create unique chat room for this migration
MIGRATION_ROOM="tailwind-migration-$(date +%s)"

# Join chat room
/chat join $MIGRATION_ROOM coordinator
```

### Step 1.2: Prepare Project Structure

```bash
# Create screenshot directories
mkdir -p screenshots/before
mkdir -p screenshots/after
mkdir -p screenshots/diffs

# Create deliverables directory
mkdir -p migration-deliverables

# Clean any existing migration files
rm -f migration-deliverables/*.md
```

### Step 1.3: Spawn Inventory Agent

```bash
# Inventory Agent prompt
/ccm "You are the Inventory Agent for a Bootstrap to Tailwind migration in a Laravel 10 application.

Your tasks:
1. Join chat room: $MIGRATION_ROOM as 'inventory-agent'
2. Scan all routes in routes/web.php and routes/api.php
3. Map all Blade templates in resources/views/
4. Identify Bootstrap 4 usage patterns:
   - Grid system (container, row, col-*)
   - Components (cards, modals, alerts, buttons)
   - Utilities (spacing, display, text)
   - JavaScript components
5. Create migration-deliverables/inventory-report.md with:
   - Complete page listing with routes
   - Bootstrap component usage per page
   - Complexity rating (1-5) per page
   - Recommended migration order
   - Laravel-specific considerations
6. Post progress updates to chat
7. When complete, post [COMPLETED] with deliverable location

Focus on:
- Blade template inheritance (@extends, @section)
- Laravel components (@component, <x-component>)
- Mix/Vite asset compilation
- Dynamic content areas

Use chat format:
[STARTED] Beginning inventory scan
[PROGRESS] Analyzing [file/component]
[FOUND] [Bootstrap pattern] in [location]
[COMPLETED] Inventory complete, report at [location]" --model opus --prompt-caching --resume $USER_DIR
```

### Step 1.4: Monitor Inventory Progress

```bash
# Check chat for updates
/chat messages $MIGRATION_ROOM --limit 10

# Wait for [COMPLETED] from inventory-agent
```

## Phase 2: Screenshot Capture (Before State)

### Step 2.1: Spawn Screenshot Agent

```bash
# Screenshot Agent prompt
/ccm "You are the Screenshot Agent for capturing 'before' state of Bootstrap pages.

Your tasks:
1. Join chat room: $MIGRATION_ROOM as 'screenshot-agent'
2. Read migration-deliverables/inventory-report.md
3. For each page listed:
   - Navigate to the page URL
   - Wait for full page load
   - Capture full-page screenshot
   - Save to screenshots/before/[page-name].png
   - Log responsive breakpoints if needed
4. Create migration-deliverables/screenshot-manifest.md with:
   - List of all captured pages
   - Any pages that failed to load
   - Special considerations (auth required, dynamic content)
5. Post progress updates to chat
6. When complete, post [COMPLETED] with manifest location

Use Playwright with:
- Full page screenshots (not just viewport)
- 1920x1080 default resolution
- Handle authentication if needed
- Wait for dynamic content

Chat format:
[STARTED] Beginning screenshot capture
[PROGRESS] Capturing [page-name]
[ISSUE] Failed to capture [page] - [reason]
[COMPLETED] Screenshots captured, manifest at [location]" --model sonnet --prompt-caching --resume $USER_DIR
```

### Step 2.2: User Checkpoint - Review Screenshots

```bash
# Post to chat for user review
/chat post $MIGRATION_ROOM coordinator "[CHECKPOINT] Before screenshots captured. Please review screenshots/before/ directory. Type 'proceed' to continue or 'adjust' to recapture specific pages."

# Wait for user confirmation
/chat wait $MIGRATION_ROOM coordinator --timeout 300
```

## Phase 3: Page-by-Page Migration

### Step 3.1: Spawn Migration Agent

```bash
# Migration Agent prompt
/ccm "You are the Migration Agent converting Bootstrap 4 to Tailwind CSS in Laravel.

Your tasks:
1. Join chat room: $MIGRATION_ROOM as 'migration-agent'
2. Read inventory-report.md and screenshot-manifest.md
3. For each page in migration order:
   - Post [MIGRATING] [page-name] to chat
   - Convert Bootstrap classes to Tailwind
   - Preserve Laravel Blade syntax
   - Update any JavaScript for component changes
   - Test the page loads correctly
   - Post [MIGRATED] [page-name] when complete
4. Create migration-deliverables/migration-log.md tracking:
   - Pages completed
   - Conversion patterns used
   - Any issues encountered
   - JavaScript changes needed
5. Follow these conversion patterns:

GRID SYSTEM:
- container → container mx-auto
- row → flex flex-wrap -mx-4
- col-md-6 → md:w-1/2 px-4
- col-lg-4 → lg:w-1/3 px-4

COMPONENTS:
- btn btn-primary → bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded
- card → bg-white rounded-lg shadow-md
- alert alert-success → bg-green-100 border border-green-400 text-green-700 px-4 py-3 rounded

UTILITIES:
- mt-3 → mt-3 (same in Tailwind)
- d-none d-md-block → hidden md:block
- text-center → text-center

MODALS:
- Convert Bootstrap modals to Alpine.js or custom Tailwind modals
- Update JavaScript initialization

Chat format:
[STARTED] Beginning migration
[MIGRATING] [page-name]
[PATTERN] Applied [Bootstrap] → [Tailwind] conversion
[MIGRATED] [page-name] complete
[ISSUE] [problem] in [page]
[COMPLETED] Migration phase complete" --model sonnet --prompt-caching --resume $USER_DIR
```

### Step 3.2: Real-Time Migration Monitoring

```bash
# Monitor migration progress
while true; do
  /chat messages $MIGRATION_ROOM --limit 5
  # Check for [ISSUE] messages that need intervention
  # Break when [COMPLETED] is posted
  sleep 30
done
```

## Phase 4: Screenshot Capture (After State)

### Step 4.1: Re-spawn Screenshot Agent for After State

```bash
# Screenshot Agent for 'after' state
/ccm "You are the Screenshot Agent capturing 'after' state of migrated Tailwind pages.

Your tasks:
1. Join chat room: $MIGRATION_ROOM as 'screenshot-agent-after'
2. Read migration-log.md for completed pages
3. For each migrated page:
   - Navigate to the page URL
   - Capture screenshot with same settings as 'before'
   - Save to screenshots/after/[page-name].png
   - Ensure consistent capture (same scroll position, data)
4. Update screenshot-manifest.md with 'after' entries
5. Post progress to chat
6. When complete, post [COMPLETED]

Ensure screenshots are directly comparable:
- Same resolution and viewport
- Same test data visible
- Same scroll position
- Same dynamic content state

Chat format:
[STARTED] Capturing after screenshots
[PROGRESS] Captured [page-name]
[COMPLETED] After screenshots complete" --model sonnet --prompt-caching --resume $USER_DIR
```

## Phase 5: Visual Verification

### Step 5.1: Spawn Verification Agent

```bash
# Verification Agent prompt
/ccm "You are the Verification Agent performing visual regression testing on the migration.

Your tasks:
1. Join chat room: $MIGRATION_ROOM as 'verification-agent'
2. For each page with before/after screenshots:
   - Compare visual differences
   - Identify layout shifts beyond acceptable threshold (5%)
   - Check responsive behavior at key breakpoints
   - Flag any missing elements or broken layouts
3. Create migration-deliverables/verification-report.md with:
   - Page-by-page comparison results
   - Visual regression percentage per page
   - Critical issues requiring fixes
   - Minor acceptable differences
   - Recommended fix priority
4. Generate diff images for significant changes in screenshots/diffs/
5. Post findings to chat
6. When complete, post [COMPLETED] with report location

Evaluation criteria:
- Layout integrity maintained
- All content visible and accessible
- Interactive elements properly styled
- Responsive breakpoints working
- No JavaScript console errors

Chat format:
[STARTED] Beginning verification
[VERIFYING] [page-name]
[PASS] [page-name] - [diff-percentage]% difference
[FAIL] [page-name] - [issue-description]
[COMPLETED] Verification complete, report at [location]" --model opus --prompt-caching --resume $USER_DIR
```

### Step 5.2: User Checkpoint - Review Verification

```bash
# Post verification summary
/chat post $MIGRATION_ROOM coordinator "[CHECKPOINT] Visual verification complete. Review:
- screenshots/before/ - Original Bootstrap pages
- screenshots/after/ - Migrated Tailwind pages  
- screenshots/diffs/ - Visual differences
- migration-deliverables/verification-report.md

Type 'approve' to proceed to fixes or 'reject [pages]' to re-migrate specific pages."

# Wait for user decision
/chat wait $MIGRATION_ROOM coordinator --timeout 600
```

## Phase 6: Fix Failed Pages

### Step 6.1: Spawn Fix Agent (Conditional)

```bash
# Only spawn if fixes are needed
/ccm "You are the Fix Agent resolving migration issues identified in verification.

Your tasks:
1. Join chat room: $MIGRATION_ROOM as 'fix-agent'
2. Read verification-report.md for failed pages
3. For each page requiring fixes:
   - Post [FIXING] [page-name] - [issue]
   - Analyze the specific problem
   - Apply targeted Tailwind corrections
   - Test the fix thoroughly
   - Post [FIXED] [page-name] when resolved
4. Create migration-deliverables/fix-log.md documenting:
   - Issues encountered and root causes
   - Specific fixes applied
   - Any remaining minor issues
   - Patterns for future migrations
5. Request screenshot recapture for fixed pages

Common fix patterns:
- Flexbox containers for Bootstrap grid replacements
- Custom spacing scales for exact matches
- JavaScript re-initialization for interactive components
- Z-index adjustments for modals/dropdowns
- Form styling consistency

Chat format:
[STARTED] Beginning fixes
[FIXING] [page-name] - [issue]
[APPLIED] [fix-description]
[FIXED] [page-name] resolved
[COMPLETED] All fixes applied" --model sonnet --prompt-caching --resume $USER_DIR
```

### Step 6.2: Re-verify Fixed Pages

```bash
# Recapture screenshots for fixed pages
# Re-run verification on fixed pages only
# Update verification-report.md with new results
```

## Phase 7: Final Review and Handoff

### Step 7.1: Spawn Review Agent

```bash
# Review Agent prompt
/ccm "You are the Review Agent performing final quality assessment of the Bootstrap to Tailwind migration.

Your tasks:
1. Join chat room: $MIGRATION_ROOM as 'review-agent'
2. Comprehensive review including:
   - All deliverable documents
   - Screenshot comparisons
   - Code quality in migrated templates
   - Consistency of Tailwind usage
   - Performance implications
3. Create migration-deliverables/final-review-report.md with:
   - Executive summary of migration
   - Success metrics (pages migrated, pass rate)
   - Remaining minor issues (if any)
   - Recommendations for production deployment
   - Tailwind optimization opportunities
   - Team review checklist
4. Create migration-deliverables/handoff-package.md with:
   - Screenshot review guide for stakeholders
   - List of all changed files
   - Deployment considerations
   - Rollback plan if needed

Chat format:
[STARTED] Beginning final review
[REVIEWING] [aspect]
[METRICS] [statistic]
[COMPLETED] Review complete, reports ready" --model opus --prompt-caching --resume $USER_DIR
```

### Step 7.2: Final User Checkpoint

```bash
# Post final summary
/chat post $MIGRATION_ROOM coordinator "[CHECKPOINT] Migration complete! 

Review complete package:
- migration-deliverables/final-review-report.md - Full assessment
- migration-deliverables/handoff-package.md - Team handoff guide
- screenshots/ - Complete visual documentation

Next steps:
1. Share screenshot folders with design team
2. Review the handoff package
3. Plan deployment strategy

Type 'accept' to finalize or 'concerns [details]' to address specific issues."
```

## Execution Examples

### Example 1: Simple Marketing Site (10 pages)

```bash
# Initialize migration
MIGRATION_ROOM="tailwind-migration-1234567890"
/chat join $MIGRATION_ROOM coordinator

# Run through all phases
# Phase 1: Inventory (15 mins)
# Phase 2: Screenshots before (10 mins)
# Phase 3: Migration (45 mins)
# Phase 4: Screenshots after (10 mins)
# Phase 5: Verification (15 mins)
# Phase 6: Fixes (20 mins)
# Phase 7: Review (10 mins)
# Total: ~2 hours
```

### Example 2: Complex Application (20 pages with auth)

```bash
# Additional considerations:
# - Login required for certain pages
# - Dynamic data that needs consistent test data
# - Complex JavaScript interactions
# - Multiple user roles to test
```

## Common Bootstrap to Tailwind Patterns

### Grid System Conversions

```html
<!-- Bootstrap -->
<div class="container">
  <div class="row">
    <div class="col-md-8">Main content</div>
    <div class="col-md-4">Sidebar</div>
  </div>
</div>

<!-- Tailwind -->
<div class="container mx-auto px-4">
  <div class="flex flex-wrap -mx-4">
    <div class="w-full md:w-2/3 px-4">Main content</div>
    <div class="w-full md:w-1/3 px-4">Sidebar</div>
  </div>
</div>
```

### Component Conversions

```html
<!-- Bootstrap Card -->
<div class="card">
  <div class="card-header">Header</div>
  <div class="card-body">
    <h5 class="card-title">Title</h5>
    <p class="card-text">Content</p>
  </div>
</div>

<!-- Tailwind Card -->
<div class="bg-white rounded-lg shadow-md overflow-hidden">
  <div class="bg-gray-100 px-6 py-4 border-b">Header</div>
  <div class="p-6">
    <h5 class="text-xl font-semibold mb-2">Title</h5>
    <p class="text-gray-700">Content</p>
  </div>
</div>
```

### Laravel Blade Considerations

```blade
{{-- Preserve Blade syntax during migration --}}
@extends('layouts.app')

@section('content')
    {{-- Convert Bootstrap classes inside Blade directives --}}
    @if($user->isAdmin())
        <div class="bg-blue-100 border-l-4 border-blue-500 p-4">
            Admin panel
        </div>
    @endif
    
    {{-- Handle Laravel components --}}
    <x-alert type="success" class="mb-4">
        Successfully migrated!
    </x-alert>
@endsection
```

## Error Handling

### Common Issues and Solutions

1. **Screenshot Capture Failures**
   - Authentication required: Provide login credentials
   - Dynamic content: Add wait conditions
   - Large pages: Increase timeout

2. **Migration Conflicts**
   - Custom Bootstrap overrides: Create Tailwind equivalents
   - JavaScript dependencies: Update or replace
   - Third-party components: Find Tailwind alternatives

3. **Visual Regression Failures**
   - Acceptable differences: Adjust tolerance threshold
   - Font rendering: Ensure consistent font loading
   - Dynamic content: Use stable test data

## Best Practices

1. **Always capture screenshots first** - Establishes baseline
2. **Migrate in complexity order** - Simple pages first
3. **Maintain Laravel patterns** - Don't break Blade inheritance
4. **Test interactivity** - Not just visual appearance
5. **Document custom utilities** - For team knowledge
6. **Use semantic class names** - Extract common patterns to components
7. **Progressive enhancement** - Ensure functionality without JavaScript

## Deliverables Summary

1. `inventory-report.md` - Complete page and component inventory
2. `screenshot-manifest.md` - Screenshot capture log
3. `migration-log.md` - Detailed migration actions
4. `verification-report.md` - Visual regression results
5. `fix-log.md` - Issue resolutions (if needed)
6. `final-review-report.md` - Quality assessment
7. `handoff-package.md` - Team handoff guide
8. `screenshots/` - Complete visual documentation

## Success Metrics

- **Visual Parity**: >95% visual similarity (configurable)
- **Zero Broken Pages**: All pages load without errors
- **Responsive Integrity**: All breakpoints function correctly
- **Performance**: No degradation in page load times
- **Code Quality**: Consistent Tailwind patterns
- **Team Approval**: Stakeholder sign-off on visuals