# AWS Deployment Demo App

## CI/CD Flow Diagram
![CI/CD Diagram](ci-cd-diagram.png)

---

## Overview

This is a **portfolio-grade demo app** deployed on **AWS EC2** using **Nginx** and automated with **GitHub Actions CI/CD**.  

It demonstrates my ability to take an application from “it works locally” to **live, secure, and reliable**, which is exactly what I deliver for clients.

---

## Features

- Fully deployed frontend application hosted on Nginx (AWS EC2)
- Automated CI/CD pipeline using GitHub Actions
- Secure passwordless sudo `deploy` user for safe, automated deployments
- Proper permissions configured for `/var/www/html`
- Pre-deployment backups with rollback support to ensure safe updates
- Automatic Nginx reload on successful deployments
- Latest commit always deployed via `git pull` in the CI/CD workflow
- Responsive and polished demo page for client presentation
- Mobile-friendly design with a clean, professional layout
- Portfolio-ready documentation including README + workflow diagram
- Hands-on DevOps skills demonstrated: AWS EC2, Nginx, Git, CI/CD

---

## Demo Page

Visit the live demo:  
`http://<YOUR_EC2_PUBLIC_IP>`  

The page includes:

- Confirmation of CI/CD deployment (live status update)
- Live updates whenever a push is made to `main`
- Responsive and mobile-friendly layout
- Polished styling with modern container design
- Clear structure to showcase production deployment
- Demonstrates secure and automated deployment workflow
- Quick visual confirmation for clients that infrastructure, Nginx, and CI/CD are all working together

---

### Backup & Rollback Support

Before each deployment, the currently live site is automatically backed up.  
This ensures that you can safely roll back to the last working version if needed.

### Automatic Backup
- The live site (`/var/www/html`) is backed up before each deployment.
- Backups are stored on the EC2 instance with a **timestamp**:

  ```/var/www/backups/html_backup_YYYY-MM-DD_HH-MM.tar.gz
  ```  
 
### Restore Previous Version
If a deployment introduces an issue, you can restore the last working version quickly:

```bash
sudo tar -xzf /var/www/backups/html_backup_latest.tar.gz -C /var/www
sudo systemctl reload nginx
```


## Folder Structure

### Repository (`demo-app`)

```text
demo-app/
├── frontend/                   # Frontend source code
│   ├── index.html              # Main demo page
│   └── style.css               # Styling for the demo page
├── .github/
│   └── workflows/
│       └── deploy.yml          # GitHub Actions CI/CD workflow
└── README.md                   # Documentation & client instructions


/home/deploy/
└── demo-app/
    ├── frontend/               # Source files pulled from GitHub
    │   ├── index.html
    │   └── style.css
    └── README.md

/var/www/html/
├── index.html                  # Live site served by Nginx
└── style.css


**Notes:**

- frontend/ contains all files **served by Nginx** on EC2.
- .github/workflows/deploy.yml` handles **automatic deployment** whenever main is updated.
- README.md explains the project, workflow, and features for clients or collaborators.
- Deployed files on EC2 live at: /var/www/html/.
- Source repo mirrors the deployment folder, ensuring a **clean and reproducible workflow**.



