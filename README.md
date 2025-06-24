
![Platform](https://img.shields.io/badge/Platform-Parrot%20OS%2FLinux-green?logo=linux)
![Dark Web](https://img.shields.io/badge/Hosted%20On-Tor%20Hidden%20Service-7a0f3e?logo=tor-project)

# 🕸️ Hosting a Ransomware Command & Control Server on the Dark Web (.onion)

This repository documents how I hosted a PHP-based ransomware C2 dashboard on the dark web using **Tor Hidden Services** and **Apache2**, as part of my graduation project.

> 👨‍💻 Developed by: **Yazan Al-Balawi**  
> 🏫 An-Najah National University – Cybersecurity Graduation Project (2025)

---

## 🔧 Overview

This setup uses:
- Apache2 to serve a PHP-based dashboard (`/var/www/html/prjrans/public`)
- Tor to expose it as a `.onion` service
- Configuration of `.htaccess` and `VirtualHost` to route URLs and allow clean access

---

## 🧱 Folder Structure (Tor + Apache)

```

/var/www/html/prjrans/public/       --> C2 Dashboard files (PHP)
├── login.php, dashboard.php, ...
/var/lib/tor/hidden\_service/        --> Tor hidden service data
/etc/apache2/sites-available/000-default.conf --> Apache virtual host config
/etc/tor/torrc                      --> Tor config for .onion site

````

---

## ⚙️ Configuration Summary

### ✅ 1. Enable Apache to serve the project

```bash
sudo nano /etc/apache2/sites-available/000-default.conf
````

**Set DocumentRoot:**

```
DocumentRoot /var/www/html/prjrans/public
```

Add directory access rules:

```apache
<Directory /var/www/html/prjrans/public>
    AllowOverride All
    Require all granted
</Directory>
```

Then restart:

```bash
sudo systemctl restart apache2
```

---

### 🔐 2. Enable .onion Hidden Service via Tor

Edit:

```bash
sudo nano /etc/tor/torrc
```

Uncomment or add:

```
HiddenServiceDir /var/lib/tor/hidden_service/
HiddenServicePort 80 127.0.0.1:80
```

Then restart Tor:

```bash
sudo systemctl restart tor
```

---

### 🔍 3. Get the .onion URL

```bash
sudo cat /var/lib/tor/hidden_service/hostname
```

This will show something like:

```
xj37cxuv4ghodd54jxrmamhy5gv3ngloco4u26yy3gbttahnx5rfhad.onion
```

Access it via Tor Browser.

---

### 🔁 4. Enable .htaccess Rewriting (Optional)

Inside `/var/www/html/prjrans/public/.htaccess`

```apache
Options -Indexes
DirectoryIndex login.php

RewriteEngine On
RewriteCond %{REQUEST_FILENAME} !-d
RewriteCond %{REQUEST_FILENAME}\.php -f
RewriteRule ^(.*)$ $1.php [L]
```

---

## 📷 Screenshots
![tor service running](https://github.com/user-attachments/assets/b98baaae-8c24-4d73-a81b-be8c3474fd30)


---

## 🎯 Goal

This setup is part of an educational project that demonstrates how real-world ransomware C2 infrastructure can be stealthily hosted on the darknet for **security research and awareness**.

---

## ⚠️ Disclaimer

> For educational and ethical research purposes only. Do **not** use in unauthorized systems or real-world environments.

---
 
