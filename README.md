# Docker MIG Manager

Unofficial minimal docker instructions for managing NVIDIA Multi-Instance GPU (MIG) in containers.

Prerequisites:
- [NVIDIA Driver](https://ubuntu.com/server/docs/nvidia-drivers-installation)
- [Docker](https://docs.docker.com/engine/install/ubuntu/)
- [NVIDIA Container Toolkit](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html)

Run:

```sh
docker run --rm -it --gpus all \
    --cap-add=SYS_ADMIN \
    -e NVIDIA_MIG_CONFIG_DEVICES=all \
    ubuntu
# in the container
nvidia-smi mig -cgi 9,3g.20gb -C
nvidia-smi mig -dci && nvidia-smi mig -dgi
```

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

Thanks **Hsu-Tzu Ting** for discussions.
