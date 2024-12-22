# Altschool Cloud Engineering Second Semester Examination Project

## Documentation of the Project: Provision a Linux Server with a simple HTML page

This documentation provides a step-by-step guide to how I:
1. Provisioned a Linux server on AWS.
2. Set up a web server.
3. Deployed an HTML page.
4. Configured networking.
Additionally, it includes a **bonus task** for configuring HTTPS using Let's Encrypt.

## STEP 1: Provisioning the Server

### Launching an EC2 Instance

1. Navigate to [aws.amazon.com](https://aws.amazon.com) and log in to the AWS Management Console.
2. Click on the **"EC2"** tab and select **"Instances"** from the dropdown menu.
3. Click on **"Launch Instance"**, provide a name for the instance, and select **"Ubuntu Server 24.04 LTS (HVM)"** as the Amazon Machine Image (AMI).
4. Choose the **t2.micro** instance type and configure the instance details.
5. Create a new key pair for secure access to the instance:
   - Provide a key pair name.
   - Leave other options as default.
   - Network options can remain unchanged at this stage.
6. Click **"Launch Instance"** and wait for it to load.
7. Navigate to **"View all Instances"** and refresh the dashboard to see the created instance running.
8. Wait until the **Status Check** column changes from **'Initializing'** to **'2/2 checks passed'**.

## STEP 2: Web Server Setup

### Connect to the EC2 Instance

1. Select the created instance and click **"Connect"**. This opens a panel to pick your desired connection method.
2. Use the **EC2 Instance Connect** option to connect to the instance. This opens the command-line interface.

### Install Web Server

Let's use the Apache2 web server:

- Update the package list:  
  ```bash
  sudo apt update
- Install Apache:  
  ```bash
  sudo apt install apache2
- Verify Apache is running:  
  ```bash
  sudo service apache2 status
  
## STEP 3: HTML Page Deployment

### Create a Simple HTML Page

1. Build a simple HTML landing page using Visual Studio Code or any text editor.  
   My `index.html` file can be found here ![...](index.html)

2. Navigate to and open the default `index.html` file in the `/var/www/html` directory using the nano text editor:  
   ```bash
   sudo nano /var/www/html/index.html

### 3. Replace the Content of the `index.html` File  
Replace the default content of the `index.html` file with the HTML file you created earlier.

### 4. Deploy the HTML Page  
- Save and close the `index.html` file:  
  - Use `Ctrl+O` to save.  
  - Use `Ctrl+X` to close.  
- Restart Apache to apply the changes:  
  ```bash
  sudo service apache2 restart


## STEP 4: Networking

### Configure Security Group

1. Go to the EC2 dashboard, select the instance, and note the security group associated with it.
2. Navigate to the side menu bar, open the **Network and Security** dropdown menu, and click the **Security Groups** option.
3. Select the security group associated with the instance and choose **"Edit inbound rules"**.
4. Add a new rule to allow HTTP traffic (port 80):  
   - **Protocol**: TCP  
   - **Port range**: 80  
   - **Source**: 0.0.0.0/0  
5. Save the rules.

### To Get the Public IP Address

1. Go to the EC2 dashboard and select the instance.
2. The public IPv4 address is displayed among other instance information. Scroll down to view it.
3. Copy the IP address and search in any browser. The public IP address for my instance is: **3.85.13.116**
   ![My Simple Landing Page shown in my browser]()


## STEP 5: Bonus Task – Configure HTTPS for the Web Server Using "Let’s Encrypt" Free SSL Certificate

## Steps to Configure HTTPS

1. Register and Activate an Account on [freedns.afraid.org](https://freedns.afraid.org) to register and activate a free account.

2. On the FreeDNS platform, create a subdomain under the **"mooo"** domain and add your server’s public IP address as the destination.  
   - For this project, my subdomain was: **kemdirim.mooo.com**.

3. Go to the security group associated with the instance and select **"Edit inbound rules"**.
   - Add a new rule to allow HTTPS traffic (port 443):  
     - **Protocol**: TCP  
     - **Port range**: 443  
     - **Source**: 0.0.0.0/0  

4. On the EC2 instance CLI, install the Let's Encrypt client using the following command:  
     ```bash
     sudo apt install certbot
     ```
5. Run the Certification Process with this command:
     ```bash
     sudo certbot --apache
     ```
6. Follow the prompts provided by Certbot to obtain an SSL certificate.

7. Create a New Apache Configuration File for HTTPS
   - Open the default SSL configuration file using the nano text editor:  
     ```bash
     sudo nano /etc/apache2/sites-available/default-ssl.conf
     ```
8. Add the Following Lines to the `default-ssl.conf` Configuration File

```apache
<VirtualHost *:443>
  ServerName Kemdirim.mooo.com
  DocumentRoot /var/www/html

  SSLEngine on
  SSLCertificateFile /etc/letsencrypt/live/Kemdirim.mooo.com/cert.pem
  SSLCertificateKeyFile /etc/letsencrypt/live/Kemdirim.mooo.com/privkey.pem
</VirtualHost>
```

*(Note: Replace `Kemdirim.mooo.com` with your own domain name.)*

9.  - Save the changes using `Ctrl+O`.  
    - Close the file using `Ctrl+X`.

10. Restart Apache to Apply Changes using the following command:  
      ```bash
      sudo service apache2 restart
      ```
11. Test the Website: Open any browser and search for the domain name that was created. 
    - For this project, the URL is: [https://kemdirim.mooo.com/](https://kemdirim.mooo.com/)

---

## Deliverables

1. **Public IP Address**:  
   `3.85.13.116`

2. **URL**:  
   [https://kemdirim.mooo.com/](https://kemdirim.mooo.com/)

3. **Screenshot of the HTML Page in a Browser**:  
   *(Attach the screenshot here.)*



