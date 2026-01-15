# Prompt: Generate Playwright Test Code from Test Specification

## Context

You are an AI agent tasked with generating executable Playwright test code from a test specification. The generated code should follow best practices, be maintainable, and accurately implement the test scenarios.

## Prerequisites

Before starting, ensure you have read and understood:
1. `BestPractices/test-automation-best-practices.md` - Playwright coding standards
2. The test specification file you're converting
3. Application structure at `../../UI/provider-search-app/`

## Input

**Test Specification File**: `TestSpecs/<STORY_ID>-<feature-name>.spec.md`

The test spec contains:
- Test scenarios with Given-When-Then structure
- Detailed test steps
- Selectors/locators for UI elements
- Test data
- Expected results

## Task

Generate Playwright test code that:
1. Implements all test scenarios from the spec
2. Uses documented selectors from the spec
3. Follows Page Object Model (POM) pattern
4. Includes proper waits and assertions
5. Is maintainable and follows best practices

## Output Structure

Generate two types of files:

### 1. Page Object(s)
**Location**: `tests/pages/<PageName>Page.ts`

```typescript
import { Page, Locator } from '@playwright/test';

export class <PageName>Page {
  readonly page: Page;
  readonly <element>: Locator;

  constructor(page: Page) {
    this.page = page;
    this.<element> = page.getByTestId('<testid>');
  }

  async goto() {
    await this.page.goto('/<path>');
  }

  async <action>() {
    // Implementation
  }

  async <verification>() {
    // Return value for assertion
  }
}
```

### 2. Test File
**Location**: `tests/<feature-name>.spec.ts`

```typescript
import { test, expect } from '@playwright/test';
import { <PageName>Page } from './pages/<PageName>Page';

test.describe('<Feature Name>', () => {
  test.beforeEach(async ({ page }) => {
    // Common setup
  });

  test('should <behavior> when <condition>', async ({ page }) => {
    // Arrange
    // Act
    // Assert
  });
});
```

## Guidelines

### 1. Page Object Model (POM)

Create page objects for each page/component:

**Structure**:
```typescript
export class LoginPage {
  readonly page: Page;
  readonly emailInput: Locator;
  readonly passwordInput: Locator;
  readonly loginButton: Locator;
  readonly errorMessage: Locator;

  constructor(page: Page) {
    this.page = page;
    this.emailInput = page.getByTestId('email');
    this.passwordInput = page.getByTestId('password');
    this.loginButton = page.getByTestId('login-btn');
    this.errorMessage = page.getByTestId('login-error');
  }

  async goto() {
    await this.page.goto('/login');
  }

  async login(email: string, password: string) {
    await this.emailInput.fill(email);
    await this.passwordInput.fill(password);
    await this.loginButton.click();
  }

  async getErrorMessage() {
    return await this.errorMessage.textContent();
  }
}
```

**Best Practices**:
- One page object per page/component
- Encapsulate all selectors
- Provide action methods (login, search, etc.)
- Provide getter methods for assertions
- Keep business logic out of page objects

### 2. Test Structure

**Use Arrange-Act-Assert Pattern**:

```typescript
test('should login successfully with valid credentials', async ({ page }) => {
  // Arrange
  const loginPage = new LoginPage(page);
  await loginPage.goto();

  // Act
  await loginPage.login('guest', 'guest');

  // Assert
  await expect(page).toHaveURL('/');
  await expect(page.getByTestId('logout-btn')).toBeVisible();
});
```

**Use test.describe for grouping**:

```typescript
test.describe('User Login', () => {
  test.describe('Successful Login', () => {
    test('should login with valid credentials', async ({ page }) => {
      // Test implementation
    });
  });

  test.describe('Failed Login', () => {
    test('should show error with invalid credentials', async ({ page }) => {
      // Test implementation
    });
  });
});
```

### 3. Selector Strategy

Use selectors in this priority order:

1. **data-testid** (Preferred)
```typescript
page.getByTestId('login-btn')
```

2. **Role with accessible name**
```typescript
page.getByRole('button', { name: 'Login' })
```

3. **Label text**
```typescript
page.getByLabel('Email')
```

4. **Placeholder**
```typescript
page.getByPlaceholder('Enter email')
```

