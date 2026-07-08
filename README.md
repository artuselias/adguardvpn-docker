# adguardvpn-docker
[AdguardTeam/AdGuardVPNCLI](https://github.com/AdguardTeam/AdGuardVPNCLI) for Linux in a docker image including the torrent client transmission-daemon and a killswitch.

## Objective
Route all data traffic of an application, in this case [Transmission](https://transmissionbt.com/), through a VPN, while leaving the host's network settings untouched.

Includes a killswitch that monitors the public IP address of the container by sending requests to ipinfo.io/ip and stops container in case it changes.

## How to install
### 1. Install Docker
For example by using the [official install script](https://get.docker.com).

### 2. Get the files from this repository
https://github.com/artuselias/adguardvpn-docker/archive/refs/heads/main.zip.

### 3. Adjust environment variables
The environment variables for this docker container are stored in a file called `.env`.

Be sure to set `DOWNLOADS_PATH` to a path in your file system where the downloaded torrents should be stored.

You can copy and adjust the file `.env.example`:
```
cd adguardvpn-docker
cp .env.example .env
nano .env
```

### 4. Build the Docker image
`sudo docker build -t adguard-vpn .`

This downloads data from LinuxServers.io and Github.

### 5. Bring the Docker container up
`sudo docker compose up -d`


### 6. Log-in
Print container logs:
`sudo docker compose logs`
At the end, there should be a link to log-in:
`You need to authorize in your browser. The following link will be available for 1799 seconds:`
Click on it and log in.

It is then tried to automatically connect your AdGuard VPN to the location set in `ADGUARD_LOCATION`. If `ADGUARD_LOCATION` is not set, it will attempt to connect to the fastest available location.

## How to check everything is working
### Check VPN status
`sudo docker exec -it adguard-vpn adguardvpn-cli status`
### Check killswitch status
`sudo docker exec -it adguard-vpn cat /vars.txt`

## How to use
The Transmission web interface is accessible on [http://localhost:9091/transmission/web/](http://localhost:9091/transmission/web/).

The downloaded files are stored in the directory specified in the environment variable `DOWNLOADS_PATH`.

## Read more
- [linuxserver/transmission](https://docs.linuxserver.io/images/docker-transmission/)
- [AdGuard VPN CLI for Linux](https://adguard-vpn.com/en/linux/overview.html)
