# CI/CD Pipeline with Jenkins, Docker & Azure

This project implements an end-to-end **CI/CD pipeline** using Jenkins to build, version, push, and deploy a Dockerized React application to **Azure Container Instances (ACI)**.

---

<img width="1453" height="734" alt="Screenshot 2026-02-27 at 10 17 51 AM" src="https://github.com/user-attachments/assets/edb5d410-1487-4188-8c84-9792e03f8f11" />
<img width="1453" height="734" alt="Screenshot 2026-02-27 at 10 17 59 AM" src="https://github.com/user-attachments/assets/ebf5c7f9-b6db-48ce-8c4e-a594b96548bf" />
<img width="1453" height="734" alt="Screenshot 2026-02-27 at 10 18 15 AM" src="https://github.com/user-attachments/assets/c5c5bc26-3e6f-4fe0-9959-711c1e5b1ed0" />

---

## Architecture

```
GitHub → Jenkins → Docker Build → DockerHub → Azure ACI
```

---

## Tech Stack

- Jenkins (Declarative Pipeline)
- Docker
- DockerHub
- Azure CLI
- Azure Container Instances
- Azure Service Principal
- Email Extension Plugin

---

## Pipeline Workflow

1. Manual approval gate  
2. Clone GitHub repository  
3. Build Docker image (`BUILD_NUMBER` tagged)  
4. Push image to DockerHub  
5. Deploy container to Azure ACI  
6. Send success/failure email notification  

---

## Image Versioning

Each build generates:

```
adityarrudola/react-app:<BUILD_NUMBER>
adityarrudola/react-app:latest
```

Ensures traceability and rollback capability.

---

## Security

- Jenkins Credentials Store used  
- DockerHub credentials secured  
- Azure Service Principal for non-interactive login  
- Secrets masked in logs  

---

## Deployment

Container deployed with:

- Linux OS
- 1 vCPU
- 1.5 GB RAM
- Public DNS endpoint
- Restart policy: Always

---

## Notifications

HTML email alerts on:
- Success  
- Failure  

Includes build number, image tag, and Jenkins URL.

---

## Outcome

Every successful run:

- Builds a versioned Docker image  
- Pushes it to DockerHub  
- Deploys to Azure  
- Sends a notification  

This project demonstrates practical DevOps skills in CI/CD automation, containerization, cloud deployment, and credential management.
