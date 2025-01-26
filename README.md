Here’s the **Linux-only README.md** file that you can **copy and paste in one click**. 🚀  

---


# 🚀 RTMP Streaming with Ngrok & FFmpeg (Linux)

This project sets up an **RTMP server** using **Nginx + RTMP Module**, and exposes it to the internet using **Ngrok**.  
It allows you to **stream video from FFmpeg (or a webcam) and watch it on any device using VLC or FFplay**.

---

## **🛠 Prerequisites**
- **Nginx with RTMP Module**
- **FFmpeg**
- **Ngrok**

---

## **📌 1. Install Nginx with RTMP Module**
```sh
sudo apt update
sudo apt install -y nginx libnginx-mod-rtmp
```

### ✅ **Configure RTMP in Nginx**
Edit the Nginx config file:
```sh
sudo nano /etc/nginx/nginx.conf
```
Add this at the bottom:
```nginx
rtmp {
    server {
        listen 1935;
        chunk_size 4096;
        application live {
            live on;
            record off;
        }
    }
}
```
Save and exit (`CTRL+X`, then `Y`, then `ENTER`).

Restart Nginx:
```sh
sudo systemctl restart nginx
```

---

## **📌 2. Expose RTMP Server to the Internet Using Ngrok**
### ✅ **Install & Start Ngrok**
1. Download Ngrok:
```sh
wget https://bin.equinox.io/c/bNyj1mQVY4c/ngrok-stable-linux-amd64.zip
unzip ngrok-stable-linux-amd64.zip
sudo mv ngrok /usr/local/bin/
```
2. Authenticate Ngrok:
```sh
ngrok authtoken YOUR_AUTH_TOKEN
```
3. Start the Ngrok tunnel:
```sh
ngrok tcp 1935
```
Ngrok will output something like:
```
Forwarding   tcp://5.tcp.eu.ngrok.io:16829 -> localhost:1935
```
This is your **public RTMP URL**.

---

## **📌 3. Stream Video to the RTMP Server**
You can now **stream a test video, local file, or webcam** to your Ngrok RTMP URL.

### ✅ **Stream a Test Video**
```sh
ffmpeg -re -f lavfi -i testsrc=size=1280x720:rate=30 -c:v libx264 -preset veryfast -b:v 3000k \
    -maxrate 3000k -bufsize 6000k -f flv rtmp://5.tcp.eu.ngrok.io:16829/live/test
```

### ✅ **Stream a Local Video File**
```sh
ffmpeg -re -i my_video.mp4 -c:v libx264 -preset veryfast -b:v 3000k -f flv \
    rtmp://5.tcp.eu.ngrok.io:16829/live/test
```

### ✅ **Stream from a Webcam**
```sh
ffmpeg -f v4l2 -i /dev/video0 -c:v libx264 -preset veryfast -b:v 3000k -f flv \
    rtmp://5.tcp.eu.ngrok.io:16829/live/webcam
```

---

## **📌 4. Watch the RTMP Stream**
### ✅ **Using FFplay**
```sh
ffplay rtmp://5.tcp.eu.ngrok.io:16829/live/test
```

### ✅ **Using VLC**
1. Open **VLC**.
2. **Media → Open Network Stream**.
3. Enter:
   ```
   rtmp://5.tcp.eu.ngrok.io:16829/live/test
   ```
4. **Click Play**.

---

## **📌 5. Convert RTMP Stream to HLS (For Web Browsers)**
If you want to **watch the RTMP stream in a web browser**, convert it to **HLS**:

```sh
ffmpeg -i rtmp://5.tcp.eu.ngrok.io:16829/live/test -c:v copy -c:a aac -b:a 128k \
    -f hls -hls_time 2 -hls_list_size 5 output.m3u8
```
Upload **output.m3u8** to a web server to play it in a browser.

---

## **🎯 Summary**
✅ **Set up an RTMP server with Nginx + RTMP Module**  
✅ **Expose it to the internet with Ngrok**  
✅ **Stream test video, local files, or webcam using FFmpeg**  
✅ **Watch RTMP streams with FFplay or VLC**  
✅ **Convert RTMP to HLS for web browser playback**  

🚀 **Now you can stream RTMP from anywhere!** 🎥🔥  
Let me know if you have any questions!

---

## **📜 License**
This project is licensed under the MIT License.

---

## **📢 Credits**
- **FFmpeg**: [https://ffmpeg.org/](https://ffmpeg.org/)
- **Ngrok**: [https://ngrok.com/](https://ngrok.com/)
- **Nginx RTMP Module**: [https://github.com/arut/nginx-rtmp-module](https://github.com/arut/nginx-rtmp-module)
```

---

### ✅ **Now You Can Copy & Paste This in One Click
