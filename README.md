alpine-traefik 
==============

## Configuration

This image runs [traefik][traefik] with monit. It is started with traefik user/group with 10001 uid/gid.

Besides, you can customize the configuration in several ways:

### Default Configuration

Traefic is installed with the default configuration and some parameters can be overrided with env variables:

- TRAEFIK_HTTP_PORT=8080								# http port > 1024 due to run as non privileged user
- TRAEFIK_HTTPS_ENABLE="false"							# "true" enables https and http endpoints. "Only" enables https endpoints and redirect http to https.
- TRAEFIK_HTTPS_PORT=8443								# https port > 1024 due to run as non privileged user
- TRAEFIK_ADMIN_PORT=8000								# admin port > 1024 due to run as non privileged user
- TRAEFIK_LOG_LEVEL="INFO"								# Log level
- TRAEFIK_LOG_FILE="/opt/traefik/log/traefik.log"}		# Log file. Redirected to docker stdout.
- TRAEFIK_ACCESS_FILE="/opt/traefik/log/access.log"}	# Access file. Redirected to docker stdout.
- TRAEFIK_SSL_PATH="/opt/traefik/certs"					# Path to search .key and .crt files
- TRAEFIK_ACME_ENABLE="false"							# Enable/disable traefik ACME feature
- TRAEFIK_ACME_EMAIL="test@traefik.io"					# Default email
- TRAEFIK_ACME_ONDEMAND="true"							# ACME ondemand parameter
- TRAEFIK_ACME_ONHOSTRULE="true"						# ACME OnHostRule parameter
- TRAEFIK_K8S_ENABLE="false"							# Enable/disable traefik K8S feature

### Custom Configuration

Traefik is installed under /opt/traefik and make use of /opt/traefik/etc/traefik.toml and /opt/traefik/etc/rules.toml.

You can edit or overwrite this files in order to customize your own configuration or certificates.

You could also include FROM rawmind/alpine-traefik at the top of your Dockerfile, and add your custom config.

### SSL Configuration

Added SSL configuration. Set TRAEFIK_HTTPS_ENABLE="< true || only >" to enable it. 

SSL certificates are located by default in /opt/traefik/certs. You need to provide .key AND .crt files to that directory, in order traefik gets automatically configured with ssl.

If you put more that one key/crt files in the certs directory, traefik gets sni enabled and configured. You also could map you cert storage volume to traefik and mount it in $TRAEFIK_SSL_PATH value.

You could also include FROM rawmind/alpine-traefik at the top of your Dockerfile, and add your custom ssl files.

### Letsencrypt Configuration

If you enable SSL configuration, you could enable traefik letsencrypt support as well (ACME). To do it, set TRAEFIK_ACME_ENABLE="true".

[traefik]: https://github.com/containous/traefik
