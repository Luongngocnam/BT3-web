# Lương Ngọc Nam - K225480106025
# K58ktp - Môn phát triển ứng dụng trên nền web
# Nội dung bài tập 3:
Yêu cầu     : LẬP TRÌNH ỨNG DỤNG WEB trên nền linux
1. Cài đặt môi trường linux: SV chọn 1 trong các phương án
 - enable wsl: cài đặt docker desktop
 - enable wsl: cài đặt ubuntu
 - sử dụng Hyper-V: cài đặt ubuntu
 - sử dụng VMware : cài đặt ubuntu
 - sử dụng Virtual Box: cài đặt ubuntu
2. Cài đặt Docker (nếu dùng docker desktop trên windows thì nó có ngay)
3. Sử dụng 1 file docker-compose.yml để cài đặt các docker container sau: 
   mariadb (3306), phpmyadmin (8080), nodered/node-red (1880), influxdb (8086), grafana/grafana (3000), nginx (80,443)
4. Lập trình web frontend+backend:
 SV chọn 1 trong các web sau:
 4.1 Web thương mại điện tử
 - Tạo web dạng Single Page Application (SPA), chỉ gồm 1 file index.html, toàn bộ giao diện do javascript sinh động.
 - Có tính năng login, lưu phiên đăng nhập vào cookie và session
   Thông tin login lưu trong cơ sở dữ liệu của mariadb, được dev quản trị bằng phpmyadmin, yêu cầu sử dụng mã hoá khi gửi login.
   Chỉ cần login 1 lần, bao giờ logout thì mới phải login lại.
 - Có tính năng liệt kê các sản phẩm bán chạy ra trang chủ
 - Có tính năng liệt kê các nhóm sản phẩm
 - Có tính năng liệt kê sản phẩm theo nhóm
 - Có tính năng tìm kiếm sản phẩm
 - Có tính năng chọn sản phẩm (đưa sản phẩm vào giỏ hàng, thay đổi số lượng sản phẩm trong giỏ, cập nhật tổng tiền)
 - Có tính năng đặt hàng, nhập thông tin giao hàng => được 1 đơn hàng.
 - Có tính năng dành cho admin: Thống kê xem có bao nhiêu đơn hàng, call để xác nhận và cập nhật thông tin đơn hàng. chuyển cho bộ phận đóng gói, gửi bưu điện, cập nhật mã COD, tình trạng giao hàng, huỷ hàng,...
 - Có tính năng dành cho admin: biểu đồ thống kê số lượng mặt hàng bán được trong từng ngày. (sử dụng grafana)
 - backend: sử dụng nodered xử lý request gửi lên từ javascript, phản hồi về json.
 4.2 Web IOT: Giám sát dữ liệu IOT.
 - Tạo web dạng Single Page Application (SPA), chỉ gồm 1 file index.html, toàn bộ giao diện do javascript sinh động.
 - Có tính năng login, lưu phiên đăng nhập vào cookie và session
   Thông tin login lưu trong cơ sở dữ liệu của mariadb, được dev quản trị bằng phpmyadmin, yêu cầu sử dụng mã hoá khi gửi login.
   Chỉ cần login 1 lần, bao giờ logout thì mới phải login lại.
 - hiển thị giá trị mới nhất của các thông số đang giám sát, khi click vào thì hiển thị đồ thị lịch sử quá trình thay đổi (gọi grafana iframe để hiển thị)
 - backend: Sử dụng nodered để đọc dữ liệu từ các cảm biến (có thể dùng api online để lấy dữ liệu theo giời gian thực), 
   nodered sẽ lưu dữ liệu mới nhất (dạng update) vào cơ sở dữ liệu mariadb (sử dụng phpmyadmin để tạp table và quản trị lần đầu)
   nodered sẽ lưu dữ liệu (insert) vào influxdb để lưu giá trị lịch sử, để cho grafana dùng để hiển thị biểu đồ.
