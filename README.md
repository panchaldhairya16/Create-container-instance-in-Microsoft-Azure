# 🐳 Azure Container Instances Lab

<div align="center">

![Azure](https://img.shields.io/badge/Azure-Container%20Instances-0078D4?style=for-the-badge&logo=microsoftazure&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-Ready-2496ED?style=for-the-badge&logo=docker&logoColor=white)
![License](https://img.shields.io/badge/License-MIT-green?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-Active-brightgreen?style=for-the-badge)

**A hands-on lab for deploying and managing Docker containers using Microsoft Azure Container Instances (ACI) — no VMs, no Kubernetes, no complexity.**

[🚀 Get Started](#installation) · [📖 Documentation](#usage) · [🐛 Report Bug](../../issues) · [💡 Request Feature](../../issues)

</div>

---

## 📋 Table of Contents

- [About](#about)
- [Features](#features)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Usage](#usage)
- [Architecture](#architecture)
- [License](#license)

---

## 🧩 About

This lab walks you through deploying containerized applications on **Azure Container Instances (ACI)** — Microsoft's serverless container service. ACI lets you run Docker containers directly in the cloud without provisioning virtual machines or orchestrating a full Kubernetes cluster.

> **Perfect for:** testing, demos, CI/CD pipelines, lightweight APIs, and short-lived workloads.

---

## ✨ Features

- ⚡ **Serverless containers** — deploy in seconds with zero infrastructure management
- 🌐 **Public DNS endpoint** — auto-generated URL (`<name>.<region>.azurecontainer.io`)
- 🐳 **Docker-compatible** — use any image from Docker Hub or Azure Container Registry
- 📊 **Live container logs** — stream logs directly from the Azure Portal or CLI
- 🔒 **Secure by default** — isolated container groups with configurable networking
- 💰 **Pay-per-second billing** — only charged for actual compute time used
- 🌍 **Multi-region support** — deploy close to your users globally

---

## 🔧 Prerequisites

Before you begin, make sure you have:

| Requirement | Version | Link |
|---|---|---|
| Azure Account | Free tier OK | [portal.azure.com](https://portal.azure.com) |
| Azure CLI (optional) | `2.x+` | [Install guide](https://learn.microsoft.com/en-us/cli/azure/install-azure-cli) |
| Docker (optional) | `20.x+` | [docker.com](https://www.docker.com) |

---

## 🚀 Installation

### Option A — Azure Portal (GUI)

1. Sign in to the [Azure Portal](https://portal.azure.com)
2. Click **Create a resource** → **Containers** → **Container Instances**
3. Fill in the basics:

```
Resource Group:  myresourcegroup
Container Name:  mycontainer
Image Source:    Quickstart Images
Image:           mcr.microsoft.com/azuredocs/aci-helloworld:latest
Region:          East Asia (or your preferred region)
```

4. Click **Next: Networking** → enter a unique DNS label
5. Click **Review + Create** → **Create**

### Option B — Azure CLI

```bash
# Login to Azure
az login

# Create a resource group
az group create --name myresourcegroup --location eastasia

# Deploy the container
az container create \
  --resource-group myresourcegroup \
  --name mycontainer \
  --image mcr.microsoft.com/azuredocs/aci-helloworld:latest \
  --dns-name-label mycontainer-$(openssl rand -hex 4) \
  --ports 80

# Get the public URL
az container show \
  --resource-group myresourcegroup \
  --name mycontainer \
  --query ipAddress.fqdn \
  --output tsv
```

---

## 📖 Usage

### Access your container

After deployment, Azure generates a public URL:

```
http://mycontainer.<unique-id>.<region>.azurecontainer.io
```

Open it in your browser — you should see the **Welcome to Azure Container Instances!** page. ✅

### View container logs

**Via Portal:**
1. Navigate to your Container Instance in the Azure Portal
2. Go to **Settings** → **Containers** → **Logs**

**Via CLI:**
```bash
az container logs \
  --resource-group myresourcegroup \
  --name mycontainer
```

### Check container status

```bash
az container show \
  --resource-group myresourcegroup \
  --name mycontainer \
  --query "instanceView.state" \
  --output tsv
```

### Clean up resources

```bash
# Delete the container instance
az container delete \
  --resource-group myresourcegroup \
  --name mycontainer \
  --yes

# (Optional) Delete the resource group entirely
az group delete --name myresourcegroup --yes --no-wait
```

---

## 🏗️ Architecture

```
  ┌─────────────────────────────────────────────┐
  │              Docker Image                    │
  │   mcr.microsoft.com/azuredocs/aci-helloworld │
  └──────────────────────┬──────────────────────┘
                         │
                         ▼
  ┌─────────────────────────────────────────────┐
  │       Azure Container Instance (ACI)         │
  │         Region: East Asia                    │
  │         CPU: 1 core  |  RAM: 1.5 GB          │
  └──────────────────────┬──────────────────────┘
                         │
                         ▼
  ┌─────────────────────────────────────────────┐
  │          Public IP + DNS Label               │
  │   mycontainer.<id>.eastasia.azurecontainer.io│
  └──────────────────────┬──────────────────────┘
                         │
                         ▼
  ┌─────────────────────────────────────────────┐
  │              Web Browser                     │
  │         http://...azurecontainer.io          │
  └─────────────────────────────────────────────┘
```

---

## 🗺️ Roadmap

- [x] Deploy sample container via Azure Portal
- [x] Configure public DNS endpoint
- [x] View container logs
- [ ] Deploy custom Docker image from ACR
- [ ] Add environment variables and secrets
- [ ] Configure persistent storage with Azure Files
- [ ] Set up multi-container groups
- [ ] Automate with GitHub Actions CI/CD

---

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

---

## 📄 License

Distributed under the MIT License. See `LICENSE` for more information.

---

## 🙏 Acknowledgements

- [Microsoft Azure Container Instances Docs](https://learn.microsoft.com/en-us/azure/container-instances/)
- [Azure CLI Reference](https://learn.microsoft.com/en-us/cli/azure/container)
- [Docker Official Docs](https://docs.docker.com/)

---

<div align="center">
Made with ❤️ for cloud learners | ⭐ Star this repo if it helped you!
</div>
