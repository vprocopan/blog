## here is a tech stack for movie watching

```
version: "3.8"

services:
  jellyfin:
    image: lscr.io/linuxserver/jellyfin:latest
    container_name: jellyfin
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Etc/UTC
    volumes:
      - ./jellyfin/config:/config
      - ./downloads/completed/movies:/data/movies
      - ./downloads/completed/tv:/data/tvshows
    ports:
      - "8096:8096"
      - "7359:7359/udp"
      - "1900:1900/udp"
    restart: unless-stopped
    depends_on:
      - transmission

  transmission:
    image: lscr.io/linuxserver/transmission:latest
    container_name: transmission
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Etc/UTC
      # - USER=admin           # optional
      # - PASS=changeme
    volumes:
      - ./transmission/config:/config
      - ./downloads:/downloads
      - ./downloads/watch:/watch
      - ./downloads/incomplete:/incomplete
    ports:
      - "9091:9091"
      - "51413:51413"
      - "51413:51413/udp"
    restart: unless-stopped

  jackett:
    image: lscr.io/linuxserver/jackett:latest
    container_name: jackett
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Etc/UTC
      # - AUTO_UPDATE=true     # optional
    volumes:
      - ./jackett/config:/config
      - ./downloads/watch:/downloads  # optional blackhole folder
    ports:
      - "9117:9117"
    restart: unless-stopped

  radarr:
    image: lscr.io/linuxserver/radarr:latest
    container_name: radarr
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Etc/UTC
    volumes:
      - ./radarr/config:/config
      - ./downloads/completed/movies:/movies
      - ./downloads:/downloads
    ports:
      - "7878:7878"
    restart: unless-stopped
    depends_on:
      - transmission
      - jackett

  sonarr:
    image: lscr.io/linuxserver/sonarr:latest
    container_name: sonarr
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Etc/UTC
    volumes:
      - ./sonarr/config:/config
      - ./downloads/completed/tv:/tv
      - ./downloads:/downloads
    ports:
      - "8989:8989"
    restart: unless-stopped
    depends_on:
      - transmission
      - jackett
```