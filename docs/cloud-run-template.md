# Cloud Run Mapping

## Configuration Variables

- `DATABASE_URL`: Use `--set-env-vars` only if the URL contains no credentials or other sensitive information. Otherwise, use Secret Manager.
- `LOG_LEVEL`: Set using `--set-env-vars`.
- `API_KEY`: Store in Google Secret Manager and inject using `--set-secrets`. Never hardcode the API key in the Dockerfile or commit it to the repository.

## 1. Non-secret Configuration

Update the Cloud Run service with non-sensitive configuration:

```bash
gcloud run services update SERVICE_NAME \
  --region REGION \
  --set-env-vars LOG_LEVEL=info
```

If `DATABASE_URL` contains no sensitive information, you can include it in `--set-env-vars` as well.

## 2. Secret Configuration

First, create a secret named `api-key` in Google Secret Manager and add the API key as a secret version. Ensure the Cloud Run service identity has permission to access that secret.

Inject the secret into the container as `API_KEY`:

```bash
gcloud run services update SERVICE_NAME \
  --region REGION \
  --set-secrets API_KEY=api-key:latest
```

If `DATABASE_URL` contains credentials, store it in Secret Manager too and map it using `--set-secrets`.

## Notes

- Replace `SERVICE_NAME` and `REGION` with the actual Cloud Run service name and region.
- Replace secret names as needed to match your Google Cloud configuration.
- These commands document the deployment configuration; they do not confirm that a Cloud Run deployment has been performed.
