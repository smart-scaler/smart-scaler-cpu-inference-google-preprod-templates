# SmartScaler CPU Inference — GCP Preprod Templates

GCP-specific variants of the inference job templates used by the
`smart-scaler-inference-deployer` provisioner for GKE tenants.

## Differences from OCI templates

| Setting | OCI | GCP |
|---|---|---|
| `REDIS_HOST` | `redis.saas-preprod.smart-scaler.io` | `smartscaler-saas-redis-master.saas-apis.svc.cluster.local` |
| `REDIS_MODE` | `OCI` | `GCP` |
| `STORAGE_PROVIDER` | *(absent — defaults to OCI)* | `GCP` |
| Redis auth | mTLS via OCI-specific certs | mTLS via same certs (unified) |
| Object storage auth | OCI Instance Principal | GKE Workload Identity (ADC) |

## Bucket naming (GCP)

Buckets are created by the tenant provisioner Terraform module as:
```
smartscaler-saas-tenant-{tenant_id}-thanos-bucket
```

The `STORAGE_PROVIDER=GCP` env var tells the inference jobs to use
`GCPStorageClient` (google-cloud-storage + ADC) instead of OCI SDK.

## How to configure the inference provisioner

Set the template URL in the `smart-scaler-inference-job-config` ConfigMap
(in `saas-inference-provisioner` namespace on GCP preprod):

```
GIT_TEMPLATE_URL: https://raw.githubusercontent.com/smart-scaler/smart-scaler-cpu-inference-google-preprod-templates/master/
```
