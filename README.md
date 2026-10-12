# AWS Static Portfolio Website

A personal portfolio website hosted on AWS using Amazon S3 and CloudFront, with a private storage bucket and HTTPS delivery.

**Live site:** https://dazhjowwwz3do.cloudfront.net

## Overview

This project deploys a static HTML/CSS portfolio website using an AWS architecture that follows real-world security practices: the storage layer (S3) is never exposed directly to the public. Instead, a CDN (CloudFront) sits in front of it, so visitors are served content through a secure, globally distributed network while the underlying storage stays locked down.

## Architecture

```
Visitor's Browser
        |
        | HTTPS
        v
   CloudFront (CDN)
        |
        | Origin Access Control (private connection)
        v
   S3 Bucket (private, Block Public Access ON)
```

**How it works:**
1. Website files (`index.html`, `style.css`, `profile.jpg`) are stored in a private S3 bucket.
2. The S3 bucket has "Block all public access" enabled, so it cannot be reached directly from the internet.
3. A CloudFront distribution is configured with the S3 bucket as its origin, using **Origin Access Control (OAC)** to authenticate requests between CloudFront and S3.
4. CloudFront serves the site over **HTTPS** automatically, using an AWS-managed certificate, and caches content at edge locations worldwide for fast load times.
5. Visitors only ever interact with CloudFront's domain, never the S3 bucket directly.

## AWS Services Used

| Service | Purpose |
|---|---|
| **Amazon S3** | Object storage for the static website files |
| **Amazon CloudFront** | Content Delivery Network (CDN); handles HTTPS, caching, and global distribution |
| **IAM** | Secure account access management (admin user with MFA, instead of using the root account) |

## Security Decisions

- **S3 bucket is fully private.** Public access is blocked at the bucket level. Only CloudFront can read from it, via Origin Access Control.
- **HTTPS enforced by default**, through CloudFront's managed TLS certificate.
- **No root account used for daily work.** A separate IAM user with MFA enabled handles all operations.
- **AWS WAF intentionally not enabled** for this project, since it's a low-traffic static portfolio site and WAF adds ongoing cost without meaningful benefit at this scale. This would be reconsidered for a production site with real traffic.

## Tech Stack

- HTML5 / CSS3 (no frameworks)
- Google Fonts (Poppins)
- Git & GitHub for version control

## Local Setup

```bash
git clone https://github.com/Yaseenshaik581/aws-static-website.git
cd aws-static-website
start index.html   # Windows
# or open index.html directly in a browser
```

## Deployment Steps (Summary)

1. Create a private S3 bucket (Block Public Access enabled).
2. Upload website files to the bucket.
3. Create a CloudFront distribution with the S3 bucket as origin.
4. Enable "Allow private S3 bucket access to CloudFront" (Origin Access Control).
5. Set `index.html` as the Default Root Object.
6. Deploy and access the site via the CloudFront domain.

## Author

**Shaik Yaseen**
Computer Engineering Graduate | Aspiring Cloud & DevOps Engineer
[GitHub](https://github.com/Yaseenshaik581) · [LinkedIn](https://www.linkedin.com/in/yaseen-shaik-66654a2a5/)
