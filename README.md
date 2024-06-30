# Docker Compose Setup for Plex with Plex_Debrid and Rclone_RD through VPN

This project sets up a Docker Compose environment for hosting several services together, including Plex, Plex_Debrid, and Rclone_RD. The setup ensures that Plex_Debrid and Rclone_RD are routed through a VPN using Gluetun, while Plex communicates internally without the VPN.

## Services Included
- **[Plex](https://www.plex.tv/)**: A media server that organizes and streams your media.
- **[Plex_Debrid](https://github.com/itsToggle/plex_debrid)**: Enhances Plex by integrating it with Real-Debrid.
- **[Rclone_RD](https://github.com/itsToggle/rclone_RD)**: Mounts Real-Debrid files as a virtual FUSE filesystem.
- **[Gluetun](https://github.com/qdm12/gluetun)**: A VPN client that routes specific services through a VPN.

## Features
- **Container Networking**: Uses Docker's networking capabilities to allow communication between containers.
- **VPN Routing**: Plex_Debrid and Rclone_RD are routed through Gluetun VPN.
- **Custom Configuration**: Easily customizable Gluetun and Rclone_RD configurations.
- **Persistent Storage**: Uses volumes to ensure data persistence across container restarts.

## Prerequisites
- Docker and Docker Compose installed on your system.
- Real-Debrid account for accessing remote files.
- Plex account for media management.
- VPN credentials compatible with Gluetun (e.g., [Mullvad](https://mullvad.net/)).
## Setup Instructions

### 1. Customize Gluetun Configuration
Update the Gluetun environment variables in the Docker Compose file to match your VPN provider's requirements. Example:
```yaml
environment:
  - PUID=1000
  - PGID=1000
  - TZ=Europe/Amsterdam
  - FIREWALL_OUTBOUND_SUBNETS=172.18.0.0/16
  - DNS_KEEP_NAMESERVER=on
  - DOT=off
  # Add specific VPN config here
```

### 2. Prepare Rclone_RD Configuration
It's recommended to set up the Rclone_RD configuration on another computer and copy the config files to the designated directory. Alternatively, you can launch the container interactively for setup

Copy the generated config to the `rclone/config` directory specified in the volumes section of the Docker Compose file.

### 3. Deploy the Services
Navigate to the directory containing your Docker Compose file and run:
```sh
docker-compose up -d
```
This command will start all the services in detached mode.

### 4. Access Plex
Once the containers are up and running, you can access Plex by navigating to `http://<your-server-ip>:32400/web` in your browser.

## Additional Notes
- Ensure the PUID and PGID match the user on your host system to avoid permission issues.
- Update the `PLEX_CLAIM` variable with your own claim token from Plex if needed.
- The containers are set to restart unless stopped, ensuring they come back up after a reboot.

## Troubleshooting
- **Network Issues**: Verify the subnet and IP address configurations to avoid conflicts.
- **Permission Errors**: Check that the user IDs match and the necessary permissions are set on the host directories.
- **VPN Configuration**: Ensure the Gluetun configuration matches your VPN provider's requirements.

## Contributing
Feel free to submit issues or pull requests if you have improvements or fixes.

## License
This project is licensed under the MIT License.