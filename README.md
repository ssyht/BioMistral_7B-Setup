# BioMistral 7B: AWS Deployment & Fine-Tuning Pipeline

![License](https://img.shields.io/badge/license-MIT-blue.svg) ![Platform](https://img.shields.io/badge/platform-AWS%20g5.2xlarge-orange.svg) ![Model](https://img.shields.io/badge/model-BioMistral%207B-green)

This repository documents the complete infrastructure setup, data preparation, and deployment strategy for running **BioMistral 7B**, a large language model specialized in medical and bioinformatics domains. 

It covers the deployment on an AWS Ubuntu instance, the configuration of the chat interface (Open WebUI), and the pipeline for fine-tuning the model using datasets from the **Harvard Personal Genome Project (PGP)**.

---

## 🏗️ Architecture Overview

The stack consists of three main layers running on an AWS GPU instance:

1.  **Inference Engine:** [Ollama](https://ollama.com/) (Backend to run the quantized model).
2.  **User Interface:** [Open WebUI](https://github.com/open-webui/open-webui) (ChatGPT-like interface for RAG and interaction).
3.  **Fine-Tuning Engine:** [LLaMA-Factory](https://github.com/hiyouga/LLaMA-Factory) (For supervised fine-tuning on custom genomic datasets).

---

## ⚙️ Hardware & AWS Configuration

To run BioMistral 7B efficiently while retaining the ability to fine-tune, specific hardware is required.

* **Instance Type:** `g5.2xlarge`
    * **GPU:** NVIDIA A10G (24GB VRAM) - *Critical for loading 7B models + gradients.*
    * **vCPUs:** 8
    * **RAM:** 32 GB
* **AMI:** Deep Learning OSS Nvidia Driver AMI GPU PyTorch 2.x (Ubuntu 22.04)
* **Storage Configuration:**
    * **Root (`/`):** 100 GB gp3 (OS & Docker images)
    * **Data (`/data`):** 500 GB gp3 (Mounted volume for Datasets & Model Weights)

### Security Group Rules
| Type | Port | Source | Purpose |
| :--- | :--- | :--- | :--- |
| SSH | 22 | My IP | Terminal Access |
| TCP | 3000 | My IP | Open WebUI (Chat) |
| TCP | 7860 | My IP | LLaMA-Factory (Training UI) |
| TCP | 11434 | Localhost | Ollama API |

---

## 🚀 Installation Guide

### 1. Storage Setup (Mounting the Data Drive)
The 500GB volume must be mounted to `/data` to prevent filling up the root partition.

```bash
# Identify the disk (usually nvme1n1)
lsblk

# Format and mount
sudo mkfs -t ext4 /dev/nvme1n1
sudo mkdir /data
sudo mount /dev/nvme1n1 /data
sudo chown -R ubuntu:ubuntu /data

# Add to fstab for persistence
echo "UUID=$(sudo blkid -s UUID -o value /dev/nvme1n1)  /data  ext4  defaults,nofail  0  2" | sudo tee -a /etc/fstab
