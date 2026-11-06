# Lesson 14: Observe Nginx Service Traffic Distribution

## Goal

Identify which Nginx replica handles each request sent through the
nginx ClusterIP Service.

The previous lesson verified Service DNS and retrieved the Nginx
welcome page, but did not identify which replica answered.

## Follow Logs from Both Replicas

```bash
kubectl logs -l app=nginx --all-containers --prefix=true --tail=0 -f
```

- `logs` displays container logs.
- `-l app=nginx` selects Pods labelled app=nginx.
- `--all-containers` includes all containers in the selected Pods.
- `--prefix=true` identifies the Pod and container on each log line.
- `--tail=0` skips existing log lines.
- `-f` follows new log output until interrupted with Ctrl+C.

This allowed requests handled by the two replicas to be distinguished
in one terminal.

## Send Repeated Requests

From another terminal, the following command was run repeatedly:

```bash
kubectl exec nginx-client -- wget -qO /dev/null http://nginx
```

- `exec nginx-client` runs a command inside the client Pod's container.
- `--` separates kubectl arguments from the container command.
- `wget` sends an HTTP request.
- `-q` hides progress messages.
- `-O /dev/null` sends the downloaded response body to /dev/null,
  which discards the data.
- `http://nginx` addresses the Service by its DNS name on HTTP port 80.

Each separate wget invocation creates a new connection.
Discarding the response body does not prevent the request from
reaching Nginx or appearing in its access log.

## Observed Results

The pasted log sample contained 15 requests:

| Nginx Pod | Requests handled |
|---|---:|
| nginx-68b8d989c5-5pld8 | 9 |
| nginx-68b8d989c5-bzz4w | 6 |
| Total | 15 |

Every response in this sample had HTTP status 200.

Nginx recorded the client source address as 10.244.0.6.

These counts describe the observed sample, rather than a guaranteed
distribution for future requests.

## What This Demonstrated

- Prefixed logs identify which Pod handled a request.
- Requests through the Service reached both Nginx replicas.
- Requests did not need to alternate between replicas.
- The observed distribution was not exactly equal: 9 versus 6.
- /dev/null discarded the response bodies while access logs provided
  evidence of the requests and response status.

## Cleanup Status

Deletion of the nginx-client Pod was requested.

The Pod was subsequently observed in Terminating.
Its final removal had not yet been confirmed when these notes
were prepared.

Possible reasons for termination delays were discussed, but no
troubleshooting cause was established. No diagnosis or fix is
claimed in this lesson.