# BT05 - Chẩn đoán và xử lý xung đột cổng mạng (Address Already in Use)

## Giả lập xung đột cổng

```bash
python3 -m http.server 8082 > /dev/null 2>&1 &
```

## Kiểm tra tiến trình chiếm cổng

```bash
$ sudo ss -tlnp | grep 8082
LISTEN 0      5                  0.0.0.0:8082       0.0.0.0:*    users:(("python3",pid=3210,fd=3))

$ ps -fp 3210
UID          PID    PPID  C STIME TTY          TIME CMD
root        3210       1  0 10:40 ?        00:00:00 python3 -m http.server 8082
```

## Giải phóng cổng và khởi động lại dịch vụ

```bash
sudo kill 3210
sudo systemctl restart spring-app.service
sudo ss -tlnp | grep 8082
LISTEN 0      100                *:8082             *:*    users:(("java",pid=3256,fd=9))
```