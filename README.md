
# 📺 Ultimate Live TV & IPTV Media Server Setup
> A complete, high-performance guide and toolkit for setting up Live TV streaming using NGINX-RTMP, HLS (M3U8), and Flussonic Media Server on Ubuntu.

![Platform](https://img.shields.io/badge/Platform-Ubuntu-orange.svg)
![Protocol](https://img.shields.io/badge/Protocol-RTMP%20%7C%20HLS-blue.svg)
![License](https://img.shields.io/badge/License-MIT-green.svg)

---

## 📖 Overview

This repository provides a complete A-to-Z guide and the necessary configuration files to build your own professional **IPTV / Live TV Streaming Server**. Whether you are looking for a free, open-source solution using **NGINX-RTMP** or a commercial-grade setup using **Flussonic Media Server**, this repository has you covered. 

It handles video ingestion from encoders (OBS, vMix, Hardware Encoders) via RTMP and distributes the feed to clients (Smart TVs, Mobile Apps, Web Players) using HLS (.m3u8).

---

## ✨ Key Features

* **Multi-Protocol Support:** Ingest via RTMP, RTSP, or SRT and deliver via HLS, DASH, or RTMP.
* **Low Latency:** Optimized configurations for minimal delay during live broadcasts.
* **Open Source & Enterprise:** Includes guides for both NGINX (Free) and Flussonic (Enterprise).
* **Cross-Platform Playback:** Output streams (`.m3u8`) are compatible with Android, iOS, Smart TVs, and web players (Video.js, HLS.js).
* **Automated Installation:** Step-by-step commands to get Ubuntu servers running in minutes.

---

## 🛠️ 1. NGINX-RTMP & HLS Setup (Open Source)

This section covers how to install NGINX with the RTMP module on an Ubuntu server to convert RTMP streams to HLS.

### Prerequisites
* Ubuntu 20.04 or 22.04 LTS
* Root or `sudo` privileges

### Installation Steps

**1. Update system and install dependencies:**
```bash
sudo apt update && sudo apt upgrade -y
sudo apt install build-essential libpcre3 libpcre3-dev libssl-dev zlib1g-dev ffmpeg -y

```

**2. Install NGINX with RTMP module:**

```bash
sudo apt install libnginx-mod-rtmp nginx -y

```

**3. Configure `nginx.conf`:**
Open your configuration file (`/etc/nginx/nginx.conf`) and append the following RTMP block:

```nginx
rtmp {
    server {
        listen 1935;
        chunk_size 4096;

        application live {
            live on;
            record off;
            
            # Turn on HLS
            hls on;
            hls_path /var/www/html/stream/hls;
            hls_fragment 3;
            hls_playlist_length 60;
        }
    }
}

```

**4. Restart NGINX:**

```bash
sudo mkdir -p /var/www/html/stream/hls
sudo chown -R www-data:www-data /var/www/html/stream
sudo systemctl restart nginx

```

---

## 🚀 2. Flussonic Media Server Setup (Enterprise)

For large-scale, high-concurrency IPTV businesses, Flussonic is an industry standard.

### Installation

**1. Install Flussonic via official repository:**

```bash
wget -q -O - [https://flussonic.com/public/repo.key](https://flussonic.com/public/repo.key) | sudo apt-key add -
echo "deb [http://apt.flussonic.com/repo/](http://apt.flussonic.com/repo/) master/" | sudo tee -a /etc/apt/sources.list
sudo apt update
sudo apt install flussonic flussonic-erlang -y

```

**2. Start the service and activate license:**

```bash
sudo systemctl start flussonic
# Access the web panel at http://YOUR_SERVER_IP:80 to input your license key.

```

---

## 📡 3. How to Stream (Live TV / M3U8)

Once your server (NGINX or Flussonic) is running, you can push a live stream and play it back.

### 📤 Pushing the Stream (Using OBS Studio)

* **Stream Type:** Custom Streaming Server
* **Server URL:** `rtmp://YOUR_SERVER_IP/live`
* **Stream Key:** `teststream`

### 📥 Playing the Stream (HLS/M3U8)

Use a compatible player like VLC Media Player or a web player (HLS.js) and enter the following URL:

```text
http://YOUR_SERVER_IP/stream/hls/teststream.m3u8

```

---

## 🤝 Contributing

Contributions, issues, and feature requests are welcome!
Feel free to check the [issues page](https://www.google.com/search?q=https://github.com/sohag1192/Live-TV-RTMP-HLS-M3U8/issues) if you want to contribute.

## 📝 License

This project is open-source and available under the [MIT License](https://www.google.com/search?q=LICENSE).

---

*Maintained by [@sohag1192*](https://github.com/sohag1192)
