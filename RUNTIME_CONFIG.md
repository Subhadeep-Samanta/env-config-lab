
# Runtime Configuration Guide

## 1. Build the image

```bash
docker build -t envlab .
```

The image contains the application, not local configuration or credentials.

## 2. Configure non-secret values

Create a local `.env` file containing the required configuration:

```dotenv
DATABASE_URL=postgresql://localhost:5432/envlab
LOG_LEVEL=info
```

The example URL is sufficient for this lab's current startup check.
The application does not establish a real database connection.

## 3. Inject secrets at runtime

Set a test API key in your shell:

```bash
export API_KEY='local-test-key'
```

Start the container:

```bash
docker run -d \
  --name app-fixed \
  -p 3000:3000 \
  --env-file .env \
  -e API_KEY="$API_KEY" \
  envlab
```

Do not put real API keys in the Dockerfile, .env.example,
or committed files.

## 4. Verify the application

```bash
docker logs app-fixed
curl http://localhost:3000/health
docker exec app-fixed printenv
```

The health endpoint should return:

```json
{"status":"ok"}
```

Inspect the environment locally. Do not publish real credentials
in screenshots, logs, or pull requests.

## 5. Cloud Run mapping

- Non-secret configuration: `--set-env-vars`
- Secrets: Google Secret Manager with `--set-secrets`

See [docs/cloud-run-template.md](docs/cloud-run-template.md).
