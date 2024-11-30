# **get-cert-cloudflare GitHub Action**

This GitHub Action automates the process of managing SSL certificates for a domain hosted on Cloudflare. It retrieves the current public IP, updates the domain’s A record, generates SSL certificates using Certbot and Cloudflare’s DNS plugin, and securely saves the certificates to the repository.

## **Features**

- Fetches the public IP address dynamically.
- Updates the Cloudflare DNS A record for the domain.
- Generates wildcard SSL certificates for the domain.
- Stores the SSL certificates securely in the repository.
- Fully automated and configurable via workflow inputs and secrets.

## **Requirements**

### **Prerequisites**
1. **Cloudflare API Token**:  
   - Must have DNS edit permissions for the target domain.
2. **GitHub Repository Secrets**:  
   - `CLOUDFLARE_API_TOKEN`: Your Cloudflare API token.  
   - `REPO_TOKEN`: GitHub token with permissions to push to the repository.

### **Tools Installed During the Workflow**
- **Certbot**: For generating SSL certificates.
- **Certbot Cloudflare DNS Plugin**: For DNS-based certificate validation.

## **How It Works**

1. **Retrieve Current Public IP**:  
   The workflow fetches the public IP address using `curl` and prepares it for the DNS update.

2. **Update Cloudflare DNS Record**:  
   The A record for the domain is updated (or created) to point to the current public IP.

3. **Install Certbot and Cloudflare Plugin**:  
   Certbot and its Cloudflare DNS plugin are installed to automate the SSL certificate generation process.

4. **Generate SSL Certificates**:  
   Requests a wildcard SSL certificate (`*.<domain>`) using DNS validation via Cloudflare.

5. **Save and Commit Certificates**:  
   The generated certificates are compressed into a `.zip` file, saved in a `certs/` directory, and pushed to the repository.

## **Inputs**

| Input  | Description                           | Default       | Required |
|--------|---------------------------------------|---------------|----------|
| domain | The domain to process SSL for.       | `example.com` | Yes      |

## **Outputs**

| Output | Description                      |
|--------|----------------------------------|
| ip     | The public IP address retrieved. |

## **Usage**

```yaml
name: Generate SSL Certificates

on:
  workflow_dispatch:
    inputs:
      domain:
        description: "Enter the domain to process"
        required: true
        default: "example.com"

jobs:
  get_cert:
    runs-on: ubuntu-latest

    steps:
      - name: Run Get-Cert Action
        uses: your-username/get-cert-cloudflare@v1.0.0
        with:
          domain: ${{ github.event.inputs.domain }}
        env:
          CLOUDFLARE_API_TOKEN: ${{ secrets.CLOUDFLARE_API_TOKEN }}
          REPO_TOKEN: ${{ secrets.REPO_TOKEN }}



## **Notes**
Certificates are stored in the repository under the certs/ directory.
The Cloudflare API token must have DNS:Edit permissions.
This action assumes certbot supports wildcard certificates using the DNS plugin.
