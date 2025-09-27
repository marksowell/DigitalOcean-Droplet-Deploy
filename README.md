# DigitalOcean Droplet Template with Cloud-init, Docker, and DNS via GitHub Actions

This project automates provisioning a DigitalOcean droplet, bootstrapping it with Docker + a test container on ports **80/443**, and creating/updating DNS records. It uses **GitHub Actions** with `doctl` and a `cloud-init.yml` file.

## Features

- Deploy a droplet from GitHub Actions with **one click**
- Provisioned with **cloud-init**
  - Installs Docker & Docker Compose
  - Waits until Docker is ready
  - Launches an **nginx** container on ports 80 and 443
- Auto-creates or updates a **DigitalOcean DNS A record** for your domain/subdomain
- Works with defaults but allows overriding region, size, image, and hostname

## Usage

### 1. Fork the Repository

1. Click the **Fork** button at the top of the GitHub repository page.
2. Once forked you can use it directly from your account.

### 2. Secrets Setup

You'll need to create the following secrets in your forked repository for the action to work:

1. **`DIGITALOCEAN_ACCESS_TOKEN`**: Your DigitalOcean API token. You can generate it in your DigitalOcean API settings.
    1. Create A New Personal Access Token and give the token a name.
    2. Choose **Custom Scopes** then the following granular scopes are needed:
       1. ssh_key - read
       2. droplet - read, create
       3. domain - read, create, update
2. **`DIGITALOCEAN_SSH_KEY_ID`**: The DigitalOcean SSH key ID you'd like to use. List your SSH key IDs with ```doctl compute ssh-key list```.

To add these secrets:

1. Go to your forked repository on GitHub.
2. Navigate to **Settings** > **Secrets and variables** > **Actions**.
3. Click **New repository secret** and add `DIGITALOCEAN_ACCESS_TOKEN` then `DIGITALOCEAN_SSH_KEY_ID`.

## Creating a Droplet from GitHub Actions

Run from GitHub Actions:

1. Go to Actions → Deploy Droplet → Run workflow
2. Fill in inputs (domain, subdomain, etc.)
3. Wait for job to complete
4. Your droplet will be live with Nginx running, accessible via http(s)://subdomain.domain

## Contributing

Contributions are welcome! Please open an issue or submit a pull request for any improvements or bug fixes.

## License

This project is licensed under the MIT License. See the [LICENSE](./LICENSE) file for details.