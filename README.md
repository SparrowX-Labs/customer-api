# Customer API

FastAPI customer management service for SparrowX. It provides CRUD/search operations, health endpoints, Prometheus metrics, and PostgreSQL persistence through the Crossplane-managed `customerdb` resource.

## API

The service exposes `/api/customers/` for create/list/search, `/api/customers/{customer_id}` for retrieve/update/delete, `/api/customers/health` for smoke tests, `/health` for probes, and `/metrics` for Prometheus. FastAPI documentation is available at `/docs` and `/redoc`.

## Platform integration

The image is built and scanned by the reusable workflows in [`workflows-templates`](https://github.com/SparrowX-Labs/workflows-templates). Argo CD deploys the Helm values from `helm-values-dev.yaml` or `helm-values-prod.yaml` into the matching namespace. `dev` and `prod` are separate namespaces on the same EKS cluster. Crossplane supplies the private PostgreSQL database and AWS Secrets Manager credentials.

Define `HEALTH_CHECK_PATH` and, only when needed, a repository-level `AWS_ROLE_NAME` override. `AWS_ACCOUNT_ID`, `AWS_REGION`, environment URLs, and Argo CD credentials are shared organization configuration.

## Local development

```bash
python -m pip install -r requirements-dev.txt
pytest
uvicorn src.main:app --reload --port 8000
```

Configure `DB_HOST`, `DB_PORT`, `DB_NAME`, `DB_USERNAME`, `DB_PASSWORD`, and `CORS_ALLOW_ORIGINS` for a database-backed run.

## License

See [LICENSE.md](LICENSE.md).
