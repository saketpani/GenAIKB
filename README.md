# Full-Stack Development with Gen AI Agents

## Overview

This project demonstrates end-to-end software development automation using Gen AI agents (Amazon Q, GitHub Copilot). It covers the complete SDLC from requirements to tested, working features.

## Project Structure

```
AgentProjects/
├── README.md                          # This file - Master overview
│
├── KB/                                # QA Knowledge Base
│   ├── BestPractices/                 # Test automation standards
│   ├── Prompts/                       # Test generation templates
│   └── Requirements/                  # Acceptance criteria standards
│
├── KB-Backend/                        # Backend Knowledge Base
│   ├── BestPractices/                 # API design standards
│   ├── Prompts/                       # API generation templates
│   └── Requirements/                  # Backend story standards
│
├── QA/                                # Test Automation Artifacts
│   ├── TestSpecs/                     # Human-readable test specs
│   ├── tests/                         # Playwright E2E tests
│   └── test-data/                     # Test fixtures
│
├── Backend/                           # Backend API
│   ├── api-specs/                     # OpenAPI specifications
│   ├── src/                           # API implementation
│   └── tests/                         # API tests
│
└── UI/                                # Frontend Application
    └── provider-search-app/           # React application
```

## Workflows

### 1. Backend API Development
```
Jira/Trello Story
    ↓
Analyze Intent (Backend vs UI)
    ↓
Generate OpenAPI Spec (YAML)
    ↓
Generate API Implementation
    ↓
Generate API Tests
```

**Command**:
```
Run @KB-Backend/Prompts/e2e-api-generation.md with jira id <STORY_ID>
```

### 2. Frontend Development
```
UI Requirements
    ↓
React Components
    ↓
Integration with Backend APIs
```

### 3. QA Test Automation
```
Jira/Trello Story (Gherkin)
    ↓
Generate Test Specification
    ↓
Generate Playwright Tests
    ↓
Execute & Validate
```

**Command**:
```
Run @KB/Prompts/e2e-test-generation.md with jira id <STORY_ID>
```

## Complete E2E Flow

```
Story: "User Login Feature"
    ↓
Backend Agent → Generates Login API (POST /auth/login)
    ↓
Frontend → Integrates with Login API
    ↓
QA Agent → Generates E2E Tests
    ↓
Result: Fully tested, working feature
```

## Key Features

### 1. **Backend Development Automation**
- Intent analysis (Backend vs UI changes)
- OpenAPI spec generation
- API endpoint implementation
- Error handling and validation
- API test generation

### 2. **QA Test Automation**
- Test specification generation
- Playwright test code generation
- Page Object Model pattern
- E2E test coverage

### 3. **Quality Gates**
- Story validation (proper format, completeness)
- Standards enforcement (best practices)
- Error handling (missing data, conflicts)
- Traceability (Story → Spec → Code)

### 4. **AI Agent Agnostic**
- Works with Amazon Q
- Works with GitHub Copilot
- Extensible to other AI agents

## Getting Started

### Prerequisites
- Node.js 16+
- Git
- Amazon Q or GitHub Copilot access
- Jira/Trello account (for story management)

### Setup

1. **Clone/Open Workspace**
   ```bash
   cd AgentProjects
   ```

2. **Install UI Dependencies**
   ```bash
   cd UI/provider-search-app
   npm install
   ```

3. **Install Backend Dependencies** (Coming soon)
   ```bash
   cd Backend
   npm install
   ```

4. **Configure Jira/Trello**
   - Update URLs in prompt templates
   - Set up API access tokens

### Usage

#### Generate Backend API
```
Run @KB-Backend/Prompts/e2e-api-generation.md with jira id PROV-101
```

#### Generate E2E Tests
```
Run @KB/Prompts/e2e-test-generation.md with jira id PROV-101
```

#### Run Application
```bash
cd UI/provider-search-app
npm start
```

#### Run Tests
```bash
cd QA
npx playwright test
```

## Knowledge Bases

### QA Knowledge Base (@KB)
- Test automation standards
- Playwright best practices
- Test generation prompts
- Gherkin acceptance criteria format

### Backend Knowledge Base (@KB-Backend)
- API design standards
- OpenAPI specification guidelines
- Backend generation prompts
- API story templates

## Use Cases

### 1. New Feature Development
```
Story → Backend API → Frontend Integration → E2E Tests
```

### 2. API Enhancement
```
Story → Analyze existing API → Add new endpoints → Update tests
```

### 3. Bug Fix
```
Bug Report → Identify affected API/UI → Generate regression tests
```

### 4. Refactoring
```
Existing code → Generate tests → Refactor → Validate with tests
```

## Benefits

### Productivity
- **60-80% reduction** in test creation time
- **50-70% reduction** in API development time
- Faster time-to-market

### Quality
- Consistent coding standards
- Comprehensive test coverage
- Reduced human error
- Built-in best practices

### Scalability
- Template-based approach
- Reusable patterns
- Easy onboarding for new team members

### Traceability
- Story ID → API Spec → Implementation → Tests
- Clear audit trail
- Easy maintenance

## Technology Stack

### Frontend
- React 19
- TypeScript
- React Router
- CSS3

### Backend (Coming)
- Node.js / Express (or your choice)
- OpenAPI 3.0
- RESTful APIs

### Testing
- Playwright
- TypeScript
- Page Object Model

### AI Agents
- Amazon Q
- GitHub Copilot

## Project Status

- ✅ QA Test Automation - Complete
- ✅ Frontend Application - Complete
- 🚧 Backend API Development - In Progress
- 📋 Integration - Planned

## Roadmap

### Phase 1: Foundation (Complete)
- [x] Provider Search UI application
- [x] QA knowledge base and standards
- [x] Test generation workflow
- [x] Playwright test automation

### Phase 2: Backend Development (Current)
- [ ] Backend knowledge base
- [ ] API generation workflow
- [ ] OpenAPI spec generation
- [ ] API implementation generation
- [ ] API test generation

### Phase 3: Integration
- [ ] Connect Backend APIs to Frontend
- [ ] End-to-end feature workflow
- [ ] Complete demo scenario

### Phase 4: Enhancement
- [ ] CI/CD integration
- [ ] Metrics dashboard
- [ ] Multi-language support
- [ ] Custom organizational standards

## Contributing

This is a demonstration project for Gen AI agent capabilities in software development automation.

## Use Cases for Organizations

### 1. Proof of Concept
Demonstrate AI-driven development to stakeholders

### 2. Training
Onboard teams to AI-assisted development

### 3. Standards Template
Use as foundation for organizational standards

### 4. Productivity Baseline
Measure and improve development efficiency

## Contact

For questions or discussions about this approach, reach out to your Agentic AI practice team.

---

**Note**: This project demonstrates the art of the possible with Gen AI agents. Adapt and extend based on your organization's specific needs, tools, and processes.
