# Environment Configuration Lab — Verification Report

## Original Failure
The original container failed to start because DATABASE_URL was undefined.
The original Dockerfile also contained a hardcoded API key and a misspelled
LOGLEVEL environment variable.

## Changes Made
- Removed the hardcoded API key from the Dockerfile.
- Removed the incorrect LOGLEVEL environment instruction.
- Configured local environment variables through an .env file.
- Added .env ignore rules to .gitignore.
- Kept .env.example for non-secret configuration placeholders.
- Injected API_KEY at runtime rather than embedding it in the image.
- Documented Cloud Run environment variables and Secret Manager mappings.
- Added runtime configuration instructions in RUNTIME_CONFIG.md.

## Verification Results

### Container Logs
The container logs showed:
- Starting app with log level: info
- Server is running on port 3000

### Runtime Environment
The running container contained:
- DATABASE_URL=postgresql://localhost:5432/envlab
- LOG_LEVEL=info
- API_KEY was supplied at runtime using a test value.

### Health Check
Command:
curl http://localhost:3000/health

Result:
{"status":"ok"}

### Security Check
The corrected Dockerfile no longer embeds API_KEY as an ENV instruction.
The API key was supplied at runtime for local testing.

## Limitations
The application checks whether DATABASE_URL is defined but does not
establish a real database connection. The health endpoint confirms HTTP
service availability, not database connectivity.

## Cloud Run
The documentation describes --set-env-vars for non-secret configuration
and --set-secrets for secrets stored in Google Secret Manager.
No Cloud Run deployment was performed.
