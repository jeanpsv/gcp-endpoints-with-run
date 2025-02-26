# gcp-endpoints-with-run
Google Cloud Endpoints with Google Cloud Run

I will follow [this](https://cloud.google.com/endpoints/docs/openapi/get-started-cloud-run) tutorial.

## Deploying sample service on Google Cloud Run

1. In the GCP [console](https://console.cloud.google.com/) go to the **Manage resources** page and create a project.
2. Enable billing to the project following [this](https://cloud.google.com/billing/docs/how-to/modify-project) tutorial.
3. Make a note of the project ID and project NUMBER. On the rest of this page, this project ID and project NUMBER is referred to as PROJECT_ID and PROJECT_NUMBER.
4. Download and install the Google Cloud SDK following [this](https://cloud.google.com/sdk/docs/quickstarts) tutorial.
5. Go to Google Cloud Run [page](https://console.cloud.google.com/run?enableapi=true).
6. Click **Create service** to display the _Create service_ form.
7. Use `gcr.io/cloudrun/hello` as a container image.
8. Select the region where you want your service located.
9. Check **Allow unauthenticated** invocations to be able to open the result in your web browser.
10. Click **Create** to deploy the image to Cloud Run and wait for the deployment to finish.
11. Click the displayed URL link to run the deployed container.

## Deploying ESP container to Google Cloud Run

Run the command:
```bash
gcloud beta run deploy SERVICE_NAME --image=gcr.io/endpoints-release/endpoints-runtime-serverless:1.30.0 --allow-unauthenticated --region=us-central1 --project=PROJECT_ID
```

replace `SERVICE_NAME` and `PROJECT_ID`. Take note of the hostname in the URL.

## Configuring Google Cloud Endpoints

1. Make a copy of `openapi-run.yaml.example` to `openapi-run.yaml`.
2. Replace `my.esp.host` to your ESP hostname. **Don't** include the protocol identifier `https://`.
3. Replace `https://my.backend.service.url` to your URL of your backend service.

## Deploying Google Cloud Endpoints

Run the command:
```bash
gcloud endpoints services deploy openapi-run.yaml --project PROJECT_ID
```

replace `PROJECT_ID`.

## Configuring Google ESP

1. gcloud beta run services update `SERVICE_NAME` --set-env-vars ENDPOINTS_SERVICE_NAME=`HOST` --region us-central1 --project `PROJECT_ID`
2. gcloud beta run services add-iam-policy-binding `BACKEND_SERVICE` --member serviceAccount:`PROJECT_NUMBER`-compute@developer.gserviceaccount.com --role roles/run.invoker --project `PROJECT_ID`

- replace `SERVICE_NAME` to ESP service name.
- replace `HOST` to cloud endpoints host.
- replace `PROJECT_ID` and `PROJECT_NUMBER`.
- replace `BACKEND_SERVICE` to your backend on Cloud Run.

## Invoking service

Run a command:
```bash
curl --request GET --header "content-type:application/json" "https://${ENDPOINTS_HOST}/hello"
```

replace `${ENDPOINTS_HOST}` to host value on endpoints configuration.
