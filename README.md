## CI/CD Flow Diagram
![CI/CD Diagram](ci-cd-diagram.png)


# AWS Deployment Demo App

## Overview

This is a **portfolio-grade demo app** deployed on **AWS EC2** using **Nginx** and automated with **GitHub Actions CI/CD**.  

It demonstrates my ability to take an application from “it works locally” to **live, secure, and reliable**, which is exactly what I deliver for clients.

---

## Features

-  Fully deployed frontend app hosted on Nginx
-  Automated CI/CD pipeline using GitHub Actions
-  Passwordless sudo deploy user for secure deployments
-  Proper permissions setup for /var/www/html
-  Automatic Nginx reload on updates
-  Latest commit always deployed via git pull in workflow
-  Responsive and polished demo page for clients
-  Mobile-friendly design with clean, professional layout
-  Portfolio-ready documentation including README + workflow diagram
-  Hands-on DevOps skills demonstrated: AWS EC2, Nginx, Git, CI/CD

---

## Demo Page

Visit the live demo:  
`http://<YOUR_EC2_PUBLIC_IP>`  

The page includes:

-  Confirmation of CI/CD deployment (live status update)
-  Live updates whenever a push is made to main
-  Responsive and mobile-friendly layout
-  Polished styling with modern container design
-  Clear structure to showcase production deployment
-  Demonstrates secure and automated deployment workflow
-  Quick visual confirmation for clients that infrastructure, Nginx, and CI/CD are all working together


---

## Folder Structure


### Repository (`demo-app`)
"
demo-app/
├── frontend/                   # Frontend source code
│   ├── index.html              # Main demo page
│   └── style.css               # Styling for the demo page
├── .github/
│   └── workflows/
│       └── deploy.yml          # GitHub Actions CI/CD workflow
└── README.md                   # Documentation & client instructions
"


