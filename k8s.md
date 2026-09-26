# 🌐 Kubernetes & DevOps Cheat Sheet

[← Back to Main Index](./README.md)

A fast-reference guide for `kubectl` workflows, pod debugging, context switching, and Helm quickstarts.

> **Tip:** `alias k=kubectl` and enable shell completion (`source <(kubectl completion bash)`, then `complete -o default -F __start_kubectl k`).

---

## 🧭 Contexts, Clusters & Namespaces

| Command | Action |
| :--- | :--- |
| `kubectl config get-contexts` | List all contexts in your kubeconfig (current one marked `*`). |
| `kubectl config current-context` | Show the active context. |
| `kubectl config use-context <context>` | Switch to another cluster/context. |
| `kubectl config set-context --current --namespace=<ns>` | Set the default namespace for the current context. |
| `kubectl get ns` | List all namespaces. |
| `kubectl create ns <ns>` | Create a namespace. |
| `kubectl cluster-info` | Show the API server and core service endpoints. |
| `kubectx` / `kubens` | Faster context / namespace switching (separate install). |

---

## 🔍 Viewing Resources

| Command | Action |
| :--- | :--- |
| `kubectl get pods` | List pods in the current namespace. |
| `kubectl get pods -A` | List pods across **all** namespaces. |
| `kubectl get pods -o wide` | Include node names and pod IPs. |
| `kubectl get pods -w` | Watch pods update in real time. |
| `kubectl get pods -l app=<name>` | Filter resources by label. |
| `kubectl get all` | List common resources (pods, services, deployments, replicasets). |
| `kubectl get deploy,svc,ing` | List several resource types at once. |
| `kubectl get <resource> <name> -o yaml` | Dump the full live YAML of a resource. |
| `kubectl describe <resource> <name>` | Show detailed state plus recent **events** (first stop when debugging). |
| `kubectl get events --sort-by=.lastTimestamp` | List namespace events, newest last. |
| `kubectl explain <resource>.spec` | Show documentation for resource fields. |
| `kubectl api-resources` | List all resource types and their short names (`po`, `svc`, `deploy`…). |

---

## 🐛 Pod Debugging

| Command | Action |
| :--- | :--- |
| `kubectl logs <pod>` | Show a pod's logs. |
| `kubectl logs -f <pod> -c <container>` | Follow logs of a specific container in a multi-container pod. |
| `kubectl logs <pod> --previous` | Show logs from the previous (crashed) container instance. |
| `kubectl logs -l app=<name> --tail=100` | Show logs from all pods matching a label. |
| `kubectl logs deploy/<name>` | Show logs from a pod of a deployment. |
| `kubectl exec -it <pod> -- sh` | Open a shell inside a running pod. |
| `kubectl debug -it <pod> --image=busybox --target=<container>` | Attach an ephemeral debug container to a running pod. |
| `kubectl run tmp --rm -it --image=busybox -- sh` | Spin up a throwaway pod for network/DNS testing. |
| `kubectl port-forward <pod> <local>:<remote>` | Forward a local port to a pod (also works with `svc/<name>`). |
| `kubectl cp <pod>:<path> <local_path>` | Copy files out of (or into) a pod. |
| `kubectl top pods` / `kubectl top nodes` | Show CPU and memory usage (needs metrics-server). |

<details markdown="block">
<summary>🔍 Common pod statuses & what to check...</summary>

| Status | Usual cause | Check with |
| :--- | :--- | :--- |
| `Pending` | No node can fit it (resources, taints, node selectors) or PVC not bound. | `kubectl describe pod <pod>` → Events |
| `ImagePullBackOff` / `ErrImagePull` | Wrong image name/tag or missing registry credentials. | `kubectl describe pod <pod>` |
| `CrashLoopBackOff` | The app starts and exits repeatedly. | `kubectl logs <pod> --previous` |
| `OOMKilled` | Container exceeded its memory limit. | `kubectl describe pod <pod>` → Last State |
| `CreateContainerConfigError` | Referenced ConfigMap/Secret or key doesn't exist. | `kubectl describe pod <pod>` |
| Running but not `Ready` | Readiness probe is failing. | `kubectl describe pod <pod>` → Events |

</details>

---

## 🚀 Deployments & Rollouts

