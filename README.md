# Portfolio Backend

A tiny Go HTTP service that powers the portfolio site's live LeetCode stats section.

## What it does

- Exposes a single endpoint (`/`) that returns the user's LeetCode profile data as JSON.
- On each request it fetches the public profile from the external LeetCode API
  (`https://alfa-leetcode-api.onrender.com/bhavesh1899287/profile`), decodes it
  into a `User` model, and passes it straight back to the caller.
- The response includes: `totalSolved`, `easySolved`, `mediumSolved`, `hardSolved`,
  `ranking`, `contributionPoint`, and `reputation`.
- Returns `502` if the upstream LeetCode API is unreachable and `500` on decoding errors.

## Tech stack

- Go 1.26.4 (module `b`)
- Go standard library only (`net/http`, `encoding/json`) — no external dependencies

## Project structure

```
cmd/main.go                     # entry point — registers routes, serves on :8045
routes/routes.go                # route registration
handlers/leetcode/getProfile.go # fetches + returns the LeetCode profile JSON
models/user.go                  # User struct (solved counts, ranking, reputation)
go.mod                          # module definition
```

## Run it locally

```bash
go run ./cmd        # starts the server on http://localhost:8045
go build ./...      # build everything
go vet ./...        # run the vet checks
```

Then open `http://localhost:8045/` — it will return the LeetCode profile JSON for the
configured username.

## Notes

- This repo is a minimal, purpose-built microservice: it exists only to relay live
  LeetCode stats to the portfolio frontend. There is no database, no auth, and no
  extra endpoints.
- The username and upstream API base URL are currently hardcoded in
  `handlers/leetcode/getProfile.go`.
