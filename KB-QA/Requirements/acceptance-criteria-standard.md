# Acceptance Criteria Standard - Gherkin Format

## Overview
This document defines the standard format for writing acceptance criteria in Trello/Jira stories using Gherkin syntax. This ensures consistency, clarity, and enables automated test generation.

## Story Structure

### Story ID
- Unique identifier from Trello/Jira (e.g., PROV-123, Card-456)

### Story Title
- Clear, concise description of the feature
- Format: `<Action> <Feature> <Context>`
- Example: "User Login with Email and Password"

### User Story
Follow the standard format:
```
As a <user type>
I want <goal>
So that <benefit>
```

**Example:**
```
As a patient
I want to search for healthcare providers by specialty
So that I can find the right doctor for my needs
```

## Acceptance Criteria - Gherkin Format

### Structure
Each story should have one or more scenarios written in Gherkin format:

```gherkin
Scenario: <Descriptive scenario name>
  Given <precondition/initial state>
  And <additional precondition> (optional)
  When <user action/trigger>
  And <additional action> (optional)
  Then <expected outcome>
  And <additional outcome> (optional)
```

### Keywords
- **Given**: Sets up the initial context/state
- **When**: Describes the action/event
- **Then**: Specifies the expected outcome
- **And**: Continues the previous keyword
- **But**: Negative continuation (use sparingly)

### Best Practices

1. **Be Specific and Measurable**
   - ❌ Bad: "User can login"
   - ✅ Good: "User successfully logs in with valid credentials and is redirected to home page"

2. **Use Present Tense**
   - ❌ Bad: "User will be redirected"
   - ✅ Good: "User is redirected"

3. **One Scenario Per Behavior**
   - Each scenario should test one specific behavior or path

4. **Include Both Happy and Unhappy Paths**
   - Success scenarios
   - Error scenarios
   - Edge cases

5. **Use Test Data Placeholders**
   - Use `<email>`, `<password>` for sensitive data
   - Specify actual test data in Notes section

## Example Stories

### Example 1: User Login

```
Story ID: PROV-101

Story Title: User Login with Email and Password

User Story:
As a patient
I want to log in to the provider search application
So that I can book appointments with healthcare providers

Acceptance Criteria (Gherkin):

Scenario: Successful login with valid credentials
  Given the user is on the login page
  When the user enters valid email "guest"
  And the user enters valid password "guest"
  And the user clicks the login button
  Then the user is redirected to the home page
  And the logout button is visible in the navigation menu
  And the login button is not visible in the navigation menu

Scenario: Failed login with invalid credentials
  Given the user is on the login page
  When the user enters invalid email "wrong@email.com"
  And the user enters invalid password "wrongpass"
  And the user clicks the login button
  Then an error message "Invalid credentials. Use guest / guest" is displayed
  And the user remains on the login page

Scenario: Login required for booking appointment
  Given the user is not logged in
  And the user is on the search results page
  When the user clicks "Book Appointment" button for any provider
  Then the user is redirected to the login page
  And after successful login, the user is redirected to the booking page

Notes:
- Test credentials: email="guest", password="guest"
- Application URL: http://localhost:3000
```

### Example 2: Provider Search

```
Story ID: PROV-102

Story Title: Search Healthcare Providers by Name or Specialty

User Story:
As a patient
I want to search for healthcare providers by name or specialty
So that I can find doctors that match my healthcare needs

Acceptance Criteria (Gherkin):

Scenario: Search providers by specialty with results
  Given the user is on the search page
  When the user enters "Cardiology" in the search input
  And the user clicks the search button
  Then the search results display all providers with "Cardiology" specialty
  And each result card shows provider name, location, specialties, and available appointments
  And each result card has a "Book Appointment" button

Scenario: Search providers by name with results
  Given the user is on the search page
  When the user enters "Dr. Sarah Johnson" in the search input
  And the user clicks the search button
  Then the search results display the provider "Dr. Sarah Johnson"
  And the provider card shows complete details

Scenario: Search with no matching results
  Given the user is on the search page
  When the user enters "Nonexistent Specialty" in the search input
  And the user clicks the search button
  Then a message "No providers found matching your search." is displayed
  And no provider cards are shown

Scenario: Search without login
  Given the user is not logged in
  And the user is on the search page
  When the user performs a search
  Then the search results are displayed
  And the user can view provider details
  But the user cannot book appointments without logging in

Notes:
- Search is case-insensitive
- Search matches partial text in name and specialties
- Minimum 10 providers in test data
```

### Example 3: Book Appointment

```
Story ID: PROV-103

Story Title: Book Appointment with Healthcare Provider

User Story:
As a logged-in patient
I want to book an appointment with a healthcare provider
So that I can schedule a visit at a convenient time

Acceptance Criteria (Gherkin):

Scenario: Successfully book appointment with selected time slot
  Given the user is logged in
  And the user is on the booking page for "Dr. Sarah Johnson"
  When the user selects a time slot "2024-02-15 10:00 AM" from the dropdown
  And the user clicks the "Confirm Booking" button
  Then a confirmation message is displayed
  And the message shows "Your appointment with Dr. Sarah Johnson is confirmed at 2024-02-15 10:00 AM"
  And a "Go to Home" button is visible

Scenario: Booking page displays provider details
  Given the user is logged in
  And the user navigates to the booking page for a provider
  Then the provider's name is displayed
  And the provider's location is displayed
  And the provider's specialties are displayed
  And a dropdown with available time slots is displayed

Scenario: Confirm button disabled without time slot selection
  Given the user is logged in
  And the user is on the booking page
  When no time slot is selected
  Then the "Confirm Booking" button is disabled

Scenario: Redirect to login when booking without authentication
  Given the user is not logged in
  And the user clicks "Book Appointment" for any provider
  Then the user is redirected to the login page
  And after successful login, the user is redirected to the booking page for that provider

Notes:
- Provider ID is passed via URL parameter
- Available time slots are fetched from provider data
- Booking confirmation is simulated (no backend persistence)
```

## Notes Section Guidelines

Include in the Notes section:
- **Test Data**: Specific values for testing (credentials, IDs, etc.)
- **Environment**: URLs, ports, configuration
- **Assumptions**: Any dependencies or prerequisites
- **Edge Cases**: Boundary conditions to test
- **Non-Functional Requirements**: Performance, accessibility, etc.

## Validation Checklist

Before finalizing acceptance criteria, ensure:
- [ ] Each scenario has a clear, descriptive name
- [ ] Given-When-Then structure is followed
- [ ] Scenarios are independent and can run in any order
- [ ] Both positive and negative test cases are included
- [ ] Expected outcomes are specific and measurable
- [ ] Test data is specified in Notes section
- [ ] Scenarios are testable and automatable

## Integration with Test Generation

This format enables:
1. **Automated Test Spec Generation**: Parse Gherkin scenarios into structured test specifications
2. **Playwright Test Generation**: Convert test specs into executable Playwright tests
3. **Traceability**: Link tests back to requirements via Story ID
4. **Documentation**: Human-readable format serves as living documentation
