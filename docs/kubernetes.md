# Kubernetes Workshop – Session 04

## 1. Deployment verification

Command:

```bash
kubectl get pods -o wide
```

Output:

```
NAME                         READY   STATUS    RESTARTS   AGE    IP           NODE
generator-84598b7f89-tjm9q   1/1     Running   0          4m3s   10.20.0.19   gke-freight-main-fa960425-3wgz
```

## 2. Deployment rollout history

Command:

```bash
kubectl rollout history deployment/generator
```

Output:

```
REVISION  CHANGE-CAUSE
2         <none>
3         <none>
```

## 3. Kubernetes self-healing

When I deleted the generator pod, Kubernetes detected that the number of running pods was below the desired replica count. The Deployment controller automatically created a replacement pod. The replacement reached the Running state, demonstrating Kubernetes self-healing.

## 4. Rolling update and rollback

I changed the generator configuration from MODE=both and RATE=1 to MODE=air and RATE=5.

Kubernetes successfully performed a rolling update. The logs showed AIR events.

I then used `kubectl rollout undo deployment/generator` to roll back to the previous revision. The logs showed both AIR and OCEAN events again.

Finally, I restored the original configuration: MODE=both and RATE=1.
