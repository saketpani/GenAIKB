# Prompt: Generate Test Specification from Story

## Context

You are an AI agent tasked with generating a detailed test specification from a Trello/Jira story with acceptance criteria. The test specification will be used by QA engineers and as input for automated Playwright test generation.

## Prerequisites

Before starting, ensure you have read and understood:
1. `../Requirements/acceptance-criteria-standard.md` - Story and Gherkin format
2. `BestPractices/test-automation-best-practices.md` - Test spec guidelines
3. `TestSpecs/test-spec-template.md` - Output format template

## Input

**Story ID**: `<STORY_ID>` (e.g., PROV-101, CARD-456)

**Story Details**:
- Story Title: `<title>`
- User Story: `<As a... I want... So that...>`
- Acceptance Criteria: `<Gherkin scenarios>`
- Notes: `<test data, environment, assumptions>`

## Task

Generate a comprehensive test specification that:
1. Translates Gherkin scenarios into detailed test steps
2. Identifies all UI elements and their selectors
3. Defines test data requirements
4. Specifies expected results clearly
5. Includes environment and configuration details

## Output Format

Follow the structure in `TestSpecs/test-spec-template.md`:

```markdown
# Test Specification

## Test Spec Metadata

**Test Spec ID**: `TS-<STORY_ID>`
**Story ID**: `<STORY_ID>`
**Feature**: `<Feature Name>`
**Created Date**: `<YYYY-MM-DD>`
**Last Updated**: `<YYYY-MM-DD>`

---

## Test Overview

**Objective**: <What this test validates>

**Scope**: <What is covered and not covered>

**Prerequisites**:
- <List all preconditions>

**Test Data**:
- <List all test data needed>

---

## Test Scenarios

### Scenario 1: <Scenario Name from Acceptance Criteria>

**Test Case ID**: `TC-<STORY_ID>-01`
**Priority**: <High/Medium/Low>
**Type**: <Functional/UI/E2E/Integration>

**Given**:
- <Precondition 1>
- <Precondition 2>

**When**:
- <Action 1>
- <Action 2>

**Then**:
- <Expected outcome 1>
- <Expected outcome 2>

**Test Steps**:
1. Navigate to <page/URL>
2. Verify <element> is visible
3. Enter <data> in <field>
4. Click <button>
5. Verify <expected result>

**Expected Results**:
- <Specific, measurable outcome 1>
- <Specific, measurable outcome 2>

**Selectors/Locators**:
- <Element name>: `data-testid="<testid>"`
- <Element name>: `role="<role>" name="<name>"`

---

## Test Environment

**Application URL**: `http://localhost:3000`
**Browser**: `Chrome/Firefox/Safari/Edge`
**Viewport**: `Desktop (1920x1080)`
**Test Framework**: `Playwright`

---

## Notes

- <Additional context>
- <Known issues or limitations>
- <Dependencies on other features>
```

## Guidelines

### 1. Test Scenario Conversion

For each Gherkin scenario in acceptance criteria:

**Given** → Preconditions and Setup
- Identify initial state
- List navigation steps
- Document authentication requirements

**When** → Actions
- Break down into specific, actionable steps
- Identify all UI interactions
- Document input data

**Then** → Expected Results
- Define specific, measurable outcomes
- Include all assertions
- Cover both positive and negative cases

### 2. Selector Identification

For every UI element mentioned:
- Prefer `data-testid` attributes
- Document the selector in the spec
- Use semantic, descriptive names
- Follow naming convention: `<element-purpose>` or `<component>-<element>`

**Examples**:
- Login button: `data-testid="login-btn"`
- Email input: `data-testid="email"`
- Error message: `data-testid="login-error"`
- Search results: `data-testid="result-item"`

### 3. Test Data Definition

Clearly specify:
- Valid test data (happy path)
- Invalid test data (error scenarios)
- Edge cases and boundary values
- Any data dependencies

**Example**:
```
**Test Data**:
- Valid credentials: email="guest", password="guest"
- Invalid credentials: email="wrong@test.com", password="wrong123"
- Provider search: specialty="Cardiology"
- Booking time slot: "2024-02-15 10:00 AM"
```

### 4. Test Steps Detail Level

Write steps that are:
- Specific and actionable
- Technology-agnostic (no code)
- Clear enough for manual testing
- Detailed enough for automation

**Good Example**:
```
1. Navigate to the login page at /login
2. Verify the email input field is visible
3. Enter "guest" in the email field
4. Enter "guest" in the password field
5. Click the "Login" button
6. Verify redirect to home page (/)
7. Verify logout button is visible in navigation
```

**Bad Example**:
```
1. Go to login
2. Login
3. Check if logged in
```

### 5. Priority Assignment

- **High**: Core functionality, critical user flows, security
- **Medium**: Important features, common use cases
- **Low**: Edge cases, nice-to-have features

### 6. Test Type Classification

- **Functional**: Business logic, feature behavior
- **UI**: Visual elements, layout, styling
- **E2E**: Complete user journeys across multiple pages
- **Integration**: Interaction between components/services

## Example Conversion

### Input: Acceptance Criteria

```gherkin
Scenario: Successful login with valid credentials
  Given the user is on the login page
  When the user enters valid email "guest"
  And the user enters valid password "guest"
  And the user clicks the login button
  Then the user is redirected to the home page
  And the logout button is visible in the navigation menu
  And the login button is not visible in the navigation menu
