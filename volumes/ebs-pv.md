# 📄 Documentation for `03_ebs_pv.yaml`

This documentation explains the `03_ebs_pv.yaml` file. The file is a **Kubernetes manifest** for creating a PersistentVolume using an existing AWS EBS (Elastic Block Store) volume. The configuration ensures that Kubernetes can use this EBS volume as storage for pods in your cluster.

---

## What Does This File Do?

- **Defines a PersistentVolume (PV) resource** in Kubernetes.
- Binds an existing AWS EBS volume to the Kubernetes cluster.
- Sets the storage, access mode, and reclaim policy for the volume.
- Uses the AWS EBS Container Storage Interface (CSI) driver.

---

## YAML Structure Breakdown

Here's a breakdown of the key fields in this YAML:

| Field                          | Description                                                                                          |
| ------------------------------ | ---------------------------------------------------------------------------------------------------- |
| `apiVersion`                   | API version (`v1`) for Kubernetes core resources.                                                    |
| `kind`                         | Resource type (`PersistentVolume`).                                                                  |
| `metadata.name`                | The name of this PV (`ebs-static`).                                                                  |
| `spec.capacity.storage`        | The volume size (`15Gi`).                                                                            |
| `spec.accessModes`             | Access policy (`ReadWriteOnce`: only one node can mount at a time).                                  |
| `spec.PersistentVolumeReclaimPolicy` | What happens to the volume after PVC deletion (`retain`: keeps data and volume).            |
| `spec.csi.driver`              | The name of the CSI driver (`ebs.csi.aws.com` for AWS EBS).                                          |
| `spec.csi.fsType`              | File system type (here, `ext4`).                                                                    |
| `spec.csi.volumeHandle`        | AWS EBS volume ID to use (`vol-0fc2f30bedd19f958`).                                                 |

---

## The YAML Code

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: ebs-static
spec:
  accessModes:
    - ReadWriteOnce
  PersistentVolumeReclaimPolicy: retain
  capacity:
    storage: 15Gi
  csi:
    driver: ebs.csi.aws.com
    fsType: ext4
    volumeHandle: vol-0fc2f30bedd19f958
```

---

## How Does It Work?

- **Provisioning**: This file does not dynamically create a new EBS volume. It uses an already-created AWS EBS volume (`vol-0fc2f30bedd19f958`).
- **Mounting**: Only one Kubernetes node can mount this volume at a time (`ReadWriteOnce`), making it suitable for single-node write scenarios.
- **Reclaim Policy**: When its associated PersistentVolumeClaim (PVC) is deleted, the EBS volume remains in AWS; Kubernetes does not delete it. This is useful if you want to keep the data for manual recovery.
- **CSI Driver**: The `ebs.csi.aws.com` driver integrates AWS EBS volumes with Kubernetes using the standardized CSI interface.

---

## Typical Usage Scenario

- You have an **existing AWS EBS volume** (for example, with preloaded data).
- You want Kubernetes workloads (pods) to use this EBS volume for persistent storage.
- You want to retain the EBS volume and its data even when the Kubernetes PVC is deleted.

---

## Interview Points (For `read.md`)

> Use these points to explain or answer questions in interviews. The language is simple and clear.

- **What is this file for?**
  - It creates a PersistentVolume in Kubernetes using an existing AWS EBS disk.
- **What is a PersistentVolume (PV)?**
  - A PV is a piece of storage in the cluster that can be used by pods.
- **What is special about this PV?**
  - It uses an existing AWS EBS volume, not a new one.
- **What does `ReadWriteOnce` mean?**
  - Only one node in the cluster can use this PV at a time.
- **What does the `retain` policy mean?**
  - The EBS volume is not deleted when the claim is removed; the data stays.
- **Why use the CSI driver?**
  - The CSI driver lets Kubernetes talk to AWS EBS and handle storage.
- **Can this PV be used by many pods?**
  - No, just one pod at a time (unless all pods are on the same node).
- **What is the `volumeHandle`?**
  - It is the unique ID of the AWS EBS volume that Kubernetes will use.

---

## Storage Integration Diagram

The following diagram shows how the EBS volume integrates with Kubernetes using this PV definition:

```mermaid
flowchart TD
  subgraph AWS Cloud
    EBS[EBS Volume vol-0fc2f30bedd19f958]
  end
  subgraph Kubernetes Cluster
    PV[PersistentVolume ebs-static]
    PVC[PersistentVolumeClaim (user request)]
    Pod[Pod needing storage]
  end

  EBS -->|Provisioned via CSI Driver| PV
  PV -->|Bound to| PVC
  PVC -->|Mounted by| Pod
```

---

## Summary Table

| Component              | Value / Description                               |
|------------------------|---------------------------------------------------|
| Storage Type           | AWS EBS Volume                                    |
| CSI Driver             | ebs.csi.aws.com                                   |
| Volume Size            | 15Gi                                              |
| File System            | ext4                                              |
| Access Mode            | ReadWriteOnce (RWO)                               |
| Reclaim Policy         | retain                                            |
| Volume ID              | vol-0fc2f30bedd19f958                             |

---

```card
{
  "title": "Best Practice",
  "content": "Always use the 'retain' reclaim policy for critical data volumes to avoid accidental deletion."
}
```

---

## Simple Explanation

- This file lets Kubernetes use an AWS EBS disk for pod storage.
- Only one node can write to this disk at a time.
- The disk and data are kept safe, even if the Kubernetes claim is deleted.
- The disk is connected using the AWS EBS CSI storage driver.

---

Feel free to use these points and the diagram in your interviews, documentation, or as reference for similar Kubernetes storage tasks!