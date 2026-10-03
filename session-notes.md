# Exploratory Session Notes

Each session follows one charter. The checks listed here were driven through Playwright scripts, which also captured the screenshots.

## Session 1: Checkout with unusual carts and customer data

**Charter:** Explore checkout as `standard_user` with empty carts and invalid customer details, to discover whether an order can be placed that should have been refused.

**What was tried**

- Opened the cart with nothing in it and pressed Checkout.
- Completed the customer form and the overview with the empty cart.
- Entered digits as a first name, symbols as a last name and free text as a postal code.
- Entered a single space in each of the three fields.
- Left each field empty in turn.

**Findings**

- The Checkout button is enabled on an empty cart, and the order completes with a total of $0.00. Reported as BUG-01.
- Any non empty text is accepted in all three customer fields, including a single space. Reported as BUG-02.
- Empty fields are refused with a clear message for each one. Working as expected.
- Subtotal, tax (8 percent) and total add up for the carts tried.

## Session 2: Cart state and the side menu

**Charter:** Explore how cart state behaves across reloads, navigation and the Reset App State action, to discover inconsistencies between what the page shows and what the cart holds.

**What was tried**

- Added items and reloaded the page.
- Removed items from the list and from the cart page.
- Used Reset App State with items in the cart, from the product list.
- Opened the product list by URL without a session.

**Findings**

- Cart contents and the badge survive a reload.
- After Reset App State the cart badge disappears, but the product keeps showing a Remove button until the page is reloaded. Reported as BUG-03.
- Opening `/inventory.html` without a session shows a clear error and stays on the login page.

## Session 3: Degraded accounts

**Charter:** Explore the purchase flow as `problem_user`, `performance_glitch_user` and `error_user`, to discover how the store behaves when parts of the front end fail.

**What was tried**

- Walked the purchase flow as `problem_user` and `error_user`.
- Compared the product list with the same list under `standard_user`.
- Opened every product detail page from the list.
- Sorted the list by name, Z to A.
- Measured login time.

**Findings**

- `problem_user`: every product shows the same image (BUG-04), sorting has no effect (BUG-05), three of six products cannot be added to the cart (BUG-06), products cannot be removed from the list (BUG-06), every product name opens a different product (BUG-07), and the last name field cannot be filled in, which blocks checkout (BUG-08).
- `error_user`: sorting raises a browser alert, checkout continues with an empty last name, and the Finish button does nothing (BUG-09).
- `performance_glitch_user`: login took 5.1 seconds against 0.1 seconds for `standard_user`, and returning to the product list took another 5.2 seconds. Functionally correct. Noted in the summary as a performance observation.

## Not covered

- The back button, and logging out and in again with items in the cart.
- Firefox, WebKit and mobile were covered only by the automated suite, not by exploratory sessions.
- `visual_user` was not explored.
- Keyboard only navigation and screen reader behaviour.
