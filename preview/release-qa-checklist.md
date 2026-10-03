# Release QA Checklist

A compact pre-release pass for a small web product or API. Mark each item `Pass`, `Fail`, or `N/A`, and attach one piece of evidence to every failure.

## 1. Build and access

- [ ] The tested build, environment, and commit or version are recorded.
- [ ] The supported browsers, devices, and API base URL are recorded.
- [ ] Test accounts and sample data are available and contain no production secrets.
- [ ] The main entry point loads over the expected protocol and redirects correctly.

## 2. Critical user flow

- [ ] A new user can reach the first useful screen.
- [ ] Sign-up, login, logout, and password reset behave as specified.
- [ ] Required fields reject empty or malformed values with useful messages.
- [ ] A successful action shows a clear confirmation and produces the expected state change.
- [ ] Refresh, back, duplicate submission, and an expired session do not corrupt state.

## 3. API smoke checks

- [ ] Authentication and authorization are checked with a valid and invalid credential.
- [ ] Required parameters, invalid types, and missing resources return documented errors.
- [ ] Response status, headers, schema, and representative values match the documentation.
- [ ] Pagination, filtering, and empty results behave consistently where supported.
- [ ] Sensitive values are absent from logs, error messages, and example responses.

## 4. Evidence and release decision

For every failure, record:

1. URL or endpoint and request context.
2. Exact reproduction steps.
3. Expected and actual behavior.
4. Severity and user impact.
5. Browser/device or API client details.
6. Screenshot, response body, or short recording.

Before release, confirm that critical failures have an owner, a retest result, and an explicit decision. If a failure cannot be reproduced, record the environment and the next diagnostic step instead of silently closing it.

## Need a focused pass?

Send the build or endpoint notes, the critical flow, and the deadline to [yizxty@gmail.com](mailto:yizxty@gmail.com?subject=Release%20QA%20checklist%20request). Fixed-scope checks start at $25; a release QA package is available for larger launches.
