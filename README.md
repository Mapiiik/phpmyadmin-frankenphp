# phpMyAdmin on FrankenPHP

Docker image for running **phpMyAdmin** on **FrankenPHP**, with the application taken from the
official `phpmyadmin` image.

This repository provides:
- a multi-stage Docker build with the MariaDB client tools
- production Docker Compose setup
- automated CI that rebuilds the image on new phpMyAdmin releases and base image updates

---

## 🚀 Features

- **FrankenPHP** runtime (Caddy + PHP in one process), automatic HTTPS
- **Non-root runtime**
- `blowfish_secret` generated on the first start
- **Release-driven CI**
  - scheduled builds only when phpMyAdmin has a new release or `dunglas/frankenphp` a new image
  - forced rebuilds on pushes to `main`
- Images published to **GitHub Container Registry**

---

## 📦 Docker Images

Images are published to:
```
ghcr.io/mapiiik/phpmyadmin-frankenphp
```

Available tags:
- `latest` – latest phpMyAdmin release
- `<version>` – specific phpMyAdmin release (e.g. `5.2.3`)
- `<major>.<minor>` – latest patch release within a minor series (e.g. `5.2`)
- `<major>` – latest release within a major series (e.g. `5`)
- `build-<version>-<base>` – release and base image digest, used by CI to skip unchanged builds
- `sha-<commit>` – infrastructure build reference

---

## 🐳 Usage (Production)

1. Copy the environment template:

```bash
cp .env.example .env
```

2. Set `SERVER_NAME` and put your servers in `config.user.inc.php` in `DATA_DIR`
   ([configuration](https://docs.phpmyadmin.net/en/latest/config.html)).

3. Start the stack:

```bash
docker compose -f compose.production.yaml up -d
```
or
```bash
cp compose.production.yaml compose.yaml
docker compose up -d
```

## ⚙️ Configuration
- `SERVER_NAME` (automatic ACME certificates; otherwise self-signed HTTPS)
- `DATA_DIR` (default `./data`) holds `config.user.inc.php`, loaded after the default configuration

Expose phpMyAdmin only to the networks of your administrators.

## 🧠 CI Behavior
- Scheduled runs read the release from the current official `phpmyadmin` image and the base image
  digest, and build only when one of them changed.
- Pushes to main always trigger a rebuild.
- No repository changes are made by CI.

## 📄 License
See individual components for their respective licenses.
