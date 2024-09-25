# Dokku and Docker Registry Login Action

This GitHub Action handles authentication for both Dokku and Docker registries. It allows you to log in to multiple Docker registries and Dokku on remote servers.

## Prerequisites

To avoid being prompted for a password when executing `dokku` and `docker` commands, you need to configure the following in your server's sudoers file. This ensures that the `github-actions` user can execute the necessary commands without requiring a password:

```bash
sudo visudo
github-actions ALL=(ALL) NOPASSWD:SETENV: /usr/bin/dokku
github-actions ALL=(ALL) NOPASSWD: /usr/bin/docker
```
## Inputs

- `registrys`: JSON object passed from the matrix containing Docker and Dokku login information.
- `USER_SSH`: The SSH user for connecting to the server.
- `IP_SERVER`: The IP address of the server.
- `PRIVATE_KEY_SERVER_DEPLOY`: The private SSH key for server authentication.

## Example Usage

```yaml
jobs:
  deploy:
    runs-on: self-hosted
    steps:
      - name: Login to Docker and Dokku
        uses: matiasAS/dokku-docker-login-action@v1.0.2
        with:
          registrys: |
            [
              {
                "registry": "registry.gitlab.com",
                "username": "user",
                "password": "${{ secrets.GITLAB_PAT }}"
              },
              {
                "registry": "registry2",
                "username": "user",
                "password": "${{ secrets.TOKEN }}"
              }
            ]

          USER_SSH: "github-actions"
          IP_SERVER: "192.168.1.100"
          PRIVATE_KEY_SERVER_DEPLOY: ${{ secrets.SSH_PRIVATE_KEY }}
