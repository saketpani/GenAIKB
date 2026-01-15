# Knowledge Base - Test Automation Standards & Guidelines

## Overview

This folder contains all documentation, standards, best practices, and prompt templates for the Gen AI QA automation workflow.

## Folder Structure

```
KB/
├── README.md                          # This file
├── BestPractices/                     # Test automation standards
│   └── test-automation-best-practices.md
├── Prompts/                           # Prompt templates
│   ├── e2e-test-generation.md         # Complete E2E workflow
│   ├── generate-test-spec.md          # Story → Test Spec
│   └── generate-playwright-test.md    # Test Spec → Playwright
└── Requirements/                      # Requirements standards
    ├── acceptance-criteria-standard.md
    └── trello-card-template.md
```

## Quick Reference

### For Developers/Testers

**Generate E2E Tests**:
```
Run @KB/Prompts/e2e-test-generation.md with jira id <STORY_ID>
```

**Key Documents**:
- `Prompts/e2e-test-generation.md` - Main workflow template
- `BestPractices/test-automation-best-practices.md` - Coding standards
- `Requirements/acceptance-criteria-standard.md` - Story format

### For AI Agents

**Context Files to Reference**:
1. This README for workflow overview
2. `BestPractices/test-automation-best-practices.md` for standards
3. `Requirements/acceptance-criteria-standard.md` for Gherkin format
4. Appropriate prompt template from `Prompts/`

## Workflow

```
Jira/Trello Story (Gherkin)
    ↓
Test Specification (Human-Readable)
    ↓
Playwright Test Code (Executable)
```

## Documents

### Best Practices
- **test-automation-best-practices.md**: Complete guide for test spec creation and Playwright code generation

### Prompts
- **e2e-test-generation.md**: Complete E2E workflow from story ID
- **generate-test-spec.md**: Convert acceptance criteria to test spec
- **generate-playwright-test.md**: Convert test spec to Playwright code

### Requirements
- **acceptance-criteria-standard.md**: Gherkin format and story structure
- **trello-card-template.md**: Template for creating stories

## Usage

### Step 1: Ensure Story is Ready
- Story has Gherkin acceptance criteria
- All required fields populated
- Test data specified

### Step 2: Generate Tests
Use the E2E prompt template with story ID

### Step 3: Review & Execute
- Review generated test spec and code
- Run tests with Playwright
- Iterate as needed

## Maintenance

- Keep standards updated with new patterns
- Add examples as they emerge
- Document lessons learned
- Maintain consistency across templates

## Integration

This knowledge base is referenced by:
- AI agents (Amazon Q, Copilot) for context
- Developers for standards and guidelines
- Test generation workflows for consistency
- CI/CD pipelines for validation

---

**Note**: All test artifacts (specs, code) are generated in the `@QA` folder. This KB folder contains only documentation and templates.