5. **Text content** (for unique text)
```typescript
page.getByText('Welcome back')
```

### 4. Waits and Synchronization

**Use Playwright's auto-waiting**:
```typescript
// Playwright waits automatically
await page.getByTestId('submit-btn').click();
```

**Explicit waits when needed**:
```typescript
// Wait for navigation
await page.waitForURL('/dashboard');

// Wait for element state
await page.getByTestId('results').waitFor({ state: 'visible' });

// Wait for network
await page.waitForLoadState('networkidle');
```

**Avoid**:
```typescript
// Don't use arbitrary timeouts
await page.waitForTimeout(5000); // ❌
```

### 5. Assertions

**Use specific assertions**:

```typescript
// URL assertions
await expect(page).toHaveURL('/dashboard');
await expect(page).toHaveURL(/.*dashboard/);

// Visibility assertions
await expect(page.getByTestId('logout-btn')).toBeVisible();
await expect(page.getByTestId('login-btn')).not.toBeVisible();

// Text assertions
await expect(page.getByTestId('welcome')).toHaveText('Welcome, Guest');
await expect(page.getByTestId('error')).toContainText('Invalid');

// Count assertions
await expect(page.getByTestId('result-item')).toHaveCount(10);

// Attribute assertions
await expect(page.getByTestId('submit-btn')).toBeEnabled();
await expect(page.getByTestId('submit-btn')).toBeDisabled();
```

### 6. Test Data Management

**Create test data file**:

```typescript
// test-data.ts
export const testUsers = {
  valid: { email: 'guest', password: 'guest' },
  invalid: { email: 'wrong@test.com', password: 'wrong123' }
};

export const testProviders = {
  cardiology: 'Cardiology',
  pediatrics: 'Pediatrics'
};

export const testUrls = {
  base: 'http://localhost:3000',
  login: '/login',
  search: '/search'
};
```

**Use in tests**:
```typescript
import { testUsers, testUrls } from './test-data';

test('should login successfully', async ({ page }) => {
  await page.goto(testUrls.login);
  await loginPage.login(testUsers.valid.email, testUsers.valid.password);
});
```

### 7. Test Independence

**Each test should be independent**:

```typescript
test.beforeEach(async ({ page }) => {
  // Reset state before each test
  await page.goto('/');
  // Clear any stored auth state if needed
});

test.afterEach(async ({ page }) => {
  // Cleanup if needed
});
```

### 8. Error Handling and Debugging

**Use test.step for complex flows**:

```typescript
test('should complete booking flow', async ({ page }) => {
  await test.step('Login', async () => {
    await loginPage.goto();
    await loginPage.login('guest', 'guest');
  });

  await test.step('Search for provider', async () => {
    await searchPage.goto();
    await searchPage.search('Cardiology');
  });

  await test.step('Book appointment', async () => {
    await searchPage.clickBookAppointment();
    await bookingPage.selectTimeSlot('2024-02-15 10:00 AM');
    await bookingPage.confirmBooking();
  });
});
```

## Example Conversion

### Input: Test Specification

```markdown
### Scenario 1: Successful Login with Valid Credentials

**Test Steps**:
1. Navigate to http://localhost:3000/login
2. Enter "guest" in email field (data-testid="email")
3. Enter "guest" in password field (data-testid="password")
4. Click login button (data-testid="login-btn")
5. Verify redirect to home page (/)
6. Verify logout button is visible (data-testid="logout-btn")

**Selectors/Locators**:
- Email input: `data-testid="email"`
- Password input: `data-testid="password"`
- Login button: `data-testid="login-btn"`
- Logout button: `data-testid="logout-btn"`
```

### Output: Playwright Code

**File: tests/pages/LoginPage.ts**
```typescript
import { Page, Locator } from '@playwright/test';

export class LoginPage {
  readonly page: Page;
  readonly emailInput: Locator;
  readonly passwordInput: Locator;
  readonly loginButton: Locator;

  constructor(page: Page) {
    this.page = page;
    this.emailInput = page.getByTestId('email');
    this.passwordInput = page.getByTestId('password');
    this.loginButton = page.getByTestId('login-btn');
  }

  async goto() {
    await this.page.goto('/login');
  }

  async login(email: string, password: string) {
    await this.emailInput.fill(email);
    await this.passwordInput.fill(password);
    await this.loginButton.click();
  }
}
```

