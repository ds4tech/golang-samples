Based on: https://cloud.google.com/run/docs/tutorials/custom-metrics-opentelemetry-sidecar

## Create repository
```
gcloud artifacts repositories create run-otel --repository-format=docker \
    --location=europe-central2 \
    --project=$PROJECT_ID
    
```
### Build and Push images

```
gcloud builds submit --tag europe-central2-docker.pkg.dev/fourkeys-386218/run-otel/sample-metrics-app
```

To update a revision or to [deploy a new revision of an existing service](https://cloud.google.com/run/docs/deploying#yaml_1), download and modify the YAML service definition:
```
gcloud builds submit collector --tag europe-central2-docker.pkg.dev/fourkeys-386218/run-otel/otel-collector-metrics
```
### Deploy:
```
gcloud run services replace cloud_run_service.yaml
```

```
gcloud run services describe SERVICE --format yaml > cloud_run_service.yaml
```

### Send request
```
curl -H \
"Authorization: Bearer $(gcloud auth print-identity-token)" \
https://custom-metrics-sample-service-uterycxntq-lm.a.run.app
```

