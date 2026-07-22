<img width="1470" height="956" alt="image" src="https://github.com/user-attachments/assets/f0b021fa-b7eb-4033-b1b6-6329e012d16a" />


# EC2 Setup with Apache on Linux

This repository contains a simple HTML page (`index.html`) and a step-by-step guide to host it on an AWS EC2 Linux instance using Apache.

## 1) Launch an EC2 instance

1. Open **AWS Console → EC2 → Launch instance**.
2. Choose a Linux AMI:
   - **Amazon Linux 2023** (recommended), or
   - **Ubuntu 22.04 LTS**.
3. Select an instance type (for example, `t2.micro` in free tier).
4. Create/select a key pair (`.pem`) for SSH access.
5. In **Security Group**, allow:
   - **SSH (22)** from your IP
   - **HTTP (80)** from `0.0.0.0/0`
6. Launch the instance and note its **Public IPv4**.

## 2) Connect to the instance

```bash
chmod 400 your-key.pem
ssh -i your-key.pem ec2-user@<PUBLIC_IP>     # Amazon Linux
# or
ssh -i your-key.pem ubuntu@<PUBLIC_IP>       # Ubuntu
```

## 3) Install and start Apache

### Amazon Linux 2023

```bash
sudo dnf update -y
sudo dnf install -y httpd
sudo systemctl start httpd
sudo systemctl enable httpd
```

### Ubuntu 22.04

```bash
sudo apt update
sudo apt install -y apache2
sudo systemctl start apache2
sudo systemctl enable apache2
```

## 4) Deploy the simple HTML page

Copy this repository's `index.html` to Apache’s default web root:

```bash
sudo cp index.html /var/www/html/index.html
```

Set readable permissions:

```bash
sudo chmod 644 /var/www/html/index.html
```

## 5) Verify the website

Open in your browser:

```text
http://<PUBLIC_IP>
```

If setup is correct, you should see the simple EC2 Apache sample page.

## 6) Troubleshooting

- Check Apache status:
  ```bash
  sudo systemctl status httpd    # Amazon Linux
  sudo systemctl status apache2  # Ubuntu
  ```
- Check firewall/security group allows port **80**.
- Restart web server after changes:
  ```bash
  sudo systemctl restart httpd
  # or
  sudo systemctl restart apache2
  ```