5. Nginx làm web-server
 - Cấu hình nginx để chạy được website qua url http://fullname.com  (thay fullname bằng chuỗi ko dấu viết liền tên của bạn)
 - Cấu hình nginx để http://fullname.com/nodered truy cập vào nodered qua cổng 80, (dù nodered đang chạy ở port 1880)
 - Cấu hình nginx để http://fullname.com/grafana truy cập vào grafana qua cổng 80, (dù grafana đang chạy ở port 3000)
# -----BÀI LÀM-----

## 1. Chọn phương án Docker Desktop + WSL2

1.1.  Bật WSL2 (Windows Subsystem for Linux)
đã cài ubuntu thành công
- <img width="1911" height="806" alt="image" src="https://github.com/user-attachments/assets/d9f50b41-a03b-4dbc-9b82-a4bdca3fac8b" />
2.1 Tải Docker Desktop
- Vào link chính thức: https://www.docker.com/
Chọn: Tải xuống cho Windows – AMD64
2.2 Bật tích hợp WSL 
- Mở Docker Desktop → Settings → Resources → WSL Integration
- Bật:
“Enable integration with my default WSL distro”
“Ubuntu”
Sau đó bấm Apply & Restart
2.3 Kiểm tra Docker trong Ubuntu
- Mở lại terminal Ubuntu (WSL2) và gõ: docker version
  
- <img width="1919" height="950" alt="image" src="https://github.com/user-attachments/assets/e16ff7fc-a7f2-49b9-91e5-69f996333d28" />

→ Docker đã hoạt động thành công 🎉

## 3. DỰNG HỆ THỐNG DOCKER BẰNG FILE docker-compose.yml

3.1 Tạo thư mục dự án

- Trong Ubuntu (WSL2), gõ:

cd /mnt/d

mkdir baitap3_web

cd baitap3_web
<img width="1907" height="1079" alt="image" src="https://github.com/user-attachments/assets/02b912f4-b4ca-4146-93ea-577374326add" />

3.2 Tạo file docker-compose.yml
nano docker-compose.yml
<img width="1912" height="1018" alt="image" src="https://github.com/user-attachments/assets/2a29002f-5f14-4610-ba8c-8494d6b6a5a5" />

- Sao chép toàn bộ nội dung bên dưới
    
```
version: "3.8"

services:
  mariadb:
    image: mariadb:10.6
    container_name: mariadb
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: root
      MYSQL_DATABASE: webdb
    ports:
      - "3306:3306"
    volumes:
      - mariadb_data:/var/lib/mysql

  phpmyadmin:
    image: phpmyadmin/phpmyadmin
    container_name: phpmyadmin
    restart: always
    environment:
      PMA_HOST: mariadb
      PMA_USER: root
      PMA_PASSWORD: root
    ports:
      - "8080:80"
    depends_on:
      - mariadb

  nodered:
    image: nodered/node-red
    container_name: nodered
    restart: always
    ports:
      - "1880:1880"
    volumes:
      - nodered_data:/data

  influxdb:
    image: influxdb:1.8
    container_name: influxdb
    restart: always
    ports:
      - "8086:8086"
    volumes:
      - influxdb_data:/var/lib/influxdb

  grafana:
    image: grafana/grafana
    container_name: grafana
    restart: always
    ports:
      - "3000:3000"
    depends_on:
      - influxdb
    environment:
      - GF_SECURITY_ADMIN_USER=admin
      - GF_SECURITY_ADMIN_PASSWORD=admin

  nginx:
    image: nginx:latest
    container_name: nginx
    restart: always
    ports:
      - "80:80"
      - "443:443"
    volumes:
      - ./nginx.conf:/etc/nginx/nginx.conf:ro
      - ./frontend:/usr/share/nginx/html

volumes:
  mariadb_data:
  influxdb_data:
  nodered_data:
```
- Nhấn Ctrl + O → Enter để lưu

- Nhấn Ctrl + X để thoát khỏi nano

3.3 Tạo file nginx.conf

- Trong thư mục /mnt/d/baitap3_web, gõ lệnh: nano nginx.conf

