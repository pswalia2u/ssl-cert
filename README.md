# get-cert GitHub Action

This GitHub Action generates an SSL certificate for a domain managed through Cloudflare. It updates the domain’s A record with the current IP address, requests an SSL certificate using Certbot with the Cloudflare DNS plugin, and saves the certificate in the repository.

## Prerequisites

- **Cloudflare API Token**: You need an API token with DNS edit permissions for the domain.
- **GitHub Repository Secrets**:
  - `CLOUDFLARE_API_TOKEN`: Your Cloudflare API token.
  - `REPO_TOKEN`: GitHub token with permissions to push to the repository.

## Workflow Overview

The workflow does the following:

1. **Retrieve the Current IP Address**: Fetches the current public IP and updates the Cloudflare A record for the domain.
2. **Update Cloudflare DNS Record**: Uses the Cloudflare API to get or create an A record that points the domain to the current IP.
3. **Install Certbot and Plugins**: Installs Certbot and the Cloudflare DNS plugin to facilitate certificate generation.
4. **Generate SSL Certificate**: Requests a wildcard SSL certificate for the domain using Cloudflare DNS verification.
5. **Store Certificates**: Copies the generated certificates to a `certs` directory in the repository.
6. **Commit and Push**: Commits and pushes the `certs` folder containing the SSL certificates to the repository.