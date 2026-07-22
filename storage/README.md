[Storage in Kubernetes](https://kubernetes.io/docs/concepts/storage/)

# [Volumes](https://kubernetes.io/docs/concepts/storage/volumes/)

Provide a way for containers in a Pod to access and share data via the filesystem.

At its core, a volume is a directory.

## Kinds of Volumes

| Kind | Lifetime |
| ---- | -------- |
| Ephemeral | Lifetime of Pod |
| Persistent | Lifetime beyond Pod |

*For any kind of volume in a given Pod, data is preserved across container* **restarts**

## How To Use

* Specify the volumes to provide for the Pod in `.spec.volumes`
* Declare where to mount those volumes into containers in `.spec.containers[*].volumeMounts`
* *Limitations*:
    - volumes cannot mount within other volumes
    - volumes cannot contain a hard link to anything in a different volume

## Types of Volumes

* [configMap](https://kubernetes.io/docs/tasks/configure-pod-container/configure-pod-configmap/)
    - Provides a way to inject configuration data into Pods
    - Always mounted as `readOnly`
    - Text data is exposed as files using UTF-8 encoding

* [downwardAPI](https://kubernetes.io/docs/concepts/workloads/pods/downward-api/)
    - Makes downward API data available to applications
    - Data is exposed as read-only files in plain text format

* [emptyDir](https://kubernetes.io/docs/concepts/storage/volumes/#emptydir)
    - Volume created when the Pod is assigned to a node
    - When a Pod is removed from a node for any reason, the data in the emptyDir is deleted permanently
    - Ephemeral

* [fc (fibr channel)](https://kubernetes.io/docs/concepts/storage/volumes/#fc)
    - Allows an existing fibre channel block storage volume to be mounted in a Pod

* [hostPath](https://kubernetes.io/docs/concepts/storage/volumes/#hostpath)
    - Mounts a file or directory from the host node's filesystem into your Pod
    - Presents many security risks, so don't use if you can avoid

* [image](https://kubernetes.io/docs/concepts/storage/volumes/#image)
    - Represents an OCI object (a container image or artifact) which is available on the kubelet's host machine

* [iscsi](https://kubernetes.io/docs/concepts/storage/volumes/#iscsi)
    - Allows an existing `iscsi` (SCSI over IP) volume to be mounted into your Pod
    - When Pod is removed, contents are preserved and volume is unmounted
    - Can be pre-populated with data, and that data can be shared between pods
    - Cannot have multiple simultaneous writers.

* [local](https://kubernetes.io/docs/concepts/storage/volumes/#local)
    - Represents a mounted local storage device such as a disk, partition or directory
    - Can only be used as a statically created PersistentVolume
    - Dynamic provisioning is not supported

* [nfs](https://kubernetes.io/docs/concepts/storage/volumes/#nfs)
    - Allows an existing NFS (Network File System) share to be mounted into a Pod
    - When Pod is removed, contents are preserved and volume is unmounted
    - Can be pre-populated with data, and that data can be shared between pods
    - Can have multiple simultaneous writers.

* [persistentVolumeClaim](https://kubernetes.io/docs/concepts/storage/volumes/#persistentvolumeclaim)
    - Used to mount a [PersistentVolume](https://kubernetes.io/docs/concepts/storage/persistent-volumes/) into a Pod
    - A way for users to "claim" durable storage without knowing the details of the particular cloud environment

* [projected](https://kubernetes.io/docs/concepts/storage/volumes/#projected)
    - Maps several existing volume sources into the same directory

* [secret](https://kubernetes.io/docs/concepts/storage/volumes/#secret)
    - Used to pass sensitive information, such as passwords, to Pods
    - You can store secrets in the Kubernetes API and mount them as files for use by Pods
    - Backed by tmpfs (RAM filesystem), so they are never written to non-volatile storage
    - Always mounted as `readOnly`

## [Using subPath](https://kubernetes.io/docs/concepts/storage/volumes/#using-subpath)

* Specifies a sub-path inside the referenced volume instead of its root
* Use `subPathExpr` field to construct `subPath` directory names from downward API environment variables


## [csi](https://github.com/container-storage-interface/spec/blob/master/spec.md)

* Defines a standard interface for container orchestration systems to expose arbitrary storage systems to their container workloads
* An [out-of-tree volume plugin](https://kubernetes.io/docs/concepts/storage/volumes/#out-of-tree-volume-plugins)

## [Read-only Mounts](https://kubernetes.io/docs/concepts/storage/volumes/#read-only-mounts)

* Make mount read-only:
`.spec.containers[*].volumeMounts[*].readOnly: true`
    - Does not make volume itself read-only
    - That specific container will not be able to write to it
* On Linux, read-only mounts are not recursively read-only be default
    - submounts may still be writable
* Enable recursive read-only mounts:
    - Set the `RecursiveReadOnlyMounts` feature gate for kubelet and kube-apiserver
    - Set `.spec.containers[*].volumeMounts[*].recursiveReadOnly` field for a Pod
        * Allowed values are `Disabled, Enabled, IfPossible`
