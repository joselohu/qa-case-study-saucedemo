# Test Plan: Sauce Demo purchase flow

## 1. Objective

Decide whether a customer can reliably find a product, add it to the cart and complete an order, and report anything that would stop a release.

## 2. Scope

In scope:

- Login and logout
- Product list: content, sorting, product detail
- Cart: add, remove, persistence
- Checkout: customer information, order overview, confirmation
- The side menu actions that change state (Reset App State, Logout)

Out of scope:

- Payment processing and shipping. The site shows fixed placeholder values.
- Account creation and password recovery. The site has neither.
- Load testing.
- Accessibility audit. It deserves its own pass with its own tooling.

## 3. Test accounts

| Account | Used for |
|---|---|
| `standard_user` | Main functional testing |
| `locked_out_user` | Access control |
| `problem_user` | Behaviour under a degraded front end |
| `performance_glitch_user` | Response time |
| `error_user` | Error handling |

## 4. Risks and where the effort goes

| Risk | Impact | Likelihood | Effort |
|---|---|---|---|
| An order can be placed with wrong or missing content | High | Medium | High |
| Totals or tax are calculated wrong | High | Low | High |
| Customer data is accepted without validation | Medium | Medium | Medium |
| Cart state is lost or shown inconsistently | Medium | Medium | Medium |
| A user reaches pages without a session | High | Low | Medium |
| Sorting or product data is wrong | Low | Medium | Low |

## 5. Approach

1. **Exploratory sessions**, time boxed, one charter per session. Notes are kept per session and findings become bug reports.
2. **Scripted checks** for the paths that must never break: login, add to cart, checkout totals, required fields. These are automated in the Playwright suite and run on Chromium, Firefox, WebKit and a mobile viewport.
3. **Boundary and negative testing** on every input: empty, whitespace only, wrong type, very long values.
4. **State testing**: reload, back button, direct URLs, and the Reset App State action at each step.

## 6. Entry and exit criteria

Entry: the site is reachable and all five accounts can be used as described.

Exit:

- Every in scope area has been covered by at least one session.
- No open critical defect on the `standard_user` purchase flow.
- The automated regression suite passes on all configured browsers.

## 7. Severity definitions

| Severity | Meaning |
|---|---|
| Critical | A purchase cannot be completed, or an invalid order can be placed |
| Major | A main function gives a wrong result and the workaround is not obvious |
| Minor | Wrong or confusing behaviour with an easy workaround |

## 8. Deliverables

Session notes, bug reports with screenshots, a one page summary, and the automated suite.