**File: tests/user-login.spec.ts**
```typescript
import { test, expect } from '@playwright/test';
import { LoginPage } from './pages/LoginPage';

test.describe('User Login', () => {
  let loginPage: LoginPage;

  test.beforeEach(async ({ page }) => {
    loginPage = new LoginPage(page);
  });

  test('should login successfully with valid credentials', async ({ page }) => {
    // Navigate to login page
    await loginPage.goto();

    // Perform login
    await loginPage.login('guest', 'guest');

    // Verify redirect to home page
    await expect(page).toHaveURL('/');

    // Verify logout button is visible
    await expect(page.getByTestId('logout-btn')).toBeVisible();

    // Verify login button is not visible
    await expect(page.getByRole('link', { name: 'Login' })).not.toBeVisible();
  });
});
```

## Configuration

**File: playwright.config.ts**
```typescript
import { defineConfig, devices } from '@playwright/test';

export default defineConfig({
  testDir: './tests',
  fullyParallel: true,
  forbidOnly: !!process.env.CI,
  retries: process.env.CI ? 2 : 0,
  workers: process.env.CI ? 1 : undefined,
  reporter: 'html',
  use: {
    baseURL: 'http://localhost:3000',
    trace: 'on-first-retry',
    screenshot: 'only-on-failure',
  },
  projects: [
    {
      name: 'chromium',
      use: { ...devices['Desktop Chrome'] },
    },
  ],
  webServer: {
    command: 'npm start',
    url: 'http://localhost:3000',
    reuseExistingServer: !process.env.CI,
  },
});
```

## Validation Checklist

Before finalizing the Playwright code, verify:

- [ ] All scenarios from test spec are implemented
- [ ] Page objects are created for reusable components
- [ ] Selectors match those documented in test spec
- [ ] Tests use Arrange-Act-Assert pattern
- [ ] Proper waits and assertions are used
- [ ] Tests are independent and can run in any order
- [ ] Test data is externalized
- [ ] Code follows naming conventions
- [ ] Error handling is appropriate
- [ ] Tests are readable and maintainable

## Common Mistakes to Avoid

❌ **Hardcoded selectors in tests**
```typescript
await page.locator('#email').fill('guest');
```

✅ **Use page objects**
```typescript
await loginPage.login('guest', 'guest');
```

❌ **Arbitrary waits**
```typescript
await page.waitForTimeout(3000);
```

✅ **Smart waits**
```typescript
await page.waitForURL('/dashboard');
```

❌ **Duplicate code**
```typescript
test('test 1', async ({ page }) => {
  await page.goto('/login');
  await page.getByTestId('email').fill('guest');
  // ...
});

test('test 2', async ({ page }) => {
  await page.goto('/login');
  await page.getByTestId('email').fill('guest');
  // ...
});
```

✅ **Reusable page objects**
```typescript
test('test 1', async ({ page }) => {
  await loginPage.goto();
  await loginPage.login('guest', 'guest');
});

test('test 2', async ({ page }) => {
  await loginPage.goto();
  await loginPage.login('guest', 'guest');
});
```

## Usage Instructions

1. **Read the test specification** file thoroughly
2. **Identify pages/components** that need page objects
3. **Create page objects** with all selectors and actions
4. **Implement test scenarios** using page objects
5. **Add test data** in separate file if needed
6. **Run tests** to verify they work
7. **Refactor** for better maintainability
8. **Document** any complex logic

## Running Tests

```bash
# Install dependencies
npm install

# Run all tests
npx playwright test

# Run specific test file
npx playwright test user-login.spec.ts

# Run in headed mode
npx playwright test --headed

# Run with UI mode
npx playwright test --ui

# Generate report
npx playwright show-report
```

## Next Steps

After generating Playwright tests:
1. Review code against best practices
2. Run tests to ensure they pass
3. Add to CI/CD pipeline
4. Maintain and update as application changes
5. Document any custom patterns or utilities

---

**Remember**: Generated tests should be production-ready, maintainable, and follow industry best practices. They should accurately implement the test specification and be easy for other developers to understand and modify.
