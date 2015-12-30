A Docker image with only sshd.

The primary use cases for this image are the tunneling and SOCKS proxying features of
sshd. It is an easy way to spin up a Docker container on AWS, a container service
or a PaaS and tunnel browsing or other traffic through it.

# Usage

```sh
docker run \
  --name sshd \
  -d \
  -p 2222:22 \
  -v ~/.ssh/id_rsa.pub:/home/proxy/.ssh/authorized_keys \
  mm62/sshd
```

This starts the container, detached, mapping port 2222 on the host to port 22 in the
container and mounts a public key as an authorized key of the `proxy` user.

When the container is running, to create a SOCKS5 proxy:

```sh
ssh \
  -f \
  -N \
  -D 127.0.0.1:1080 \
  -p 2222 \
  proxy@<host ip address>
```

Then configure your browser to use 127.0.0.1:1080 as a SOCKS proxy.

# Implementation Details

This image is little more than the Void Linux image with a user added and sshd
started.
