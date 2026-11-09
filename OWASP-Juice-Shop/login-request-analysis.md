# OWASP Juice Shop – Login Request Analysis

## 1. Overview

This write-up documents my investigation of the login process in OWASP Juice Shop using the authorized TryHackMe lab environment.

The purpose was to understand how a browser communicates with a web server during authentication.

## 2. Lab Environment

- **Platform:** TryHackMe
- **Target application:** OWASP Juice Shop
- **Browser:** Firefox
- **Tool:** Firefox Developer Tools
- **Feature used:** Network tab – Fetch/XHR

## 3. Objective

To inspect a login request and understand:

- HTTP request methods.
- Request and response headers.
- JSON request payloads.
- HTTP response status codes.

## 4. Methodology

1. Started the TryHackMe AttackBox.
2. Opened the OWASP Juice Shop application.
3. Navigated to the login page.
4. Opened Firefox Developer Tools.
5. Selected the Network tab.
6. Filtered network traffic using Fetch/XHR.
7. Submitted invalid test credentials.
8. Selected the resulting login request and inspected its details.

## 5. Findings

| Property | Observation |
|---|---|
| HTTP method | POST |
| Status code | 401 Unauthorized |
| Request Content-Type | application/json |
| JSON fields | email, password |
| Response Content-Type | text/html; charset=UTF-8 |

## 6. Technical Analysis

### A. POST Method

The browser used the POST method to submit login information to the server.

Unlike URL query parameters, POST data is normally carried in the request body.

However, POST does not encrypt information. HTTPS/TLS provides encryption during transmission.

### B. JSON Payload

The request contained two fields:

- `email`
- `password`

JSON is a structured format used to exchange data between applications.

It does not provide encryption by itself.

### C. HTTP 401 Status Code

The server returned HTTP 401 Unauthorized after the invalid login attempt.

This indicated that the submitted credentials were not accepted.

### D. Content-Type

The request used JSON, while the observed response Content-Type was HTML with UTF-8 character encoding.

Content-Type tells the receiving application how to interpret the content.

## 7. Security Lessons

1. POST requests do not automatically make sensitive data secure.
2. HTTPS/TLS protects information while it travels between browser and server.
3. JSON is a data format, not an encryption mechanism.
4. HTTP status codes help identify the outcome of requests.
5. Browser Developer Tools are useful for understanding web application communication.

## 8. Conclusion

This investigation improved my understanding of HTTP authentication requests and browser-server communication.

I gained practical experience inspecting request methods, headers, payload fields and server responses using Firefox Developer Tools.

## 9. Scope and Ethics

This exercise was conducted in the authorized TryHackMe training environment. No external systems or real user accounts were tested.

## 10. Evidence

Screenshots will be added after the lab is restarted.

- Login page
- Network request showing POST and status 401
- Request headers showing JSON Content-Type
- Payload showing field names, with sensitive values hidden
## 6. Additional Investigation: Security Answers API

### Objective

Investigate how the registration form submits security-question information to the server.

### Observations

| Property | Finding |
|---|---|
| HTTP method | POST |
| Endpoint | `/api/SecurityAnswers/` |
| HTTP status | 200 OK |
| Request format | application/json |
| Payload fields | answer, securityId, userId |
| Application status | success |
| Response data | null |

### Technical Analysis

The browser sent security-question information to the Security Answers API using a POST request.

The payload contained three fields:

- `answer`: the submitted security answer.
- `securityId`: identifies the selected security question.
- `userId`: identifies the associated user account.

The application returned a success status, while the data field was null.

### Security Consideration

The presence of a userId in a request does not automatically indicate an authorization vulnerability.

Authorization checks must ensure that users can only perform permitted actions on the relevant account data.

### Lessons Learned

- API endpoints handle specific application operations.
- JSON is commonly used for API communication.
- HTTP status codes and application-level status values are different.
- A successful API response does not automatically prove that the operation is secure.
