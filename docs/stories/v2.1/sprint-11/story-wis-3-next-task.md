# Story WIS-3: `*next` Task Implementation

<!-- Source: Epic WIS - Workflow Intelligence System -->
<!-- Context: Core WIS functionality - suggest next commands -->
<!-- Created: 2025-12-23 by @sm (River) -->

## Status: Ready

**Priority:** 🟡 MEDIUM
**Sprint:** 11
**Effort:** 11h
**Lead:** @dev (Dex)
**Approved by:** @po (Pax) - 2025-12-25

---

## Story

**As an** AIOS user,
**I want** a `*next` task that suggests my next commands based on current context,
**So that** I can navigate workflows efficiently without memorizing command sequences.

---

## Background

WIS-1 investigation revealed:
- WorkflowNavigator prototype already generates suggestions
- GreetingBuilder integrates workflow context
- Session state tracks command history and agent sequence
- 75% of infrastructure exists

This story creates the user-facing `*next` task that leverages WIS-2's registry and scoring.

### User Value

```
Before: User finishes a task, wonders "what's next?"
After:  User runs *next, gets context-aware suggestions with confidence scores
```

---

## 🤖 CodeRabbit Integration

### Story Type Analysis

**Primary Type**: Implementation
**Secondary Type(s)**: CLI Development, Integration
**Complexity**: Medium

### Specialized Agent Assignment

**Primary Agents**:
- @dev (Dex): Implement `*next` task and integration

**Supporting Agents**:
- @qa (Quinn): Test suggestion accuracy and edge cases
- @architect (Aria): Ensure integration matches ADR-WIS-001

### Quality Gate Tasks

- [ ] Pre-Commit (@dev): Verify task implementation
  - **Pass criteria:** All ACs met, tests pass, latency <100ms
  - **Fail criteria:** Missing features, broken integration, slow response
- [ ] Pre-PR (@qa): Validate suggestion accuracy
  - **Pass criteria:** 85% accuracy on test scenarios, edge cases handled
  - **Fail criteria:** Low accuracy, crashes, unclear output

### Self-Healing Configuration

**Mode:** light (Primary Agent: @dev)
**Max Iterations:** 2
**Time Limit:** 15 minutes
**Severity Threshold:** CRITICAL only

| Severity | Auto-Fix | Behavior |
|----------|----------|----------|
| CRITICAL | Yes | Block merge, auto-fix if possible |
| HIGH | No | Report only |
| MEDIUM | No | Report only |
| LOW | No | Ignore |

### Focus Areas

- CLI output formatting correctness
- Suggestion engine performance (<100ms)
- Context detection accuracy
- Task completion hook integration

---

## Acceptance Criteria

### AC 3.1: `*next` Task Definition

- [ ] Create task file at `.aios-core/development/tasks/next.md`
- [ ] Task accepts optional arguments:
  - `--story <path>`: Explicit story context
  - `--all`: Show all suggestions (not just top 3)
  - `--help`: Show usage documentation
- [ ] Task integrates with SuggestionEngine from WIS-2

**Task Definition Structure:**
```yaml
name: next
description: Suggest next commands based on current workflow context
args:
  - name: story
    type: path
    optional: true
    description: Explicit story path for context
  - name: all
    type: flag
    optional: true
    description: Show all suggestions instead of top 3
  - name: help
    type: flag
    optional: true
    description: Show usage documentation
```

### AC 3.2: Suggestion Engine Integration

- [ ] Create `suggestion-engine.js` in `.aios-core/workflow-intelligence/engine/`
- [ ] Integrate with:
  - WorkflowRegistry (from WIS-2)
  - ConfidenceScorer (from WIS-2)
  - ContextDetector (existing)
- [ ] Implement `suggestNext(context)` method returning scored suggestions

**API Contract:**
```javascript
// Input
const context = {
  agentId: 'dev',
  lastCommands: ['develop'],
  storyPath: 'docs/stories/v2.1/sprint-10/story-wis-3.md',
  branch: 'feature/wis-3',
  projectState: { modifiedFiles: ['src/index.js'], testsPassing: true }
};

// Output
const result = {
  workflow: 'story_development',
  currentState: 'in_development',
  confidence: 0.92,
  suggestions: [
    { command: '*review-qa', args: '${story_path}', description: 'Run QA review', confidence: 0.95, priority: 1 },
    { command: '*run-tests', args: '', description: 'Execute test suite', confidence: 0.80, priority: 2 }
  ]
};
```

