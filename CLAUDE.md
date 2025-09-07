# CLAUDE.md - AI Assistant Project Guidelines

This file provides critical guidance for Claude Code and other AI assistants when working with this codebase.

## 📋 Project-Specific Configuration

### [ADD YOUR PROJECT DETAILS HERE]

```yaml
# Example configuration - customize for your project
project:
  name: "Your Project Name"
  type: "web|cli|library|service"
  language: "TypeScript|Python|Go|etc"
  
architecture:
  frontend: "React|Vue|Angular|none"
  backend: "Node|Python|Go|etc"
  database: "PostgreSQL|MySQL|MongoDB|etc"
  
paths:
  source: "./src"
  tests: "./tests"
  docs: "./docs"
  
conventions:
  style_guide: "link-to-style-guide"
  test_framework: "jest|pytest|etc"
  min_coverage: 80
```

## 🛑 CRITICAL: Root Cause Analysis Required

### The Junior Developer Trap

**NEVER DO THIS:**
- ❌ Forcing features to be enabled when health checks fail
- ❌ Hardcoding mock data when APIs return errors  
- ❌ Adding try/catch blocks that swallow errors silently
- ❌ Writing workarounds without understanding the problem
- ❌ Assuming something is "just broken" without investigating

### Required Approach for ALL Issues

When encountering ANY error or unexpected behavior:

1. **STOP AND INVESTIGATE** (No code until you understand)
   - What is the actual error message?
   - What service/component is failing and why?
   - Is this a configuration issue or a real bug?
   - What does the error tell us about the system state?

2. **TRACE THE FULL CHAIN**
   - Follow the data flow through all layers
   - Where exactly does it break?
   - What are the actual vs expected values at each step?

3. **VERIFY ASSUMPTIONS**
   - Check if services are actually running
   - Verify network connectivity between components
   - Confirm configuration matches reality
   - Test the simplest case first

4. **ONLY THEN FIX THE ROOT CAUSE**
   - No bandaids, no workarounds
   - Fix the actual problem
   - If you can't fix it, explain why and ask for guidance

### The Mindset Shift

**From**: "How can I make this error go away?"  
**To**: "What is this error teaching me about the system?"

Every error is valuable diagnostic information. Hiding it is like putting tape over the check engine light.

## 🛑 Universal Rules

### 1. NEVER Use Mock Data When Real Endpoints Exist
- If API returns error, find and fix the issue
- Check correct endpoint paths and methods
- NEVER replace API calls with fake data

### 2. Frontend Must Adapt to Backend
- Backend APIs define the contract
- NEVER change backend to match frontend expectations
- Frontend must handle what backend provides

### 3. Follow Existing Patterns
- Study the codebase before implementing
- Use established patterns and conventions
- Don't introduce new patterns without discussion

## 📁 Common Patterns to Follow

### API Patterns

```typescript
// Frontend example - customize for your stack
import { getApiUrl } from '@/utils/api';
import { API_PATHS } from '@/constants/apiPaths';

// NEVER hardcode paths
const url = getApiUrl(API_PATHS.RESOURCE.GET(id));
```

```python
# Backend example - customize for your framework
async def endpoint(
    service: ServiceInterface = Depends(get_service),
    current_user: User = Depends(get_current_user)
):
    pass
```

## 🧪 Testing Requirements

- **Coverage Target**: [Set your minimum, e.g., 80%]
- **Test Categories**: unit, integration, e2e
- **Test Structure**: Mirror source code structure
- **New Features**: Must include tests
- **Bug Fixes**: Must include regression tests

## 🛠️ Development Guidelines

### When Implementing Features

1. Search codebase for existing functionality
2. Use established patterns and conventions
3. Write tests alongside implementation
4. Run linting and type checking
5. Document complex logic

### When Fixing Issues

1. Find root cause, don't patch symptoms
2. Fix all occurrences, not just reported one
3. Add tests to prevent regression
4. Document why the fix was needed
5. Verify fix doesn't break other features

## 📝 Task Management

Use TodoWrite tool for complex tasks:

```python
# Example task breakdown
todos = [
    {"content": "Analyze requirements", "status": "pending"},
    {"content": "Design solution", "status": "pending"},
    {"content": "Implement core logic", "status": "pending"},
    {"content": "Write tests", "status": "pending"},
    {"content": "Update documentation", "status": "pending"}
]
```

