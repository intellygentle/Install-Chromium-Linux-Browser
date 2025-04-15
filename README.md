# Install Chromium Linux Browser
Chromium is an open-source browser project that aims to build a safer, faster, and more stable build by Google
* You can easily access a browser in your non-gui Linux server
* You can easily run your Node Extensions 

## Install Docker
```bash
sudo apt update -y && sudo apt upgrade -y
```

```bash
for pkg in docker.io docker-doc docker-compose podman-docker containerd runc; do sudo apt-get remove $pkg; done
```


```bash
sudo apt-get update
```

```bash
sudo apt-get install ca-certificates curl gnupg
```

```bash
sudo install -m 0755 -d /etc/apt/keyrings
```

```bash
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
```

```bash
sudo chmod a+r /etc/apt/keyrings/docker.gpg
```

```bash
echo \
  "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```


```bash
sudo apt update -y && sudo apt upgrade -y
```

```bash
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```


# Docker version check
```
docker --version
```

```bash
screen -S huddle01
```


## Timezone check
```
realpath --relative-to /usr/share/zoneinfo /etc/localtime
```

## Install Chromium
**1. Create directory**
```
mkdir chromium
```

```bash
cd chromium
```

**2. Create `docker-compose.yaml` file**
```
nano docker-compose.yaml
```

**3. Paste the following code in it**
* `CUSTOM_USER` & `PASSWORD`: Replace your favorite credentials to login to chromium
* `TZ`: Replace with your server timezone
* `CHROME_CLI`: The main page when you open browser
* `ports`: You can replace `3010` & `3011` if they have conflict
```
services:
  chromium:
    image: lscr.io/linuxserver/chromium:latest
    container_name: chromium
    security_opt:
      - seccomp:unconfined # optional
    environment:
      - HTTP_USER=yourUsername
      - HTTP_PASSWORD=yourPassword
      - PUID=1000
      - PGID=1000
      - TZ=America/Los_Angeles
      - CHROME_CLI=https://www.google.com
    volumes:
      - /root/chromium/config:/config
    ports:
      - 3010:3000
      - 3011:3001
    shm_size: "1gb"
    restart: unless-stopped

```
> To save and exit: `Ctrl+X+Y+Enter` 

## Run Chromium
```
docker compose up -d
```
**The application can be accessed by going to one of these addresses in your local PC browser**
* http://yourIPaddress:3010/
* https://Server_IP:3011/

-------------------------------- STOP HERE FOR CHROMIUM BROWSER ----------------------------------------

# ⭐ Install Proxy on Chromium
## 1) Buy Proxy
* You can use any reliable platform to buy a **Static Residential** proxy.
* I bought a proxy via crypto payments on [iproyal](https://iproyal.com/?r=835672)

## 2) Install Proxy in Docker
**1- Stop currently running container**
```bash
docker compose down -v
```

**2- Update your `docker-compose.yml`:**
* Replace `CUSTOM_USER` & `PASSWORD` with your Chromium credentials.
* Delete one of `CHROME_CLI` lines depending your proxy is `http` or `socks5` and replace `proxy.example.com:1080` with your proxy address and port.
```yaml
---
---
services:
  chromium:
    image: lscr.io/linuxserver/chromium:latest
    container_name: chromium
    security_opt:
      - seccomp:unconfined #optional
    environment:
      - CUSTOM_USER=     #Replace username
      - PASSWORD=    #Replace password
      - PUID=1000
      - PGID=1000
      - TZ=Europe/London
      - CHROME_CLI=--proxy-server=http://proxy.example.com:1080 https://google.com
      - CHROME_CLI=--proxy-server=socks5://proxy.example.com:1080 https://google.com
    volumes:
      - /root/chromium/config:/config
    ports:
      - 3010:3000   #Change 3010 to your favorite port if needed
      - 3011:3001   #Change 3011 to your favorite port if needed
    shm_size: "1gb"
    restart: unless-stopped
```


**3- Start container**
```bash
docker compose down -v
docker compose up -d
```

**4- Access to your Chromium using `http://Server_IP:3010/` or `https://Server_IP:3011/`**
* First it asks you to enter your `chromium` credential, then it asks for `proxy` credential (if your proxy has credential).

![image](https://github.com/user-attachments/assets/50a05730-b4c3-45cd-967a-f3a8e156e22d)


## Optional: Stop and Delete Chromium
```
docker stop chromium
docker rm chromium
docker system prune
```
