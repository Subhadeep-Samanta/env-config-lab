# Cloud Run Mapping

## Configuration Variables

- `DATABASE_URL`: Use `--set-env-vars` only if the URL contains no
  credentials or other sensitive information. Otherwise, use Secret Manager.
- `LOG_LEVEL`: Set using `--set-env-vars`.

Example:

```bash
gcloud run services update SERVICE_NAME \
  --region REGION \
  --set-env-vars LOG_LEVEL=info
