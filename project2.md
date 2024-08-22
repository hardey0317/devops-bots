# Setup Multiple Static Websites on a Single Server Using Nginx Virtual Hosts

In this project, i will be completing the concept of subdomains and hosting multiple websites on a single server using Nginx Virtual Host configuration.

|S/N | Project Tasks                                                                            |
|----|------------------------------------------------------------------------------------------|
| 1  |Install and configure Nignx on a server                                                   |
| 2  |Create two website directories with two different website templates.                      |
| 3  |Create two subdomains                                                                     |
| 4  |Add the IP of the server as A record to the two subdomains.                               |
| 5  |Configure the Virtual host to point two subdomains to two different website directories.  |
| 6  |Validate the setup by accessing the subdomains.                                              |
| 7  |Create a certbot SSL certificate for the root Domain.                        |
| 8  |Configure certbot on Nginx for two websites.                                         |
| 9  |Validate the subdomain websites’ SSL using OpenSSL utility. 

## Key Concepts Covered

- AWS (EC2 and Route 53)
- EC2
- Linux(Ubuntu)
- Nginx
- DNS
- **Subdomains**
- SSL (certbot)
- OpenSSL command

## Documentation

- Spin up my Ubuntu server, create an elastic IP, and then link it to my instance.
![pj1](img/pj2.1.png)

### Install Nginx and Setup Your Website

- Execute the following commands.

`sudo apt update`

`sudo apt upgrade`

`sudo apt install nginx`

- Starting My Nginx server by running the `sudo systemctl start nginx` command, enable it to start on boot by executing `sudo systemctl enable nginx`, and then confirm if it's running with the `sudo systemctl status nginx` command.
![pj2](img/pj2.3.png)

- Visit my instances IP address in a web browser to view the default Nginx startup page.
![pj2](img/pj2.2.png)

- I am going to download the website template from tooplate.

- Right click and select **Inspect** from the drop down menu.

- Click on the **Network** tab and then click **Download** button.

- Right click on the website name, select **Copy** and click on **Copy link address**.
![pj2](img/pj2.4.png)

- To install the unzip tool, run the following command: **`sudo apt install unzip`**.

- Execute the command to download and unzip your website files `sudo curl -o /var/www/html/2098_health.zip https://www.tooplate.com/zip-templates/2098_health.zip && sudo unzip -d /var/www/html/ /var/www/html/2098_health.zip && sudo rm -f /var/www/html/2098_health.zip`.
![pj2](img/pj2.5.png)

**Here's an explanation of the command:**

The command **`sudo curl -o /var/www/html/2098_health.zip https://www.tooplate.com/zip-templates/2098_health.zip && sudo unzip -d /var/www/html/ /var/www/html/2098_health.zip && sudo rm -f /var/www/html/2098_health.zip`** performs a series of actions to download, unzip, and clean up a website template file. Here’s a breakdown of each part of the command:

1. **`sudo curl -o /var/www/html/2098_health.zip https://www.tooplate.com/zip-templates/2098_health.zip`**: 
   - `sudo`: Runs the command with superuser (root) privileges.
   - `curl -o /var/www/html/2098_health.zip`: Downloads the file from the specified URL (`https://www.tooplate.com/zip-templates/2098_health.zip`) and saves it as `2098_health.zip` in the `/var/www/html` directory.

2. **`&&`**: Logical AND operator, which ensures that the next command runs only if the previous command succeeds.

3. **`sudo unzip -d /var/www/html/ /var/www/html/2098_health.zip`**:
   - `sudo`: Runs the command with superuser (root) privileges.
   - `unzip -d /var/www/html/`: Extracts the contents of the zip file into the `/var/www/html/` directory.
   - `/var/www/html/2098_health.zip`: Specifies the path to the zip file to be unzipped.

4. **`&&`**: Logical AND operator, which ensures that the next command runs only if the previous command succeeds.

5. **`sudo rm -f /var/www/html/2098_health.zip`**:
   - `sudo`: Runs the command with superuser (root) privileges.
   - `rm -f`: Removes (deletes) the specified file forcefully (without prompting for confirmation).
   - `/var/www/html/2098_health.zip`: Specifies the path to the zip file to be deleted.

