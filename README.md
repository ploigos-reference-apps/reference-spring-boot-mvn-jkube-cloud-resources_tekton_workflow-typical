# reference-spring-boot-mvn-jkube-cloud-resources (Tekton) (Workflow: Typical)
The cloud resources for the [reference-spring-boot-mvn-jkube](https://github.com/ploigos-reference-apps/reference-spring-boot-mvn-jkube)
application when using Tekton and the Typical workflow.

## Install

### 0. Prerequisite

* TODO

### 1. Deploy Ploigos Workflow
Since Tekton is a purly Kubernetes based workflow runner service its entire configuration can be
installed with Helm.

```bash
WORKFLOW_NAMESPACE=
helm dependency update charts/reference-spring-boot-mvn-jkube-workflow

helm secrets upgrade --install \
    ref-spring-boot-jkube-typ ./charts/reference-spring-boot-mvn-jkube-workflow \
    -f charts/reference-spring-boot-mvn-jkube-workflow/values.yaml \
    -f charts/reference-spring-boot-mvn-jkube-workflow/secrets.yaml \
    --namespace ${WORKFLOW_NAMESPACE} \
    --render-subchart-notes
```

### 2. Configure Source Control Service webhook

1. Configure your Source Control Service repository hosting your application to send Webhook events
to the EventListener Route:
    * HTTP method: POST
    * POST Content Type: application/json
    * Trigger On:
      - Repository Events
        * Push
      - Issue Events
        * Pull Request
        * Pull Request Synchronized
    * Branch filter
      - `main`
