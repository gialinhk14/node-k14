# node-k14

# 🚀 Hướng dẫn chạy Windows 10 trên Docker (Ubuntu/Debian/WSL2/VPS)

## 🧰 Yêu cầu:
- Ubuntu 20.04+ / Debian
- Có quyền sudo
- Hỗ trợ ảo hóa (VT-x / AMD-V)
- Có `/dev/kvm`

---

## 🪜 Cài đặt Docker & Docker Compose:

```bash
sudo apt update
sudo apt install docker.io docker-compose -y
docker -v
docker compose version
```

---

## 📂 Tạo file `windows10.yml` với nội dung:

```yaml
version: "3.8"
services:
  windows:
    image: dockurr/windows
    container_name: windows
    environment:
      VERSION: "10"
      USERNAME: "MASTER"
      PASSWORD: "admin@123"
      RAM_SIZE: "8G"
      CPU_CORES: "4"
      DISK_SIZE: "600G"
      DISK2_SIZE: "200G"
    devices:
      - /dev/kvm
      - /dev/net/tun
    cap_add:
      - NET_ADMIN
    ports:
      - "8006:8006"
      - "3389:3389/tcp"
      - "3389:3389/udp"
    stop_grace_period: 2m
```

---

## ▶️ Khởi động Windows:

```bash
sudo docker-compose -f windows10.yml up -d
```

---

## 🐛 Lỗi thường gặp:

- `no configuration file`: dùng `-f windows10.yml`
- Không thấy `/dev/kvm`: kiểm tra bằng `ls -la /dev/kvm`
- `permission denied /dev/kvm`: `sudo usermod -aG kvm $USER && newgrp kvm`
- Không tải được image: thử `docker pull dockurr/windows`

---

## 🧼 Gỡ cài đặt:

```bash
sudo docker-compose -f windows10.yml down
sudo apt remove docker.io docker-compose
```

---

## 🌐 Test mạng: [https://fast.com](https://fast.com)
