# kubernetes-observability-lab
Assignment 8

## Why use `helm upgrade --install`?

- **Avoids errors if the release already exists:**  
  Using `helm upgrade --install` prevents failures in case the Helm release is already present. Instead of receiving a *"release already exists"* error, Helm will upgrade the existing deployment.

- **Single command for install and upgrade:**  
  The same command works for both initial installation and future updates. There is no need to remember separate `install` and `upgrade` commands.

- **Ensures idempotency:**  
  The command can be safely executed multiple times. If the release does not exist, it will be installed; if it exists, it will be upgraded to match the desired state defined in the values files.  
  This supports a declarative and GitOps-friendly workflow.


## Logging with Loki and Fluent Bit

- Fluent Bit collects Kubernetes pod logs from the cluster nodes and forwards them to Loki.
- Loki stores logs centrally and allows querying them using labels such as namespace, pod, and container.
- Grafana can use Loki as a data source to explore, search, and visualize logs in a unified interface.

