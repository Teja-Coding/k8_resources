🚀 Kubernetes emptyDir Volume – Interview-Ready Notes

This project demonstrates how two containers inside the same Pod share data using a Kubernetes emptyDir volume — a common interview topic and practical real-world pattern.

📘 Understanding the Concept
🧩 What This Pod Does

The Pod contains two containers working together:

Container	Purpose	Path Mounted	Access
nginx	Writes log files	/var/log/nginx	Read-Write
almalinux	Reads nginx logs	/mnt/nginx-log	Read-Only

Both containers share the same emptyDir volume, enabling inter-container communication.

📂 Architecture Diagram

📌 This diagram visually represents how emptyDir is shared between the two containers.

📦 What Is emptyDir? (Interview Explanation)

emptyDir is a temporary storage volume created when a Pod starts.

✔ Exists only while the Pod is running
✔ Deleted automatically when the Pod stops
✔ Stored on node local storage
✔ Used for sharing files between containers in a Pod

🛠 Common Use Cases

Log sharing between containers

Caching temporary data

Scratch space for processing

Sidecar logging / monitoring agents

🔒 Why Use readOnly for the Second Container?

Ensures only nginx writes logs

Prevents accidental file modification

Cleaner separation of responsibilities

Matches real-world sidecar patterns (e.g., log collectors)

📝 Key Interview Takeaways

emptyDir is non-persistent and tied to the Pod lifecycle

Designed for data sharing between containers within the same Pod

Supports sizeLimit for storage control

Frequently used with logging sidecars

Great example to explain multi-container Pod architecture

🚀 Deploying the Demo
1️⃣ Apply the Pod YAML
kubectl apply -f emptydir-pod.yaml

2️⃣ Check logs inside the nginx container
kubectl exec -it empty-dir-demo -c nginx -- ls /var/log/nginx

3️⃣ Verify the same logs from the almalinux container
kubectl exec -it empty-dir-demo -c almalinux -- ls /mnt/nginx-log