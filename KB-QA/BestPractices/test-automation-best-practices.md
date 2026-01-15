# Test Automation Best Practices

## Overview
This document defines best practices for generating test specifications and Playwright test code from acceptance criteria. Follow these guidelines to ensure consistent, maintainable, and reliable test automation.

---

## Test Specification Best Practices

### 1. Structure and Organization

**DO:**
- Use clear, hierarchical structure (Feature → Scenario → Test Steps)
- Include metadata (Test ID, Story ID, Priority, Type)
- Group related test scenarios together
- Maintain traceability to requirements via Story ID

**DON'T:**
- Mix multiple features in one test spec
- Skip metadata or documentation
- Create overly complex nested structures

### 2. Test Scenario Design

**DO:**
- Write independent, isolated test scenarios
- Follow Given-When-Then structure
- Include both positive and negative test cases
- Test one behavior per scenario
- Use descriptive scenario names that explain what is being tested

**DON'T:**
- Create dependencies between test scenarios
- Test multiple unrelated behaviors in one scenario
- Use vague names like "Test 1" or "Check functionality"

### 3. Test Data Management

**DO:**
- Define test data explicitly in the spec
- Use realistic, representative data
- Document data requirements and constraints
- Separate test data from test logic
- Use placeholders for sensitive data

**DON'T:**
- Hardcode production data
- Use random or unpredictable data
- Mix test data with test steps

### 4. Selectors and Locators

**DO:**
- Prefer `data-testid` attributes for element selection
- Document all selectors used in the test
- Use semantic, descriptive test IDs
- Follow consistent naming conventions

**DON'T:**
- Rely on CSS classes or XPath for primary selectors
- Use brittle selectors (nth-child, complex CSS)
- Skip documenting selectors

**Selector Priority:**
1. `data-testid` (Preferred)
2. `role` with accessible name
3. `label` text
4. `placeholder` text
5. CSS/XPath (Last resort)

---

## Playwright Test Code Best Practices

### 1. Test Structure

**DO:**
```typescript
test.describe('Feature Name', () => {
  test.beforeEach(async ({ page }) => {
    // Setup common to all tests
  });

  test('should do specific behavior', async ({ page }) => {
    // Arrange
    // Act
    // Assert
  });
});
```

**DON'T:**
```typescript
// Avoid single large test file
test('test everything', async ({ page }) => {
  // Too many actions and assertions
});
```

### 2. Page Object Model (POM)

**DO:**
- Create page objects for reusable components
- Encapsulate selectors in page objects
- Use methods for common actions
- Keep page objects focused and cohesive

**Example:**
```typescript
class LoginPage {
  constructor(private page: Page) {}

  async goto() {
    await this.page.goto('/login');
  }

  async login(email: string, password: string) {
    await this.page.getByTestId('email').fill(email);
    await this.page.getByTestId('password').fill(password);
    await this.page.getByTestId('login-btn').click();
  }

  async getErrorMessage() {
    return await this.page.getByTestId('login-error').textContent();
  }
}
```

**DON'T:**
- Duplicate selectors across test files
- Put business logic in page objects
- Create god objects with too many responsibilities

### 3. Assertions

**DO:**
- Use specific, meaningful assertions
- Assert on visible behavior, not implementation
- Use Playwright's auto-waiting assertions
- Group related assertions logically

**Example:**
```typescript
await expect(page.getByTestId('logout-btn')).toBeVisible();
await expect(page.getByTestId('login-btn')).not.toBeVisible();
await expect(page).toHaveURL('/');
```

**DON'T:**
```typescript
// Avoid generic assertions
expect(true).toBe(true);

// Avoid manual waits
await page.waitForTimeout(5000);
```

### 4. Waits and Synchronization

**DO:**
- Rely on Playwright's auto-waiting
- Use `waitForLoadState` for page loads
- Use `waitForSelector` with specific states when needed

**Example:**
```typescript
await page.waitForLoadState('networkidle');
await page.getByTestId('results').waitFor({ state: 'visible' });
```

**DON'T:**
- Use arbitrary `waitForTimeout`
- Poll for conditions manually
- Skip waiting for critical elements

### 5. Test Data and Configuration

**DO:**
- Store test data in separate files or fixtures
- Use environment variables for configuration
- Create test data factories for complex objects

**Example:**
```typescript
// test-data.ts
export const testUsers = {
  validUser: { email: 'guest', password: 'guest' },
  invalidUser: { email: 'wrong', password: 'wrong' }
};

// test file
import { testUsers } from './test-data';
await loginPage.login(testUsers.validUser.email, testUsers.validUser.password);
```

**DON'T:**
- Hardcode test data in test files
- Use production credentials
- Share mutable test data between tests

### 6. Error Handling and Debugging

**DO:**
- Add descriptive test names and comments
- Use `test.step` for complex test flows
- Capture screenshots on failure
- Log meaningful debug information

**Example:**
```typescript
test('should book appointment successfully', async ({ page }) => {
  await test.step('Navigate to search page', async () => {
    await page.goto('/search');
  });

  await test.step('Search for provider', async () => {
    await page.getByTestId('search-input').fill('Cardiology');
    await page.getByTestId('search-btn').click();
  });

  await test.step('Book appointment', async () => {
    await page.getByRole('button', { name: 'Book Appointment' }).first().click();
  });
});
```

