# Test Plan Document

**Project:** AI Smart PR Reviewer   
**QA Owner:** Sanjiwani Suryawanshi
**Team:** 4 Frontend Developers, 1 DevOps, 1 QA  
**Purpose:** Automatically review PRs for accessibility, design, testing, and code hygiene issues 

## 1. Objective

Ensure the AI Smart Reviewer Bot:
- Produces accurate and relevant comments on UI pull requests
- Behaves correctly on edge cases and malformed inputs
- Is reliable, efficient, and safe for real-world use

## 2. Scope

### In Scope:
- UI diffs with JSX/TSX/HTML changes
- Review comments related to:
    - Accessibility (ARIA labels)
    - Design system adherence (tokens, inline styles)
    - Test coverage (missing test files)
    - Code hygiene (unused imports, typos)
- Manual triggering

### Out of Scope:
- CI/CD deployment workflows
- Performance under production-scale load

## 3. Test Strategy

| Area | What Will Be Tested |
|------|---------------------|
| Functional Correctness | Validity of AI comments on real diffs |
| Consistency | Similar input yields similar output |
| Hallucination Avoidance | No made-up issues or warnings |
| Error Handling | Graceful behavior on broken/invalid PRs |
| Performance | Response time and review throughput |
| Edge Case Handling | Empty diffs, conflicts, non-UI changes |

## 4. Test Environment

| Component | Details |
|-----------|---------|
| Runtime | Local (npm run dev) |
| API | Gemini via .env.local key |
| UI | Localhost dashboard (http://localhost:3000) |
| Tools | Chrome, Postman, Excel, Notion |

## 5. Test Scenarios

| TC ID | Scenario | Input | Expected Outcome |
|-------|----------|-------|------------------|
| TC01 | Missing ARIA label | Button w/o label | Warn about accessibility |
| TC02 | Inline styles | h1 with style | Suggest using design tokens |
| TC03 | No test file | Component added | Prompt to add test |
| TC04 | Clean PR | Uses tokens, a11y | No comment expected |
| TC05 | Refactor only | File moved | No comment expected |
| TC06 | Class name typo | btnn_submit | Warn on naming convention |
| TC07 | Conflict markers | <<<<<<< HEAD | Detect and report conflict |
| TC08 | Malicious JSX | `<script>` inside JSX | Warn about XSS risk |
| TC09 | Empty PR | No code changes | Bot says: No issues |
| TC10 | Non-UI PR | README only | No comments expected |

## 6. Test Data
- .diff files created from test branches
- .tsx and .test.tsx components for coverage
- GitHub PRs with known content

## 7. Evaluation Log Format

| PR Type | Bot Comment | Expected? | Result | Notes |
|---------|-------------|-----------|--------|-------|
| PR#1 ARIA | Missing "Missing aria-label" | Yes | Pass | OK |
| PR#2 Refactor | "Optimize render" | No | Fail | False positive |
| PR#3 Script tag | XSS alert | Yes | Pass | Good |

## 8. Performance Targets

| Metric | Target |
|--------|--------|
| Latency | < 10 seconds |
| Accuracy | > 80% valid comments |
| False positives | < 10% of total |
| Stability | No crashes |

## 9. Risk Assessment

| Risk | Mitigation |
|------|------------|
| Hallucinated advice | Manual validation + prompt tuning |
| API failure | Retry logic or fallback comment |
| Inconsistent output | Freeze prompts for stable results |
| Sensitive content | Avoid submitting user data |

## 10. Deliverables
- Completed test case log
- AI response evaluation summary
- QA report of gaps and suggestions
- Screenshots for reference
