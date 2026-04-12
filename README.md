# openclaw-ubi

[OpenClaw](https://github.com/openclaw/openclaw) autonomous AI agent packaged on Red Hat UBI 10 + Node.js 24.

**Image:** `ghcr.io/rarguello/openclaw-ubi`

## Quick start (Podman)

```sh
cp .env.example .env        # fill in your API keys
./run.sh
```

Or pull directly:

```sh
podman run -d \
  --env-file .env \
  --env HOME=/var/lib/openclaw \
  --volume ~/.local/share/openclaw-ubi:/var/lib/openclaw:Z \
  --tmpfs /tmp \
  --publish 18789:18789 \
  --userns=keep-id:uid=1001,gid=1001 \
  ghcr.io/rarguello/openclaw-ubi:latest
```

## OpenShift

```sh
oc new-project openclaw-ubi
oc create secret generic openclaw-ubi --from-env-file=.env --namespace=openclaw-ubi
oc apply -k manifests/
```

Access via port-forward (no Route — gateway token is the auth boundary):

```sh
oc port-forward pod/openclaw-ubi-0 18789:18789 -n openclaw-ubi
```

## Systemd (Quadlet)

```sh
cp openclaw-ubi.container ~/.config/containers/systemd/
mkdir -p ~/.config/openclaw-ubi && cp .env.example ~/.config/openclaw-ubi/env
# edit ~/.config/openclaw-ubi/env
systemctl --user daemon-reload && systemctl --user start openclaw-ubi
```

## Updates

New upstream releases are detected every 6 hours and built automatically.
