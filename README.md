**step-by-step guide to install and configure WordPress on an EC2 instance** 👇

---

## 🚀 Step 1: Launch EC2 Instance

1. Go to **AWS Console → EC2 → Launch Instance**
2. Choose **Amazon Linux 2** or **Ubuntu 22.04 LTS**
3. Select **t2.micro** (Free Tier)
4. Configure security group:

   * Allow **SSH (22)** from your IP
   * Allow **HTTP (80)** and **HTTPS (443)** from anywhere
5. Launch with a key pair and connect via SSH:

```bash
ssh -i your-key.pem ec2-user@<EC2-Public-IP>
```

---

## ⚙️ Step 2: Install Required Packages

On **Amazon Linux 2**:

```bash
sudo yum update -y
sudo amazon-linux-extras enable php8.0
sudo yum install -y httpd php php-mysqlnd mariadb105 git wget unzip
```

On **Ubuntu**:

```bash
sudo apt update -y
sudo apt install -y apache2 php php-mysql mariadb-server wget unzip
```

---

## 🛠 Step 3: Start Apache and MariaDB

```bash
sudo systemctl start httpd   # or apache2 on Ubuntu
sudo systemctl enable httpd
sudo systemctl start mariadb
sudo systemctl enable mariadb
```

---

## 🔑 Step 4: Secure MySQL/MariaDB

```bash
sudo mysql_secure_installation
```

* Set root password
* Remove anonymous users
* Disallow root login remotely
* Remove test DB

---

## 🗄 Step 5: Create WordPress Database & User

```bash
sudo mysql -u root -p
```

Inside MySQL shell:

```sql
CREATE DATABASE wordpress;
CREATE USER 'wpuser'@'localhost' IDENTIFIED BY 'StrongPassword123';
GRANT ALL PRIVILEGES ON wordpress.* TO 'wpuser'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```

---

## 🌐 Step 6: Download and Configure WordPress

```bash
cd /tmp
wget https://wordpress.org/latest.tar.gz
tar -xzf latest.tar.gz
sudo rsync -av wordpress/* /var/www/html/
```

Set permissions:

```bash
sudo chown -R apache:apache /var/www/html/
sudo chmod -R 755 /var/www/html/
```

---

## ⚡ Step 7: Configure WordPress

```bash
cd /var/www/html
cp wp-config-sample.php wp-config.php
```

Edit `wp-config.php`:

```php
define('DB_NAME', 'wordpress');
define('DB_USER', 'wpuser');
define('DB_PASSWORD', 'StrongPassword123');
define('DB_HOST', 'localhost');
```

---

## 🔥 Step 8: Restart Apache

```bash
sudo systemctl restart httpd
```

For **Ubuntu**:

```bash
sudo systemctl restart apache2
```

---

## 🌍 Step 9: Access WordPress

Open in browser:

```
http://<EC2-Public-IP>
```

Follow the **WordPress installation wizard**, set site title, username, and password.

---

✅ Now WordPress is ready on your EC2 instance!

Would you like me to also give you a **Terraform + User Data script** version so you can launch an EC2 with WordPress automatically (no manual setup)?
