# Running the Antidot Framework Application

Once your Antidot PHP Framework application is ready for production, you need to set up a process manager 
to ensure that the application runs continuously and can automatically recover from failures. This section 
will guide you through the process of running the Antidot Framework server using process management tools 
like Supervisor or Systemd. Additionally, we'll cover how to configure a load balancer, such as Nginx or 
Caddy, to handle incoming requests and distribute the traffic to your Antidot application.

### Supervisor

Supervisor is a process control system for Unix-like operating systems that provides a reliable way 
to manage and monitor processes. To use Supervisor to manage your Antidot Framework application,
follow these steps:

1. Install Supervisor by running the following command:

   ```shell
   sudo apt-get install supervisor
   ```

2. Create a Supervisor configuration file for your application. You can create a file named `antidot.conf`
in the `/etc/supervisor/conf.d/` directory:

   ```shell
   sudo nano /etc/supervisor/conf.d/antidot.conf
   ```

3. In the Supervisor configuration file, add the following configuration:

   ```ini
   [program:antidot]
   command=/usr/bin/php /path/to/your/application/bin/console serve
   directory=/path/to/your/application
   autostart=true
   autorestart=true
   stderr_logfile=/var/log/antidot/error.log
   stdout_logfile=/var/log/antidot/access.log
   ```

   Make sure to replace `/path/to/your/application` with the actual path to your Antidot Framework application.

4. Save the file and exit the text editor.

5. Reload the Supervisor configuration to apply the changes:

   ```shell
   sudo supervisorctl reread
   ```

6. Start the Antidot application process:

   ```shell
   sudo supervisorctl start antidot
   ```

   Your Antidot Framework application should now be running under the control of Supervisor.

### Systemd

Systemd is a suite of basic building blocks for a Linux system that provides a system and service manager. 
To use Systemd to manage your Antidot Framework application, follow these steps:

1. Create a Systemd service unit file for your application. You can create a file named `antidot.service`
in the `/etc/systemd/system/` directory:

   ```shell
   sudo nano /etc/systemd/system/antidot.service
   ```

2. In the Systemd service unit file, add the following configuration:

   ```ini
   [Unit]
   Description=Antidot Framework Application
   After=network.target

   [Service]
   ExecStart=/usr/bin/php /path/to/your/application/bin/console serve
   WorkingDirectory=/path/to/your/application
   Restart=always
   User=your_user
   Group=your_group

   [Install]
   WantedBy=multi-user.target
   ```

   Make sure to replace `/path/to/your/application` with the actual path to your Antidot Framework 
application. Also, replace `your_user` and `your_group` with the appropriate user and group under 
which the application should run.

3. Save the file and exit the text editor.

4. Reload the Systemd configuration to apply the changes:

   ```shell
   sudo systemctl daemon-reload
   ```

5. Start the Antidot application service:

   ```shell
   sudo systemctl start antidot
   ```

   Your Antidot Framework application should now be running as a Systemd service.

### Monitoring the Application

With Supervisor or Systemd in place, your Antidot Framework application will run continuously, 
automatically restarting if it crashes or if the server restarts. You can monitor the application's 
logs to troubleshoot any issues or check its status.

- For Supervisor, the error and access logs specified in the configuration file 
(`/var/log/antidot/error.log` and `/var/log/antidot/access.log`) can be used to monitor 
the application's output.

- For Systemd, you can check the application's logs using the `journalctl` command:

  ```shell
  sudo journalctl -u antidot
  ```

  This will display the logs related to the Antidot Framework application.


### Load Balancer Configuration (Nginx)

To distribute the traffic to your Antidot Framework application and provide additional features like
SSL termination and caching, you can use a load balancer like Nginx. Here's an example configuration 
for Nginx:

```nginx
server {
    listen 80;
    server_name your-domain.com;

    location / {
        proxy_pass http://localhost:3000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }
}
```

In the above configuration, replace `your-domain.com` with your actual domain name or IP address. 
The `proxy_pass` directive specifies the address of your Antidot application server, which is running
on port 3000 by default. Adjust the configuration as necessary if you're using a different port.

### Load Balancer Configuration (Caddy)

If you prefer to use Caddy as your load balancer, here's an example configuration:

```
your-domain.com {
    reverse_proxy localhost:3000
}
```

Replace `your-domain.com` with your actual domain name or IP address. The `reverse_proxy` directive
specifies the address of your Antidot application server, which is running on port 3000 by default.
Adjust the configuration if you're using a different port.

### Conclusion

Running the Antidot Framework application in a production environment requires proper port binding and
load balancer configuration. By specifying a custom port for the server and using tools like Nginx or 
Caddy as load balancers, you can ensure that the application runs on the desired port and handles
incoming requests efficiently. Also crucial for ensuring its availability and resilience. By using process
management tools like Supervisor or Systemd, you can ensure that the application runs continuously,
automatically restarts when necessary, and provides monitoring capabilities.

