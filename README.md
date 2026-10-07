# Bài 3: Cấu hình tường lửa UFW và chẩn đoán cổng mạng

## 1. Cấu hình tường lửa UFW

### Thiết lập chính sách mặc định
```bash
sudo ufw default deny incoming
sudo ufw default allow outgoing
```

### Mở cổng SSH (22/tcp) và Web Application (8080/tcp)
```bash
sudo ufw allow 22/tcp
sudo ufw allow 8080/tcp
```

### Kích hoạt UFW
```bash
sudo ufw enable
```

---

## 2. Kiểm tra và chẩn đoán cổng mạng

### Trạng thái tường lửa chi tiết (`sudo ufw status verbose`)
```text
Status: active
Logging: on (low)
Default: deny (incoming), allow (outgoing), disabled (routed)
New profiles: skip

To                         Action      From
--                         ------      ----
22/tcp                     ALLOW IN    Anywhere                  
8080/tcp                   ALLOW IN    Anywhere                  
22/tcp (v6)                ALLOW IN    Anywhere (v6)             
8080/tcp (v6)              ALLOW IN    Anywhere (v6)             
```

### Các cổng đang lắng nghe thực tế (`ss -tlnp`)
```text
State    Recv-Q   Send-Q     Local Address:Port      Peer Address:Port   Process                                          
LISTEN   0        128              0.0.0.0:22             0.0.0.0:*       users:(("sshd",pid=812,fd=3))                    
LISTEN   0        4096             0.0.0.0:8080           0.0.0.0:*       users:(("app",pid=1420,fd=6))                    
LISTEN   0        128                 [::]:22                [::]:*       users:(("sshd",pid=812,fd=4))                    
LISTEN   0        4096                [::]:8080              [::]:*       users:(("app",pid=1420,fd=7))                    
```

### Kiểm tra kết nối ứng dụng (`curl`)
```bash
curl -I http://localhost:8080/health
```

Output:
```text
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
Date: Wed, 07 Oct 2026 12:25:00 GMT
```
