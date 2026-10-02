# Local certificate

To generate sha hash on browser for PKCE flow the server has to run in SSL. We are using [MiniCA](https://github.com/jsha/minica) which is an open source tool to generate trusted SSL certificate for local development.

## Steps to generate SSL certificate
First build docker image

```bash
make build
```

Then run the docker image to generate the certificate. The `minica` make target runs miniCA in docker and copies the result into `~/.certs`.

```bash
make minica ARGS="-domains example.com,*.example.com"
```

To force a clean rebuild of the image (e.g. after bumping `MINICA_VERSION` in the Makefile):

```bash
make rebuild
```

**[Import MiniCa](https://gist.github.com/mwidmann/115c2a7059dcce300b61f625d887e5dc)**
