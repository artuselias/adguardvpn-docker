# adguardvpn-docker
[AdguardTeam/AdGuardVPNCLI](https://github.com/AdguardTeam/AdGuardVPNCLI) for Linux in a docker image including the torrent client transmission-daemon and a killswitch.

## Objective
Route all data traffic of an application, in this case [Transmission](https://transmissionbt.com/), through a VPN, while leaving the host's network settings untouched.

Optionally includes a killswitch that monitors the public IP address of the container and drops the connection in case it changes.

## How to install
### 1. Install Docker
For example by using the [official install script](https://get.docker.com).

### 2. Get the files from this repository
For example, using Git: `git pull git@github.com:artuselias/adguardvpn-docker.git`.

Make sure Git is installed (`sudo apt install git` on Ubuntu) and that the current directory is writable.

### 3. Adjust environment variables
The environment variables for this docker container are stored in a file called `.env`.

`ADGUARD_USERNAME` and `ADGUARD_PASSWORD` need to be specified in order to enable automatic login.

Be sure to set `DOWNLOADS_PATH` to a path in your file system where the downloaded torrents should be stored.

Note that if `KILLSWITCH` is set to `enabled`, the container may send requests to `ipinfo.io`.

You can copy and adjust the file `.env.example`:
```
cd adguardvpn-docker
cp .env.example .env
nano .env
```

### 4. Build the Docker image
`sudo docker build -t adguard-vpn .`

Note that this downloads data from LinuxServers.io and Github.

### 5. Bring the Docker container up
`sudo docker compose up -d`

Note that if `ADGUARD_USERNAME` and `ADGUARD_PASSWORD` are specified, this tries to automatically login to your account on `adguard-vpn.com`.

Note that if `ADGUARD_AUTOCONNECT` is set to `yes`, this tries to automatically connect your AdGuard VPN to the location set in `ADGUARD_LOCATION`. If `ADGUARD_LOCATION` is not set, it will attempt to connect to the fastest available location.

## How to check everything is working
### Check VPN status
`sudo docker exec -it adguard-vpn adguardvpn-cli status`
### Check killswitch status
`sudo docker exec -it adguard-vpn killswitch status`

## How to use
The Transmission web interface is accessible on [http://localhost:9091/transmission/web/](http://localhost:9091/transmission/web/).

The downloaded files are stored in the directory specified in the environment variable `DOWNLOADS_PATH`.

## Read more
- [linuxserver/transmission](https://docs.linuxserver.io/images/docker-transmission/)
- [AdGuard VPN CLI for Linux](https://adguard-vpn.com/en/linux/overview.html)