```

### Output: Test Specification

```markdown
### Scenario 1: Successful Login with Valid Credentials

**Test Case ID**: `TC-PROV-101-01`
**Priority**: `High`
**Type**: `E2E`

**Given**:
- The user is on the login page (/login)
- The user is not authenticated

**When**:
- The user enters valid email "guest"
- The user enters valid password "guest"
- The user clicks the login button

**Then**:
- The user is redirected to the home page (/)
- The logout button is visible in the navigation menu
- The login button is not visible in the navigation menu

**Test Steps**:
1. Navigate to http://localhost:3000/login
2. Verify the login page is loaded (heading "Login" is visible)
3. Locate the email input field using data-testid="email"
4. Enter "guest" in the email field
5. Locate the password input field using data-testid="password"
6. Enter "guest" in the password field
7. Locate the login button using data-testid="login-btn"
8. Click the login button
9. Wait for navigation to complete
10. Verify the current URL is http://localhost:3000/
11. Locate the logout button using data-testid="logout-btn"
12. Verify the logout button is visible
13. Verify the login link is not present in the navigation

**Expected Results**:
- Login page loads successfully with email, password fields and login button
- After clicking login, user is redirected to home page within 2 seconds
- Home page displays with logout button visible in navigation
- Login link is not visible in navigation (conditional rendering based on auth state)
- No error messages are displayed

**Selectors/Locators**:
- Email input: `data-testid="email"`
- Password input: `data-testid="password"`
- Login button: `data-testid="login-btn"`
- Logout button: `data-testid="logout-btn"`
- Login link: `Link with text "Login"`
- Page heading: `h2` with text "Login"
```

## Validation Checklist

Before finalizing the test spec, verify:

- [ ] All scenarios from acceptance criteria are included
- [ ] Each scenario has clear Given-When-Then structure
- [ ] Test steps are detailed and actionable
- [ ] All UI elements have documented selectors
- [ ] Test data is clearly defined
- [ ] Expected results are specific and measurable
- [ ] Priority and type are assigned
- [ ] Test environment details are complete
- [ ] Prerequisites are listed
- [ ] Scope is clearly defined

## Common Mistakes to Avoid

❌ **Vague test steps**
```
1. Login to the application
```

✅ **Specific test steps**
```
1. Navigate to http://localhost:3000/login
2. Enter "guest" in email field (data-testid="email")
3. Enter "guest" in password field (data-testid="password")
4. Click login button (data-testid="login-btn")
```

❌ **Missing selectors**
```
Click the submit button
```

✅ **Documented selectors**
```
Click the submit button (data-testid="submit-btn")
```

❌ **Generic expected results**
```
The page should work correctly
```

✅ **Specific expected results**
```
- User is redirected to /dashboard
- Welcome message "Hello, Guest" is displayed
- Navigation menu shows 5 menu items
```

## Usage Instructions

1. **Fetch the story** from Trello/Jira using the Story ID
2. **Extract** the acceptance criteria in Gherkin format
3. **Apply this prompt** with the story details
4. **Generate** the test specification following the template
5. **Save** the output as `TestSpecs/<STORY_ID>-<feature-name>.spec.md`
6. **Review** against the validation checklist
7. **Iterate** if needed to improve clarity and completeness

## Next Step

After generating the test specification, use `Prompts/generate-playwright-test.md` to convert it into executable Playwright test code.

---

**Remember**: The test specification is a bridge between business requirements and automated tests. It should be clear enough for humans to understand and detailed enough for AI agents to generate accurate test code.
