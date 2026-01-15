Developer: "Run @e2e-test-generation.md with jira id PROV-1234"

Agent:

1. ✅ Validates story exists and has proper Gherkin format
2. ✅ Generates test specification
3. ✅ Generates Playwright code (Page Objects + Tests)
4. ✅ Validates against checklist
5. ✅ Provides summary with file locations

Developer: Reviews generated files and can naturally:

- Request changes: "Please refactor the login test to use async/await"
- Ask questions: "Why did you use this selector?"
- Request additions: "Add a test for invalid email format"
