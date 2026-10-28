# Test Nginx Access Through a ClusterIP Service

## Goal

Verify that a client inside the Kubernetes cluster can resolve the
Nginx Service name and retrieve the Nginx webpage through the Service.

## Environment

- Minikube cluster with one node
- Namespace: `default`
- Nginx Deployment: two replicas using `nginx:stable`
- Service: `nginx`, type `ClusterIP`
- Service selector: `app=nginx`
- Service port: `80/TCP`
- Target port: `80/TCP`

## Check the Service and Its Destination Pods

```bash
kubectl describe service nginx
kubectl get pods -l app=nginx -o wide
```

The Service endpoints matched the two Nginx Pod IPs:

| Pod | Endpoint |
|---|---|
| nginx-68b8d989c5-5pld8 | 10.244.0.3:80 |
| nginx-68b8d989c5-bzz4w | 10.244.0.4:80 |

The Service ClusterIP was `10.110.35.27`.

These addresses were observed during this test. Pod IPs may change
when Pods are replaced.

## Create a Temporary Client

```bash
kubectl run nginx-client --image=busybox:1.37 --restart=Never --command -- sleep 3600
```

This creates a standalone Pod containing BusyBox utilities.
The sleep command keeps its container running for one hour.
The Pod must be deleted separately after testing.

Check its status:

```bash
kubectl get pod nginx-client
```

The Pod reached `Running` with `1/1` containers ready.

## Request the Nginx Webpage

```bash
kubectl exec nginx-client -- wget -qO- http://nginx
```

- `exec` runs a command inside the client container.
- `--` separates kubectl arguments from the container command.
- `wget` requests the webpage.
- `-q` hides progress messages.
- `-O-` prints the response instead of saving it to a file.
- `http://nginx` uses the Service name and the default HTTP port, 80.

Because the client and Service are in the same namespace, the short
Service name `nginx` can be used.

## Observed Result

The command returned the Nginx default page, including:

```html
<h1>Welcome to nginx!</h1>
```

The test confirmed Service name resolution and a working HTTP path
from the client through the Service to an Nginx Pod.

The response did not identify which Pod answered, so this test
does not prove that both replicas served requests.

## Cleanup

```bash
kubectl delete pod nginx-client
```

The temporary client Pod was deleted after testing.

## What I Learned

- A ClusterIP Service provides an internal destination for clients.
- Its selector identifies destination Pods by their labels.
- Service port 80 forwards connections to target port 80.
- A temporary client Pod can test connectivity inside the cluster.
- Successful endpoint discovery and successful HTTP access are
  separate checks.