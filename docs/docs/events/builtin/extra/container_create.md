
# container_create

## Intro

**container_create** - A derived event that signifies the creation of a new
*container.

## Description

The `container_create` event is intricately linked to container orchestration
and offers a precise method to monitor the initiation of containers.

Every container initiation spawns a new directory within the `cgroupfs`. By
leveraging the `cgroup_mkdir` event and examining the metadata within the
`cgroupfs` subdirectories, the `container_create` event is able to discern if
the new directory corresponds to a freshly instantiated container.

As a result, it gathers detailed information about the container, including its
runtime, image details, and related pod data. This gives a clear view of how
containers are set up and run within the system.

## Arguments

- **runtime** (`const char*`): The container runtime used (e.g., Docker, containerd, etc.).
- **container_id** (`const char*`): The unique identifier for the container.
- **ctime** (`unsigned long`): Creation timestamp of the container.
- **container_image** (`const char*`): Image used to create the container.
- **container_image_digest** (`const char*`): Digest of the container image.
- **container_name** (`const char*`): Name of the container.
- **pod_name** (`const char*`): Name of the pod that this container belongs to (if applicable).
- **pod_namespace** (`const char*`): Namespace of the pod.
- **pod_uid** (`const char*`): Unique identifier for the pod.
- **pod_sandbox** (`bool`): Indicates if the pod is acting as a sandbox.

## Derivation Logic

The `container_create` event is derived from the `cgroup_mkdir` event. First, it
checks if the cgroup event belongs to a container root directory being created.
It then uses the `cgroup_id`, from the cgroup directory inode, to retrieve
container-specific information (enriched by tracee when talking to runtime
daemons).

## Example Use Case

1. Security Monitoring: Detecting the creation of unexpected or malicious containers.
2. Compliance Audits: Ensuring that only approved container images are used in production environments.
3. Performance Monitoring: Identifying newly created containers that may consume significant resources.

## Related Events

- cgroup_mkdir: The primary event from which `container_create` is derived. It provides details about the creation of cgroup directories.

> Note: This document was generated by OpenAI with a human review process.