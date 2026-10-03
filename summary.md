# Test Summary: Sauce Demo purchase flow

Tested 2026-10-03. Chromium for exploratory sessions; Chromium, Firefox, WebKit and a mobile viewport for the automated suite.

## Recommendation

**Do not release checkout as it stands.** Two defects let the flow end in the wrong place: an order can be confirmed with nothing in it (BUG-01), and one class of account cannot complete an order at all (BUG-08).

## What was done

| Activity | Amount |
|---|---|
| Exploratory sessions | 3, one charter each |
| Accounts exercised | 5 |
| Automated regression tests | 22 per browser, on 3 desktop browsers, plus 3 on mobile |
| Defects reported | 9 |

## Defects by severity

| Severity | Count | IDs |
|---|---|---|
| Critical | 2 | BUG-01, BUG-08 |
| Major | 4 | BUG-04, BUG-06, BUG-07, BUG-09 |
| Minor | 3 | BUG-02, BUG-03, BUG-05 |

Three defects affect the normal account (BUG-01, BUG-02, BUG-03). The other six occur in accounts where Sauce Demo seeds defects for practice.

## What works

- Login, logout and access control, including direct URLs without a session.
- Product list content and all four sort orders for the normal account.
- Cart add, remove and persistence across reloads.
- Order totals: subtotal, 8 percent tax and total add up.
- Required field messages for empty customer fields.

## Performance observation

Login for `performance_glitch_user` took 5.1 seconds, against 0.1 seconds for `standard_user`. Returning to the product list took 5.2 seconds. The result is correct, only slow. These are single measurements from one machine, not a load test.

## Not tested

- Exploratory testing on Firefox, WebKit and mobile.
- The `visual_user` account.
- Accessibility.
- Behaviour under load.

## Suggested next steps

1. Fix BUG-01 by disabling Checkout on an empty cart, and add an automated test for it.
2. Decide what a valid name and postal code are, then fix BUG-02 against that rule.
3. Run an accessibility pass on login and checkout.
