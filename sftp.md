# SFTP Usage Guide

SFTP (SSH File Transfer Protocol) is a secure file transfer protocol that runs over SSH. This guide will cover how to use SFTP to transfer files to and from your VPS.

## Table of Contents

1. [Connecting to a Remote Server](#connecting-to-a-remote-server)
2. [Navigating Directories](#navigating-directories)
3. [Transferring Files](#transferring-files)
4. [SFTP Command Reference](#sftp-command-reference)

## Connecting to a Remote Server

1. Open a terminal on your local machine.

2. Use the following command to connect to your VPS:

   ```bash
   sftp username@your_server_ip
   ```

   Replace `username` with your actual username and `your_server_ip` with your server's IP address or domain name.

3. If this is your first time connecting, you may see a fingerprint warning. Type `yes` to continue.

4. Enter your password when prompted.

## Navigating Directories

Once connected, you can navigate both local and remote directories:

- `pwd`: Print working directory (remote)
- `lpwd`: Print working directory (local)
- `ls`: List files and directories (remote)
- `lls`: List files and directories (local)
- `cd directory`: Change directory (remote)
- `lcd directory`: Change directory (local)

## Transferring Files

To transfer files between your local machine and the VPS:

1. Upload a file:
   ```bash
   put localfile.txt
   ```

2. Download a file:
   ```bash
   get remotefile.txt
   ```

3. Upload a directory recursively:
   ```bash
   put -r localdirectory
   ```

4. Download a directory recursively:
   ```bash
   get -r remotedirectory
   ```

## SFTP Command Reference

Here are some other useful SFTP commands:

- `mkdir directory`: Create a directory on the remote server
- `rmdir directory`: Remove an empty directory on the remote server
- `rm file`: Remove a file on the remote server
- `rename oldname newname`: Rename a file on the remote server
- `chmod mode file`: Change permissions of a file on the remote server
- `exit` or `bye`: Exit the SFTP session

Tips:
- Use `!command` to execute a command on your local machine without exiting SFTP.
- Use `help` or `?` to see a list of available commands.

Remember, SFTP runs over SSH, so it uses the same authentication methods. If you've set up SSH key authentication, SFTP will use that automatically.

By mastering these SFTP commands, you'll be able to efficiently transfer files between your local machine and your VPS securely.