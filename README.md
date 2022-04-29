# Example of converting a (nested) portion of values.yaml in Helm to a JSON configmap mounted to the pod

1. Enter the values in YAML format under the `config` key of values.yaml
```yaml
config:
  test: TEST_VAL
  production: "false"
  backend_url: BACKEND_URL
  jupyterhub_url: JUPYTERHUB_URL
  services:
    auth:
      responseType: RESPONSE_TYPE
      url: URL
      clientId: CLIENT_ID
      clientSecret: CLIENT_SECRET
      redirectUrl: REDIRECT_URL
      tenant: TENANT
    logging:
      url: SERVICES_LOG_URL
      application: SERVICES_LOG_APP
      environment: SERVICES_LOG_ENV
      subTag: SERVICES_LOG_TAG
```
2. Install the Helm chart 
```
helm install . --generate-name
```
3. Check that configmap was properly converted and mounted as JSON:
```
kubectl exec -it chart-1651254858-json-86d54fb69f-hdknn -- bash -c "cat /config.json"
```
Modify the pod name to the pod autocreated for you by Helm (check with `kubectl get pods`). Observer the output:
```
{
  "backend_url": "BACKEND_URL",
  "jupyterhub_url": "JUPYTERHUB_URL",
  "production": "false",
  "services": {
    "auth": {
      "clientId": "CLIENT_ID",
      "clientSecret": "CLIENT_SECRET",
      "redirectUrl": "REDIRECT_URL",
      "responseType": "RESPONSE_TYPE",
      "tenant": "TENANT",
      "url": "URL"
    },
    "logging": {
      "application": "SERVICES_LOG_APP",
      "environment": "SERVICES_LOG_ENV",
      "subTag": "SERVICES_LOG_TAG",
      "url": "SERVICES_LOG_URL"
    }
  },
  "test": "TEST_VAL"
}
```

