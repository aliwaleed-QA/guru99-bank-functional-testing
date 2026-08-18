# Guru99 Bank — Functional Test Suite & Defect Report

A manual functional testing portfolio project verifying core banking workflows, input validation rules, session security, and server stability on the Guru99 Bank demo platform.

---

## 📌 Project Overview
* **Application Under Test:** Guru99 Banking Demo Web Application
* **Testing Scope:** Functional Validation, Input Validation, Session Management, Exploratory Testing
* **Test Design Techniques Applied:** Equivalence Partitioning (EP), Boundary Value Analysis (BVA), State Transition Testing, Decision Table Testing, Error Guessing
* **Specification Document:** [`SRS_guru 99.pdf`](SRS_guru%2099.pdf)

---

## 🔍 Modules in Scope & Coverage Strategy
Rather than writing redundant tests across all 12 modules, this suite applies **Risk-Based Testing** across high-impact business flows:
1. **Authentication & Authorization:** Login credential validations (positive/negative), session logout, and cache navigation security.
2. **Customer Management:** Form validation rules for New Customer creation, field boundary constraints (PIN length, character restrictions), and duplicate record handling.
3. **Transaction Flow:** Cash deposits, fund transfer balance checks, and source/target account integrity validation.
4. **Account Management (Exploratory):** Testing account modification endpoints and server response stability.

---

## 📊 Test Execution Summary

| Total Test Cases | Passed | Failed | Blocked | Pass Rate |
| :---: | :---: | :---: | :---: | :---: |
| **17** | **16** | **1** | **0** | **94.1%** |

*Note: In addition to the 17 scripted test cases, exploratory testing uncovered 1 severe backend crash defect.*

---

## 🐛 Defect Reports

### `BUG_AUTH_01`: Manager Dashboard Renders via Browser Back Navigation After Logout
* **Severity:** High (Security / Session State Leak)
* **Priority:** P1
* **Module:** Authentication & Authorization
* **Test Case ID:** `TC_AUTH_06`
* **SRS Requirement:** `F29 / Security`

#### Steps to Reproduce:
1. Navigate to the Guru99 Bank login page.
2. Log in with valid Manager credentials (`mngr665252`).
3. Click **Log out** from the left navigation menu.
4. Accept the browser logout confirmation alert.
5. On the redirected login page, click the browser's **Back** button.

#### Expected Result:
The system terminates the session, clears cached responses, and restricts access by redirecting the user to the login screen.

#### Actual Result:
`Managerhomepage.php` is displayed from browser history cache, exposing internal navigation menus with a blank `Manger Id :` parameter.

#### Evidence:
<details>
  <summary>📸 <b>Click to expand defect screenshot</b></summary>
  <br>
  <img src="Screenshot 2026-08-17 232825.png" alt="Back Navigation Defect" width="800">
</details>

---

### `BUG_ACC_01`: HTTP 500 Internal Server Error on Edit Account Submission
* **Severity:** High (Backend Crash / Functional Blocker)
* **Priority:** P1
* **Module:** Account Management
* **Test Case ID:** `N/A (Exploratory Testing)`
* **Endpoint:** `demo.guru99.com/V4/manager/editAccountPage.php`

#### Steps to Reproduce:
1. Log in with valid Manager credentials.
2. Click **Edit Account** from the left navigation menu.
3. Enter an existing valid Account ID (`185325`).
4. Click **Submit**.
5. When prompted by the browser (`about:neterror`), click **Resend** to retry the POST request.

#### Expected Result:
The system loads `editAccount.php` populated with current account details ready for modification.

#### Actual Result:
The backend script crashes and returns an unhandled `500 Internal Server Error` on initial submit and subsequent resubmissions.

#### Evidence:
<details>
  <summary>📸 <b>Click to expand defect screenshots</b></summary>
  <br>

  **1. Server 500 Error Screen:**
  <br>
  <img src="Screenshot 2026-08-19 000616.png" alt="Edit Account 500 Error" width="750">
  <br><br>
  
  **2. Browser Resend POST Confirmation:**
  <br>
  <img src="Screenshot 2026-08-19 000713.png" alt="Firefox Resend Prompt" width="750">
</details>
### `BUG_TX_02`: Blank Confirmation Page on Fund Transfer Submission
* **Severity:** High (Functional Defect / Missing Transaction Summary)
* **Priority:** P1
* **Module:** Transaction Management (Fund Transfer)
* **Test Case ID:** `TC_TX_03`
* **Requirement ID:** `F5`

#### Steps to Reproduce:
1. Log in with valid Manager credentials.
2. Click **Fund Transfer** from the left navigation menu.
3. Enter valid Payer Account (`185325`), Payee Account, Amount (`1000`), and Description (`Rent`).
4. Click **Submit**.

#### Expected Result:
The system renders a complete transaction receipt table displaying `From Account Number`, `To Account Number`, `Amount`, `Description`, and a generated `Transaction ID`.

#### Actual Result:
The page navigates to the `Fund Transfer Details` header, but the entire transaction receipt table fails to render, leaving the page body blank.

#### Evidence:
<details>
  <summary>📸 <b>Click to expand defect screenshot</b></summary>
  <br>
  <img src="Screenshot 2026-08-19 004529.png" alt="Blank Fund Transfer Details" width="800">
</details>
---

## 📁 Repository Artifacts
* `SRS_guru 99.pdf` — Original software requirements specification.
* `Screenshot 2026-08-17 232825.png` — Defect evidence for `BUG_AUTH_01`.
* `Screenshot 2026-08-19 000616.png` & `Screenshot 2026-08-19 000713.png` — Defect evidence for `BUG_ACC_01`.
* `Screenshot 2026-08-19 003433.png` — Defect evidence for `BUG_TX_01`.
* `Screenshot 2026-08-19 004529.png` — Defect evidence for `BUG_TX_02`.