**DON'T:**
- Write tests without clear steps
- Ignore test failures
- Skip debugging aids

### 7. Test Independence and Cleanup

**DO:**
- Ensure tests can run in any order
- Clean up test data after each test
- Use `beforeEach` and `afterEach` hooks
- Isolate test state

**Example:**
```typescript
test.beforeEach(async ({ page }) => {
  await page.goto('/');
  // Reset application state
});

test.afterEach(async ({ page }) => {
  // Cleanup if needed
});
```

**DON'T:**
- Create test dependencies
- Leave test data behind
- Share state between tests

### 8. Performance and Reliability

**DO:**
- Run tests in parallel when possible
- Use `test.describe.configure({ mode: 'parallel' })`
- Minimize test execution time
- Retry flaky tests strategically

**Example:**
```typescript
test.describe.configure({ mode: 'parallel' });

test.describe('Provider Search', () => {
  test('scenario 1', async ({ page }) => { });
  test('scenario 2', async ({ page }) => { });
});
```

**DON'T:**
- Run all tests serially
- Create unnecessarily slow tests
- Ignore flaky tests

---

## Naming Conventions

### Test Files
- Format: `<feature-name>.spec.ts`
- Example: `login.spec.ts`, `provider-search.spec.ts`

### Test Descriptions
- Format: `should <expected behavior> when <condition>`
- Example: `should display error message when login fails`

### Test IDs (data-testid)
- Format: `<element-purpose>` or `<component>-<element>`
- Example: `login-btn`, `search-input`, `result-item`
- Use kebab-case
- Be descriptive and semantic

### Page Objects
- Format: `<PageName>Page`
- Example: `LoginPage`, `SearchPage`, `BookingPage`

### Variables and Constants
- Use camelCase for variables
- Use UPPER_CASE for constants
- Use descriptive names

---

## Test Spec to Playwright Conversion Guidelines

### Mapping Gherkin to Playwright

**Given** → Setup/Navigation
```typescript
// Given the user is on the login page
await page.goto('/login');
```

**When** → Actions
```typescript
// When the user enters email "guest"
await page.getByTestId('email').fill('guest');

// And the user clicks the login button
await page.getByTestId('login-btn').click();
```

**Then** → Assertions
```typescript
// Then the user is redirected to home page
await expect(page).toHaveURL('/');

// And the logout button is visible
await expect(page.getByTestId('logout-btn')).toBeVisible();
```

### Example Conversion

**Acceptance Criteria:**
```gherkin
Scenario: Successful login with valid credentials
  Given the user is on the login page
  When the user enters valid email "guest"
  And the user enters valid password "guest"
  And the user clicks the login button
  Then the user is redirected to the home page
  And the logout button is visible
```

**Playwright Test:**
```typescript
test('should login successfully with valid credentials', async ({ page }) => {
  // Given the user is on the login page
  await page.goto('/login');

  // When the user enters valid credentials
  await page.getByTestId('email').fill('guest');
  await page.getByTestId('password').fill('guest');
  
  // And clicks the login button
  await page.getByTestId('login-btn').click();

  // Then the user is redirected to home page
  await expect(page).toHaveURL('/');
  
  // And the logout button is visible
  await expect(page.getByTestId('logout-btn')).toBeVisible();
});
```

---

## Code Quality Standards

### 1. Readability
- Write self-documenting code
- Use meaningful variable names
- Add comments for complex logic only
- Keep functions small and focused

### 2. Maintainability
- Follow DRY (Don't Repeat Yourself)
- Extract reusable functions
- Use consistent formatting
- Update tests when requirements change

### 3. Reliability
- Handle edge cases
- Add proper error handling
- Use stable selectors
- Avoid race conditions

### 4. Coverage
- Test happy paths
- Test error scenarios
- Test edge cases
- Test accessibility

---

## Common Anti-Patterns to Avoid

❌ **Brittle Selectors**
```typescript
await page.locator('div > div > button:nth-child(3)').click();
```

✅ **Stable Selectors**
```typescript
await page.getByTestId('submit-btn').click();
```

❌ **Hard-coded Waits**
```typescript
await page.waitForTimeout(5000);
```

✅ **Smart Waits**
```typescript
await page.getByTestId('results').waitFor({ state: 'visible' });
```

❌ **Test Interdependence**
```typescript
test('test 1', () => { /* creates data */ });
test('test 2', () => { /* depends on test 1 data */ });
```

✅ **Independent Tests**
```typescript
test('test 1', () => { /* setup, test, cleanup */ });
test('test 2', () => { /* setup, test, cleanup */ });
```

---

## Checklist for Test Generation

Before generating Playwright tests, ensure:

- [ ] Test spec follows the template structure
- [ ] All selectors are documented with `data-testid`
- [ ] Test data is defined and realistic
- [ ] Scenarios are independent
- [ ] Given-When-Then structure is clear
- [ ] Expected results are specific and measurable
- [ ] Page objects are identified for reusable components
- [ ] Test environment details are specified

---

## References

- [Playwright Best Practices](https://playwright.dev/docs/best-practices)
- [Playwright Locators](https://playwright.dev/docs/locators)
- [Playwright Assertions](https://playwright.dev/docs/test-assertions)
- [Page Object Model](https://playwright.dev/docs/pom)
