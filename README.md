# KAC Sample Detections

Sample Kubernetes manifests that trigger the default rules in the CrowdStrike Falcon Kubernetes Admission Controller (KAC). Use them to demo KAC detections to a customer.

Each file in `rules/` is a hardened Pod (or a Service for 106569 and 106570) with one deliberate violation, aimed at a single rule. Some rules can't be triggered in isolation, so a few files also fire related rules. The header comment in each file names the target rule, what triggers it, and any other rules that also fire.

## Usage

```sh
kubectl apply -f namespace.yaml
kubectl apply -f rules/106557-privileged-container.yaml
```

- In prevent mode KAC blocks the request at admission. In visibility-only mode the workload is admitted and appears as a detection in the Falcon console.
- All manifests use the `kac-demo` namespace except 106575, which uses `default` on purpose.
- Clean up with `kubectl delete namespace kac-demo`.

## Caveats

- 106564 (unsafe procMount) needs a cluster with the `ProcMountType` and `UserNamespacesSupport` features enabled.
- 106569 (LoadBalancer) creates a real cloud load balancer if admitted.
- 106580 (no securityContext) also fires several other rules.
- The manifests are deliberately insecure. Don't run them on a production cluster.
