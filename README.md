# LAMP-Stack-Home-Lab

<h1>LAMP Stack</h1>


<h2>Description</h2>
This home lab is built around the LAMP stack (Linux, Apache, MySQL/MariaDB, PHP), a fundamental component of contemporary web development. It guides users through the core aspects of the LAMP environment, encompassing Linux OS management, as well as development with HTML, SQL, PHP, and related technologies. Utilizing CentOS (Linux), Apache, MariaDB, and PHP, the lab introduces the essential components and facilitates the exploration of HTML and SQL.
<br />


<h2>Languages and Utilities Used</h2>

- <b>HTML</b> 
- <b>SQL</b>
- <b>PHP</b>
- <b>Javascript</b>

<h2>Environments Used </h2>

- <b>Fedora VM (Linux)</b>

<h2>Procedure walk-through:</h2>

<h3>Task 1: Set Up Fedora VM</h3>

1. **Build Fedora VM**:
   - If you haven't already, set up a clean Fedora virtual machine (VM)

2. **Log in and Gain Root Access**:
   - Log in to the Fedora VM.
   - Gain root access by running `sudo -i` or by prefixing commands with `sudo`.

3. **Update the Operating System**:
   - Update the operating system by running:
     ```bash
     sudo dnf update
     ```
   - After the update completes, reboot the system.

> Note: Replace any reference to `192.168.x.x` with the IP address of your Fedora VM throughout this guide.

<h3>Task 2: LAMP Setup</h3>

The objective is to install the required software to get the LAMP stack (Linux, Apache, MySQL, and PHP) up and running on Fedora Linux.

1. **Install Apache, MySQL, and PHP**:
   - Install the necessary packages with one command:
     ```bash
     sudo dnf install -y httpd mariadb-server mariadb php php-pdo php-pdo_mysql
     ```
   - For Debian-based systems (e.g., Ubuntu), use:
     ```bash
     sudo apt -y install apache2 mariadb-server mariadb php libapache2-mod-php php-mysql
     ```

2. **Start and Enable Services**:
   - Start Apache:
     ```bash
     sudo systemctl start httpd
     ```
   - Enable Apache to start on boot:
     ```bash
     sudo systemctl enable httpd
     ```
   - Start MariaDB:
     ```bash
     sudo systemctl start mariadb
     ```
   - Enable MariaDB to start on boot:
     ```bash
     sudo systemctl enable mariadb
     ```
   - PHP starts automatically with Apache.

3. **Verify Services**:
   - Check that services are running by checking ports 3306 and 80:
     ```bash
     netstat –plant | grep -E '3306|80|443'
     ```

4. **Logs and Configuration**:
   - Apache logs: `/var/log/httpd/` (includes `access_log` and `error_log`)
     - Monitor errors:
       ```bash
       tail -f /var/log/httpd/error_log
       ```
   - MariaDB logs: `/var/log/mariadb/mariadb.log`
   - Apache configuration: `/etc/httpd/conf/httpd.conf`
   - PHP configuration: `/etc/php.ini`
   - Document root: `/var/www/html`

> Note: Port 443 will not show as listening until later. Other ports may not be active until interacted with.

<h3>Task 3: Configuring the Host Firewall</h3>

Configure the firewall to allow HTTP and HTTPS traffic using `firewalld`.

1. **Allow HTTP and HTTPS Traffic**:
   - Run the following commands to allow HTTP and HTTPS through the firewall:
     ```bash
     sudo firewall-cmd --permanent --add-service=http
     sudo firewall-cmd --permanent --add-service=https
     sudo firewall-cmd --reload
     ```

2. **View Zone Configurations**:
   - To see the zone configurations:
     ```bash
     sudo firewall-cmd --list-all-zones
     ```

3. **View Permitted Services and Ports**:
   - To see the permitted services:
     ```bash
     sudo firewall-cmd --list-services
     ```
   - To see the permitted ports:
     ```bash
     sudo firewall-cmd --list-ports
     ```

4. **Manage Firewall Service**:
   - View the status of the firewall:
     ```bash
     sudo systemctl status firewalld
     ```
   - Stop the firewall service:
     ```bash
     sudo systemctl stop firewalld
     ```
   - Start the firewall service:
     ```bash
     sudo systemctl start firewalld
     ```
   - Restart the firewall service:
     ```bash
     sudo systemctl restart firewalld
     ```


<h3>Task 4: Create a Default Page</h3>

Create a basic HTML page to serve as the default web page.

1. **Create `index.htm`**:
   - In the `/var/www/html` directory, create a file named `index.htm` with the following content:
     ```html
     <!DOCTYPE html>
     <html>
     <head>
       <title>My Hello World Page</title>
     </head>
     <body>
       <p>Hello World!</p>
       <p>This is my first web page</p>
     </body>
     </html>
     ```

2. **Verify the Page**:
   - If everything is configured correctly, running the following command should return the `index.htm` file:
     ```bash
     curl 127.0.0.1/index.htm
     ```

3. **Access the Page from Another VM**:
   - Open the Kali Linux VM.
   - Open Firefox within Kali and navigate to the Fedora system's IP address:
     - Enter the IP address of the Fedora system to see the default Apache page.
     - Use `http://<IP address of Fedora Server>/index.htm` to see the HTML document created above.

<h3>Task 5: Configuring PHP</h3>

Ensure the PHP engine in Apache is working correctly.

1. **Verify Services**:
   - Return to the Fedora machine to confirm that the services are listening and there are no errors.

2. **Create a PHP Test File**:
   - Navigate to Apache’s home directory: `/var/www/html`
   - Create a file called `phptest.php` with the following content:
     ```php
     <?php phpinfo(); ?>
     ```

3. **Test the PHP File**:
   - Use the `curl` command to test the PHP file:
     ```bash
     curl 127.0.0.1/phptest.php
     ```
   - Note: The output will be long and hard to decipher.

4. **Use a Web Browser for Testing**:
   - Open the web browser in your Kali Linux virtual machine.
   - Browse to `http://<IP address of Fedora Server>/phptest.php`, which should return a complete output of PHP information associated with the specific build.

5. **Troubleshoot if Needed**:
   - If the PHP information does not display correctly, run the following command on the Fedora machine and reload the same page in the browser to narrow down the issue:
     ```bash
     tail -f /var/log/httpd/error_log
     ```


<h3>Task 6: Change the Default Page</h3>

The Apache server is set by default to serve the index.html page SPECIFICALLY.

1. **Verify Apache Default Page**:
   - Copy `index.htm` to `index.html` in the `/var/www/html` directory:
     ```bash
     sudo cp /var/www/html/index.htm /var/www/html/index.html
     ```
   - Open Firefox in the Kali Linux VM and navigate to `http://<IP address of Fedora Server>/`. This should display the default page.

2. **Test PHP as Default Page**:
   - Delete the `index.html` file:
     ```bash
     sudo rm /var/www/html/index.html
     ```
   - Copy `phptest.php` to `index.php`:
     ```bash
     sudo cp /var/www/html/phptest.php /var/www/html/index.php
     ```
   - Navigate to `http://<IP address of Fedora Server>/` in the Kali Firefox browser. This should now display the PHP information page.














<!--

<p align="center">
Launch the utility: <br/>
<img src="linked file" height="80%" width="80%" alt="Alt photo name"/>
<br />
<br />
Select the disk:  <br/>
<img src="linked file" height="80%" width="80%" alt="Alt photo name"/>
<br />
<br />

</p>


 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>
