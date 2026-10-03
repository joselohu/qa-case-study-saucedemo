# QA Case Study: Sauce Demo Checkout

A short, complete example of how I test a feature: a plan, exploratory sessions, bug reports with evidence, and a summary a product owner can act on.

The application is [Sauce Demo](https://www.saucedemo.com), a public practice store. The work was done on 2026-10-03 in Chromium at 1280 x 800.

| Document | Purpose |
|---|---|
| [Test plan](test-plan.md) | Scope, risks, approach and exit criteria |
| [Session notes](session-notes.md) | Three exploratory sessions with charters and findings |
| [Bug reports](bug-reports.md) | Nine defects with steps, expected and actual results, and screenshots |
| [Summary](summary.md) | One page result and release recommendation |

## Result in brief

- 9 defects reported: 2 critical, 4 major, 3 minor.
- 3 of them affect the normal `standard_user` account, including an order that can be placed with an empty cart.
- 6 affect the `problem_user` and `error_user` accounts. Sauce Demo seeds defects in those accounts on purpose for practice. They are reported here the way they would be on a real project.
- Recommendation: do not release checkout until BUG-01 and BUG-08 are fixed.

## Related work

The regression suite that automates the passing paths of this feature is in the `playwright-e2e-framework` repository.
