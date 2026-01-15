# QA - Test Automation Artifacts

## Overview

This folder contains all test automation artifacts including test specifications and Playwright test code.

## Folder Structure

```
QA/
├── README.md              # This file
├── TestSpecs/             # Human-readable test specifications
│   └── *.spec.md          # Generated from Jira/Trello stories
├── tests/                 # Playwright test code
│   ├── pages/             # Page Object Models
│   └── *.spec.ts          # Test files
└── test-data/             # Test data and fixtures
```

## Quick Start

### Generate E2E Tests from Story

```
Run @KB/Prompts/e2e-test-generation.md with jira id <STORY_ID>
```

This will generate:
- Test Specification in `TestSpecs/`
- Page Objects in `tests/pages/`
- Test Files in `tests/`

### Run Tests

```bash
# Navigate to UI app
cd ../UI/provider-search-app

# Run all tests
npx playwright test

# Run specific test
npx playwright test tests/user-login.spec.ts

# Run in UI mode
npx playwright test --ui
```

## Knowledge Base

All documentation, standards, and prompt templates are in the `@KB` folder:
- `@KB/README.md` - Complete workflow documentation
- `@KB/BestPractices/` - Test automation standards
- `@KB/Prompts/` - Prompt templates for generation
- `@KB/Requirements/` - Acceptance criteria standards

## Test Artifacts

### Test Specifications (`TestSpecs/`)
Human-readable test specs generated from Jira/Trello stories following Gherkin format.

**Naming**: `<STORY_ID>-<feature-name>.spec.md`

### Playwright Tests (`tests/`)
Executable test code following Page Object Model pattern.

**Structure**:
- `pages/` - Page Object classes
- `*.spec.ts` - Test files

### Test Data (`test-data/`)
Shared test data, fixtures, and configuration.

## Workflow

1. **Story Ready** - Jira/Trello story with Gherkin acceptance criteria
2. **Generate Tests** - Use `@KB/Prompts/e2e-test-generation.md`
3. **Review** - Check generated test spec and code
4. **Execute** - Run tests with Playwright
5. **Maintain** - Update as application changes

## Support

For detailed workflow, standards, and guidelines, refer to `@KB/README.md`
