# JupyterHub + OpenLDAP Docker Compose

One-command multi-user JupyterHub deployment with **OpenLDAP** authentication via Docker Compose. Spin up a shared Jupyter notebook server where every user authenticates against a central LDAP directory — no manual user management.

## ✨ Features

- **Multi-user JupyterHub** — each user gets their own isolated notebook container
- **OpenLDAP authentication** — users authenticate against a central LDAP server
- **phpLDAPadmin** — web UI to manage users/groups in the LDAP directory
- **Docker-based spawning** — each user's notebook runs in its own container
- **Single command** — `docker compose up` and it's running

## 🚀 Quick Start

```bash
docker compose up -d
```

Then open **http://localhost:8000** and log in with an LDAP user.

### Default admin

The compose file sets `JUPYTERHUB_ADMIN=admin`. Manage users via phpLDAPadmin at **https://localhost:6443**.

## 🧱 Architecture

| Service | Image | Port | Purpose |
|---------|-------|------|---------|
| `hub` | `jupyterhub` (built) | 8000 | JupyterHub server, spawns user containers |
| `openldap` | `rroemhild/test-openldap:2.1` | 389/636 | Central LDAP directory |
| `phpldapadmin` | `osixia/phpldapadmin:0.9.0` | 6443 | LDAP web admin UI |

The hub binds the host Docker socket (`/var/run/docker.sock`) so it can spawn per-user notebook containers on the `jupyterhub-network`.

## ⚙️ Configuration

Key settings live in `docker-compose.yml`:

- `JUPYTERHUB_ADMIN` — initial admin username
- `DOCKER_NOTEBOOK_IMAGE` — notebook image spawned for users (default `quay.io/jupyter/base-notebook:latest`)
- `DOCKER_NOTEBOOK_DIR` — working directory inside user containers
- `jupyterhub_config.py` — mounted read-only into the hub for advanced JupyterHub config

## 📁 Project Layout

```
.
├── docker-compose.yml        # All services, networks, volumes
├── Dockerfile.jupyterhub     # JupyterHub image build
├── jupyterhub_config.py      # JupyterHub runtime configuration
└── README.md
```

## 🛠️ Tech Stack

JupyterHub · Docker Compose · OpenLDAP · phpLDAPadmin · Python

## 📄 License

MIT
