
# 📺 Ultimate Live TV & IPTV Media Server Setup
> A complete end-to-end guide for building a professional IPTV / Live TV Streaming System. From Hardware HDMI Encoders to Cloud Media Servers (NGINX-RTMP & Flussonic) and HLS Playback.

![Platform](https://img.shields.io/badge/Platform-Ubuntu-orange.svg)
![Protocol](https://img.shields.io/badge/Protocol-RTMP%20%7C%20HLS%20%7C%20SRT-blue.svg)
![Hardware](https://img.shields.io/badge/Hardware-HDMI%20Encoders-lightgrey.svg)
![License](https://img.shields.io/badge/License-MIT-green.svg)

---

## 📖 Overview

This repository provides a complete A-to-Z guide to building a broadcast-grade Live TV streaming infrastructure. It covers how to capture video from physical devices using a **Hardware Encoder**, send that feed to a **Media Server** (using either free NGINX or enterprise Flussonic), and distribute it globally via **HLS (M3U8)** to Smart TVs, smartphones, and web players.

## 🏗️ System Architecture

Below is the complete flow of how the video travels from the source to the viewer:

```text
  [ 1. VIDEO SOURCES ]             [ 2. HARDWARE ENCODER ]               [ 3. MEDIA SERVER ]                   [ 4. END CLIENTS ]
                                                                                                               
 +------------------+                                                                                         +------------------+
 |  4K Camera       | --(HDMI)-+                                                                         +--> |  Smart TV        |
 +------------------+          |                                                                         |    +------------------+
                               v   +-----------------------+           +-------------------+             |
 +------------------+              |               |                   |   Ubuntu Server   |             |    +------------------+
 |  Playstation     | --(HDMI)---> | 4K 60Hz HDMI Encoder  | --(WAN)-> |                   | ---(HLS)----+--> |  Mobile Phone    |
 +------------------+              | (H.264 / H.265)       | (RTMP/SRT)|  NGINX-RTMP   OR  | (.m3u8)     |    |  (iOS / Android) |
                               ^   +-----------------------+           |  Flussonic        |             |    +------------------+
 +------------------+          |                                       +-------------------+             |
 |  Set-Top Box /   | --(HDMI)-+                                                 |                       |    +------------------+
 |  DVD Player      |                                                            | (Recording)           +--> |  Web / PC Player |
 +------------------+                                                            v                            |  (VLC, HLS.js)   |
                                                                       +-------------------+                  +------------------+
                                                                       |  Storage / VOD    |
                                                                       +-------------------+

```

---

## 🔌 Step 1: Hardware Encoder Setup (Ingest)

To capture physical devices (Cameras, Consoles, DVD players), we use a dedicated hardware encoder like the **TBS2801 Professional 4K HDMI Encoder**.

1. **Connect Video Sources:** Plug your cameras, gaming consoles, or set-top boxes into the **HDMI In** ports on the encoder.
2. **Network Connection:** Connect the encoder to your router/switch via Ethernet.
3. **Configure the Encoder:**
* Access the encoder's web panel via its IP address (e.g., `192.168.x.x`).
* Set the video encoding format to **H.264** or **H.265**.
* Set the output protocol to **RTMP** or **SRT**.
* Set the Target URL to point to your Media Server: `rtmp://YOUR_SERVER_IP/live/stream_key`



---

## 🛠️ Step 2: Media Server Setup (Choose One)

You need a server to receive the encoder's feed and distribute it to thousands of users. Choose either the Open-Source (Free) or Enterprise method.

### Option A: NGINX-RTMP & HLS (Open Source / Free)

*Best for budget setups, small-to-medium streaming, and developers.*

**1. Install NGINX and RTMP module on Ubuntu:**

```bash
sudo apt update && sudo apt upgrade -y
sudo apt install build-essential ffmpeg libnginx-mod-rtmp nginx -y

```

**2. Configure `nginx.conf`:**
Add the following to `/etc/nginx/nginx.conf` to convert incoming RTMP to HLS:

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

**3. Restart the Server:**

```bash
sudo mkdir -p /var/www/html/stream/hls
sudo chown -R www-data:www-data /var/www/html/stream
sudo systemctl restart nginx

```

---

### Option B: Flussonic Media Server (Enterprise)

*Best for large-scale IPTV businesses, high concurrency, and advanced DRM/Recording features.*

**1. Install Flussonic:**

```bash
wget -q -O - [https://flussonic.com/public/repo.key](https://flussonic.com/public/repo.key) | sudo apt-key add -
echo "deb [http://apt.flussonic.com/repo/](http://apt.flussonic.com/repo/) master/" | sudo tee -a /etc/apt/sources.list
sudo apt update
sudo apt install flussonic flussonic-erlang -y

```

**2. Start and Activate:**

```bash
sudo systemctl start flussonic

```

*Access the web panel at `http://YOUR_SERVER_IP:80` to input your license key and manage your streams visually.*

---

## 📡 Step 3: Playback & Distribution

Once your encoder is pushing video to the server, you can watch it anywhere!

**The HLS (M3U8) Playback URL:**

```text
http://YOUR_SERVER_IP/stream/hls/stream_key.m3u8

```

### Where to play it:

* **Smart TVs:** Add the `.m3u8` link to IPTV apps like Smarters Pro or TiviMate.
* **Computers:** Open **VLC Media Player** -> Media -> Open Network Stream -> Paste the URL.
* **Websites:** Embed the stream on your own website using HTML5 players like [Video.js](https://videojs.com/) or [HLS.js](https://github.com/video-dev/hls.js/).

---

## 🤝 Contributing

Contributions, issues, and feature requests are welcome!
Check the [issues page](https://www.google.com/search?q=https://github.com/sohag1192/Live-TV-RTMP-HLS-M3U8/issues) if you want to contribute.

## 📝 License

This project is open-source and available under the [MIT License](https://www.google.com/search?q=LICENSE).

---

*Maintained by [@sohag1192*](https://github.com/sohag1192)

