# Bug Reports

Environment for all reports: https://www.saucedemo.com, Chromium, 1280 x 800, tested 2026-10-03. All accounts use the password shown on the site's login page.

| ID | Title | Severity | Account |
|---|---|---|---|
| [BUG-01](#bug-01) | An order can be placed with an empty cart | Critical | standard_user |
| [BUG-02](#bug-02) | Checkout accepts any text as name and postal code | Minor | standard_user |
| [BUG-03](#bug-03) | Reset App State leaves products showing Remove | Minor | standard_user |
| [BUG-04](#bug-04) | Every product shows the same image | Major | problem_user |
| [BUG-05](#bug-05) | Sorting the product list has no effect | Minor | problem_user |
| [BUG-06](#bug-06) | Three products cannot be added, and none can be removed from the list | Major | problem_user |
| [BUG-07](#bug-07) | Product names open the wrong product | Major | problem_user |
| [BUG-08](#bug-08) | Last name cannot be entered, checkout is blocked | Critical | problem_user |
| [BUG-09](#bug-09) | Finish button does nothing, order cannot be completed | Major | error_user |

BUG-04 to BUG-09 occur in accounts where Sauce Demo seeds defects for practice.

---

## BUG-01

**An order can be placed with an empty cart**

Severity: Critical. Account: `standard_user`.

**Steps**

1. Log in and make sure the cart is empty.
2. Click the cart icon.
3. Click Checkout.
4. Enter a first name, last name and postal code, then click Continue.
5. Click Finish.

**Expected:** Checkout is not available while the cart is empty, or the user is told to add a product first.

**Actual:** The overview shows no items and "Total: $0.00". Finish completes the order and shows "Thank you for your order!".

**Impact:** The store confirms orders that contain nothing. On a real store this creates empty order records and confirmation messages.

**Evidence:** [overview](evidence/bug-01-empty-cart-overview.png), [confirmation](evidence/bug-01-empty-cart-order-complete.png)

---

## BUG-02

**Checkout accepts any text as name and postal code**

Severity: Minor. Account: `standard_user`.

**Steps**

1. Add a product to the cart and start checkout.
2. Enter `12345` as first name, `!!!` as last name and `not a postal code` as postal code.
3. Click Continue.
4. Repeat with a single space in each field.

**Expected:** Values that cannot be a name or a postal code are refused with a message, and whitespace only values are treated as empty.

**Actual:** Both attempts continue to the order overview without any message.

**Impact:** Orders can be placed with customer details that cannot be used for delivery. Empty fields are validated correctly, so the rule exists but only checks for presence.

**Evidence:** [form](evidence/bug-02-invalid-customer-data-form.png), [accepted](evidence/bug-02-invalid-customer-data-accepted.png)

---

## BUG-03

**Reset App State leaves products showing Remove**

Severity: Minor. Account: `standard_user`.

**Steps**

1. On the product list, click Add to cart on Sauce Labs Backpack. The button changes to Remove and the cart badge shows 1.
2. Open the side menu and click Reset App State.

**Expected:** The cart is emptied and the button returns to Add to cart.

**Actual:** The cart badge disappears, but the button still says Remove. It returns to Add to cart only after a page reload.

**Impact:** The page shows two contradicting states. A user who wants the product again has to click Remove first or reload.

**Evidence:** [screenshot](evidence/bug-03-reset-app-state-button-still-remove.png)

---

## BUG-04

**Every product shows the same image**

Severity: Major. Account: `problem_user`.

**Steps**

1. Log in as `problem_user`.
2. Look at the product list.

**Expected:** Each of the six products shows its own photo, as it does for `standard_user`.

**Actual:** All six products show the same image of a dog. All six `img` elements point to one file, `sl-404`.

**Impact:** Customers cannot see what they are buying.

**Evidence:** [screenshot](evidence/bug-04-problem-user-same-image.png)

---

## BUG-05

**Sorting the product list has no effect**

Severity: Minor. Account: `problem_user`.

**Steps**

1. Log in as `problem_user`.
2. Choose "Name (Z to A)" in the sort menu.

**Expected:** The list is reordered and the menu shows the chosen option.

**Actual:** The order does not change, Sauce Labs Backpack stays first, and the menu still shows "Name (A to Z)".

**Impact:** Customers cannot sort by name or price.

**Evidence:** [screenshot](evidence/bug-05-problem-user-sort-no-effect.png)

---

## BUG-06

**Three products cannot be added, and none can be removed from the list**

Severity: Major. Account: `problem_user`.

**Steps**

1. Log in as `problem_user`.
2. Click Add to cart on each of the six products.
3. Click Remove on Sauce Labs Backpack.

**Expected:** The badge shows 6 after step 2 and 5 after step 3.

**Actual:** After step 2 the badge shows 3. Sauce Labs Bolt T-Shirt, Sauce Labs Fleece Jacket and Test.allTheThings() T-Shirt (Red) keep showing Add to cart. After step 3 the Backpack still shows Remove and the badge still shows 3.

**Impact:** Half of the catalogue cannot be bought, and products can only be removed from the cart page.

**Evidence:** [screenshot](evidence/bug-06-problem-user-add-to-cart.png)

---

## BUG-07

**Product names open the wrong product**

Severity: Major. Account: `problem_user`.

**Steps**

1. Log in as `problem_user`.
2. Click each product name in the list and note the product that opens.

**Expected:** The detail page shows the product that was clicked.

**Actual:** Every one of the six opens a different product, and one opens an error page.

| Clicked | Opened |
|---|---|
| Sauce Labs Backpack | Sauce Labs Fleece Jacket |
| Sauce Labs Bike Light | Sauce Labs Bolt T-Shirt |
| Sauce Labs Bolt T-Shirt | Sauce Labs Onesie |
| Sauce Labs Fleece Jacket | ITEM NOT FOUND |
| Sauce Labs Onesie | Test.allTheThings() T-Shirt (Red) |
| Test.allTheThings() T-Shirt (Red) | Sauce Labs Backpack |

**Impact:** A customer who adds a product from its detail page buys something other than what they chose.

**Evidence:** [screenshot](evidence/bug-07-problem-user-wrong-detail.png)

---

## BUG-08

**Last name cannot be entered, checkout is blocked**

Severity: Critical. Account: `problem_user`.

**Steps**

1. Log in as `problem_user`, add a product and start checkout.
2. Type `Ada` in First Name.
3. Type `Lovelace` in Last Name.
4. Type `01010` in Zip/Postal Code and click Continue.

**Expected:** The last name appears in its field and checkout continues to the overview.

**Actual:** The Last Name field stays empty. Each key typed there replaces the content of First Name, which ends up showing `e`. Continue shows "Error: Last Name is required".

**Impact:** The account cannot complete any purchase. There is no workaround in the interface.

**Evidence:** [screenshot](evidence/bug-08-problem-user-last-name.png)

---

## BUG-09

**Finish button does nothing, order cannot be completed**

Severity: Major. Account: `error_user`.

**Steps**

1. Log in as `error_user`, add Sauce Labs Backpack and start checkout.
2. Enter a first name and a postal code. The Last Name field does not accept input.
3. Click Continue.
4. On the overview, click Finish.

**Expected:** Step 3 is refused because the last name is empty. If the overview is reached, Finish completes the order.

**Actual:** Continue goes to the overview with an empty last name. Finish has no visible effect and the page stays on the overview. The browser console logs `Cannot read properties of undefined (reading 'value')`.

Also seen with this account: choosing a sort order shows a browser alert, "Sorting is broken! This error has been reported to Backtrace."

**Impact:** The order cannot be placed and the user gets no message explaining why.

**Evidence:** [screenshot](evidence/bug-09-error-user-finish-does-nothing.png)
