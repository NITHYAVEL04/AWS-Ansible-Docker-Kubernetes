# AWS-Ansible-Docker-Kubernetes
Automate server provisioning using Infrastructure as Code, containerize a web application with Docker, and orchestrate it using Kubernetes on an AWS EC2 cloud instance

# ☁️ Cloud-Native Deployment Pipeline: Ansible, Docker, & Kubernetes on AWS

An automated, containerized cloud infrastructure project built entirely on a remote AWS EC2 instance to bypass local hardware limitations.

---

## 🏗️ Architecture & Tech Stack
* **Cloud Infrastructure:** AWS EC2 (Ubuntu)
* **Automation:** Ansible (Local connection playbook)
* **Containerization:** Docker & Nginx
* **Orchestration:** Kubernetes (Minikube single-node cluster)

---

## 🔄 Project Flow & Step-by-Step Execution

1. **Server Provisioning & Tool Setup (Ansible):** 
   Instead of manually configuring the server, we ran an Ansible playbook (`setup-k8s.yml`) that automatically installs Docker, `kubectl`, and Minikube.
   
   <img width="1202" height="667" alt="Ansible playbook status" src="https://github.com/user-attachments/assets/afce3195-8276-4f55-ab32-cded3aa6b303" />
   *Title Card:* `Ansible Automated Provisioning`

3. **Cluster Initialization & Containerization:** 
   We booted Minikube on the cloud instance and built a custom Nginx web app image using Docker within Minikube's local environment.

  <img width="972" height="167" alt="Docker Image" src="https://github.com/user-attachments/assets/ef1ac31b-47a3-4fd8-bec4-da8049881df6" />
   *Title Card:* `Minikube Cluster & Docker Image Build`

4. **Kubernetes Deployment & Exposure:** 
   We deployed the container using a declarative YAML manifest (`deployment.yaml`) and exposed it via a NodePort service.

   <img width="632" height="82" alt="Pod status" src="https://github.com/user-attachments/assets/8d83450b-8b15-41d5-97f5-a1364779d75b" />
   <img width="777" height="132" alt="Service status" src="https://github.com/user-attachments/assets/e54c992a-1583-4055-a42c-b7307dfb204f" />
   *Title Card:* `Kubernetes Pods & Service Deployment`

5. **Live Access & Verification:** 
   We forwarded traffic using `kubectl port-forward` to access the application live through the browser.

   <img width="872" height="202" alt="Browser output" src="https://github.com/user-attachments/assets/91bd814a-0c10-4f9c-8f4b-e171c3ad8a26" />
   *Title Card:* `Live Web Application Running on Kubernetes`

---

## 🐛 Real-World Challenges & Solutions Faced

* **AWS Disk Space Limits (`No space left on device`):** 
  * *Challenge:* Default EC2 storage filled up rapidly due to Kubernetes pre-loaded image caches and Docker build layers, crashing Minikube startup.
  * *Solution:* Executed aggressive storage pruning (`docker system prune -a --volumes -f`), cleaned system logs, and tuned Minikube memory parameters (`--memory=2048mb`) to fit safely within cloud limits.
* **Kubernetes CLI Flag Deprecation:** 
  * *Challenge:* Newer versions of `kubectl` dropped support for inline flags like `--image-pull-policy` inside command lines.
  * *Solution:* Migrated to robust declarative management using custom YAML manifests (`deployment.yaml`) specifying `imagePullPolicy: Never` to pull local container images seamlessly.
