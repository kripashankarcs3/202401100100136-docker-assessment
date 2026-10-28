# FestPass — Docker Assessment

| | |
|---|---|
| Name | Kripashankar Yadav |
| REGNO | 202401100100136 |
| PORT | 8036 |
| Docker Hub username | kripashankar3 |
| Docker Hub repository | https://hub.docker.com/r/kripashankar3/fest-backend |

## Running the stack

```bash
cp .env.example .env      # then set a real DB_PASSWORD
docker compose up -d
# open http://localhost:8036
```

- `frontend` (nginx) and `backend` share the `frontnet` network; `backend` and `db` share `backnet`. The frontend cannot reach the database.
- PostgreSQL data lives in the named volume `pgdata`, so registrations survive `docker compose down`.

## AI tools used

- Claude Code (Anthropic): helped write the Dockerfile, `.dockerignore`, `compose.yaml`, `.env.example`, `.gitignore` and this README; fixed the indentation of the starter code copied from the PDF; suggested the library upgrades and base image for Stage 3; explained the Trivy and cosign output. I ran the evidence commands and took the screenshots myself.

## Docker Hub digest

`kripashankar3/fest-backend:1.0.0` → `TODO`

## Trivy: before and after (Stage 3)

| Image | Base | CRITICAL | HIGH | Size |
|---|---|---|---|---|
| fest-backend:0.1 | python:3.9 | TODO | TODO | TODO |
| fest-backend:0.2 | python:3.13-slim | TODO | TODO | TODO |

`trivy image --severity CRITICAL --ignore-unfixed --exit-code 1 fest-backend:0.2` exits with code 0.

## Signing

`cosign.pub` is the public key. 1.0.0 is signed by digest and verifies; 1.0.1 was pushed unsigned and `cosign verify` rejects it.

## Steps not completed

- None.
