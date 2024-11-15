---
layout: post
title: "AWS Cloud에 project(workwise) 올리기"
date: 2024-11-14
categories: [AWS Cloud]
tags: [AWS Cloud]
---

Below are the detailed steps to deploy a Django project on an EC2 instance using Nginx and Gunicorn.

---

### 1. Launch EC2 Instance and Configure Ports

- In the EC2 security group, allow inbound rules for **HTTP (80)**, **HTTPS (443)**, and **SSH (22)** to enable external access.

### 2. Connect to the EC2 Instance via SSH

- Use the **.pem key file** downloaded when launching the instance to connect to the EC2 instance from your local machine.

```bash
ssh -i /path/to/your-key.pem ubuntu@your-ec2-public-ip
```

### 3. Access EC2 Instance via SSH

- Connect to the instance using the public IP address of the EC2 instance.

```bash
ssh -i /path/to/your-key.pem ubuntu@your-ec2-public-ip
```

### 4. Update System and Install Required Packages

- Update the system and install the necessary packages.

```bash
sudo apt update
sudo apt install -y python3-pip python3-venv nginx postgresql postgresql-contrib libpq-dev git
```

### 5. Clone the Project Repository from GitHub

- Use a **Personal Access Token** to clone the repository from GitHub. Generate this token in **Developer Settings** under **Personal access tokens** in your GitHub account.

```bash
git clone https://<your-github-token>@github.com/your-repo-url.git
```

### 6. Move to the Project Folder

```bash
cd workwise/
```

### 7. Create a Python Virtual Environment

- Create a virtual environment to keep project dependencies isolated.

```bash
python3 -m venv venv
```

### 8. Activate the Virtual Environment

```bash
source venv/bin/activate
```

### 9. Navigate to the Project Directory

- Go to the `workwise-01/` directory within the project folder.

```bash
cd workwise-01/
```

### 10. Install Python Dependencies

- Install the required packages listed in `requirements.txt`.

```bash
pip install -r requirements.txt
```

### 11. Set Up PostgreSQL

- Configure PostgreSQL as the database for Django.
- Create a database and user in PostgreSQL:
    
    ```bash
    sudo -i -u postgres
    psql
    CREATE DATABASE yourdbname;
    CREATE USER yourdbuser WITH PASSWORD 'yourpassword';
    ALTER ROLE yourdbuser SET client_encoding TO 'utf8';
    ALTER ROLE yourdbuser SET default_transaction_isolation TO 'read committed';
    ALTER ROLE yourdbuser SET timezone TO 'UTC';
    GRANT ALL PRIVILEGES ON DATABASE yourdbname TO yourdbuser;
    \q
    exit
    ```
    
- Update the database settings in Django’s `settings.py`:
    
    ```python
    DATABASES = {
        'default': {
            'ENGINE': 'django.db.backends.postgresql',
            'NAME': 'yourdbname',
            'USER': 'yourdbuser',
            'PASSWORD': 'yourpassword',
            'HOST': 'localhost',
            'PORT': '5432',
        }
    }
    ```
    

### 12. Update Django Project Settings

- Add the EC2 public IP to `ALLOWED_HOSTS` in Django’s `settings.py`.

```python
ALLOWED_HOSTS = ['your-ec2-public-ip']
```

- **Collect Static Files**:
Run the following command to collect static files so Nginx can serve them.
    
    ```bash
    python manage.py collectstatic
    ```
    

### 13. Migrate Database and Create Superuser

- Apply migrations to set up the database and create an admin account.

```bash
python manage.py migrate
python manage.py createsuperuser
```

### 14. Run Application with Gunicorn

- Use Gunicorn to run the Django application.

```bash
gunicorn --bind 0.0.0.0:8000 backend.wsgi:application
```

- If there’s an error, use the full path to Gunicorn:

```bash
/home/ubuntu/workwise/venv/bin/python -m gunicorn --bind 0.0.0.0:8000 backend.wsgi:application
```

### 15. Configure Nginx

- Edit the `/etc/nginx/sites-available/default` file to configure Nginx to serve the Django application through Gunicorn.

```
server {
    listen 80;
    server_name your-ec2-public-ip;

    location / {
        proxy_pass http://127.0.0.1:8000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }

    location /static/ {
        alias /home/ubuntu/workwise/workwise-01/backend/static/;
    }

    location /media/ {
        alias /home/ubuntu/workwise/workwise-01/backend/media/;
    }
}
```

### 16. Enable and Restart Nginx (** this is important when the nginx server is not working properly **)

- Test the Nginx configuration:

```bash
sudo nginx -t
```

- Enable the Nginx configuration by creating a symbolic link and restart Nginx.

```bash
sudo ln -s /etc/nginx/sites-available/default /etc/nginx/sites-enabled
sudo systemctl restart nginx
```

### 17. Configure Gunicorn as a Service (Optional)

- Set up Gunicorn as a systemd service for better process management and to start the application automatically on server reboot.
- Create a **Gunicorn service file**:
    
    ```bash
    sudo nano /etc/systemd/system/gunicorn.service
    ```
    
    Add the following content:
    
    ```
    [Unit]
    Description=gunicorn daemon
    After=network.target
    
    [Service]
    User=ubuntu
    Group=www-data
    WorkingDirectory=/home/ubuntu/workwise/workwise-01/backend
    ExecStart=/home/ubuntu/workwise/venv/bin/gunicorn --workers 3 --bind unix:/home/ubuntu/workwise/workwise-01/backend/backend.sock backend.wsgi:application
    
    [Install]
    WantedBy=multi-user.target
    ```
    
- Enable and start the Gunicorn service:
    
    ```bash
    sudo systemctl start gunicorn
    sudo systemctl enable gunicorn
    ```
    

### 18. Update Nginx to Use Socket (Optional)

- For socket-based communication, update the Nginx configuration.
    
    ```
    server {
        listen 80;
        server_name your-ec2-public-ip;
    
        location / {
            proxy_pass http://unix:/home/ubuntu/workwise/workwise-01/backend/backend.sock;
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto $scheme;
        }
    
        location /static/ {
            alias /home/ubuntu/workwise/workwise-01/backend/static/;
        }
    
        location /media/ {
            alias /home/ubuntu/workwise/workwise-01/backend/media/;
        }
    }
    ```
    
- Restart Nginx to apply the changes:
    
    ```bash
    sudo systemctl restart nginx
    ```