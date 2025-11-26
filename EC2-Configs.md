# Required EC2 Instance Configurations

## Hardware: 

(i) **For a 7B parameter model, you need at least 24GB of VRAM to fine-tune comfortably without crashing.**

(ii) **Instance Type**: g5.2xlarge (*Recommended*)

(iii) **GPU**: 1 x NVIDIA A10G (24GB VRAM)

(iv) **vCPUs**: 8

(v) **RAM**: 32 GB

Why this one? The A10G GPU is the modern standard for 7B models. The older g4dn instances (Tesla T4) only have 16GB VRAM, which makes fine-tuning very difficult (you would have to use aggressive quantization, which hurts accuracy for medical/bio data).

## The Storage (The "Hard Drive")

(i) **Bioinformatics datasets are large, and model checkpoints (saves) take up massive space. Do not use the default 8GB root volume.**

(ii) **Root Volume**: 100 GB gp3 (General Purpose SSD)

(iii) **Why**: To hold the OS, Docker images, and Python libraries.

(iv) **Data Volume (Separate)**: 500 GB gp3

(v) **Why**: Mount this at /data. Download your PGP datasets here. If you terminate the instance to save money, you can detach this volume and keep your data safe.

## The Operating System (The "AMI")

(i) **Do not install Ubuntu "vanilla" and try to install NVIDIA drivers yourself (it is a nightmare). Use the pre-configured AWS image.**

(ii) **AMI Name**: Deep Learning OSS Nvidia Driver AMI GPU PyTorch 2.x (Ubuntu 22.04)

(iii) **Search for**: "Deep Learning AMI" in the AWS launch wizard.

(iv) **Why**: It comes with CUDA, cuDNN, Docker, and NVIDIA Drivers pre-installed and tested.

## Security Group (The "Firewall")

**You need to open specific ports to access the Chat UI and the Training UI from your browser.**

(i) **Allow SSH (Port 22)**: From My IP (for your terminal access).

(ii) **Allow Custom TCP (Port 7860)**: From My IP (for LLaMA-Factory Training UI).

(iii) **Allow Custom TCP (Port 3000)**: From My IP (for Open WebUI Chat Interface).
