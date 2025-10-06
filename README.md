# DigitalOcean Droplet Deploy GitHub Action

This GitHub Action deploys a **DigitalOcean Droplet** with Docker and an **Nginx** container on ports 80 and 443. It automatically creates free Let’s Encrypt TLS certificates and manages DNS records in DigitalOcean.

<br>
<p>
  <a href="https://www.digitalocean.com">
    <img src="https://opensource.nyc3.cdn.digitaloceanspaces.com/attribution/assets/PoweredByDO/DO_Powered_by_Badge_blue.svg" alt="Powered by DigitalOcean" width="200px" />
  </a>
</p>

## Features

- Deploy a droplet from GitHub Actions with **one click**
- Uses `doctl` and a `cloud-init.yml` file
- Provisioned with **cloud-init**
  - Installs Docker & Docker Compose
  - Installs Certbot and issues a valid Let’s Encrypt certificate
  - Waits until Docker is ready
  - Launches an **nginx** container on ports 80 and 443
  - Serves HTTPS using the Let’s Encrypt cert
  - Auto-renews certs and restarts Nginx after renewal
- Auto-creates or updates a **DigitalOcean DNS A record** for your domain/subdomain
- Works with defaults but allows overriding region, size, image, and hostname

## Requirements

- A **DigitalOcean account** with:
  - An existing **SSH key** already uploaded (you’ll provide its ID in the secrets).
- A **domain managed in DigitalOcean DNS**:
  - The GitHub Action automatically creates or updates an **A record** for your chosen subdomain.
  - Your domain must already be added to [DigitalOcean’s DNS control panel](https://cloud.digitalocean.com/networking/domains).
  - If your domain is currently managed elsewhere (Cloudflare, registrar DNS, etc.), you must first point your domain’s nameservers to DigitalOcean:
    ```txt
    ns1.digitalocean.com
    ns2.digitalocean.com
    ns3.digitalocean.com
    ```
- A **GitHub repository fork** with required secrets set up (see [Secrets Setup](#2-secrets-setup)).

## Usage

### 1. Fork the Repository

1. Click the **Fork** button at the top of the GitHub repository page.
2. Once forked you can use it directly from your account.

### 2. Secrets Setup

You'll need to create the following secrets in your forked repository for the action to work:

1. **`DIGITALOCEAN_ACCESS_TOKEN`**: Your DigitalOcean API token. You can generate it in your DigitalOcean API settings.
    1. Create A New Personal Access Token and give the token a name.
    2. Choose **Custom Scopes** then select the following granular scopes:
       1. `ssh_key:read`
       2. `droplet:read,create`
       3. `domain:read,create,update`
2. **`DIGITALOCEAN_SSH_KEY_ID`**: The DigitalOcean SSH key ID you'd like to use. List your SSH key IDs with ```doctl compute ssh-key list```.

To add these secrets:

1. Go to your forked repository on GitHub.
2. Navigate to **Settings** > **Secrets and variables** > **Actions**.
3. Click **New repository secret** and add `DIGITALOCEAN_ACCESS_TOKEN` then `DIGITALOCEAN_SSH_KEY_ID`.

### 3. Run the Included Workflow

After forking this repository into your own GitHub account:

1. Go to your forked repo’s **Actions** tab.
2. Select **Deploy Droplet to DigitalOcean**.  
3. Click **Run workflow** and fill in the inputs:
   - `domain` - Root domain (example.com).
   - `subdomain` - Subdomain (e.g., app, www, test). Leave blank for root.
   - `hostname` - Droplet hostname.
   - Optionally override region, size, or image.
4. Wait for job to complete.
5. Your droplet will be live with:
   - Nginx serving HTTP → HTTPS redirect.
   - Valid HTTPS certificate from Let’s Encrypt.
   - Auto-renew enabled.
   - Nginx running, accessible via http(s)://subdomain.domain.

### Alternate Usage: Use as a GitHub Action in Your Own Workflow

Instead of forking, you can also use this as an Action directly in your own workflows:

```yaml
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Deploy Droplet
        uses: marksowell/DigitalOcean-Droplet-Deploy@v1
        with:
          domain: example.com
          subdomain: app
          hostname: app-server
        env:
          DIGITALOCEAN_ACCESS_TOKEN: ${{ secrets.DIGITALOCEAN_ACCESS_TOKEN }}
          DIGITALOCEAN_SSH_KEY_ID: ${{ secrets.DIGITALOCEAN_SSH_KEY_ID }}
```

## Contributing

Contributions are welcome! Please open an issue or submit a pull request for any improvements or bug fixes.

## License

This project is licensed under the MIT License. See the [LICENSE](./LICENSE) file for details.
