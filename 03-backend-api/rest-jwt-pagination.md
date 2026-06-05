# REST, JWT, Middleware, and Pagination

## REST Methods

| Method | Use | Idempotent |
|---|---|---|
| GET | read resource | yes |
| POST | create/action | no by default |
| PUT | replace resource | yes |
| PATCH | partial update | usually no |
| DELETE | delete resource | yes |

## Status Codes

| Code | Meaning |
|---|---|
| 200 | OK |
| 201 | Created |
| 204 | No content |
| 400 | Bad request |
| 401 | Unauthenticated |
| 403 | Unauthorized |
| 404 | Not found |
| 409 | Conflict |
| 422 | Validation error |
| 429 | Too many requests |
| 500 | Server error |

## API Versioning

Common options:

```text
/v1/users
Accept: application/vnd.company.v1+json
```

Path versioning is simplest for interviews.

## JWT Basics

JWT contains claims:

```json
{
  "sub": "user_123",
  "exp": 1893456000,
  "role": "admin"
}
```

Server validates:

- signature
- expiration
- issuer/audience if used
- token type

Access tokens should be short-lived. Refresh tokens can live longer and should be stored more carefully.

## Middleware

Middleware wraps handlers.

```go
func Auth(next http.Handler) http.Handler {
    return http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
        userID, err := validateToken(r.Header.Get("Authorization"))
        if err != nil {
            http.Error(w, "unauthorized", http.StatusUnauthorized)
            return
        }

        ctx := context.WithValue(r.Context(), userIDKey, userID)
        next.ServeHTTP(w, r.WithContext(ctx))
    })
}
```

Store user ID in request context, not a global variable.

## Offset Pagination

```http
GET /posts?offset=100&limit=20
```

Problems:

- slow for large offsets
- unstable when rows are inserted/deleted

## Cursor Pagination

```http
GET /feed?cursor=2026-06-05T10:00:00Z_abc123&limit=20
```

Benefits:

- stable for feeds/messages
- efficient with indexed ordering

Common index:

```text
(user_id, created_at, id)
```

