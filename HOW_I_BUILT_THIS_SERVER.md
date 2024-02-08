##How I Built My PalWorld Dedicated Server on ARM64 Architecture

I initially encountered stability issues running PalWorld servers on my IDC server, possibly due to resource limitations. To explore alternative hosting options, I attempted to deploy the server on various platforms, including Raspberry Pi 4 with ARM64 architecture.

After researching ARM64-compatible solutions, I discovered the feasibility of hosting on Oracle Cloud's ARM servers, leveraging FEX-Emu to emulate x86-64 environments. This led me to a GitHub repository (https://github.com/nitrog0d/palworld-arm64) containing valuable resources for running PalWorld servers on ARM64.

However, attempting to deploy directly on Raspberry Pi box86/box64 proved challenging due to the need for manual compilation. Subsequently, I turned to Dockerized SteamCMD solutions tailored for ARM64, such as (https://github.com/TeriyakiGod/steamcmd-docker-arm64), which operated successfully on Ubuntu 22.04.

With the infrastructure in place, I tackled the task of configuring the PalWorld Dedicated Server within the Docker environment. Leveraging Docker Compose, I established volume mappings for data persistence and ensured that the necessary ports were accessible.

While SteamCMD installation succeeded within the Docker container, I encountered issues with missing dependencies, particularly the steamclient.so file. Symbolic links from relevant directories to ~/.steam/sdk64 and ~/.steam/sdk32 partially resolved the issue, but further investigation revealed additional library dependencies located in /lib/x86_64-linux-gui, necessitating similar linking.

Despite these challenges, I persisted in refining the Docker setup. Drawing inspiration from existing Docker images (https://github.com/jammsen/docker-palworld-dedicated-server), I integrated automated environment configuration via environment variables, implemented scheduled backups using Supercronic, and ensured seamless server updates through SteamCMD.

Finally, I crafted a Docker entrypoint script to orchestrate these processes, ensuring smooth deployment and operation of the PalWorld Dedicated Server on ARM64 architecture. The resulting Docker image is available on Docker Hub (https://hub.docker.com/r/charliejadek/palworld-arm64-dedicated-server), offering a streamlined deployment process for enthusiasts to host their own PalWorld servers.

For detailed instructions on installation and usage, refer to the accompanying documentation (how-to-made-this-git.md) on GitHub. Feel free to explore the server hosted on my IDC at palserver.4scour.com:8211.
