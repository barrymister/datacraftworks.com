# Cloudflare Hosting and Reverse Proxy Setup Checklist

## 1. Hosting Custom Domains with MFA for Private Sections
- [ ] **Register Custom Domains with Cloudflare**
  - [ ] Add your domain to Cloudflare.
  - [ ] Update DNS nameservers to point to Cloudflare.
  - [ ] Configure DNS records for the domain.

- [ ] **Set Up Cloudflare Pages**
  - [ ] Create a GitHub repository for your website.
  - [ ] Connect the repository to Cloudflare Pages.
  - [ ] Deploy your site.

- [ ] **Enable MFA for Private Sections**
  - [ ] Navigate to the Zero Trust dashboard in Cloudflare.
  - [ ] Configure an Access Policy for private paths.
  - [ ] Set up authentication methods (e.g., Google, GitHub).
  - [ ] Define access rules (e.g., emails or user groups).

---

## 2. Migrating from No-IP to Cloudflare for DDNS
- [ ] **Enable Dynamic DNS**
  - [ ] Create a Cloudflare API token with DNS edit permissions.
  - [ ] Install a DDNS client (e.g., `cloudflare-ddns`) or write a script.
  - [ ] Configure the client to update Cloudflare DNS records dynamically.

- [ ] **Update Devices**
  - [ ] Replace No-IP hostname with your Cloudflare domain.
  - [ ] Verify DDNS client updates your public IP correctly.

---

## 3. Integrating Traefik Reverse Proxy Behind Cloudflare
- [ ] **Set Up Cloudflare Proxy**
  - [ ] Enable proxying (orange cloud) for relevant DNS records in Cloudflare.
  - [ ] Configure caching and security settings as needed.

- [ ] **Configure Traefik**
  - [ ] Obtain Cloudflare’s IP ranges.
  - [ ] Update Traefik to whitelist Cloudflare IPs.
  - [ ] Configure middleware to enforce IP whitelisting.

- [ ] **Enable TLS**
  - [ ] Use Cloudflare’s SSL/TLS feature and enable HTTPS for your domain.
  - [ ] Select **Full (strict)** mode for end-to-end encryption.
  - [ ] Install certificates for secure communication between Cloudflare and Traefik.

- [ ] **Configure Subdomains**
  - [ ] Create subdomains in Cloudflare for each self-hosted service.
  - [ ] Update Traefik routes to handle subdomain traffic.

---

## 4. Combined Architecture Testing
- [ ] Verify Cloudflare is serving public-facing pages.
- [ ] Test private sections for MFA enforcement via Cloudflare Access.
- [ ] Ensure DDNS updates correctly reflect your public IP.
- [ ] Verify Traefik routes traffic correctly behind Cloudflare.
- [ ] Confirm SSL/TLS security using Cloudflare’s strict mode.

---

## Notes
- Track progress by marking tasks as complete.
- Adjust based on specific requirements or changes to your setup.
