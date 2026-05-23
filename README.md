# Molten Hub Code: Whisperer (Example)

Docker Compose setup for running `moltenai/moltenhub-code:vnext` with a
local `faster-whisper` speech service.

The stack starts:

- `agent`: runs `harness hub --ui-listen :7777` and exposes the hub UI on
  `http://localhost:3331`.
- `whisper`: runs `lscr.io/linuxserver/faster-whisper:latest`, persists its
  config in `./data/whisper`, and exposes Whisper on host port `10300` by
  default.

## Prerequisites

- Docker with the Docker Compose plugin.

This repository does not include application source, package manifests, or
language-specific build steps. Runtime behavior is defined by
`docker-compose.yml`.

## Run

```sh
docker compose up -d
```

Open `http://localhost:3331`.

Useful commands:

```sh
docker compose logs -f
docker compose pull
docker compose down
```

## Configuration

Set overrides in your shell or in a local `.env` file before starting the
stack. All variables below are optional.

| Variable | Default | Purpose |
| --- | --- | --- |
| `PUID` | `1000` | User ID for the Whisper container. |
| `PGID` | `1000` | Group ID for the Whisper container. |
| `TZ` | `America/Vancouver` | Time zone used by the Whisper container. |
| `WHISPER_MODEL` | `auto` | Whisper model loaded by faster-whisper. |
| `WHISPER_LANG` | `auto` | Language setting passed to faster-whisper. |
| `WHISPER_BEAM` | `1` | Beam size passed to faster-whisper. |
| `WHISPER_PORT` | `10300` | Host port forwarded to the Whisper service. |

The agent talks to Whisper over the Compose network at `whisper:10300`.
Changing `WHISPER_PORT` only changes the host-side port mapping.

No secret environment variables are required by the Compose file.

## Contributing

Before changing runtime configuration, validate the Compose file:

```sh
docker compose config
```

If the change affects startup behavior, also run the stack locally and inspect
logs:

```sh
docker compose up -d
docker compose logs -f
```
