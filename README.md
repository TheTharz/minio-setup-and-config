# MinIO Setup Guide

## Overview

MinIO is a high-performance, S3-compatible object storage system. This guide provides step-by-step instructions to set up a MinIO server on your local machine or a remote server.

## Prerequisites

Before setting up MinIO, ensure you have the following:

- A server or local machine with a 64-bit operating system (Linux, macOS, or Windows)
- Minimum of 1 GB RAM and 1 CPU
- Disk space sufficient to store your data
- Access to the terminal or command line interface

## Installation

### Linux & macOS

1. **Download MinIO binary**

   ```sh
   wget https://dl.min.io/server/minio/release/linux-amd64/minio
   chmod +x minio
   ```

2. **Run MinIO server**

   ```sh
   ./minio server /data
   ```

   Replace `/data` with the directory where you want to store your data.

### Windows

1. **Download MinIO executable**

   Download the MinIO executable from the [MinIO official website](https://min.io).

2. **Run MinIO server**

   Open the Command Prompt and run:

   ```sh
   minio.exe server D:\data
   ```

   Replace `D:\data` with the directory where you want to store your data.

## Configuration

### Environment Variables

Set up environment variables to configure the MinIO server. The most commonly used variables are:

- `MINIO_ROOT_USER`: Set the root user (default: `minioadmin`).
- `MINIO_ROOT_PASSWORD`: Set the root password (default: `minioadmin`).

```sh
export MINIO_ROOT_USER=yourusername
export MINIO_ROOT_PASSWORD=yourpassword
```

### Running as a Service (Linux)

To run MinIO as a systemd service:

1. **Create a systemd service file**

   ```sh
   sudo nano /etc/systemd/system/minio.service
   ```

2. **Add the following content to the file**

   ```ini
   [Unit]
   Description=MinIO
   Documentation=https://docs.min.io
   Wants=network-online.target
   After=network-online.target

   [Service]
   User=minio-user
   Group=minio-user
   EnvironmentFile=-/etc/default/minio
   ExecStart=/usr/local/bin/minio server /data
   Restart=always
   LimitNOFILE=65536

   [Install]
   WantedBy=multi-user.target
   ```

3. **Reload systemd and start MinIO**

   ```sh
   sudo systemctl daemon-reload
   sudo systemctl start minio
   sudo systemctl enable minio
   ```

## Accessing MinIO

After starting the MinIO server, you can access the MinIO browser interface:

1. Open your web browser.
2. Navigate to `http://your-server-ip:9000`.
3. Log in with the credentials set in the `MINIO_ROOT_USER` and `MINIO_ROOT_PASSWORD` environment variables.

## Using MinIO Client (mc)

MinIO Client (mc) provides a modern alternative to UNIX commands like `ls`, `cat`, `cp`, `mirror`, etc. It is compatible with Amazon S3 cloud storage service.

### Installing MinIO Client

```sh
wget https://dl.min.io/client/mc/release/linux-amd64/mc
chmod +x mc
```

### Configuring MinIO Client

```sh
./mc alias set myminio http://your-server-ip:9000 yourusername yourpassword
```

Replace `your-server-ip`, `yourusername`, and `yourpassword` with your MinIO server IP and login credentials.

### Basic Commands

- **List buckets**

  ```sh
  ./mc ls myminio
  ```

- **Make a bucket**

  ```sh
  ./mc mb myminio/mybucket
  ```

- **Copy a file**

  ```sh
  ./mc cp myfile.txt myminio/mybucket/myfile.txt
  ```

## Additional Resources

- [MinIO Documentation](https://docs.min.io)
- [MinIO GitHub Repository](https://github.com/minio/minio)
- [MinIO Community Slack](https://slack.min.io)

## Conclusion

You have now set up and configured a MinIO server. You can start using it to store and manage your data efficiently. For more advanced configurations and usage, refer to the official MinIO documentation.

Happy coding!