Dán nội dung dưới đây:
```
events {}

http {
  server {
    listen 80;
    server_name luongnam.com;

    # Trang web chính (Frontend)
    location / {
      root /usr/share/nginx/html;
      index index.html;
    }

    # Truy cập Node-RED qua http://luongnam.com/nodered
    location /nodered/ {
      proxy_pass http://nodered:1880/;
      proxy_set_header Host $host;
      proxy_set_header X-Real-IP $remote_addr;
    }

    # Truy cập Grafana qua http://luongnam.com/grafana
    location /grafana/ {
      proxy_pass http://grafana:3000/;
      proxy_set_header Host $host;
      proxy_set_header X-Real-IP $remote_addr;
    }
  }
}
````
<img width="1638" height="878" alt="image" src="https://github.com/user-attachments/assets/73ddd578-9299-4fdc-84be-50cd5fe07da9" />
Nhấn Ctrl + O → Enter để lưu
Nhấn Ctrl + X để thoát

3.4 Tạo thư mục giao diện web
- Trong Ubuntu ( ở thư mục /mnt/d/baitap3_web), gõ: mkdir frontend
- Tạo file index.html cơ bản để kiểm tra: nano frontend/index.html
  
  ```
<!DOCTYPE html>
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Website Lương Ngọc Nam</title>
    <link href="https://fonts.googleapis.com/css2?family=Montserrat:wght@400;700&display=swap" rel="stylesheet">
    <style>
        body {
       
            font-family: 'Montserrat', sans-serif;
      
            background: linear-gradient(135deg, #FF6B6B, #556270);
            color: #ffffff; /* Chữ trắng */
            text-align: center;
            padding: 80px;
            min-height: 100vh;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            margin: 0;
        }
        h1 {
            font-size: 52px;
            margin-bottom: 25px;
            font-weight: 700;
            text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.3);
        }
        p {
            font-size: 22px;
            margin-bottom: 40px;
        }
        .btn-container {
            display: flex;
            gap: 20px; 
        }
        .btn {
            background-color: #ffffff;
            color: #FF6B6B; 
            padding: 14px 30px;
            text-decoration: none;
            border-radius: 50px; 
            font-weight: bold;
            font-size: 18px;
            transition: all 0.4s ease; 
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
        }
        .btn:hover {
            background-color: #556270; 
            color: #ffffff; 
            transform: translateY(-3px); 
            box-shadow: 0 6px 8px rgba(0, 0, 0, 0.2);
        }
    </style>
</head>
<body>
    <h1>🚀 Website Lương Ngọc Nam</h1>
    <p>Chào mừng đến với không gian cá nhân của Nam. Hệ thống đang chạy trên Docker + WSL2!</p>
    <div class="btn-container">
        <a href="/nodered/" class="btn">Truy cập Node-RED</a>
        <a href="/grafana/" class="btn">Xem biểu đồ Grafana</a>
    </div>
</body>
</html>
```

Lưu lại file
Ctrl + O → Enter 
Ctrl + X
3.5 Chạy toàn bộ hệ thống
- Giờ đã có đủ 3 thành phần:
docker-compose.yml
nginx.conf
frontend/index.html
- Chạy: docker compose up -d
Docker sẽ bắt đầu tải và chạy 6 container:
mariadb, phpmyadmin, nodered, influxdb, grafana, nginx

<img width="1581" height="841" alt="image" src="https://github.com/user-attachments/assets/a35fb1b2-7597-4b6a-888a-e1570434a215" />

3.6 Kiểm tra trên trình duyệt

Dịch vụ	Cổng	Truy cập
Trang chính (Nginx)	80	http://localhost
<img width="1919" height="1078" alt="image" src="https://github.com/user-attachments/assets/aae71cdd-c05d-4671-9ed2-b7d7b4578203" />

phpMyAdmin	8080	http://localhost:8080
<img width="1919" height="1079" alt="image" src="https://github.com/user-attachments/assets/d4a7aa77-169f-4461-82d7-182259326c88" />

Node-RED	1880	http://localhost:1880
<img width="1919" height="1079" alt="image" src="https://github.com/user-attachments/assets/aff063d6-038f-4cea-9bcd-f60c3a147f82" />

Grafana	3000	http://localhost:3000
<img width="1919" height="1079" alt="image" src="https://github.com/user-attachments/assets/50f76a92-37e1-49cc-ae94-ceb2ef33cf75" />











- 
