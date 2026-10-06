# API Test Scenarios

## Purpose

This document summarizes the API test scenarios implemented in the Postman collection for the DummyJSON REST API.

The scenarios cover product retrieval, search, pagination, simulated write operations, authentication, and negative authentication checks.

## Product Read Scenarios

| ID | Request | Scenario | Expected Result | Automated Assertions |
|---|---|---|---|---|
| API-001 | GET `/products/1` | Retrieve an existing product | `200 OK`; product ID is 1; required fields and selected data types are valid | Yes |
| API-002 | GET `/products/999999` | Request a nonexistent product | `404 Not Found`; appropriate error message is returned | Yes |
| API-003 | GET `/products` | Retrieve the product list | Product list is returned successfully | No |
| API-004 | GET `/products?limit=10` | Limit the number of returned products | Response is limited according to the query parameter | No |
| API-005 | GET `/products?limit=10&skip=10` | Retrieve paginated products | `200 OK`; 10 products returned; `limit` and `skip` values match the request | Yes |
| API-006 | GET `/products/search?q=phone` | Search for products | Search results are returned for the supplied query | No |
| API-007 | GET `/products/search?q=xyznonexistent999` | Search with no matching results | `200 OK`; empty `products` array; `total` equals 0 | Yes |

## Product Write Scenarios

| ID | Request | Scenario | Expected Result | Automated Assertions |
|---|---|---|---|---|
| API-008 | POST `/products/add` | Create a product | `201 Created`; generated ID is returned; response data matches request body | Yes |
| API-009 | PATCH `/products/1` | Partially update product price | `200 OK`; product ID remains unchanged; price is updated; selected existing data is preserved | Yes |
| API-010 | PUT `/products/1` | Update product title using PUT | `200 OK`; product ID remains unchanged; title is updated; selected existing data is preserved | Yes |
| API-011 | DELETE `/products/1` | Delete a product | `200 OK`; correct product is returned; `isDeleted` equals `true` | Yes |

## Authentication Scenarios

| ID | Request | Scenario | Expected Result | Automated Assertions |
|---|---|---|---|---|
| API-012 | POST `/auth/login` | Log in with valid test credentials | `200 OK`; expected user is returned; access token is generated and stored in the active environment | Yes |
| API-013 | GET `/auth/me` | Access protected user data with a valid token | `200 OK`; authenticated user data matches the logged-in test user | Yes |
| API-014 | GET `/auth/me` | Access protected endpoint without a token | `401 Unauthorized`; missing-token message is returned; protected user data is not returned | Yes |
| API-015 | GET `/auth/me` | Access protected endpoint with an invalid token | `401 Unauthorized`; invalid/expired-token message is returned; protected user data is not returned | Yes |

## Notes and Limitations

- DummyJSON simulates create, update, and delete operations; changes are not persisted on the server.
- `access_token` is obtained during login and stored dynamically in the active Postman environment.
- The portfolio environment template does not contain a password or access token.
- Selected informational requests are intentionally kept without automated assertions where additional assertions would mainly duplicate coverage already provided by other scenarios.