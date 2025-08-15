# 🐞 BugBox: A Local Web Pentesting Lab (DVWA + bWAPP)
# 💻 Vulnerable Web Applications Setup for Pentesting Practice

This guide walks you through setting up **DVWA** (Damn Vulnerable Web Application) and **bWAPP** (Buggy Web Application) — two purposely vulnerable web apps designed for practicing various web attacks and penetration testing techniques safely and legally.

Ideal for cybersecurity students, bug bounty hunters, and anyone diving deeper into web application security!

---

## 📌 Table of Contents

- [🔧 Prerequisites](#-prerequisites)
- [🚀 DVWA Installation](#-dvwa-installation)
- [🐝 bWAPP Installation](#-bWAPP-installation)
  - [Step 1 - Download bWAPP](#step-1---download-bwapp)
  - [Step 2 - Setup Directory](#step-2---setup-directory)
  - [Step 3 - Unzip and Configure](#step-3---unzip-and-configure)
  - [Step 4 - Configure `settings.php`](#step-4---configure-settingsphp)
  - [Step 5 - Database Fix & Final Steps](#step-5---database-fix--final-steps)
- [✅ Login Credentials](#-login-credentials)
- [🙌 Final Words](#-final-words)

---

## 🔧 Prerequisites

- 🐧 OS: Ubuntu Server or Kali Linux (recommended)
- 🌍 Internet connection
- 🧠 Basic understanding of Linux commands
- 🛜 Apache, MySQL and PHP stack (included in install scripts)

---

## 🚀 DVWA Installation

DVWA is a PHP/MySQL web application that is damn vulnerable by design.

### 🔗 Repository: [https://github.com/digininja/DVWA](https://github.com/digininja/DVWA)

### 📥 Install Steps:

1. **Download the install script:**

    ```bash
    wget https://raw.githubusercontent.com/IamCarron/DVWA-Script/main/Install-DVWA.sh
    ```

2. **Make it executable:**

    ```bash
    chmod +x Install-DVWA.sh
    ```

3. **Install DVWA with root privileges:**

    ```bash
    sudo ./Install-DVWA.sh
    ```

DVWA should now be available at:  
➡️ `http://localhost/dvwa`

---

## 🐝 bWAPP Installation

bWAPP stands for **Buggy Web Application** and it contains over 100 different vulnerabilities to explore ethical hacking and web application security.

---

### Step 1 - Download bWAPP

1. Open your browser and search for:

    ```
    download bWAPP
    ```

2. Or go directly to the [SourceForge page](https://sourceforge.net/projects/bwapp/) and download the latest `bWAPP_latest.zip` file.

---

### Step 2 - Setup Directory

1. Navigate to `/var`:

    ```bash
    cd /var
    ```

2. Make sure the directory `/var/www/html` exists. If not:

    ```bash
    sudo mkdir -p /var/www/html
    ```

> 📝 It’s important to use the default name `www` and `html` to avoid compatibility issues.

---

### Step 3 - Unzip and Configure

1. Move to Downloads folder:

    ```bash
    cd ~/Downloads
    ```

2. Unzip bWAPP:

    ```bash
    sudo unzip bWAPP_latest.zip -d /var/www/html/
    ```

3. Make it executable:

    ```bash
    sudo chmod -R +x /var/www/html/bWAPP
    ```

4. Start services:

    ```bash
    sudo service apache2 start
    sudo service mysql start
    ```

---

### Step 4 - Configure `settings.php`

1. Go to:

    ```
    /var/www/html/bWAPP/admin/
    ```

2. Open `settings.php`:

    ```bash
    sudo nano /var/www/html/bWAPP/admin/settings.php
    ```

3. Locate this section:

    ```php
    // Database credentials
    $db_password = "bug";
    ```

4. Change the database password to an empty string:

    ```php
    $db_password = "";
    ```

5. Save and exit (`CTRL+O`, `Enter`, then `CTRL+X`).

---

### Step 5 - Database Fix & Final Steps

#### 🔧 Fixing MySQL Connection Error (if needed)

If you face connection issues at `localhost/bWAPP/install.php`, try the following:

1. Open terminal:

    ```bash
    mysql -u root -p
    ```

2. Enter MySQL password (usually `toor` or blank)

3. Inside MySQL shell:

    ```sql
    CREATE DATABASE bWAPP;
    CREATE USER 'myuser'@'localhost' IDENTIFIED BY 'mypassword';
    GRANT ALL PRIVILEGES ON bWAPP.* TO 'myuser'@'localhost';
    FLUSH PRIVILEGES;
    EXIT;
    ```

4. Update the `settings.php` file again with:

    ```php
    $db_username = "myuser";
    $db_password = "mypassword";
    ```

5. Save and exit the file.

---

## ✅ Login Credentials

### 🌐 DVWA

- URL: `http://localhost/dvwa`
- Default Login:
  - **Username:** `admin`
  - **Password:** `password`

---

### 🌐 bWAPP

- URL: `http://localhost/bWAPP/login.php`
- Default Login:
  - **Username:** `bee`
  - **Password:** `bug`

---

> 🛠 If login fails, reinstall the DB via: `http://localhost/bWAPP/install.php`

---

## 📸 Screenshots (Optional)

> Include screenshots of:
> - DVWA login page
> - bWAPP login page
> - Installed file structure
> - Configuration settings

---

## 🙌 Final Words

Both **bWAPP** and **DVWA** are fantastic labs to practice real-world web vulnerabilities including:

- SQL Injection
- XSS (Cross-site scripting)
- CSRF
- RFI/LFI
- Command Injection
- Broken Auth
- File Upload vulnerabilities
- ...and more!

### 🧠 Always remember:

🛡️ Practice Safe Hacking - only in local or controlled environments.

---

## 💬 Contributing

Pull requests welcome! For major changes, please open an issue first to discuss what you would like to change.

---

## 📜 License

This project is open source and for ethical hacking **education purposes only**.

> Made with ❤️ for future hackers.
