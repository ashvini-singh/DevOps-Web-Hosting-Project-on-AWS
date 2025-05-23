**🌐 Secure NGINX Web Server on AWS with Let's Encrypt**

This project sets up a public-facing NGINX web server on an AWS EC2 instance using a custom domain (triofrionter.click) secured with Let's Encrypt SSL. It includes full configuration and deployment steps.


---

## 📦 Stack Used

- **Amazon EC2 (T3.medium)** – Web server
- **Amazon VPC** – Custom public subnet with internet access
- **NGINX** – Web server installed on Ubuntu
- **Let's Encrypt / Certbot** – Free SSL certificate
- **Route 53** – Domain management and DNS
- **Ubuntu 22.04** – OS

---

**✅ Project Goals**

- Launch EC2 Instance (t3.medium) in a public subnet
- Install and configure NGINX
- Set up a domain name with Route 53
- Install SSL certificate with Let's Encrypt
- Create a custom "Hello DevOps" landing page

---

## 🧱 Architecture

![ChatGPT Image May 23, 2025, 09_14_13 AM](https://github.com/user-attachments/assets/2b2d86ba-f33b-47d8-95a9-8252fa2b32a2)


---

## 🚀 Setup Instructions

### 1. Create an EC2 Instance

- Launch EC2 with **T3.medium** in a **public subnet**
- Open inbound ports **80 (HTTP)** and **443 (HTTPS)** in the Security Group


### 2. Install NGINX

```bash
sudo apt update
sudo apt install nginx -y
sudo systemctl enable nginx

```


**3. Update Default NGINX Configuration**
- Edit the default site file
  
  ```
  sudo vim /etc/nginx/sites-available/default
  
  ```



**Replace the content with:**


```bash
server {
    listen 80 default_server;
    listen [::]:80 default_server;

    root /var/www/html;
    index index.html index.htm;

    server_name triofrionter.click www.triofrionter.click;

    location / {
        try_files $uri $uri/ =404;
    }
}

```


---


**Then restart Nginx**
  
  ```bash
  
  sudo systemctl restart nginx

  ```



**4. Create Your Custom Web Page**

```bash
echo "<!DOCTYPE html><html><head><title>Hello</title></head><body><h1>Hello DevOps from triofrionter.click</h1></body></html>" | sudo tee /var/www/html/index.html

```


## 5. Point Domain to EC2 Instance


Go to **AWS Route 53 → Hosted Zones → triofrionter.click**, and follow these steps:

1. Click **"Create Record"**
2. Configure the record as follows:

| Field         | Value                |
|---------------|----------------------|
| **Record type** | A – IPv4 address     |
| **Record name** | @ *(root domain)*    |
| **Value**       | 13.127.61.60 *(your EC2 Elastic IP)* |
| **TTL (seconds)** | 300 *(default)*    |

> 💡 The "@" symbol denotes the root domain (e.g., `triofrionter.click`).



**6. Install SSL with Let's Encrypt**

```bash
sudo apt install certbot python3-certbot-nginx -y
sudo certbot --nginx -d triofrionter.click
```


**Test renewal:**

```bash

sudo certbot renew --dry-run
```



**📚 Learning Outcomes**

- Hosting a production-grade web server on AWS EC2
- Configuring DNS with Route 53 
- Using Certbot to manage SSL certificates
- Basic Linux system administration and firewall config



**7. Verify HTTPS**

- Visit: https://triofrionter.click
- You should see your custom Hello DevOps



**📂 File Structure**

```
.
├── /var/www/html/index.html                        # Custom Hello World page
├── /etc/nginx/sites-available/                     # NGINX site configuration files
├── /etc/letsencrypt/                               # Certbot SSL certificates (auto-generated)
└── A_diagram_of_a_cloud-based_NGINX_web_server_archit.png   # Architecture diagram

```


**📸 Screenshots**

![image](https://github.com/user-attachments/assets/e0af585a-70be-4827-ad81-62f5891db9eb)


![image](https://github.com/user-attachments/assets/aef89d2e-9f74-4241-bfff-c038f61ad9da)



---

**✍️ Author**

Ashvini Singh

🔗 [GitHub](https://github.com/ashvini-singh)

---
**📜 License**

This project is open-source and free to use under the MIT License.

