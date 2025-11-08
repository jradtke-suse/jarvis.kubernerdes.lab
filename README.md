# Readme

This repo will provide details how I built my AI rig for personal use relying on SUSE bits.

* SLES 15 - SP7
* RKE2
* SUSE AI 1.0
* SUSE Observability (optional/future)
* SUSE Security (optional/future)

Motherboard   | Asus ROG Strix Z490-E LGA 1200 ATX Gaming Motherboard
Processor     | Intel i9-108500K 
Memory        | Corsair 128GB
Graphics Card | Asus ProArt 4060 Ti 16GB

Install SLES
- enable Software Modules
  - Basesystem
  - Python 3 Module
  - Server Applications Module
  - Desktop Applications Module (this is a choice as I use this system as a Desktop)
    - Workstation (this will be automatically selected after you choose Desktop Applications)

Update SLES
- Install "cloud-native tooling"
  - git, podman, nodejs
  - ensure the following are installed (python313, 
- Enable/Install NVIDIA bits
  - switch from open to "nv" bits (nv-prefer-signed-open-driver)
  - enable NVIDIA repos (to pull toolkit)


Install RKE2
- need to figure out persistent storage (local-path-provisioner)

# References
https://documentation.suse.com/suse-ai/1.0/html/AI-deployment-intro/index.html (Nov 6, 2025)
