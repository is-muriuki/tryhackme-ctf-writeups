# OWASP Juice Shop – Session and Basket Request Analysis

## 1. Objective

The objective of this investigation was to understand how OWASP Juice Shop handles session cookies, order-history requests, and basket operations through HTTP requests and responses.

## 2. Environment and Tools

- Platform: TryHackMe
- Target: OWASP Juice Shop
- Browser: Firefox
- Tool: Firefox Developer Tools
- Network panel: Fetch/XHR
- Storage panel: Cookies

All activities were performed within the authorized practice environment using my own test account.

## 3. Session Cookie Analysis

I inspected the token cookie through Firefox Developer Tools under Storage → Cookies.

The following attributes were observed:

| Attribute | Observed value |
|---|---|
| HttpOnly | false |
| Secure | false |
| SameSite | Empty |
| Expires | Date displayed; no exact time shown |

### Analysis

- **HttpOnly=false:** JavaScript may be able to access the cookie. This can increase exposure if a cross-site scripting vulnerability exists.
- **Secure=false:** The cookie is not restricted by this attribute to HTTPS connections.
- **SameSite empty:** No explicit SameSite value was displayed in the Storage Inspector.
- **Expires:** The browser displayed an expiration date.

These observations identify session-cookie security settings worth reviewing. They do not independently prove that the application is vulnerable.

## 4. Order History Request Analysis

I navigated to Account → Orders → Order History and inspected the corresponding network request.

### Observations

| Property | Result |
|---|---|
| HTTP Method | GET |
| HTTP Status | 200 OK |
| Endpoint | `/rest/order-history` |
| Response status | success |
| Response data | Empty array `[]` |

### Analysis

The application successfully processed the request but returned an empty array because my test account had not placed any orders.

This helped me understand the difference between a successful request and a response containing records.

I also observed Cookie and Authorization headers in the request. The Authorization header used the Bearer authentication scheme.

## 5. Basket Request Analysis

I added a product to my own basket and inspected the resulting request using Firefox Developer Tools.

### Observations

| Property | Result |
|---|---|
| HTTP Method | POST |
| HTTP Status | 200 OK |
| Request fields | BasketId, ProductId, quantity |
| Response status | success |
| Response data | Array |

### Analysis

The POST request sends information to the server to process a basket operation.

The observed fields represent:

- **BasketId:** Identifies the basket.
- **ProductId:** Identifies the selected product.
- **quantity:** Represents the requested quantity.

The successful response indicates that the server processed the operation.

## 6. Security Concepts Learned

During this investigation, I learned:

1. How HTTP GET and POST requests are used by web applications.
2. How browsers store session cookies.
3. The security implications of HttpOnly and Secure cookie attributes.
4. The difference between authentication and authorization.
5. Why object-level authorization matters when accessing orders and baskets.
6. How request payloads and server responses can be inspected using browser developer tools.

## 7. Conclusion

This investigation improved my understanding of how web applications handle authentication, session information, order history, and basket operations.

I identified several session-cookie configuration settings that deserve further review.

No security vulnerability was confirmed during these observations.

## 8. Next Steps

- Continue investigating input validation.
- Study how the application processes user-supplied data.
- Learn how secure applications enforce authorization.
- Continue documenting findings with supporting evidence.

**Ethical Scope:** All testing was conducted on the authorized OWASP Juice Shop practice environment using my own test account.
