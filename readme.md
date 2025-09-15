
## รัน docker

```bash
docker-compose up -d
```

## หยุด/ลบ docker
```bash
docker-compose down
```

## ติดตั้ง phpmyadmin
```bash
docker exec -it my_phpmyadmin bash
```

## ดู log container
```bash
docker-compose logs -f
```

## เข้า shell container
```bash
docker exec -it my_php bash
```

- PHP container → เปิดเว็บ http://localhost:8080
- phpMyAdmin → จัดการ MySQL ผ่าน http://localhost:8081
- MySQL container → port 3307 สำหรับเชื่อมจาก client อื่น (user: user, password: password)