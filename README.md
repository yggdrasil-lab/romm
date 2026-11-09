# RomM Stack

Self-hosted RomM (ROM Manager) retro gaming warehouse for Yggdrasil. Deployed on the Muspelheim worker node via Docker Swarm, routing through Traefik at `romm.${DOMAIN_NAME}`.

## Architecture
- **romm**: Core RomM web application, API, and embedded Valkey cache worker.
- **romm-db**: MariaDB 11.4 database for accounts, game metadata, and library index. Attached to `aether-net`.
- **syncthing**: Lightweight continuous save synchronization target paired to Steam Deck `/Emulation/saves/`. Exposed via Traefik at `saves.${DOMAIN_NAME}` and raw sync traffic on host port 22000.

## Storage
- `/mnt/storage/roms/library`: Bulk storage for ROM collections by platform (mergerfs pool).
- `/mnt/storage/roms/assets`: User uploads, screenshots, and game saves (mergerfs pool).
- `/mnt/storage/roms/resources`: Downloaded metadata, cover art, and box art (mergerfs pool).
- `/mnt/storage/emulation/saves`: Continuous game save vault synchronized with Steam Deck (mergerfs pool).
- `/opt/romm/config`: Application configuration (local SSD).
- `/opt/romm/redis`: Task worker cache (local SSD).
- `/opt/romm/db`: MariaDB database persistence (local SSD, strictly avoiding mergerfs).
- `/opt/romm/syncthing`: Syncthing configuration and device keys (local SSD).

## Deployment Secrets & Variables
- `vars.DOMAIN_NAME`: Cluster apex domain.
- `vars.IGDB_CLIENT_ID`: Twitch Developer Application Client ID for IGDB scraping.
- `secrets.IGDB_CLIENT_SECRET`: Twitch Developer Application Client Secret.
- `secrets.ROMM_DB_PASSWORD`: MariaDB password for user `romm`.
- `secrets.ROMM_DB_ROOT_PASSWORD`: MariaDB root password.
- `secrets.ROMM_AUTH_SECRET_KEY`: Hex-encoded 32-byte secret key for user sessions and JWT signing.
