# Docker MIG Manager

Unofficial minimal docker instructions for managing NVIDIA Multi-Instance GPU (MIG) in containers.

Prerequisites:

- [NVIDIA Driver](https://ubuntu.com/server/docs/nvidia-drivers-installation)
- [Docker](https://docs.docker.com/engine/install/ubuntu/)
- [NVIDIA Container Toolkit](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html)
- [Enable MIG Mode](https://docs.nvidia.com/datacenter/tesla/mig-user-guide/#enable-mig-mode)

Take A100 as an example, run:

```sh
docker run --rm -it --gpus all \
    --cap-add=SYS_ADMIN \
    -e NVIDIA_MIG_CONFIG_DEVICES=all \
    ubuntu

# in the container
# Create two `3g.20gb` GPU instances (GI) and corresponding compute instances (CI)
nvidia-smi mig -cgi 9,3g.20gb -C
# List the available CIs and GIs
nvidia-smi mig -lgi; nvidia-smi mig -lci;
# Destroy all the CIs and GIs
nvidia-smi mig -dci; nvidia-smi mig -dgi;
```

This should also work on A30/H100/H200 by substituting the MIG profile to [a supported one](https://docs.nvidia.com/datacenter/tesla/mig-user-guide/#supported-mig-profiles).

Note: `--runtime=nvidia`, `-e NVIDIA_VISIBLE_DEVICES=all`, and `-e NVIDIA_DRIVER_CAPABILITIES=all` may be required depending on your environment and use cases.

## References

Some references I found useful during the investigation.

- [MIG User Guide](https://docs.nvidia.com/datacenter/tesla/mig-user-guide/)
  - [9.4. Creating GPU Instances](https://docs.nvidia.com/datacenter/tesla/mig-user-guide/#creating-gpu-instances)
  - [9.6. Destroying GPU Instances](https://docs.nvidia.com/datacenter/tesla/mig-user-guide/#destroying-gpu-instances)
  - [10. Device Nodes and Capabilities](https://docs.nvidia.com/datacenter/tesla/mig-user-guide/#device-nodes-and-capabilities)
- [The NVIDIA Container Runtime](https://github.com/NVIDIA/nvidia-container-toolkit/tree/main/cmd/nvidia-container-runtime#nvidia_mig_config_devices)
  - [`NVIDIA_MIG_CONFIG_DEVICES`](https://github.com/NVIDIA/nvidia-container-toolkit/tree/main/cmd/nvidia-container-runtime#nvidia_mig_config_devices)
- [NVIDIA MIG Manager For Kubernetes](https://catalog.ngc.nvidia.com/orgs/nvidia/teams/cloud-native/containers/k8s-mig-manager/layers)

## Acknowledgement

Thanks [@Irene-Ting](https://github.com/Irene-Ting) for discussions.
