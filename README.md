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

## Notes for marking

- Stage 1 asks for a `python:3.9` Dockerfile; Stage 3 replaces it. The original is in the `Stage 1 done` commit (`git show 3e54bef:backend/Dockerfile`).
- Where one piece of evidence did not fit one screen it is split into two files, e.g. `S1-2a.png` and `S1-2b.png`.

## Docker Hub digest

`kripashankar3/fest-backend:1.0.0` → `sha256:87b5decad777e573e5184a02eaf49aedf491f5a02e9c74f9196c52c0188406d0`

This is the image index digest printed by `docker push` (and the one signed in Stage 5). The Tags page on Docker Hub lists the linux/amd64 manifest inside it, `sha256:0e0a7d7073ae…`.

## Trivy: before and after (Stage 3)

| Image | Base | CRITICAL | HIGH | Size |
|---|---|---|---|---|
| fest-backend:0.1 | python:3.9 | 234 | 2249 | 1.61 GB |
| fest-backend:0.2 | python:3.13.18-slim | 0 | 44 | 207 MB |

Fixes in 0.2: slim Debian 13 base pinned to Python 3.13.18, Flask 3.1.3 / Werkzeug 3.1.9 / psycopg2-binary 2.9.13, runs as non-root `appuser`, and pip/setuptools are removed after install (their vendored copies of msgpack and urllib3 had fixable HIGH CVEs). The 44 HIGH left are Debian OS packages with no fix released yet; `trivy image --ignore-unfixed fest-backend:0.2` reports 0.

`trivy image --severity CRITICAL --ignore-unfixed --exit-code 1 fest-backend:0.2` exits with code 0.

## Signing

`cosign.pub` is the public key. 1.0.0 is signed by digest and verifies; 1.0.1 (`sha256:c7e3fbbedf062c4f27e20f2f2f76c4968fef05a18af20a126cd4a657c1b1947e`) was pushed unsigned and `cosign verify` rejects it with "no signatures found".

