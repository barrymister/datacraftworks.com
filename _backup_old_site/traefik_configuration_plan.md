
# Plan for Configuring Reverse Proxy and Secure Certificates with Traefik

## Checklist

### 1. Prepare the Environment
- [ ] Verify Traefik is installed and running in Docker within Portainer.
- [ ] Confirm your DNS records are correctly configured and resolving to your public IP:
  - `pve.kq4tnv.net` for Proxmox.
  - `ptn.kq4tnv.net` for Portainer.
  - `tfk.kq4tnv.net` for Traefik.
  - `urf556.kq4tnv.net` for URF Reflector.
- [ ] Ensure ports 80 (HTTP) and 443 (HTTPS) are forwarded from the router to the Ubuntu VM (192.168.1.87) for Traefik.

### 2. Reconfigure Apache for the URF Reflector
- [ ] Update the Apache configuration for the URF Reflector dashboard to listen on a different internal port (e.g., 8081).
- [ ] Test the URF dashboard on `http://192.168.1.161:8081` to confirm functionality.

### 3. Configure Traefik
- [ ] Update the Traefik `traefik.yml` or Docker Compose stack to include:
  - [ ] HTTP to HTTPS redirection.
  - [ ] Let's Encrypt resolver for automatic certificate generation.
  - [ ] Dynamic routing rules for reverse proxying.
- [ ] Create or update the `dynamic.yml` file for Traefik's dynamic configuration to define services and routing rules for all web UIs.

### 4. Create Individual Traefik Services for WebUIs
- [ ] Define a service and router in the dynamic configuration for Proxmox WebUI (`pve.kq4tnv.net`):
  - [ ] Map domain `pve.kq4tnv.net` to Traefik and forward to Proxmox's internal IP `192.168.1.223:8006`.
- [ ] Define a service and router in the dynamic configuration for Portainer WebUI (`ptn.kq4tnv.net`):
  - [ ] Map domain `ptn.kq4tnv.net` to Traefik and forward to Portainer's internal IP `192.168.1.87:9000`.
- [ ] Define a service and router in the dynamic configuration for Traefik WebUI (`tfk.kq4tnv.net`):
  - [ ] Map domain `tfk.kq4tnv.net` to Traefik itself.
- [ ] Define a service and router in the dynamic configuration for the URF Reflector WebUI (`urf556.kq4tnv.net`):
  - [ ] Map domain `urf556.kq4tnv.net` to Traefik and forward to the URF Reflector's internal IP `192.168.1.161:8081`.

### 5. Test Reverse Proxy Setup
- [ ] Validate that each domain points to the correct service through Traefik:
  - [ ] `pve.kq4tnv.net` for Proxmox.
  - [ ] `ptn.kq4tnv.net` for Portainer.
  - [ ] `tfk.kq4tnv.net` for Traefik WebUI.
  - [ ] `urf556.kq4tnv.net` for the URF Reflector WebUI.
- [ ] Confirm that HTTPS is automatically used for each domain and certificates are issued by Let's Encrypt.

### 6. Update and Test Apache on URF Reflector
- [ ] Remove the standalone Let's Encrypt configuration in Apache for the URF Reflector, as Traefik will handle certificates.
- [ ] Verify the URF dashboard works through the Traefik reverse proxy (`https://urf556.kq4tnv.net`).

### 7. Document and Monitor
- [ ] Document the final configuration for each service and domain for future reference.
- [ ] Set up Traefik's monitoring (optional) to observe metrics and logs.
