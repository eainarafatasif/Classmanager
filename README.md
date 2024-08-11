# classmanager
A Student-Teacher Portal built using HTML, CSS, Python and Django

Class Manager is a Student-Teacher Portal where techers and student can sign up and teachers can add students in their class.

Class Manager contains more features like:
1. Teachers can add or edit their student's marks after adding them in the class.
2. Teachers can also write notice which will be sent to all students in their class.
3. Teachers can upload assignments which will be sent to all students in their class and students can download the assignments.
4. Students can also submit their assignment but once submitted can't be changed later.
5. Teachers can also see all the mark given by them to a student through their marks profile and can edit them if necessary.
6. Students can see marks given to them by teachers in marks section.
7. Student can see the list of all teachers in the portal and can message any of them.
8. Teachers can see all the messages written by students in Inbox.
9. User can search any student through search option at the top of the student list page. Same goes for teachers list.
10. User can see their profile through profile option.
11. User can add profile picture and edit their profile through edit profile.
12. User can also change their password if necessary.

## Screenshots

### Home Page

![classmanager](https://user-images.githubusercontent.com/59278577/85334196-73729680-b4f8-11ea-90b6-a42336e1d7dd.PNG)
![classmanager-homepage](https://user-images.githubusercontent.com/59278577/85334362-c2203080-b4f8-11ea-973c-e9ff6b481810.PNG)
![classmanager-homepage1](https://user-images.githubusercontent.com/59278577/85334481-f398fc00-b4f8-11ea-88fc-ba3371076930.PNG)

### Login Page

![classmanager-loginpage](https://user-images.githubusercontent.com/59278577/85334573-1deab980-b4f9-11ea-86b9-4e1367e78057.PNG)

### Options Avalaible for teachers

![classmanager-teacheroptions](https://user-images.githubusercontent.com/59278577/85334843-8cc81280-b4f9-11ea-8162-2ac5756f3884.PNG)

### Options available for students

![classmanager-studentsoptionlist](https://user-images.githubusercontent.com/59278577/85336072-ac603a80-b4fb-11ea-87b5-a942ce294a2b.PNG)

### Profile Page
User can edit their profile by clicking on Edit profile

![classmanager-profilepic](https://user-images.githubusercontent.com/59278577/85335035-f34d3080-b4f9-11ea-9478-bc4632798eef.PNG)

### Marks given by teacher
Here, teachers can see all the marks given by them to a particular student of her class and can update them.

![classroom-marksgiven](https://user-images.githubusercontent.com/59278577/85335383-8d14dd80-b4fa-11ea-8257-797c5a0fe52a.PNG)

### Marks list obtained by Student

![classmanager-marksobtained](https://user-images.githubusercontent.com/59278577/85335564-d6fdc380-b4fa-11ea-8219-09d40f96f8e7.PNG)

### Assignments uploaded by teacher
Students can download assignment given by their teacher and can submit their work too.

![classmanager-assignmentpage](https://user-images.githubusercontent.com/59278577/85335929-6c995300-b4fb-11ea-883d-48ab096dd89a.PNG)

### Assignment submission by students
Teacher can check submissions made by their students  and can give them marks for that.
![classmanager-submissionlist](https://user-images.githubusercontent.com/59278577/85335777-2e039880-b4fb-11ea-8d7d-0edc517ac11e.PNG)

Class Manager uses the multi-user concept of Django where student and teacher are different types of user and have different functionalities.
Also adding features like notice, messages, Assignment, adding students to the class etc. requires a lot of Django concepts.
Projects like Class Manager is a great choice to practice your Django skills and test yourself. 

To deploy your Django project from GitHub using Nginx, MySQL, and an SSL certificate on an Ubuntu server, follow these steps:

### Prerequisites
- An Ubuntu server with root or sudo access.
- A domain name pointing to your server's IP address.
- A GitHub repository containing your Django project.
- A virtual environment set up for your project.
  
### 1. **Update and Install Dependencies**
   ```bash
   sudo apt update && sudo apt upgrade -y
   sudo apt install python3-pip python3-dev libpq-dev nginx curl git
   sudo apt install mysql-server libmysqlclient-dev
   ```
Once all the packages are installed, you can start the Apache service and configure it to run on startup by entering the following commands:

```bash
 systemctl start Nginx
 systemctl enable Nginx

```
Now Allow the firewall
```bash
ufw allow 80
ufw allow 443

```

### 2. **Clone Your GitHub Repository**
   ```bash
   cd /var/www/html/
   sudo git clone https://github.com/unfoldadmin/django-unfold.git
   cd django-unfold
   ```

### 3. **Set Up Virtual Environment**
   ```bash
   sudo apt install python3-venv
   python3 -m venv projectenv
   source projectenv/bin/activate
   pip install django
   pip install mysqlclient
   pip install --upgrade pip
   pip install -r requirements.txt
   ```

### 4. **Configure Django Settings**
- Update your `settings.py` to include your domain in the `ALLOWED_HOSTS`:
   ```python
   ALLOWED_HOSTS = ['yourdomain.com', 'www.yourdomain.com']
   ```
- Ensure your `DATABASES` setting points to your MySQL database.

### 5. **Set Up MySQL Database**
   ```bash
   sudo apt install mysql-server
   sudo mysql_secure_installation
   ```
   - Log in to MySQL and create a database and user:
     ```bash
     sudo mysql -u root -p
     ```
     ```sql
     CREATE DATABASE django_unfold;
     CREATE USER 'youruser'@'localhost' IDENTIFIED BY 'yourpassword';
     GRANT ALL PRIVILEGES ON django_unfold.* TO 'youruser'@'localhost';
     FLUSH PRIVILEGES;
     EXIT;
     ```
   - Update your Django `settings.py` to connect to the MySQL database.

  ```bash
  DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': 'your_db_name',
        'USER': 'your_db_user',
        'PASSWORD': 'your_db_password',
        'HOST': 'localhost',
        'PORT': '3306',
    }
}

 ```

### 6. **Run Migrations and Collect Static Files**
   ```bash
   python manage.py makemigrations
   python manage.py migrate
   python manage.py collectstatic
   ```

### 7. **Set Up Gunicorn**
   ```bash
   pip install gunicorn
   gunicorn --bind 0.0.0.0:8000 djangoproject.wsgi
   ```
Next, you need to create a systemd service file to manage the Django. First, create a Gunicorn socket file.
 ```bash
    nano /etc/systemd/system/gunicorn.socket
```
Add the following configuration.
  ```bash 
        [Unit]
        Description=gunicorn socket
        [Socket]
        ListenStream=/run/gunicorn.sock
        [Install]
        WantedBy=sockets.target
   ```
   - Create a system service file for Gunicorn:
     ```bash
     sudo nano /etc/systemd/system/gunicorn.service
     ```
     - Add the following:
       ```bash
       [Unit]
        Description=gunicorn daemon
        Requires=gunicorn.socket
        After=network.target
        [Service]
        User=root
        Group=www-data
        WorkingDirectory=/root/project
                ExecStart=/root/project/djangoenv/bin/gunicorn --access-logfile - --workers 3 --bind unix:/run/gunicorn.sock djangoproject.wsgi:application
            [Install]
        WantedBy=multi-user.target
       ```
     - Enable and start the Gunicorn service:
       ```bash
       sudo systemctl start gunicorn
       sudo systemctl enable gunicorn
       systemctl start gunicorn.socket
       systemctl enable gunicorn.socket
       
       ```

### 8. **Configure Nginx**
   - Create an Nginx server block:
     ```bash
     sudo nano /etc/nginx/sites-available/django_unfold
     ```
     - Add the following:
       ```nginx
       server {
           listen 80;
           server_name yourdomain.com www.yourdomain.com;

           location = /favicon.ico { access_log off; log_not_found off; }
           location /static/ {
               root /var/www/html/django-unfold;
           }

           location / {
               include proxy_params;
               proxy_pass http://unix:/var/www/html/django-unfold/gunicorn.sock;
           }
       }
       ```
   - Enable the configuration and restart Nginx:
     ```bash
     sudo ln -s /etc/nginx/sites-available/django_unfold /etc/nginx/sites-enabled
     sudo nginx -t
     sudo systemctl restart nginx
     ```

Ensure Correct Permissions for Static and Media Files
If you have static and media directories outside of the main project directory, set their ownership and permissions similarly:

```bash
sudo chown -R www-data:www-data /var/www/html/django-unfold/
sudo chmod -R 755 /var/www/html/django-unfold/

```

### 9. **Obtain an SSL Certificate**
   - Install Certbot:
     ```bash
     sudo apt install certbot python3-certbot-nginx
     ```
   - Obtain and install the SSL certificate:
     ```bash
     sudo certbot --nginx -d yourdomain.com -d www.yourdomain.com
     ```
   - Follow the prompts to complete the SSL installation. Certbot will automatically configure your Nginx server block to use SSL.

### 10. **Testing**
   - Visit `https://yourdomain.com` to ensure everything is working correctly.

### 11. **Set Up Automatic Renewals for SSL**
   ```bash
   sudo systemctl status certbot.timer
   ```

This setup will get your Django project deployed with Nginx, MySQL, and an SSL certificate on Ubuntu.

