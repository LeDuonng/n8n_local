# Hướng Dẫn Cài Đặt n8n Local

## Các Bước Thiết Lập

### 1. Clone Project
```bash
git clone https://github.com/your-repo/n8n_local.git
cd n8n_local
```

### 2. Đăng Ký và Cài Đặt Ngrok

1. Truy cập [ngrok.com](https://ngrok.com) và đăng ký tài khoản
2. Tải và cài đặt ngrok cho Windows
3. Xác thực ngrok bằng authtoken:
```bash
ngrok config add-authtoken YOUR_AUTH_TOKEN
```

### 3. Cấu Hình Docker Compose

Mở file `docker-compose.yml` và cập nhật các biến môi trường sau với địa chỉ ngrok của bạn:
```yaml
- N8N_PROTOCOL=https
- N8N_HOST=your-ngrok-address.ngrok-free.app
- WEBHOOK_URL=https://your-ngrok-address.ngrok-free.app
- VUE_APP_URL_BASE_API=https://your-ngrok-address.ngrok-free.app
```

### 4. Khởi Động Docker Containers

Chọn một trong các lệnh sau tùy theo phần cứng của bạn:

```bash
# Cho GPU NVIDIA:
docker compose --profile gpu-nvidia up

# Cho CPU:
docker compose --profile cpu up

# Cho GPU AMD:
docker compose --profile gpu-amd up
```

### 5. Khởi Động Ngrok

Mở terminal mới và chạy:
```bash
ngrok http --url=your-ngrok-address.ngrok-free.app 5678
```

## Truy Cập n8n

- **URL**: https://your-ngrok-address.ngrok-free.app

## Sử Dụng File Batch (Tùy Chọn)

Để tự động hóa việc khởi động, tạo file `run.bat` với nội dung:
```batch
@echo off
wt -w 0 nt -p "Command Prompt" cmd /k "docker compose --profile gpu-nvidia up"
wt -w 0 nt -p "Command Prompt" cmd /k "ngrok http --url=your-ngrok-address.ngrok-free.app 5678"
```

## Cấu Trúc Project

```
n8n_local/
├── .env                 # Cấu hình môi trường
├── docker-compose.yml   # Cấu hình Docker
├── n8n/
│   └── backup/         # Thư mục backup
│       ├── credentials/
│       └── workflows/
└── shared/             # Thư mục dữ liệu chia sẻ
```

## Các Dịch Vụ

- **n8n**: Máy chủ chính (port 5678)
- **PostgreSQL**: Cơ sở dữ liệu
- **Ollama**: Máy chủ AI (port 11434)
- **Qdrant**: Vector database (port 6333)

## Ghi Chú

- Đảm bảo Docker Desktop đã được cài đặt và đang chạy
- Với GPU NVIDIA, cần cài đặt NVIDIA Container Toolkit
- Các workflow và credentials sẽ tự động được import từ thư mục backup
