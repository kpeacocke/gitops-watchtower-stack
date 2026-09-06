# GitOps Watchtower stack

Reusable, label-scoped image-update monitoring for each Docker host managed by
Portainer.

## Policy

Watchtower runs in **monitor-only** mode. It reports newer images but never
replaces containers. Updates remain explicit GitOps operations: review the
release, update or retain the image reference, then let Portainer redeploy.

Only containers carrying this label are checked:

```yaml
labels:
  com.centurylinklabs.watchtower.enable: "true"
```

## Portainer deployment

Create one Git stack per Docker environment using:

- Repository: `https://github.com/kpeacocke/gitops-watchtower-stack`
- Compose path: `stack/docker-compose.yml`
- Environment variables copied from `stack/.env.sample`

Set `ENVIRONMENT_NAME` to a useful host identity such as `alexandria`,
`pi-mdns`, `pi-terror`, or `pi-harmony`.

`WATCHTOWER_NOTIFICATION_URL` accepts a Shoutrrr URL. Leave it empty to keep
reports in container logs. Each instance can inspect only the Docker engine
whose socket it mounts, so remote Pi environments require separate deployments.

The Docker socket grants host-level Docker control even though this instance is
monitor-only. Do not publish ports and do not expose the socket over TCP.
