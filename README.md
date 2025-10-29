# Multi-Cloud Weather Tracker with Terraform

A production-ready weather tracking web application deployed across AWS and Azure using Infrastructure as Code (Terraform), featuring CloudFront CDN, SSL/TLS security, and automated DNS failover for disaster recovery.

# Multi-Cloud Weather Tracker with Terraform

![AWS](https://img.shields.io/badge/AWS-FF9900?style=for-the-badge&logo=amazonaws&logoColor=white)
![Azure](https://img.shields.io/badge/Microsoft_Azure-0078D4?style=for-the-badge&logo=microsoft-azure&logoColor=white)
![Terraform](https://img.shields.io/badge/Terraform-7B42BC?style=for-the-badge&logo=terraform&logoColor=white)
![Amazon S3](https://img.shields.io/badge/Amazon_S3-569A31?style=for-the-badge&logo=amazons3&logoColor=white)
![CloudFront](https://img.shields.io/badge/CloudFront-FF9900?style=for-the-badge&logo=amazonaws&logoColor=white)
![Route 53](https://img.shields.io/badge/Route_53-FF9900?style=for-the-badge&logo=amazonaws&logoColor=white)
![SSL/TLS](https://img.shields.io/badge/SSL/TLS-0078D4?style=for-the-badge&logo=letsencrypt&logoColor=white)
![DNS](https://img.shields.io/badge/DNS_Failover-0078D4?style=for-the-badge&logo=cloudflare&logoColor=white)


---

## 📋 Table of Contents
- [Project Overview](#-project-overview)
- [Prerequisites](#-prerequisites)
- [Define AWS Resources on Terraform](#️-define-aws-resources-on-terraform)
- [Define Azure Resources using Terraform](#-define-azure-resources-using-terraform)
- [Implement Disaster Recovery with Route 53 DNS Failover](#-implement-disaster-recovery-with-route-53-dns-failover)
- [Challenges & Solutions](#-challenges--solutions)
- [Conclusion & Clean-up](#-conclusion--clean-up)

---

## ☁️ Project Overview

This project involves deploying a weather tracker application across AWS and Azure, incorporating disaster recovery capabilities. The app's front-end (HTML, CSS, JS) is hosted statically on AWS S3 (with CloudFront for CDN) and Azure Blob Storage. The entire infrastructure is managed using Terraform, automating deployments on both cloud platforms.

### Key Tasks
- Host the weather app on AWS S3 and Azure Blob Storage
- Register a domain through Namecheap for DNS configuration
- Implement disaster recovery with Route 53 DNS failover using AWS and Azure endpoints
- Automate the infrastructure setup with Terraform

### 🛠️ Tech Stack

![Terraform](https://img.shields.io/badge/Terraform-7B42BC?style=for-the-badge&logo=terraform&logoColor=white)
![Amazon S3](https://img.shields.io/badge/Amazon_S3-569A31?style=for-the-badge&logo=amazons3&logoColor=white)
![Azure Blob Storage](https://img.shields.io/badge/Azure_Blob_Storage-0078D4?style=for-the-badge&logo=microsoft-azure&logoColor=white)
![CloudFront](https://img.shields.io/badge/CloudFront-FF9900?style=for-the-badge&logo=amazonaws&logoColor=white)
![Route 53](https://img.shields.io/badge/Route_53-FF9900?style=for-the-badge&logo=amazonaws&logoColor=white)
![SSL/TLS](https://img.shields.io/badge/SSL/TLS-0078D4?style=for-the-badge&logo=letsencrypt&logoColor=white)
![HTML5](https://img.shields.io/badge/HTML5-E34F26?style=for-the-badge&logo=html5&logoColor=white)
![CSS3](https://img.shields.io/badge/CSS3-1572B6?style=for-the-badge&logo=css3&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black) 

- **Terraform**: Automate multi-cloud infrastructure provisioning [Infrastructure as Code]
- **AWS S3**: Host the weather app statically [Hosting]
- **Azure Blob Storage**: Secondary hosting for failover [Hosting]
- **AWS CloudFront**: Distribute content globally [CDN]
- **Route 53**: Automate failover between AWS and Azure [DNS Failover]
- **SSL/TLS via ACM**: Enable HTTPS encryption for secure connections [Security]
- **HTML5, CSS3, & JavaScript**: Create interactive weather tracker interface [Frontend]



### Estimated Time & Cost ⚙️
- **Time**: 2-3 hours
- **Cost**: ~$2 (Domain Name registration) + minimal AWS/Azure usage

### Architecture Diagram

<img width="1536" height="466" alt="image" src="https://github.com/user-attachments/assets/d07cc870-5b93-426b-a9ce-bc219e338c59" />
        


---
### Final Result 

<img width="1440" height="810" alt="Screenshot 2025-10-28 at 8 22 05 PM" src="https://github.com/user-attachments/assets/dde4a04d-f3c1-45ef-b963-266f267efb35" />


<img width="1440" height="807" alt="Screenshot 2025-10-28 at 8 22 51 PM" src="https://github.com/user-attachments/assets/2dc6fafb-d79d-47f3-b087-b07bfa0ab700" />

---
## 🔧 Prerequisites

### Step 1: Install Terraform

Terraform is an open-source Infrastructure as Code (IaC) tool that simplifies cloud infrastructure provisioning.

**For macOS:**
```bash
brew install terraform
```

**For Windows:**
1. Download from [Terraform Downloads page](https://www.terraform.io/downloads)
2. Extract the ZIP file and add the executable to your PATH

**For Linux:**
```bash
# Download and extract
wget https://releases.hashicorp.com/terraform/1.6.0/terraform_1.6.0_linux_amd64.zip
unzip terraform_1.6.0_linux_amd64.zip
sudo mv terraform /usr/local/bin/
```

**Verify Installation:**
```bash
terraform --version
```

### Step 2: Setup Azure and AWS Accounts

- **Azure**: Sign up for a [free Azure account](https://azure.microsoft.com/free/) (includes $200 in credits for 30 days)
- **AWS**: Sign up for [AWS Free Tier](https://aws.amazon.com/free/) (includes free services for 1 year)

### Step 3: Configure Access for Terraform

#### Azure Configuration:

**1. Install Azure CLI:**
```bash
# macOS
brew install azure-cli

# Windows - Download from Microsoft
# Linux
curl -sL https://aka.ms/InstallAzureCLIDeb | sudo bash
```

**2. Log in and get credentials:**
```bash
# Login
az login

# Get subscription details
az account show
```
Note the **Subscription ID** and **Tenant ID** from the output.

**3. Create a Service Principal:**
```bash
az ad sp create-for-rbac --role="Contributor" --scopes="/subscriptions/<subscription-id>"
```
Replace `<subscription-id>` with your actual subscription ID.

Note from the output:
- `appId` = **Azure Client ID**
- `password` = **Azure Client Secret**
- `tenant` = **Tenant ID**

#### AWS Configuration:

**1. Install AWS CLI:**
```bash
# macOS
brew install awscli

# Windows - Download MSI installer
# Linux
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
```

**2. Configure AWS CLI:**
```bash
aws configure
```

Provide:
- AWS Access Key ID
- AWS Secret Access Key
- Default region (e.g., `us-east-1`)

Note the **Access Key ID** and **Secret Access Key** for Terraform configuration.

### Step 4: Create Terraform Project Structure

**1. Create project directory:**
```bash
mkdir multi-cloud-weather-tracker
cd multi-cloud-weather-tracker
```

**2. Create configuration files:**

**`aws_credentials.tf`:**
```hcl
# AWS credentials
variable "aws_access_key" {
  type      = string
  sensitive = true
}

variable "aws_secret_key" {
  type      = string
  sensitive = true
}
```

**`azure_credentials.tf`:**
```hcl
# Azure credentials
variable "azure_client_id" {
  type      = string
  sensitive = true
}

variable "azure_client_secret" {
  type      = string
  sensitive = true
}

variable "azure_subscription_id" {
  type      = string
  sensitive = true
}

variable "azure_tenant_id" {
  type      = string
  sensitive = true
}
```

**`main.tf`:**
```hcl
# AWS provider
provider "aws" {
  access_key = var.aws_access_key
  secret_key = var.aws_secret_key
  region     = "us-east-1"
}

# Azure provider
provider "azurerm" {
  features {
    resource_group {
      prevent_deletion_if_contains_resources = false
    }
  }
  
  client_id       = var.azure_client_id
  client_secret   = var.azure_client_secret
  subscription_id = var.azure_subscription_id
  tenant_id       = var.azure_tenant_id
}
```

### Step 5: Secure Sensitive Information

**Create `terraform.tfvars`:**
```hcl
aws_access_key = "your-aws-access-key"
aws_secret_key = "your-aws-secret-key"

azure_client_id       = "your-azure-client-id"
azure_client_secret   = "your-azure-client-secret"
azure_subscription_id = "your-azure-subscription-id"
azure_tenant_id       = "your-azure-tenant-id"
```

**Create `.gitignore`:**
```
# Terraform
.terraform/
*.tfstate
*.tfstate.*
*.tfvars
.terraform.lock.hcl
crash.log

# Credentials
*credentials*
secrets/

# OS files
.DS_Store
Thumbs.db
```

### Step 6: Initialize Terraform

```bash
terraform init
```

### Step 7: Validate Configuration

```bash
terraform validate
```

---

## ☁️ Define AWS Resources on Terraform

### Step 1: Get the Website Code

**1. Download the website code:**
- Visit: https://github.com/techwithlucy/ztc-projects
- Click the green **Code** button → **Download ZIP**
- Extract the ZIP file

**2. Organize files:**
```bash
# Rename and move the folder
mv weather-tracker-app-main website
```

**Final structure:**
```
multi-cloud-weather-tracker/
├── website/
│   ├── index.html
│   ├── styles.css
│   ├── script.js
│   └── assets/
│       ├── cloud.png
│       ├── humidity.png
│       └── ...
├── main.tf
├── variables.tf
├── aws_credentials.tf
├── azure_credentials.tf
└── terraform.tfvars
```

### Step 2: Define S3 Bucket and Upload Files

**Add to `main.tf`:**

```hcl
# Define an S3 bucket for static website hosting
resource "aws_s3_bucket" "weather_app" {
  bucket = "weather-tracker-app-bucket-102725"  # Use a globally unique name
  
  lifecycle {
    prevent_destroy = true  # Prevent accidental deletion
  }
}

# Separate website configuration
resource "aws_s3_bucket_website_configuration" "weather_app" {
  bucket = aws_s3_bucket.weather_app.id

  index_document {
    suffix = "index.html"
  }

  error_document {
    key = "error.html"
  }
}

# Public access configuration
resource "aws_s3_bucket_public_access_block" "public_access" {
  bucket = aws_s3_bucket.weather_app.id

  block_public_acls       = false
  block_public_policy     = false
  ignore_public_acls      = false
  restrict_public_buckets = false
}

# Upload website files
resource "aws_s3_object" "website_index" {
  bucket       = aws_s3_bucket.weather_app.id
  key          = "index.html"
  source       = "website/index.html"
  content_type = "text/html"
}

resource "aws_s3_object" "website_style" {
  bucket       = aws_s3_bucket.weather_app.id
  key          = "styles.css"
  source       = "website/styles.css"
  content_type = "text/css"
}

resource "aws_s3_object" "website_script" {
  bucket       = aws_s3_bucket.weather_app.id
  key          = "script.js"
  source       = "website/script.js"
  content_type = "application/javascript"
}

# Upload assets
resource "aws_s3_object" "website_assets" {
  for_each = fileset("website/assets", "*")
  
  bucket = aws_s3_bucket.weather_app.id
  key    = "assets/${each.value}"
  source = "website/assets/${each.value}"
}

# Bucket policy for public access
resource "aws_s3_bucket_policy" "bucket_policy" {
  bucket = aws_s3_bucket.weather_app.id
  
  depends_on = [aws_s3_bucket_public_access_block.public_access]
  
  policy = jsonencode({
    Version = "2012-10-17",
    Statement = [
      {
        Sid       = "PublicReadGetObject",
        Effect    = "Allow",
        Principal = "*",
        Action    = "s3:GetObject",
        Resource  = "arn:aws:s3:::${aws_s3_bucket.weather_app.id}/*"
      }
    ]
  })
}
```

### Step 3: Apply Configuration

```bash
# Plan the deployment
terraform plan -var-file="terraform.tfvars"

# Apply the configuration
terraform apply -var-file="terraform.tfvars"
```

### Step 4: Verify S3 Website

Access your website at:
```
http://weather-tracker-app-bucket-102725.s3-website-us-east-1.amazonaws.com
```

---

## 🔷 Define Azure Resources using Terraform

### Step 1: Setup Resource Group and Storage Account

**Add to `main.tf`:**

```hcl
# Define Resource Group
resource "azurerm_resource_group" "rg" {
  name     = "rg-static-website"
  location = "East US"
}

# Define Storage Account with Static Website
resource "azurerm_storage_account" "storage" {
  name                     = "weatherstorage102825"
  resource_group_name      = azurerm_resource_group.rg.name
  location                 = azurerm_resource_group.rg.location
  account_tier             = "Standard"
  account_replication_type = "LRS"
  account_kind             = "StorageV2"
  
  static_website {
    index_document = "index.html"
  }
}
```

**Note:** Make sure the storage account name is globally unique. Change the trailing numbers if needed.

### Step 2: Upload Static Files to Azure Blob Storage

**Add to `main.tf`:**

```hcl
# Upload index.html
resource "azurerm_storage_blob" "index_html" {
  name                   = "index.html"
  storage_account_name   = azurerm_storage_account.storage.name
  storage_container_name = "$web"
  type                   = "Block"
  content_type           = "text/html"
  source                 = "website/index.html"
}

# Upload styles.css
resource "azurerm_storage_blob" "styles_css" {
  name                   = "styles.css"
  storage_account_name   = azurerm_storage_account.storage.name
  storage_container_name = "$web"
  type                   = "Block"
  content_type           = "text/css"
  source                 = "website/styles.css"
}

# Upload script.js
resource "azurerm_storage_blob" "scripts_js" {
  name                   = "script.js"
  storage_account_name   = azurerm_storage_account.storage.name
  storage_container_name = "$web"
  type                   = "Block"
  content_type           = "application/javascript"
  source                 = "website/script.js"
}

# Upload all assets
resource "azurerm_storage_blob" "assets" {
  for_each = fileset("website/assets", "**/*")

  name                   = "assets/${each.value}"
  storage_account_name   = azurerm_storage_account.storage.name
  storage_container_name = "$web"
  type                   = "Block"
  content_type = lookup(
    {
      "png"  = "image/png"
      "jpg"  = "image/jpeg"
      "jpeg" = "image/jpeg"
      "gif"  = "image/gif"
      "svg"  = "image/svg+xml"
      "ico"  = "image/x-icon"
    },
    split(".", each.value)[length(split(".", each.value)) - 1],
    "application/octet-stream"
  )
  source = "website/assets/${each.value}"
}
```

### Step 3: Apply Configuration

```bash
# Initialize with upgrade
terraform init --upgrade

# Plan the deployment
terraform plan -var-file="terraform.tfvars"

# Apply the configuration
terraform apply -var-file="terraform.tfvars"
```

### Step 4: Verify Azure Website

Your website will be accessible at:
```
https://weatherstorage102825.z20.web.core.windows.net/
```

---

## 🔄 Implement Disaster Recovery with Route 53 DNS Failover

### Step 1: Buy and Register a Domain Name

**1. Go to Namecheap:**
- Visit [Namecheap.com](https://www.namecheap.com)
- Search for available domains
- **Tip:** Use extensions like `.site`, `.online`, or `.tech` for budget-friendly options (~$1)

**2. Purchase your domain** (e.g., `weather-track.site`)

### Step 2: Create a Hosted Zone in Route 53

**Add to `main.tf`:**

```hcl
resource "aws_route53_zone" "main" {
  name = "weather-track.site"  # Replace with your domain
}
```

### Step 3: Request an SSL Certificate for CloudFront

**1. Go to AWS Certificate Manager (ACM) in `us-east-1` region**

**2. Request a public certificate:**
- Add domain names:
  - `weather-track.site`
  - `www.weather-track.site`
- Choose **DNS validation**
- Click **Request**

**3. Add DNS validation records:**
- Copy the CNAME records from ACM
- Go to Namecheap → Advanced DNS
- Add both CNAME records (see [Challenges & Solutions](#-challenges--solutions) for detailed steps)
- Wait 10-30 minutes for validation

### Step 4: Configure CloudFront with Alternate Domain Names

**Add to `main.tf`:**

```hcl
resource "aws_cloudfront_distribution" "website" {
  enabled             = true
  default_root_object = "index.html"
  aliases             = ["weather-track.site", "www.weather-track.site"]

  origin {
    domain_name = aws_s3_bucket_website_configuration.weather_app.website_endpoint
    origin_id   = "S3-Website"

    custom_origin_config {
      http_port              = 80
      https_port             = 443
      origin_protocol_policy = "http-only"
      origin_ssl_protocols   = ["TLSv1.2"]
    }
  }

  default_cache_behavior {
    allowed_methods        = ["GET", "HEAD"]
    cached_methods         = ["GET", "HEAD"]
    target_origin_id       = "S3-Website"
    viewer_protocol_policy = "redirect-to-https"

    forwarded_values {
      query_string = false
      cookies {
        forward = "none"
      }
    }
  }

  viewer_certificate {
    acm_certificate_arn      = "arn:aws:acm:us-east-1:YOUR_ACCOUNT:certificate/YOUR_CERT_ID"
    ssl_support_method       = "sni-only"
    minimum_protocol_version = "TLSv1.2_2021"
  }

  restrictions {
    geo_restriction {
      restriction_type = "none"
    }
  }
}
```

### Step 5: Define Health Checks for Failover

**Add to `main.tf`:**

```hcl
# AWS (Primary) Health Check
resource "aws_route53_health_check" "aws_health_check" {
  type              = "HTTPS"
  fqdn              = aws_cloudfront_distribution.website.domain_name
  port              = 443
  request_interval  = 30
  failure_threshold = 3
}

# Azure (Secondary) Health Check
resource "aws_route53_health_check" "azure_health_check" {
  type              = "HTTPS"
  fqdn              = azurerm_storage_account.storage.primary_web_host
  port              = 443
  request_interval  = 30
  failure_threshold = 3
}
```

### Step 6: Setup Route 53 Records for Failover

**Add to `main.tf`:**

```hcl
# Primary Record (AWS - CloudFront)
resource "aws_route53_record" "primary" {
  zone_id = aws_route53_zone.main.zone_id
  name    = "weather-track.site"
  type    = "A"

  alias {
    name                   = aws_cloudfront_distribution.website.domain_name
    zone_id                = aws_cloudfront_distribution.website.hosted_zone_id
    evaluate_target_health = true
  }

  failover_routing_policy {
    type = "PRIMARY"
  }

  set_identifier  = "primary"
  health_check_id = aws_route53_health_check.aws_health_check.id
}

# WWW Primary Record
resource "aws_route53_record" "www_primary" {
  zone_id = aws_route53_zone.main.zone_id
  name    = "www.weather-track.site"
  type    = "A"

  alias {
    name                   = aws_cloudfront_distribution.website.domain_name
    zone_id                = aws_cloudfront_distribution.website.hosted_zone_id
    evaluate_target_health = true
  }

  failover_routing_policy {
    type = "PRIMARY"
  }

  set_identifier  = "www-primary"
  health_check_id = aws_route53_health_check.aws_health_check.id
}

# Secondary Record (Azure - Blob Storage)
resource "aws_route53_record" "secondary" {
  zone_id = aws_route53_zone.main.zone_id
  name    = "weather-track.site"
  type    = "CNAME"
  ttl     = 300

  records = [azurerm_storage_account.storage.primary_web_host]

  failover_routing_policy {
    type = "SECONDARY"
  }

  set_identifier  = "secondary"
  health_check_id = aws_route53_health_check.azure_health_check.id
}
```

### Step 7: Update Namecheap Nameservers

**1. Apply Terraform to create Route 53 hosted zone:**
```bash
terraform apply -var-file="terraform.tfvars"
```

**2. Get Route 53 nameservers:**
- Go to Route 53 → Hosted zones → your domain
- Note the 4 nameservers (e.g., `ns-123.awsdns-45.com`)

**3. Update in Namecheap:**
- Log in to Namecheap
- Go to Domain List → Manage → Advanced DNS
- Under Nameservers, select **Custom DNS**
- Add all 4 Route 53 nameservers
- Save changes

**4. Wait for DNS propagation** (5-60 minutes)

### Step 8: Verify DNS Propagation

**Check using online tools:**
- Visit [DNS Checker](https://dnschecker.org)
- Enter `weather-track.site`
- Verify A records are resolving globally

**Check via command line:**
```bash
dig weather-track.site
dig www.weather-track.site
```

**Test your website:**
```bash
curl -I https://weather-track.site
```

### Expected Behavior and Limitations

#### Primary Site (AWS CloudFront):
✅ HTTPS enabled and secure  
✅ Custom domain working with SSL certificate  
✅ Fast global delivery via CDN

#### Failover Site (Azure Blob Storage):
✅ Failover functionality works correctly  
⚠️ Shows "Not secure" warning - **this is expected behavior**  
⚠️ Azure Blob Storage static websites don't support HTTPS with custom domains by default  
ℹ️ In production environments, you would configure Azure CDN to enable HTTPS

#### Testing Failover:

To test failover functionality:
1. Restrict S3 bucket public access (or disable CloudFront distribution)
2. Visit your domain - it should redirect to Azure
3. You'll see "Not secure" warning - this is normal for this setup
4. The site should remain functional despite the warning

**Note:** The disaster recovery mechanism is working correctly even with the HTTP warning on Azure. In production setups, you'd configure Azure CDN to enable HTTPS, but that's beyond the scope of this tutorial.

---

## 🚧 Challenges & Solutions

### Challenge 1: Terraform Configuration Syntax Errors
**Problem:** Initial `terraform init` failed with multiple syntax errors:
- `lifecycle` block was placed outside resource block
- Variable declarations had incorrect syntax with credentials 
- Azure storage account missing closing brace

**Solution:** 
- Moved `lifecycle` block inside the S3 bucket resource
- Created separate `terraform.tfvars` file for actual credential values
- Added proper closing braces and validated HCL syntax

**Commands used:**
```bash
# Validate syntax
terraform validate
```

**Key Lesson:** Always separate variable declarations from values. Use `terraform validate` to catch syntax errors early.

---

### Challenge 2: S3 Public Access Blocked
**Problem:** `terraform apply` failed with error: "User is not authorized to perform: s3:PutBucketPolicy because public policies are blocked"

**Solution:** Added `aws_s3_bucket_public_access_block` resource with explicit settings:
```hcl
resource "aws_s3_bucket_public_access_block" "public_access" {
  bucket = aws_s3_bucket.weather_app.id

  block_public_acls       = false
  block_public_policy     = false
  ignore_public_acls      = false
  restrict_public_buckets = false
}
```

Added `depends_on` to bucket policy to ensure proper order:
```hcl
resource "aws_s3_bucket_policy" "bucket_policy" {
  bucket = aws_s3_bucket.weather_app.id
  depends_on = [aws_s3_bucket_public_access_block.public_access]
  # ... policy configuration
}
```

**Key Lesson:** AWS enables Block Public Access by default on S3 buckets for security. For static websites, these settings need explicit configuration, and resource dependencies must be properly managed.

---

### Challenge 3: Website Files Not Found
**Problem:** Terraform couldn't find website files: "no such file or directory: website/index.html"

**Solution:**
Files were in `~/Downloads/weather-tracker-app-main/` instead of project directory.

**Steps taken:**
```bash
# Check current location
pwd

# Find the files
find ~ -name "index.html" -type f

# Create website directory
mkdir website

# Try to copy files
cp /Users/jholly88/Downloads/weather-tracker-app-main/*.html website/
# Encountered: "Operation not permitted"
```

**Workaround for macOS Permission Issue:**
- Used Finder GUI to copy files (Cmd+C, Cmd+V)
- This bypassed terminal permission restrictions
- Alternative: Grant Terminal "Full Disk Access" in System Preferences → Security & Privacy

**Key Lesson:** File paths in Terraform are relative to the working directory. Always verify file locations before applying. macOS has strict security for Downloads folder access from terminal.

---

### Challenge 4: SSL Certificate DNS Validation
**Problem:** ACM certificate stuck in "Pending validation" status for multiple domains. One domain showed "Success" while the other remained pending.

**Root Cause:** ACM generates **two separate CNAME records** (one for root domain, one for www subdomain), and both must be added.

**Solution:**

**Correct CNAME format in Namecheap:**

For root domain validation:
```
ACM shows: _abc123.weather-track.site
Namecheap Host field: _abc123
```

For www subdomain validation:
```
ACM shows: _xyz789.www.weather-track.site
Namecheap Host field: _xyz789.www  ← Keep the "www" part!
```

**Steps in Namecheap:**
1. Go to Domain List → Manage → Advanced DNS
2. Scroll to Host Records section
3. Click "Add New Record"
4. Select "CNAME Record"
5. For root: Host = `_validationstring`, Value = (full ACM value)
6. For www: Host = `_validationstring.www`, Value = (full ACM value)
7. Save all changes

**Timeline:** Validation completed in ~15 minutes after adding both records correctly.

**Key Lesson:** DNS validation requires ALL CNAME records to be added. Remove only `.yourdomain.com` from the end of the Host field, keep everything else (including subdomain prefixes like `www`).

---

### Challenge 5: Azure Resource Group Deletion Loop
**Problem:** `terraform destroy` hung for 5+ minutes with message:
```
azurerm_resource_group.rg: Still destroying... [5m00s elapsed]
```

Then failed with error:
```
Error: the Resource Group still contains Resources
* /subscriptions/.../storageAccounts/mystorageaccount102725
```

**Solution:** Updated Azure provider with feature flag to allow force deletion:
```hcl
provider "azurerm" {
  features {
    resource_group {
      prevent_deletion_if_contains_resources = false
    }
  }
  
  client_id       = var.azure_client_id
  client_secret   = var.azure_client_secret
  subscription_id = var.azure_subscription_id
  tenant_id       = var.azure_tenant_id
}
```

**Alternative solutions:**
```bash
# Manual deletion via Azure CLI
az group delete --name rg-static-website --yes --no-wait

# Or delete in Azure Portal
# Resource Groups → rg-static-website → Delete
```

**Key Lesson:** Azure provider has safety mechanisms to prevent accidental resource deletion. Configure provider features for desired behavior. Azure deletions can take 5-15 minutes - this is normal.


