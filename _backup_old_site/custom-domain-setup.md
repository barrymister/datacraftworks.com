# Setting Up a GitHub Pages Site with a Custom Domain

This guide outlines the process of configuring DNS and setting up a GitHub Pages site for a custom domain.

---

## 1. Prepare the GitHub Repository
- **Create a GitHub Repository**:
  - Name the repository after your domain (e.g., `barrymister.com` or `datacraftworks.com`) for clarity.
  - Add a `README.md` file during initialization if desired.

- **Enable GitHub Pages**:
  - Go to **Settings > Pages**.
  - Choose the `main` branch (or another branch) and set the source directory (`/root` or `/docs`).
  - Once enabled, GitHub provides a temporary domain like `https://username.github.io/repo-name/`.

- **Add Content**:
  - Create an `index.md` or `index.html` file in the repository root directory. This file becomes the homepage for your site.

---

## 2. Configure Your Domain Host (DNS Setup)
- **Access the Domain DNS Settings**:
  - Log in to your domain host (e.g., GoDaddy, Hostinger).
  - Navigate to the DNS settings for your domain (or subdomain).

- **Set Up A Records for the Apex Domain**:
  - Add four `A` records for the root domain (`@`) pointing to GitHub’s Pages IP addresses:
    - `185.199.108.153`
    - `185.199.109.153`
    - `185.199.110.153`
    - `185.199.111.153`

- **Set Up CNAME Record for Subdomains**:
  - Add a `CNAME` record for `www` (or other subdomains) pointing to `username.github.io` (replace `username` with your GitHub username).

- **Remove Conflicting Records**:
  - Delete any conflicting `A`, `CNAME`, or `AAAA` records for the same hostnames (e.g., `@` or `www`).

---

## 3. Configure GitHub for the Custom Domain
- **Set the Custom Domain**:
  - Go to your repository's **Settings > Pages**.
  - Under "Custom Domain," enter your domain (e.g., `barrymister.com` or `datacraftworks.com`).
  - GitHub will validate your DNS settings and display a green checkmark when correctly configured.

- **Enable HTTPS**:
  - Enable the "Enforce HTTPS" option to ensure all traffic is secure. GitHub will generate a free Let’s Encrypt SSL certificate for your site.

---

## 4. Test and Verify
- **Wait for DNS Propagation**:
  - DNS changes can take a few minutes to several hours to propagate globally.
  - Use tools like [DNSChecker](https://dnschecker.org/) to verify that your domain points to GitHub’s IPs or `username.github.io`.

- **Visit Your Domain**:
  - Check your custom domain (e.g., `https://datacraftworks.com`) to ensure it loads your GitHub Pages content.

---

## 5. Optional: Favicon and Additional Configurations
- **Add a Favicon**:
  - Create a `favicon.ico` (or `.png`) and upload it to your repository root.
  - Add the following `<link>` tag in your `index.html` (or equivalent layout file):
    ```html
    <link rel="icon" type="image/x-icon" href="favicon.ico">
    ```

- **Customize Further**:
  - Use Jekyll or other tools for more advanced layouts and themes.
  - Add a `_config.yml` file to configure your Jekyll theme and other options.

---

## Summary of Key Steps:
1. **GitHub Repository**:
   - Create, enable Pages, add content, set up the domain in settings.
2. **Domain DNS**:
   - Add `A` and `CNAME` records, verify no conflicts.
3. **Validation**:
   - Wait for DNS propagation, test the site.
4. **Enhancements**:
   - Add a favicon and further customize.
