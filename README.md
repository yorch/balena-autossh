# balena-autossh

Reverse SSH tunnel with AutoSSH for [Balena](https://www.balena.io/).

This allows you to deploy a device like a Raspberry Pi with Balena OS and Balena Cloud, and run a
reverse SSH proxy on it, so even if it's behind a NAT, you will be able to access it.

My specific use case was to send a Raspberry Pi overseas to a family's house and have it plugged in there,
without having to configure their router to expose it to the Internet, basically, just sitting behind
their router NAT. I can then start a SSH proxy and use it to watch local internet TV that is blocked
outside of that country.

## Requirements

- Free Balena Cloud account.
- An public facing SSH server like a cheap $5 VPS (ie: from providers like [Vultr]() or [Digital Ocean](https://m.do.co/c/386aa021b4aa)).
- Raspberry Pi or another supported device by Balena. Raspberry Pi 1 is a bit too slow, so I recommend Raspberry Pi 2+.

## Configuration

The configuration is done through ENV variables that can be set at the Fleet or Device level in Balena Cloud.

- `SSH_BIND_IP`: This is the IP in the external server / VPS where the reverse proxy will be listening to. You
  can set it to `0.0.0.0` or `*` in case you want to expose it directly to the Internet, or set it to `localhost`
  or `127.0.0.1` so you will be only be able to access the proxy from within the VPS.
- `SSH_PRIVATE_KEY`: This is the SSH private key that is authorized in the external server / VPS. Balena Cloud UI
  will convert to a single line and remove line breaks, but the code in this repo will transform it back.
- `SSH_REMOTE_HOST`: This is the IP or server DNS name of the external server / VPS.
- `SSH_REMOTE_PORT`: This is the port where the SSH server in the external server is listening on, usually `22`.
- `SSH_REMOTE_USER`: This is the user configured in the external server / VPS with SSH access, the public key
  obtained from the private key specified in `SSH_PRIVATE_KEY` should be added to this user's `~/.ssh/authorized_keys` file
- `SSH_TARGET_HOST`: In Balena, this should be `localhost`.
- `SSH_TARGET_PORT`: In Balena, this should be `22222`, which is the port the Balena OS's SSH server runs.
- `SSH_TUNNEL_PORT`: This is in the external server (ie VPS)'s port where the proxy will be reachable.

## Credits

This repo is based on https://github.com/jnovack/autossh

## License

MIT
