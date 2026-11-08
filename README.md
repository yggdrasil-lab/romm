# RomM Stack

Self-hosted RomM (ROM Manager) retro gaming warehouse for Yggdrasil. Deployed on the Muspelheim worker node via Docker Swarm, routing through Traefik at `romm.${DOMAIN_NAME}`.

## Architecture
- **romm**: Core RomM web application, API, and embedded Valkey cache worker.
- **database**: MariaDB 11.4 database for accounts, game metadata, and library index.

## Storage
- `/mnt/storage/roms/library`: Bulk storage for ROM collections by platform (mergerfs pool).
- `/mnt/storage/roms/assets`: User uploads, screenshots, and game saves (mergerfs pool).
- `/mnt/storage/roms/resources`: Downloaded metadata, cover art, and box art (mergerfs pool).
- `/opt/romm/config`: Application configuration (local SSD).
- `/opt/romm/redis`: Task worker cache (local SSD).
- `/opt/romm/db`: MariaDB database persistence (local SSD, strictly avoiding mergerfs).

## Deployment Secrets & Variables
- `vars.DOMAIN_NAME`: Cluster apex domain.
- `secrets.ROMM_DB_PASSWORD`: MariaDB password for user `romm`.
- `secrets.ROMM_DB_ROOT_PASSWORD`: MariaDB root password.
- `secrets.ROMM_AUTH_SECRET_KEY`: Hex-encoded 32-byte secret key for user sessions and JWT signing.