Rules:
- One task `in_progress` at a time
- Mark complete immediately when done
- Break complex tasks into steps
- Track all multi-step operations

## 🚀 Common Commands

```bash
# Customize these for your project
# Development
npm run dev          # or: python manage.py runserver
npm run build        # or: cargo build --release
npm run test         # or: pytest
npm run lint         # or: pylint src/

# Docker (if applicable)
docker compose up -d
docker compose logs -f [service]
docker compose down

# Version Control
git status
git diff
git log --oneline -10
```

## 🎯 Quality Checklist

Before completing any task:

- [ ] Code follows project conventions
- [ ] Tests are written and passing
- [ ] Linting passes without errors
- [ ] Type checking passes (if applicable)
- [ ] Documentation is updated
- [ ] No hardcoded values or mock data
- [ ] Error handling is appropriate
- [ ] Security best practices followed

## 📚 Sub-Agent Strategy

For complex tasks, consider using specialized agents:

- **Code Review Agent**: Quality and security checks
- **Debugging Agent**: Complex issue investigation
- **Refactoring Agent**: Large-scale code updates
- **Test Implementation Agent**: Coverage improvements
- **Documentation Agent**: Comprehensive docs

## ⚠️ Common Mistakes to Avoid

1. **Creating Mock Responses**
   - ❌ Returning hardcoded data when API fails
   - ✅ Finding and fixing the actual issue

2. **Ignoring Existing Patterns**
   - ❌ Introducing new patterns arbitrarily
   - ✅ Following established conventions

3. **Patching Symptoms**
   - ❌ Quick fixes that hide problems
   - ✅ Root cause analysis and proper fixes

4. **Skipping Tests**
   - ❌ "I'll add tests later"
   - ✅ TDD or tests alongside implementation

5. **Poor Error Handling**
   - ❌ Swallowing exceptions silently
   - ✅ Appropriate logging and user feedback

## 🤝 Multi-Model Collaboration

### MCP Server Integration (When Available)

If your environment supports MCP (Model Context Protocol) servers, you can leverage direct integration:

```yaml
# Example MCP server configuration
mcp_servers:
  codex:
    description: "OpenAI Codex/GPT for rapid implementation"
    capabilities: ["code", "test", "docs", "parallel_tasks"]
    
  playwright:
    description: "Browser automation for testing"
    capabilities: ["e2e_tests", "ui_testing", "screenshots"]
```

**Note**: MCP servers may not be available in all environments. Test with a simple command first to verify availability.

### Working with Other AI Assistants

When working with multiple AI models (Claude, GPT-4, Codex, etc.), establish clear roles:

### Collaboration Visibility Protocol

**IMPORTANT**: Always announce when collaborating with other models:

```
🤝 Working with [Model] on: [task description]
   Reason: [why this model is suited for this]
   Expected outcome: [what will be delivered]
```

### When to Use Multi-Model Collaboration

Consider using multiple models for:

1. **Test Generation** - Comprehensive test suites after features
2. **Parallel Implementation** - Multiple independent components
3. **Boilerplate Code** - CRUD operations, standard patterns
4. **Code Transformation** - Format conversions, migrations
5. **Documentation** - API docs, inline docs, README updates

### Task Assignment by Strength

**Primary Assistant (Claude/GPT-4) handles:**
- Architecture design and planning
- Complex reasoning and debugging
- Security analysis and review
- Code refactoring strategies
- Understanding legacy code
- Root cause analysis

**Delegate to specialized tools/models:**
- Rapid parallel implementation
- Test suite generation
- API documentation
- Boilerplate and CRUD
- Code format conversions
- Repetitive updates across files

## 🔄 Continuous Improvement

This document should evolve with your project:

1. Add project-specific rules as discovered
2. Document architectural decisions
3. Update patterns when they change
4. Record lessons learned from issues
5. Keep commands and configs current

## Remember

1. Understand before implementing
2. Follow patterns, don't create new ones
3. Test everything you build
4. Document why, not just what
5. Collaborate transparently

---

*Customize this template for your specific project needs*
