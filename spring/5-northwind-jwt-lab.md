# Lab: Securing the API with JWT and Spring Security

In this lab you will add JWT-based security to your northwind-api so that certain endpoints require the caller to prove they have the right role before the request is allowed through.

You will not build a login system. Instead, a token generator endpoint is provided for you. Your job is to get the plumbing in place, understand what is inside a token, and then protect the `POST`, `PUT`, and `DELETE` endpoints on the `ProductController` so that only callers with an admin role can use them.

---

## Part 1 — Add the Dependencies

Open `pom.xml` and add the following dependencies inside the `<dependencies>` block:

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-security</artifactId>
</dependency>

<dependency>
    <groupId>io.jsonwebtoken</groupId>
    <artifactId>jjwt-api</artifactId>
    <version>0.12.6</version>
</dependency>

<dependency>
    <groupId>io.jsonwebtoken</groupId>
    <artifactId>jjwt-impl</artifactId>
    <version>0.12.6</version>
    <scope>runtime</scope>
</dependency>

<dependency>
    <groupId>io.jsonwebtoken</groupId>
    <artifactId>jjwt-jackson</artifactId>
    <version>0.12.6</version>
    <scope>runtime</scope>
</dependency>
```

Reload Maven after saving.

---

## Part 2 — Add the Provided Security Classes

Four files have been provided for you inside the [jwt-additions.zip](./jwt-additions.zip) . Copy them into your project exactly as given (the specific location where to copy them is outlined below) — you do not need to understand every line right now.

**Create a `security` package** inside `com.yearup.northwindapi` and add these three files:

- `JwtUtil.java` — generates and validates tokens
- `JwtFilter.java` — reads the `Authorization` header on every request and sets up the security context
- `SecurityConfig.java` — configures Spring Security to use your filter and enables method-level security

**Add this file** to your existing `controller` package:

- `TokenController.java` — provides two endpoints that mint tokens on demand

Restart the application. It should start cleanly. If it does, the plumbing is working.

---

## Part 3 — Get Your Tokens

The token controller exposes two endpoints. Call both of them in Insomnia now and save the responses — you will need these tokens for the rest of the lab.

| Endpoint | What it returns |
|---|---|
| `GET /api/token/user` | A JWT with `ROLE_USER` |
| `GET /api/token/admin` | A JWT with `ROLE_ADMIN` |

No request body or headers are needed — just a plain GET request.

You will get back a long string of characters that looks something like this:

```
eyJhbGciOiJIUzI1NiJ9.eyJzdWIiOiJhZG1pbnVzZXIiLCJyb2xlcyI6...
```

That is your JWT. Copy it and keep it open.

---

## Part 4 — Inspect the Token at jwt.io

Go to **[https://jwt.io](https://jwt.io)** and paste your admin token into the Encoded box on the left.

The website will decode it and show you three sections on the right:

**Header** — describes the token itself:
```json
{
  "alg": "HS256",
  "typ": "JWT"
}
```

**Payload** — the claims, meaning the data embedded in the token:
```json
{
  "sub": "adminuser",
  "roles": ["ROLE_ADMIN"],
  "iat": 1718000000,
  "exp": 1718086400
}
```

**Signature** — a cryptographic hash that proves the token has not been tampered with.

The payload is the important part. Notice that the token carries:

- `sub` — the subject, which is the username
- `roles` — a list of roles this user has been granted
- `iat` — issued at (a Unix timestamp)
- `exp` — expiration time (24 hours after issue)

This is what Spring Security reads when you send the token with a request. The `JwtFilter` extracts the `roles` claim and tells Spring Security what this caller is allowed to do. `@PreAuthorize` then checks those roles before a controller method is allowed to run.

Now paste your **user** token into jwt.io and compare. The only difference in the payload is `"roles": ["ROLE_USER"]`. That single difference is what will determine whether a request is allowed or rejected once you add the annotations in Part 5.

---

## Part 5 — Protect the Product Endpoints

Open `ProductController.java`. Add `@PreAuthorize("hasRole('ROLE_ADMIN')")` to the three methods that modify data:

- The method mapped to `POST /api/products`
- The method mapped to `PUT /api/products/{id}` (if you have one)
- The method mapped to `DELETE /api/products/{id}`

The annotation goes directly above the method signature, below the mapping annotation:

```java
@DeleteMapping("/{id}")
@PreAuthorize("hasRole('ROLE_ADMIN')")
public ResponseEntity<Void> deleteProduct(@PathVariable Integer id) {
    ...
}
```

You do not need to change anything else. Restart the application.

---

## Part 6 — Test the Security

Use Insomnia to test each scenario below. To send a token, add an `Authorization` header with the value `Bearer ` followed by the token (note the space after Bearer):

```
Authorization: Bearer eyJhbGciOiJIUzI1NiJ9...
```

**Test 1 — No token:**
Send `DELETE /api/products/1` with no Authorization header.
You should receive a `401 Unauthorized` response.

**Test 2 — User token on a protected endpoint:**
Send `DELETE /api/products/1` with the `ROLE_USER` token in the header.
You should receive a `403 Forbidden` response.

**Test 3 — Admin token on a protected endpoint:**
Send `DELETE /api/products/1` with the `ROLE_ADMIN` token in the header.
You should receive a `204 No Content` response — the delete went through.

**Test 4 — Either token on a read endpoint:**
Send `GET /api/products` with either token.
You should receive a `200 OK` response — read endpoints are not restricted by role.

**Test 5 — User token to create a product:**
Send `POST /api/products` with the `ROLE_USER` token and a valid JSON body.
You should receive a `403 Forbidden` response.

**Test 6 — Admin token to create a product:**
Send `POST /api/products` with the `ROLE_ADMIN` token and a valid JSON body.
You should receive a `200 OK` response with the created product.

---

> **Why 401 vs 403?**
> `401 Unauthorized` means no token was provided — the caller has not identified themselves at all. `403 Forbidden` means a valid token was provided but the caller's role does not grant access to that resource. They are different problems: one is about identity, the other is about permission.

---

## Checklist

- [ ] All four dependencies added to `pom.xml` and Maven reloaded
- [ ] `security` package created containing `JwtUtil.java`, `JwtFilter.java`, and `SecurityConfig.java`
- [ ] `TokenController.java` added to the `controller` package
- [ ] Application starts without errors
- [ ] `GET /api/token/user` returns a token
- [ ] `GET /api/token/admin` returns a token
- [ ] Both tokens decoded at jwt.io — payload fields `sub`, `roles`, `iat`, `exp` identified
- [ ] `@PreAuthorize("hasRole('ROLE_ADMIN')")` added to `POST`, `PUT`, and `DELETE` methods on `ProductController`
- [ ] Test 1 passes — no token returns `401`
- [ ] Test 2 passes — user token on delete returns `403`
- [ ] Test 3 passes — admin token on delete returns `204`
- [ ] Test 4 passes — either token on GET returns `200`
- [ ] Test 5 passes — user token on POST returns `403`
- [ ] Test 6 passes — admin token on POST returns `200`
