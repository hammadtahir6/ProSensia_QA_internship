# Test Plan: User Login Screen

## 1. Objective
Validate that the User Login screen functions correctly, ensuring that valid users can log in successfully and that invalid inputs are handled gracefully without compromising security or user experience.

## 2. Scope
- UI validation of the login form (fields, buttons, error messages)
- Functional testing of login logic
- Security testing (e.g., no password exposure)
- Session management after login

## 3. Features to be Tested
- Username/email field
- Password field
- “Login” button
- “Forgot Password?” link
- “Remember Me” checkbox (if available)
- Error messages
- Redirection after successful login

## 4. Features Not Tested
- Backend performance under heavy load
- Third-party SSO login (unless explicitly scoped)
- Password reset flow (covered separately)

## 5. Test Environment
- **Browsers**: Chrome (latest), Firefox (latest), Safari (latest)
- **Devices**: Desktop (Windows/macOS), Mobile (iOS/Android)
- **Network**: Standard Wi-Fi, slow 3G (for timeout/edge)
- **Backend**: Staging environment with test user accounts

## 6. Test Cases

| TC ID | Test Case Description | Type | Steps | Expected Result |
|-------|----------------------|------|-------|------------------|
| TC-01 | **Valid login with correct credentials** | Positive | 1. Enter valid username/email and password.<br>2. Click Login. | User is redirected to the dashboard/home page. No error message shown. |
| TC-02 | **Invalid password** | Negative | 1. Enter valid username, incorrect password.<br>2. Click Login. | Error message: “Invalid username or password.” User remains on login page. |
| TC-03 | **Non-existent username** | Negative | 1. Enter username not registered, any password.<br>2. Click Login. | Error message: “Invalid username or password.” No hint whether username or password is wrong. |
| TC-04 | **Empty fields (both blank)** | Negative | 1. Leave username and password empty.<br>2. Click Login. | Error message: “Please enter username and password.” Fields are highlighted. |
| TC-05 | **Password field masking** | Edge | 1. Type in password field. | Password characters are shown as dots/asterisks. No plain text visible. |
| TC-06 | **Maximum field length** | Edge | 1. Enter 256-character username/email.<br>2. Enter 128-character password.<br>3. Click Login. | System handles gracefully (truncates or shows error if too long). No crash. |
| TC-07 | **SQL injection attempt** | Negative / Security | 1. Enter `' OR '1'='1` in username field, any password.<br>2. Click Login. | Login fails. Error message displayed (no DB compromise). |
| TC-08 | **Login after session expiry** | Edge | 1. Login successfully.<br>2. Wait for session timeout (or clear cookies).<br>3. Try to access protected page. | User is redirected to login screen with message: “Session expired. Please log in again.” |

## 7. Pass/Fail Criteria
- **Pass**: All expected results match actual outcomes.
- **Fail**: Any crash, security flaw, incorrect error message, or successful login with invalid credentials.

## 8. Risks & Assumptions
- **Risk**: Backend API rate limiting may affect repeated negative tests.
- **Assumption**: Test accounts exist in the staging environment.
- **Risk**: Password recovery link not tested here (separate plan).

## 9. Exit Criteria
- All planned test cases executed.
- No critical or high-severity bugs remain open.
- Test report reviewed and signed off.

## 10. Deliverables
- This test plan document
- Executed test case results (Excel/Test management tool)
- Bug reports (if any)