### AC 3.3: Task Completion Hook

- [ ] Enhance `context-loader.js` with task completion tracking
- [ ] Implement `onTaskComplete(taskName, result)` method
- [ ] Update session state with:
  - Last command executed
  - Workflow state transition
  - Timestamp
- [ ] Hook integrates with existing task execution flow

### AC 3.4: Formatted CLI Output

- [ ] Suggestions displayed with clear formatting:
  ```
  🧭 Workflow: story_development
  📍 State: in_development (confidence: 92%)

  Next steps:
  1. `*review-qa docs/stories/v2.1/sprint-10/story-wis-3.md` - Run QA review
  2. `*run-tests` - Execute test suite manually
  3. `*pre-push-quality-gate` - Final quality checks

  Type a number to execute, or press Enter to continue manually.
  ```
- [ ] Low-confidence suggestions (<50%) marked as "uncertain"
- [ ] Colors used for visual hierarchy (if terminal supports)

### AC 3.5: Context Override

- [ ] `--story` flag sets explicit story context
- [ ] Story path validated before use
- [ ] Override context merged with auto-detected context
- [ ] Clear feedback when using override

### AC 3.6: Help Integration

- [ ] `*next --help` displays usage documentation
- [ ] Help includes examples:
  ```
  Usage: *next [options]

  Suggests next commands based on current workflow context.

  Options:
    --story <path>  Explicit story path for context
    --all           Show all suggestions (not just top 3)
    --help          Show this help message

  Examples:
    *next                                    # Auto-detect context
    *next --story docs/stories/v2.1/sprint-10/story-wis-3.md
    *next --all                              # Show all suggestions
  ```

### AC 3.7: Performance

- [ ] Suggestion latency <100ms (measured)
- [ ] Caching utilized for workflow patterns (5-minute TTL)
- [ ] Lazy loading of WIS modules
- [ ] Performance test included

### AC 3.8: Testing

- [ ] Unit tests for SuggestionEngine
- [ ] Integration tests for full flow
- [ ] Test scenarios:
  - New session (no history)
  - Mid-workflow (clear context)
  - Ambiguous context (multiple matches)
  - No matching workflow
- [ ] Performance test: measure latency

---

## Technical Design

### Component Integration

```mermaid
sequenceDiagram
    participant User
    participant CLI as *next Task
    participant SE as Suggestion Engine
    participant CD as Context Detector
    participant WR as Workflow Registry
    participant CS as Confidence Scorer

    User->>CLI: *next
    CLI->>CD: detectContext()
    CD-->>CLI: context
    CLI->>SE: suggestNext(context)
    SE->>WR: matchWorkflow(commands)
    WR-->>SE: workflowDef
    SE->>CS: score(suggestions, context)
    CS-->>SE: scoredSuggestions
    SE-->>CLI: result
    CLI-->>User: Formatted output
```

### Directory Structure

```
.aios-core/
├── workflow-intelligence/
│   ├── engine/
│   │   ├── suggestion-engine.js    # NEW - Core engine
│   │   └── confidence-scorer.js    # From WIS-2
│   ├── registry/
│   │   └── workflow-registry.js    # From WIS-2
│   └── index.js                    # Public API
│
├── core/session/
│   └── context-loader.js           # ENHANCED with task hook
│
└── development/tasks/
    └── next.md                     # NEW - Task definition
```

---

## Dependencies

### Blocked By
- **WIS-2:** Workflow Registry Enhancement ✅ (Done - provides registry and scoring)

### Blocks
- **WIS-4:** Wave Analysis Engine (extends suggestions with wave info)
- **WIS-5:** Pattern Capture (captures usage of *next)

### Related
- **@dev agent:** Will receive new `*next` command
- **GreetingBuilder:** Integrates with workflow context

---

## Success Criteria

1. `*next` returns context-aware suggestions
2. Suggestion latency <100ms
3. 85% suggestion accuracy on test scenarios
4. Help documentation accessible
5. Existing functionality unaffected
6. All tests pass

---

## Non-Functional Requirements (NFR)

### Performance
| Metric | Target |
|--------|--------|
| Suggestion latency | <100ms |
| Cache TTL | 5 minutes |
| Module loading | Lazy (on-demand) |
| Memory footprint | <10MB additional |

### Reliability
- [ ] Graceful degradation if registry unavailable
- [ ] Fallback to generic suggestions when context unclear
- [ ] No crashes on malformed input

