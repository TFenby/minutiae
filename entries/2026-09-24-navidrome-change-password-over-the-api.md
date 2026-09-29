# Navidrome 0.64: changing a user's password over the API

The Subsonic `changePassword` endpoint returns HTTP 501 in Navidrome 0.64. The native API that the web UI uses works:

1. `POST /auth/login` with `username` and `password` to get a JWT.
2. `PUT /api/user/{id}` with the full user object plus `currentPassword`, `password`, and `changePassword: true`, sending the header `x-nd-authorization: Bearer <token>`.

Verified 2026-09-24 against Navidrome 0.64.
