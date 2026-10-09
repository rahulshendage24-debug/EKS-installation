# Downloading and Installing Jenkins on AWS EC2

This guide outlines the steps to download, install, and configure Jenkins on an AWS EC2 instance.

> **Note:** The steps below are tailored for **Amazon Linux 2**. If you are using **Amazon Linux 2023**, replace `yum` with `dnf`. While `yum` is aliased or symlinked to `dnf` for backward compatibility, using `dnf` directly is recommended.

---

## 1. Installation Steps

### Step 1: Update Software Packages
Ensure the system packages are up to date:
```bash
sudo yum update -y
```

### Step 2: Add Jenkins Repository
Add the official Jenkins repository configuration:
```bash
sudo wget -O /etc/yum.repos.d/jenkins.repo \
    [https://pkg.jenkins.io/rpm-stable/jenkins.repo](https://pkg.jenkins.io/rpm-stable/jenkins.repo)
```

### Step 3: Import GPG Key
Import the repository signing key:
```bash
sudo rpm --import [https://pkg.jenkins.io/rpm-stable/jenkins.io-2026.key](https://pkg.jenkins.io/rpm-stable/jenkins.io-2026.key)
```

### Step 4: Upgrade Repositories
```bash
sudo yum upgrade -y
```

### Step 5: Install Java (Amazon Corretto 21)
Jenkins requires Java to run:
```bash
sudo yum install java-21-amazon-corretto -y
```

### Step 6: Install Jenkins
```bash
sudo yum install jenkins -y
```

### Step 7: Enable and Start Jenkins Service
Enable Jenkins to launch automatically on system boot and start the daemon:
```bash
# Enable service at boot
sudo systemctl enable jenkins

# Start the service
sudo systemctl start jenkins
```

### Step 8: Verify Service Status
Check if Jenkins is running properly:
```bash
sudo systemctl status jenkins
```

---

## 2. Initial Setup & Configuration

Once Jenkins is active, complete the initial setup via the web interface:

1. **Access Jenkins UI:**  
   Open your browser and navigate to:
   ```text
   http://<your_server_public_DNS_or_IP>:8080
   ```
   *(Ensure port `8080` is allowed in your AWS EC2 Security Group inbound rules).*

2. **Retrieve the Initial Admin Password:**  
   Run the following command on your EC2 instance to view the unlock key:
   ```bash
   sudo cat /var/lib/jenkins/secrets/initialAdminPassword
   ```
   Copy the output, paste it into the unlock prompt in your browser, and proceed.

3. **Install Plugins:**  
   On the **Customize Jenkins** screen, select **Install suggested plugins**.

4. **Create Admin User:**  
   Fill in your details under **Create First Admin User**, then click **Save and Continue**.

5. **Install Amazon EC2 Plugin:**
   - Go to **Manage Jenkins** → **Manage Plugins** (or **Plugins**).
   - Switch to the **Available plugins** tab.
   - Search for `Amazon EC2`.
   - Check the box next to **Amazon EC2 plugin** and select **Install without restart**.
