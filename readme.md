# Hi, I'm B-Jayree 👋

I'm a **DevOps Engineer** focused on AWS, infrastructure as code, container platforms, and automated delivery. I develop my skills through hands-on projects that bring application, infrastructure, and deployment configuration together across multiple repositories.

## Featured project: MartialGame

MartialGame is a containerized web application deployed to Amazon EKS through a GitOps workflow. Its application, Kubernetes configuration, and cloud infrastructure are maintained in separate repositories:

- **[Application](https://github.com/B-Jayree/martialgame):** app source and GitHub Actions delivery workflow.
- **[Helm and Argo CD](https://github.com/B-Jayree/martialgame-helm):** Kubernetes deployment configuration and environment values.
- **[Terraform infrastructure](https://github.com/B-Jayree/martialgame-infra):** AWS networking, EKS, IAM, and ECR resources.

**Live demo:** [martialgame.voidcode-beni.xyz](https://martialgame.voidcode-beni.xyz)

### Delivery workflow

GitHub Actions validates changes and, following a merge to `main`, builds and publishes the container image to Amazon ECR. The workflow uses GitHub OIDC to obtain AWS credentials, tags images with the source commit, and updates the Helm configuration repository. Argo CD detects the configuration change and reconciles the application on EKS.

## Other projects

### [Void Inventory Manager](https://github.com/B-Jayree/Void_Inventory_Manager)

A full-stack inventory and sales management application for shops, featuring product and stock management, sales and refunds, reporting, customer records, role-based access, multi-currency support, and real-time updates.

**Technologies:** Next.js, MongoDB, NextAuth, Socket.IO

### [VProfile deployment project](https://github.com/B-Jayree/vprofile-app)

A course project focused on deploying an existing Java/Spring application as an orchestrated workload on Amazon EKS. I did not write the application code; my work focused on the supporting deployment workflow and platform. In this project, I configured EKS OIDC/IRSA for the EBS CSI driver so Kubernetes could provision persistent volume claims (PVCs). This is separate from MartialGame's GitHub Actions OIDC integration for AWS access.

**Deployment technologies:** Docker, Amazon EKS, Terraform, Helm, Argo CD, GitHub Actions

**Related repositories:** [Terraform infrastructure](https://github.com/B-Jayree/vprofile-infra) · [Helm and Argo CD configuration](https://github.com/B-Jayree/vprofile-helm)

## Technical skills

- **Cloud:** AWS, Amazon EKS, ECR, VPC, IAM, S3, OIDC/IRSA
- **Infrastructure as code:** Terraform
- **Containers and orchestration:** Docker, Docker Compose, Kubernetes, Helm
- **CI/CD and GitOps:** GitHub Actions, Argo CD
- **Systems and automation:** Linux, bash scripting, Python, Vagrant
- **Application development:** Next.js, MongoDB
- **Code quality:** SonarQube, automated testing, Checkstyle, ESLint

## Education & training

- **High school:** Completed July 2026.
- **DevOps course:** Completed on Udemy. [View certificate](https://www.udemy.com/certificate/UC-67a790f2-d31c-4565-be82-e2879fafb438/).

## Connect

- [GitHub](https://github.com/B-Jayree)
