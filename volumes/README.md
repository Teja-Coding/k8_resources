🚀 Kubernetes emptyDir Volume – Simple Interview Notes
This project demonstrates how two containers inside the same Pod share data using a Kubernetes emptyDir volume.

📌 What This Pod Does
The Pod contains two containers:
nginx writes logs to /var/log/nginx
almalinux reads those logs from /mnt/nginx-log (read-only)
Both containers use the same emptyDir volume, allowing them to share data.

📌 What Is emptyDir? (Interview Explanation)
emptyDir is a temporary directory created when a Pod starts.
It is deleted as soon as the Pod is removed.
Data exists only for the lifetime of the Pod.
Used to share files between containers inside the same Pod.

Good for:
Log sharing
Caching
Temporary working files
Sidecar patterns (log collectors, monitoring tools)

📌 Why Use readOnly on the Second Container?
Ensures that only nginx can write logs.
Almalinux can only read logs → prevents accidental modifications.
This is a common sidecar pattern in real deployments.

📝 Key Interview Points
emptyDir is temporary, non-persistent, and tied to the Pod lifecycle.
Used for sharing data between containers in the same Pod.
Supports sizeLimit to control disk usage.
Commonly used in sidecar logging and monitoring patterns.
Great example for demonstrating multi-container Pod communication.

▶️ How to Deploy
kubectl apply -f emptydir-pod.yaml
Check logs inside nginx container:
kubectl exec -it empty-dir-demo -c nginx -- ls /var/log/nginx
Check the same logs from the second container:
kubectl exec -it empty-dir-demo -c almalinux -- ls /mnt/nginx-log