In summary, this command downloads the website template file, extracts its contents to the web server directory, and then deletes the downloaded zip file to clean up the directory.

---
- Download the 2nd website template by running the following command:

```
sudo curl -o /var/www/html/2132_clean_work.zip https://www.tooplate.com/zip-templates/2132_clean_work.zip && sudo unzip -d /var/www/html/ /var/www/html/2132_clean_work.zip && sudo rm -f /var/www/html/2132_clean_work.zip
```
> [!NOTE]
Replace the placeholders in the code with your own website URL. For example, substitute **`https://www.tooplate.com/zip-templates/2132_clean_work.zip`** with your chosen URL, and change **`2132_clean_work.zip`** accordingly.

- To set up your website's configuration, start by creating a new file in the Nginx sites-available directory. Use the following command to open a blank file in a text editor: **`sudo nano /etc/nginx/sites-available/cleaning`**

![pj2](img/pj2.6.png)

- Edit the `root` directive within your server block to point to the directory where your downloaded website content is stored.

![pj2](img/pj2.7.png)

- Configure your second website by creating a new file in the Nginx sites-available directory with the following command: `sudo nano /etc/nginx/sites-available/health`.

![pj2](img/pj2.8.png)

- Create a symbolic link for both websites by running the following command.
`sudo ln -s /etc/nginx/sites-available/cleaning /etc/nginx/sites-enabled/`
`sudo ln -s /etc/nginx/sites-available/health /etc/nginx/sites-enabled/`

![pj2](img/pj22.9.png)

- Run the **`sudo nginx -t`** command to check the syntax of the Nginx configuration file.

- Delete the default files in the sites-available and sites-enabled directories by executing the following commands:

```
sudo rm /etc/nginx/sites-available/default
sudo rm /etc/nginx/sites-enabled/default
```

- Restart the Nginx server by executing the following command: **`sudo systemctl restart nginx`**.

---

### Create An A Record

I will be setting up a DNS record. I did this by buying my domain from Qservers and then moving hosting to AWS Route 53, where I set up an A record.

- In route 53, I select the domain name and click on **Create record**.
I paste in my IP address and then click on **Create record**.
Then i create record again, to create the record for my sub domain.
After creating my my record, i Input the **Record name①**, paste my **IP address②** and then click on **Create records③**.

![pj2](img/pj2.10.png)

- I Open my terminal and run **`sudo nano /etc/nginx/sites-available/cleaning`** to edit my settings. I Enter the name of My domain and then save My settings.
![pj2](img/pj2.11.png)

- I run **`sudo nano /etc/nginx/sites-available/health`** to edit my settings. I enter the name of my domain and then save my settings.
![pj2](img/pj2.12.png)

- I restart my nginx server by running the **`sudo systemctl restart nginx`** command.

- i go to my domain name in a web browser to verify that my website is accessible.

![pj2](img/pj2.13.png)
![pj2](img/pj2.14.png)

> [!NOTE]
You may notice the sign that says **Not secure**. Next, I will use certbot to obtain the SSL certificate necessary to enable HTTPS on my site.

---

### Install certbot and Request For an SSL/TLS Certificate

- Install certbot by executing the following commands:
`sudo apt update`
`sudo apt install python3-certbot-nginx`
`sudo certbot --nginx`

![pj2](img/PJ2.15.png)

- I execute the **`sudo certbot --nginx`** command to request my certificate. Follow the instructions provided by certbot and select the domain name for which you would like to activate HTTPS.

![pj2](img/PJ2.16.png)

> [NOTE]
In this case, i simply pressed Enter to accept the default selection (**all available records**).

- I verify the website's SSL using the OpenSSL utility with the command: **`openssl s_client -connect cleaning.cloudghoul.online:443`**.



- Visit **`My domain`** to view my websites.

![pj2](img/pj2.18.png)
![pj2](img/pj2.19.png)

---
---

#### The End Of Project 2