### Maintainability
- [ ] Modular engine architecture (SuggestionEngine, ConfidenceScorer)
- [ ] Clear API contracts with JSDoc
- [ ] Extensible for future workflow types

### Security
- [ ] No sensitive data in suggestions output
- [ ] Story paths validated before use
- [ ] No command injection vectors

---

## Testing

**Test Location:** `tests/unit/workflow-intelligence/suggestion-engine.test.js`

**Validation:**
1. Run unit tests: `npm test -- suggestion-engine`
2. Run integration tests: `npm test -- wis-integration`
3. Performance benchmark: `npm run benchmark:wis`

**Test Scenarios:**

| Scenario | Input | Expected Output |
|----------|-------|-----------------|
| New session | No command history | Generic suggestions, low confidence (<50%) |
| Mid-workflow (dev) | After `*develop` | `*run-tests`, `*review-qa` with high confidence |
| Mid-workflow (po) | After `*validate-story` | `*backlog-add`, `*sync-story` |
| Ambiguous context | Multiple workflows match | Highest confidence workflow selected |
| No matching workflow | Unknown command sequence | "Unable to determine workflow" message |
| Story override | `--story` flag provided | Use explicit story context |
| Help flag | `--help` flag | Display usage documentation |

**Performance Tests:**

| Test | Metric | Target |
|------|--------|--------|
| Cold start | First suggestion | <200ms |
| Warm cache | Subsequent suggestions | <50ms |
| Registry load | Pattern matching | <100ms |

---

## File List

| File | Status | Description |
|------|--------|-------------|
| `docs/stories/v2.1/sprint-11/story-wis-3-next-task.md` | Ready | This story |
| `.aios-core/development/tasks/next.md` | To Create | Task definition |
| `.aios-core/workflow-intelligence/engine/suggestion-engine.js` | To Create | Core suggestion engine |
| `.aios-core/core/session/context-loader.js` | To Modify | Add task completion hook |
| `tests/unit/workflow-intelligence/suggestion-engine.test.js` | To Create | Unit tests |
| `tests/integration/workflow-intelligence/wis-integration.test.js` | To Create | Integration tests |

---

## Tasks / Subtasks

- [ ] **Task 1: Create Task Definition** (AC: 3.1)
  - [ ] Create `.aios-core/development/tasks/next.md`
  - [ ] Define arguments (--story, --all, --help)
  - [ ] Document integration with SuggestionEngine

- [ ] **Task 2: Implement Suggestion Engine** (AC: 3.2)
  - [ ] Create `suggestion-engine.js`
  - [ ] Integrate with WorkflowRegistry
  - [ ] Integrate with ConfidenceScorer
  - [ ] Implement `suggestNext(context)` method

- [ ] **Task 3: Task Completion Hook** (AC: 3.3)
  - [ ] Enhance `context-loader.js`
  - [ ] Implement `onTaskComplete(taskName, result)`
  - [ ] Update session state tracking

- [ ] **Task 4: CLI Output Formatting** (AC: 3.4)
  - [ ] Implement formatted suggestion display
  - [ ] Add confidence indicators
  - [ ] Add color support (optional)

- [ ] **Task 5: Context Override** (AC: 3.5)
  - [ ] Implement `--story` flag
  - [ ] Validate story paths
  - [ ] Merge with auto-detected context

- [ ] **Task 6: Help Integration** (AC: 3.6)
  - [ ] Implement `--help` flag
  - [ ] Write usage documentation

- [ ] **Task 7: Performance Optimization** (AC: 3.7)
  - [ ] Implement caching (5-min TTL)
  - [ ] Lazy loading of WIS modules
  - [ ] Verify <100ms latency

- [ ] **Task 8: Testing** (AC: 3.8)
  - [ ] Write unit tests for SuggestionEngine
  - [ ] Write integration tests
  - [ ] Write performance tests
  - [ ] Verify all scenarios pass

---

## Dev Agent Record

### Agent Model Used
*(To be filled by @dev)*

### Debug Log References
*(To be filled during development)*

### Completion Notes
*(To be filled after implementation)*

---

## Change Log

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0 | 2025-12-23 | @sm (River) | Initial draft from MVP scope |
| 1.1 | 2025-12-25 | @po (Pax) | PO Validation: APPROVED - Added NFR, Testing sections, Tasks breakdown, updated dependencies |
