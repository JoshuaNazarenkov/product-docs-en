[waf-mode-instr]:                   ../admin-en/configure-wallarm-mode.md
[logging-instr]:                    ../admin-en/configure-logging.md
[proxy-balancer-instr]:             ../admin-en/using-proxy-or-balancer-en.md
[scanner-whitelisting-instr]:       ../admin-en/scanner-ips-whitelisting.md
[process-time-limit-instr]:         ../admin-en/configure-parameters-en.md#wallarm_process_time_limit
[default-ip-blocking-settings]:     ../admin-en/configure-ip-blocking-nginx-en.md
[wallarm-acl-directive]:            ../admin-en/configure-parameters-en.md#wallarm_acl
[allocating-memory-guide]:          ../admin-en/configuration-guides/allocate-resources-for-waf-node.md
[enable-libdetection-docs]:         ../admin-en/configure-parameters-en.md#wallarm_enable_libdetection
[nginx-waf-directives]:             ../admin-en/configure-parameters-en.md
[mount-config-instr]:               #run-the-container-mounting-the-configuration-file

# Updating the running Docker NGINX‑based image

These instructions describe the steps to update the running Docker NGINX‑based image to the version 2.16.

!!! warning "Using credentials of already existing Wallarm node"
    We do not recommend to use the already existing Wallarm node of the previous version. Please follow these instructions to create a new filtering node of the version 2.16 and deploy it as the Docker container.

## Requirements

--8<-- "../include/waf/installation/requirements-docker.md"

## Step 1: Download the updated filtering node image

```bash
docker pull wallarm/node:2.16.0-9
```

## Step 2: Stop the running container

```bash
docker stop <RUNNING_CONTAINER_NAME>
```

## Step 3: Run the container using the updated image

### Options for running the container

When running the container using the updated image, you can pass the same configuration parameters that were passed when running a previous image version. If some parameters are deprecated or added in the new Wallarm node version, the appropriate information is published in the list of the [new version changes](what-is-new.md).

--8<-- "../include/waf/installation/docker-running-options.md"

### Run the container passing the environment variables

You can pass the following basic filtering node settings to the container via the option `-e`:

--8<-- "../include/waf/installation/nginx-docker-all-env-vars-218.md"

To run the image, use the command:

=== "EU Cloud"
    ```bash
    docker run -d -e DEPLOY_USER='deploy@example.com' -e DEPLOY_PASSWORD='very_secret' -e NGINX_BACKEND='example.com' -e TARANTOOL_MEMORY_GB=16 -p 80:80 wallarm/node:2.16.0-9
    ```
=== "US Cloud"
    ```bash
    docker run -d -e DEPLOY_USER='deploy@example.com' -e DEPLOY_PASSWORD='very_secret' -e NGINX_BACKEND='example.com' -e WALLARM_API_HOST='us1.api.wallarm.com' -e TARANTOOL_MEMORY_GB=16 -p 80:80 wallarm/node:2.16.0-9
    ```

The command does the following:

* Automatically creates new filtering node of the version 2.16 in the Wallarm Cloud. Created filtering node will be displayed in the Wallarm Console → **Nodes**.
* Creates the file `default` with minimal NGINX configuration and passes filtering node configuration in the `/etc/nginx/sites-enabled` container directory.
* Creates files with filtering node credentials to access the Wallarm Cloud in the `/etc/wallarm` container directory:
    * `node.yaml` with filtering node UUID and secret key
    * `license.key` with Wallarm license key
* Protects the resource `http://NGINX_BACKEND:80`.

### Run the container mounting the configuration file

You can mount the prepared configuration file to the Docker container via the `-v` option. The file must contain the following settings:

* [Filtering node directives](../admin-en/configure-parameters-en.md)
* [NGINX settings](https://nginx.org/en/docs/beginners_guide.html)

??? info "See an example of the mounted file with minimal settings"
    ```bash
    server {
        listen 80 default_server;
        listen [::]:80 default_server ipv6only=on;
        #listen 443 ssl;

        server_name localhost;

        #ssl_certificate cert.pem;
        #ssl_certificate_key cert.key;

        root /usr/share/nginx/html;

        index index.html index.htm;

        wallarm_mode monitoring;
        # wallarm_instance 1;
        # wallarm_acl default;

        location / {
                proxy_pass http://example.com;
                include proxy_params;
        }
    }
    ```

To run the image:

1. Pass required environment variables to the container via the `-e` option:

    --8<-- "../include/waf/installation/nginx-docker-env-vars-to-mount.md"

2. Mount the directory with the configuration file `default` to the `/etc/nginx/sites-enabled` container directory via the `-v` option.

    === "EU Cloud"
        ```bash
        docker run -d -e DEPLOY_USER='deploy@example.com' -e DEPLOY_PASSWORD='very_secret' -v /configs/default:/etc/nginx/sites-enabled/default -p 80:80 wallarm/node:2.16.0-9
        ```
    === "US Cloud"
        ```bash
        docker run -d -e DEPLOY_USER='deploy@example.com' -e DEPLOY_PASSWORD='very_secret' -e WALLARM_API_HOST='us1.api.wallarm.com' -v /configs/default:/etc/nginx/sites-enabled/default -p 80:80 wallarm/node:2.16.0-9
        ```

The command does the following:

* Automatically creates new filtering node of the version 2.16 in the Wallarm Cloud. Created filtering node will be displayed in the Wallarm Console → **Nodes**.
* Mounts the file `default` into the `/etc/nginx/sites-enabled` container directory.
* Creates files with filtering node credentials to access Wallarm Cloud in the `/etc/wallarm` container directory:
    * `node.yaml` with filtering node UUID and secret key
    * `license.key` with Wallarm license key
* Protects the resource `http://example.com`.

!!! info "Mounting other configuration files"
    The container directories used by NGINX:

    * `/etc/nginx/conf.d` — common settings
    * `/etc/nginx/sites-enabled` — virtual host settings
    * `/var/www/html` — static files

    If required, you can mount any files to the listed container directories. The filtering node directives should be described in the `/etc/nginx/sites-enabled/default` file.

## Step 4: Test the filtering node operation

--8<-- "../include/waf/installation/test-waf-operation-no-stats.md"

## Step 5: Delete the filtering node of the previous version

If the deployed image of the version 2.16 operates correctly, you can delete the filtering node of the previous version in the Wallarm Console → **Nodes** section.

## Configuring the use cases

* [Logging configuration](../admin-en/installation-docker-en.md#logging-configuration)
* [Monitoring configuration](../admin-en/installation-docker-en.md#monitoring-configuration)

The configuration file mounted to the Docker container should describe the filtering node configuration in the [available directive](../admin-en/configure-parameters-en.md). Below are some commonly used filtering node configuration options:

--8<-- "../include/waf/installation/common-customization-options-docker-216.md"
