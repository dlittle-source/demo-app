# EC2 Nginx Deployment & CI/CD How-To Guide

This guide walks you through:

* Installing and configuring **Nginx** on an EC2 instance
* Deploying a simple **static demo app**
* Preparing the server for **CI/CD with GitHub Actions** using a dedicated `deploy` user

All steps below have been **tested and verified**.

---

## 🔥 Hot Nginx Commands (Quick Reference)

```bash
sudo nginx -T | less        # View full Nginx config
sudo systemctl status nginx # Check Nginx status
sudo systemctl restart nginx
```

---

## 1️⃣ EC2: Update System & Install Dependencies

```bash
sudo apt update -y
sudo apt upgrade -y

sudo apt install nginx git -y
```

Enable and start Nginx:

```bash
sudo systemctl enable nginx
sudo systemctl start nginx
```

Verify Nginx is running:

```bash
sudo systemctl status nginx
```

---

## 2️⃣ Setup App Directory & Demo App

### Create Web Root (usually exists by default)

```bash
sudo mkdir -p /var/www/html
```

### Create Project Structure

```bash
cd ~
mkdir demo-app
cd demo-app
mkdir frontend
```

### Create Frontend Files

```bash
nano frontend/index.html
nano frontend/style.css
```

---

## 3️⃣ Deploy App to Nginx Web Root

```bash
sudo rm -rf /var/www/html/*
sudo cp -r ~/demo-app/frontend/* /var/www/html/
sudo chown -R www-data:www-data /var/www/html
```

---

## 4️⃣ Configure Nginx (Default Site)

Edit the default config:

```bash
sudo nano /etc/nginx/sites-available/default
```

Use this configuration:

```nginx
server {
    listen 80 default_server;
    server_name _;

    root /var/www/html;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }
}
```

Restart Nginx:

```bash
sudo systemctl restart nginx
```

### Test Your App

Visit:

```
http://YOUR_EC2_PUBLIC_IP
```

✅ **Your application is now live and operational**

---

## 🚀 Next Step: CI/CD with GitHub Actions

Once the app is running manually, we move to automated deployments.

---

## 5️⃣ Create a Dedicated `deploy` User (Recommended)

### Why a deploy user?

* Follows **principle of least privilege**
* Separates **human access** from **automation**
* Clients expect this in production setups

| User   | Purpose                     |
| ------ | --------------------------- |
| ubuntu | Manual admin / SSH work     |
| deploy | Automated CI/CD deployments |

---

### Create the User

```bash
sudo adduser deploy
sudo usermod -aG sudo deploy
```

Switch to deploy:

```bash
su - deploy
```

---

## 6️⃣ Configure SSH Access for `deploy`

If you already have SSH keys (recommended):

```bash
mkdir -p ~/.ssh
chmod 700 ~/.ssh
sudo cp /home/ubuntu/.ssh/authorized_keys ~/.ssh/
chmod 600 ~/.ssh/authorized_keys
sudo chown -R deploy:deploy ~/.ssh
```

Test SSH access:

```bash
ssh -i your-key.pem deploy@YOUR_EC2_PUBLIC_IP
```

---

## 7️⃣ Fix Web Directory Permissions (Critical)

Allow deploy to deploy files and Nginx to serve them:

```bash
sudo chown -R deploy:www-data /var/www/html
sudo chmod -R 775 /var/www/html
```

### Sanity Check

```bash
touch /var/www/html/test.txt
ls -l /var/www/html
rm /var/www/html/test.txt
```

You should see:

```
deploy www-data
```

---

## 8️⃣ Optional: Prevent Permission Drift

```bash
sudo chmod g+s /var/www/html
```

Test again:

```bash
echo "CI/CD test" > /var/www/html/perm-test.txt
```

Open:

```
http://YOUR_EC2_PUBLIC_IP/perm-test.txt
```

If it loads → permissions are correct.

Delete:

```bash
rm /var/www/html/perm-test.txt
```

---

## 9️⃣ Enable Passwordless Sudo for Deploy (CI/CD Friendly)

Edit sudoers safely:

```bash
sudo visudo
```

Add at the bottom:

```
deploy ALL=(ALL) NOPASSWD:ALL
```

Verify:

```bash
su - deploy
sudo ls /root
```

---

## 🔑 SSH Key for GitHub Actions (Optional if Existing)

```bash
ssh-keygen -t ed25519 -C "github-actions-deploy"
ssh-copy-id -i github-actions.pub deploy@YOUR_EC2_PUBLIC_IP
```

Test:

```bash
ssh -i github-actions deploy@YOUR_EC2_PUBLIC_IP
```

---

## 🔟 Prepare Repo on EC2

Clone your repo once:

```bash
cd ~
git clone https://github.com/YOUR_USERNAME/demo-app.git
```

Expected structure:

```
/home/deploy/demo-app/frontend
```

---

## 1️⃣1️⃣ Test the CI/CD Pipeline

Update `index.html`:

```html
<p>CI/CD deployment successful 🚀</p>
```

Commit & push:

```bash
git add .
git commit -m "Test CI/CD deployment"
git push origin main
```

If GitHub Actions runs and the site updates → ✅ **CI/CD is working**

---

## 🎉 Final Status

✔ Nginx installed and configured
✔ Static app deployed
✔ Secure deploy user created
✔ Permissions validated
✔ Ready for GitHub Actions CI/CD

You are now running a **production-style EC2 deployment pipeline**.
