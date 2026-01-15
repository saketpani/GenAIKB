# E2E Test Generation from Story ID

## Usage

Replace `{{STORY_ID}}` with your actual Trello/Jira story ID and run this prompt.

**Example**:

- For Trello: Replace `{{STORY_ID}}` with `PROV-101`
- For Jira: Replace `{{STORY_ID}}` with `PROJ-1234`

---

## Prompt Template

I need to generate end-to-end Playwright tests for the following story:

**Story ID**: `{{STORY_ID}}`

**Trello Board URL**: `https://trello.com/b/YOUR_BOARD_ID` (Update with your board URL)
**Jira Project URL**: `https://your-domain.atlassian.net/browse/{{STORY_ID}}` (Update with your Jira URL)

Please follow this workflow:

### Step 1: Fetch Story Details

**CRITICAL**: Before proceeding, validate the story exists and has required information.

Retrieve the story details from Trello/Jira including:

- Story Title
- User Story (As a... I want... So that...)
- Acceptance Criteria (in Gherkin format)
- Notes (test data, environment, assumptions)

**Validation Checks**:

- ✅ Story ID `{{STORY_ID}}` exists and is accessible
- ✅ Story has a title
- ✅ Story has user story in proper format
- ✅ Acceptance criteria are present and in Gherkin format (Given-When-Then)
- ✅ All required fields are populated

**If ANY validation fails**:

- STOP the workflow immediately
- Inform the user about the missing/incorrect information
- Request clarification or correction
- DO NOT proceed to Step 2

**If ALL validations pass**:

- Proceed to Step 2

### Step 2: Generate Test Specification

Using the story details and following these guidelines:
- Read `@KB/README.md` for workflow context
- Follow `@KB/Requirements/acceptance-criteria-standard.md` for Gherkin format
- Use `@KB/Prompts/generate-test-spec.md` as the generation guide
- Follow `@KB/BestPractices/test-automation-best-practices.md` for standards
- Use `@QA/TestSpecs/test-spec-template.md` as the output template

Generate a comprehensive test specification and save it as:
`QA/TestSpecs/{{STORY_ID}}-<feature-name>.spec.md`

### Step 3: Generate Playwright Test Code

Using the generated test specification:
- Follow `@KB/Prompts/generate-playwright-test.md` as the generation guide
- Follow `@KB/BestPractices/test-automation-best-practices.md` for coding standards
- Create Page Objects for reusable components
- Implement all test scenarios with proper assertions

Generate Playwright test files:
- Page Objects: `UI/provider-search-app/tests/pages/<PageName>Page.ts`
- Test File: `UI/provider-search-app/tests/<feature-name>.spec.ts`

### Step 4: Validation

Ensure the generated tests:

- [ ] Cover all acceptance criteria scenarios
- [ ] Use documented selectors (prefer data-testid)
- [ ] Follow Page Object Model pattern
- [ ] Include proper waits and assertions
- [ ] Are independent and can run in any order
- [ ] Follow naming conventions
- [ ] Are maintainable and readable

### Step 5: Summary

Provide a comprehensive summary including:
- Test specification file location
- Playwright test files created (Page Objects and Test Files)
- Number of test scenarios implemented
- Coverage of acceptance criteria
- Instructions to run the tests

---

## Configuration

**Application Details**:

- Application: Provider Search App
- Base URL: `http://localhost:3000`
- Framework: React with TypeScript
- Test Framework: Playwright

**Test Credentials**:

- Email: `guest`
- Password: `guest`

**Selector Strategy**:

- Primary: `data-testid` attributes
- Fallback: ARIA roles and labels

---

## Expected Output

After running this prompt, you should have:

1. **Test Specification**: `QA/TestSpecs/{{STORY_ID}}-<feature-name>.spec.md`

   - Complete test scenarios
   - Documented selectors
   - Test data and environment details

2. **Page Objects**: `UI/provider-search-app/tests/pages/<PageName>Page.ts`

   - Encapsulated selectors
   - Reusable action methods
   - Clean, maintainable code

3. **Test File**: `UI/provider-search-app/tests/<feature-name>.spec.ts`

   - All scenarios implemented
   - Proper assertions
   - Following best practices

4. **Execution Instructions**: Commands to run the generated tests

---

## Notes

**IMPORTANT - Error Handling**:

- If Story ID `{{STORY_ID}}` is not found in Trello/Jira, STOP and ask for clarification
- If acceptance criteria are missing or not in Gherkin format, STOP and request proper format
- If required fields (User Story, Title) are missing, STOP and ask user to complete the story
- If selectors (data-testid) are not present in the application, STOP and notify the developer
- DO NOT generate tests with placeholder or assumed data
- DO NOT proceed if any validation step fails

**Validation Before Generation**:

1. Verify story exists and is accessible
2. Confirm acceptance criteria are in proper Gherkin format
3. Ensure all required story fields are populated
4. Validate that the application has the necessary test IDs
5. Only proceed if all validations pass

**Quality Standards**:
- Ensure the story has acceptance criteria in Gherkin format
- Follow the standards and templates strictly for consistency
- Generated tests should be production-ready and maintainable
- All generated code must be reviewed against best practices
- Developer/tester will review generated artifacts and can request changes as needed
