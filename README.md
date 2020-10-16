# WordPress Docker

![GitHub License](https://img.shields.io/github/license/ArmandPhilippot/wordpress-docker?logo=Github&style=for-the-badge) ![GitHub package.json version](https://img.shields.io/github/package-json/v/ArmandPhilippot/wordpress-docker?logo=Github&style=for-the-badge)

A WordPress installation using Docker, Traefik & mkcert.

## Presentation

This is a WordPress installation method using Docker, [Traefik](https://doc.traefik.io/traefik/) & mkcert for local usage. It allows you to install WordPress locally while having `https` access.

## Requirements

- [Docker](https://www.docker.com/)
- [Docker Compose](https://docs.docker.com/compose/)
- [mkcert](https://github.com/FiloSottile/mkcert)

On Manjaro Linux, you can install them with `pacman`.

```
sudo pacman -S docker docker-compose mkcert
```

## Installation

1. Clone the repo.
2. You can delete `.git`, `.gitignore`, `keepachangelog.hbs` and `package.json`.
3. Fill the dotenv files (at least `DWP_DOMAIN_NAME`)
4. Generate a certificate with mkcert & save it in `certs` folder.
5. Replace the certificate name in `/traefik/ssl.yml`.
6. On Linux, use ACLs on `/wp-content`

Find the details below for the use of mkcert and the management of ACLs.

### Mkcert

Once installed, create a new local CA:

```
mkcert -install
```

Then, generate a new certificate:

```
mkcert -cert-file ./certs/your-domain.tld.pem -key-file ./certs/your-domain.tld-key.pem your-domain.tld "*.your-domain.tld"
```

Consider replacing `your-domain.tld`.

### ACLs

To keep write permissions while giving them to Apache, you must use ACLs. Otherwise you will be asked for FTP access to add plugins via the administration or to use the import for example.

```
sudo setfacl -Rm user:yourUsername:rwx wp-content
sudo setfacl -Rdm user:yourUsername:rwx wp-content

sudo setfacl -Rm user:http:rwx wp-content
sudo setfacl -Rdm user:http:rwx wp-content
```

Consider replacing `yourUsername`. On Manjaro, the Apache user is `http`, you may need to replace it on another distribution.

## Usage

```
docker-compose up -d
```

Then, you can access:

- WordPress at `www.your-domain.tld`
- Traefik dashboard at `traefik.your-domain.tld`
- PHPMyAdmin at `pma.your-domain.tld`

## Disclaimer

I have not tested this method on Windows or Mac. The step regarding ACLs is probably different.

This method is for local use for development purposes. It should not be used in production.

## License

This project is open source and available under the [MIT License](https://github.com/ArmandPhilippot/wordpress-docker/blob/master/LICENSE).