| Command | Action |
| :--- | :--- |
| `kubectl apply -f <file_or_dir>` | Create or update resources declaratively. |
| `kubectl diff -f <file>` | Preview what `apply` would change. |
| `kubectl delete -f <file>` | Delete the resources defined in a file. |
| `kubectl create deploy <name> --image=<image>` | Quickly create a deployment imperatively. |
| `kubectl create deploy <name> --image=<image> --dry-run=client -o yaml > deploy.yaml` | Generate a YAML manifest without creating anything. |
| `kubectl scale deploy <name> --replicas=3` | Change the number of replicas. |
| `kubectl set image deploy/<name> <container>=<image>:<tag>` | Update a deployment's container image. |
| `kubectl rollout status deploy/<name>` | Watch a rollout until it finishes. |
| `kubectl rollout history deploy/<name>` | List previous revisions. |
| `kubectl rollout undo deploy/<name>` | Roll back to the previous revision (`--to-revision=<n>` for a specific one). |
| `kubectl rollout restart deploy/<name>` | Restart all pods of a deployment (e.g. to pick up a new ConfigMap). |
| `kubectl expose deploy <name> --port=80 --target-port=8080` | Create a Service in front of a deployment. |

---

## 🔐 ConfigMaps & Secrets

| Command | Action |
| :--- | :--- |
| `kubectl create configmap <name> --from-file=<file>` | Create a ConfigMap from a file. |
| `kubectl create configmap <name> --from-literal=KEY=value` | Create a ConfigMap from key/value pairs. |
| `kubectl create secret generic <name> --from-literal=KEY=value` | Create a Secret. |
| `kubectl create secret generic <name> --from-env-file=.env` | Create a Secret from a `.env` file. |
| `kubectl get secret <name> -o jsonpath='{.data.KEY}' \| base64 -d` | Decode a single Secret value. |

> ⚠️ Secrets are only base64-**encoded**, not encrypted. Don't commit them to git.

---

## 🖥️ Nodes & Cluster Maintenance

| Command | Action |
| :--- | :--- |
| `kubectl get nodes -o wide` | List nodes with IPs, OS and runtime versions. |
| `kubectl describe node <node>` | Show node capacity, allocated resources, and conditions. |
| `kubectl cordon <node>` | Mark a node unschedulable (existing pods keep running). |
| `kubectl drain <node> --ignore-daemonsets --delete-emptydir-data` | Evict pods from a node before maintenance. |
| `kubectl uncordon <node>` | Make a node schedulable again. |
| `kubectl auth can-i <verb> <resource>` | Check whether you have permission (e.g. `can-i delete pods`). |

---

## ⛵ Helm Quickstart

| Command | Action |
| :--- | :--- |
| `helm repo add <name> <url>` | Add a chart repository. |
| `helm repo update` | Refresh the chart index of all repositories. |
| `helm search repo <keyword>` | Search added repositories for charts. |
| `helm show values <repo>/<chart> > values.yaml` | Dump a chart's default values to customize. |
| `helm install <release> <repo>/<chart> -f values.yaml -n <ns> --create-namespace` | Install a chart with custom values. |
| `helm upgrade --install <release> <repo>/<chart> -f values.yaml` | Install or upgrade in one idempotent command (CI-friendly). |
| `helm list -A` | List releases across all namespaces. |
| `helm status <release>` | Show the status and notes of a release. |
| `helm get values <release>` | Show the values a release was installed with. |
| `helm history <release>` | List revisions of a release. |
| `helm rollback <release> <revision>` | Roll a release back to a previous revision. |
| `helm uninstall <release>` | Remove a release. |
| `helm template <release> <chart> -f values.yaml` | Render manifests locally without installing. |
| `helm create <chart_name>` | Scaffold a new chart. |
| `helm lint <chart_dir>` | Check a chart for problems. |

---

## 📄 Manifest Templates

<details markdown="block">
<summary>🔍 Minimal Deployment + Service...</summary>

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-app
  labels:
    app: my-app
spec:
  replicas: 2
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
        - name: my-app
          image: my-registry/my-app:1.0.0
          ports:
            - containerPort: 8080
          envFrom:
            - configMapRef:
                name: my-app-config
          resources:
            requests:
              cpu: 100m
              memory: 128Mi
            limits:
              memory: 256Mi
          readinessProbe:
            httpGet:
              path: /healthz
              port: 8080
---
apiVersion: v1
kind: Service
metadata:
  name: my-app
spec:
  selector:
    app: my-app
  ports:
    - port: 80
      targetPort: 8080
```

</details>
