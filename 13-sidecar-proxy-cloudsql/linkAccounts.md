* https://cloud.google.com/sql/docs/postgres/connect-kubernetes-engine
```
gcloud iam service-accounts add-iam-policy-binding --role roles/iam.workloadIdentityUser \
--member "serviceAccount:<GCLOUD_PROJECT_NAME>.svc.id.goog[<NAMESPACE_USUALLY_IS:default>/<SERVICE_ACCOUNT_NAME_KSA>]" \
postgres-service-account-to-gk@<GCLOUD_PROJECT_NAME>.iam.gserviceaccount.com
```
```
kubectl annotate serviceaccount <SERVICE_ACCOUNT_NAME> \
iam.gke.io/gcp-service-account=<SERVICE_ACCOUNT_AS_GCP_GSA>@<GCLOUD_PROJECT_NAME>.iam.gserviceaccount.com
```