# SheetsBackendApi
A backend API in GO to update google sheets that's being used as the backend database for all this collections data

## API Endpoints

- `GET /hello` - Returns "hello"

## Local Development

Run the application locally:

```bash
go run main.go
```

The server will start on port 8080. Test the endpoint:

```bash
curl http://localhost:8080/hello
```

## Deployment to Google Cloud Run

### Prerequisites

1. Google Cloud Project with billing enabled
2. gcloud CLI installed and authenticated
3. Enable required APIs:
   ```bash
   gcloud services enable cloudbuild.googleapis.com
   gcloud services enable run.googleapis.com
   gcloud services enable artifactregistry.googleapis.com
   ```

### Setup

1. Create an Artifact Registry repository:
   ```bash
   gcloud artifacts repositories create sheetsbackendapi \
     --repository-format=docker \
     --location=us-central1 \
     --description="Docker repository for SheetsBackendApi"
   ```

2. Configure Docker authentication:
   ```bash
   gcloud auth configure-docker us-central1-docker.pkg.dev
   ```

### Deploy

Deploy using Cloud Build:

```bash
gcloud builds submit --config cloudbuild.yaml
```

### Configuration

You can customize the deployment by modifying the substitution variables in `cloudbuild.yaml`:

- `_REGION`: The Google Cloud region (default: us-central1)
- `_REPOSITORY`: The Artifact Registry repository name (default: sheetsbackendapi)
- `_SERVICE_NAME`: The Cloud Run service name (default: sheetsbackendapi)

### Access Your Deployed Service

After deployment, Cloud Build will output the service URL. You can also get it with:

```bash
gcloud run services describe sheetsbackendapi --region us-central1 --format='value(status.url)'
```

Test your deployed endpoint:

```bash
curl https://YOUR-SERVICE-URL/hello
```