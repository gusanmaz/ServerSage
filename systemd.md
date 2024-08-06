# Systemd Service Management

Systemd is an init system and system manager that is widely used in modern Linux distributions. This guide will cover how to create, manage, and monitor systemd services.

## Table of Contents

1. [Creating a Systemd Service](#creating-a-systemd-service)
2. [Managing Services with Systemctl](#managing-services-with-systemctl)
3. [Viewing Service Logs with Journalctl](#viewing-service-logs-with-journalctl)

## Creating a Systemd Service

1. Create a new service file in `/etc/systemd/system/` with a `.service` extension:

   ```bash
   sudo nano /etc/systemd/system/myapp.service
   ```

2. Add the following content to the file:

   ```ini
   [Unit]
   Description=My Custom Application
   After=network.target

   [Service]
   ExecStart=/path/to/your/application
   Restart=always
   User=youruser

   [Install]
   WantedBy=multi-user.target
   ```

   Adjust the `ExecStart` and `User` fields as necessary for your application.

3. Save the file and exit the editor.

4. Reload the systemd manager configuration:

   ```bash
   sudo systemctl daemon-reload
   ```

## Managing Services with Systemctl

Systemctl is the main command used to manage systemd services. Here are some common operations:

1. Start a service:
   ```bash
   sudo systemctl start myapp.service
   ```

2. Stop a service:
   ```bash
   sudo systemctl stop myapp.service
   ```

3. Restart a service:
   ```bash
   sudo systemctl restart myapp.service
   ```

4. Enable a service to start on boot:
   ```bash
   sudo systemctl enable myapp.service
   ```

5. Disable a service from starting on boot:
   ```bash
   sudo systemctl disable myapp.service
   ```

6. Check the status of a service:
   ```bash
   sudo systemctl status myapp.service
   ```

7. List all active services:
   ```bash
   systemctl list-units --type=service
   ```

## Viewing Service Logs with Journalctl

Journalctl is used to query and display logs from journald, systemd's logging service.

1. View logs for a specific service:
   ```bash
   journalctl -u myapp.service
   ```

2. Follow logs in real-time:
   ```bash
   journalctl -u myapp.service -f
   ```

3. View logs since the last boot:
   ```bash
   journalctl -u myapp.service -b
   ```

4. View logs within a specific time range:
   ```bash
   journalctl -u myapp.service --since "2023-08-01" --until "2023-08-07"
   ```

5. Display logs in a more readable format:
   ```bash
   journalctl -u myapp.service -o short-precise
   ```

Remember to replace `myapp.service` with the name of your actual service in all these commands.

By mastering these systemd commands, you'll be able to efficiently manage and monitor services on your VPS.