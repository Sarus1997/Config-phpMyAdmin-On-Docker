# 🐳 Docker Development Environment บน Windows (PHP + MySQL + phpMyAdmin)

## 1️⃣ เตรียมเครื่อง Windows

### เปิด Virtualization ใน BIOS
```bash
1. รีสตาร์ทเครื่อง → เข้า BIOS (F2/DEL)  
2. เปิด **Intel VT-x** หรือ **AMD-V / SVM Mode** → Save & Restart  

### เปิดฟีเจอร์ Windows ที่จำเป็น
เปิด PowerShell (Admin) แล้วรัน:
```
```powershell
dism.exe /online /enable-feature /featurename:Microsoft-Hyper-V-All /all /norestart
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
```

- รีสตาร์ทเครื่อง 1 ครั้ง

### ติดตั้ง WSL2
```bash
wsl --install
wsl --status   # ตรวจสอบว่า Default Version = 2
```

## 2️⃣ ติดตั้ง Docker Desktop

<span> 1. ดาวน์โหลด: Docker Desktop </span>
<span> 2. ติดตั้งและเปิดโปรแกรม </span>
<span> 3. ตั้งค่า: </span>
<ul>
    <li>
        Settings → General → Use the WSL 2 based engine ✅   
    </li>
    <li>
        Settings → General → Use the WSL 2 based engine ✅
    </li>
    <li>
        Settings → Resources → WSL Integration → Enable Ubuntu ✅
    </li>
</ul>
<span> 4. ทดสอบ Docker: </span>
```bash
docker run hello-world
```

## 3️⃣ สร้างโฟลเดอร์โปรเจกต์

```powershell
mkdir D:\my-project
cd D:\my-project
mkdir www
```
- เก็บโค้ด PHP ใน www
- ตัวอย่างไฟล์ index.php:
```php
<?php
phpinfo();
?>
```

## 4️⃣ สร้างไฟล์ docker-compose.yml
ไฟล์: ` D:\my-project\docker-compose.yml `

```bash
services:
  web:
    image: php:8.2-apache
    container_name: my_php
    ports:
      - "8080:80"
    volumes:
      - ./www:/var/www/html
    depends_on:
      - db

  db:
    image: mysql:8.0
    container_name: my_db
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: root
      MYSQL_DATABASE: mydb
      MYSQL_USER: user
      MYSQL_PASSWORD: password
    ports:
      - "3307:3306"
    volumes:
      - db_data:/var/lib/mysql

  phpmyadmin:
    image: phpmyadmin/phpmyadmin
    container_name: my_phpmyadmin
    restart: always
    environment:
      PMA_HOST: db
      PMA_USER: user
      PMA_PASSWORD: password
    ports:
      - "8081:80"
    depends_on:
      - db

volumes:
  db_data:

```

## 6️⃣ การใช้งานเบื้องต้น

```bash
cd D:\my-project
```
ตรวจสอบ container:
```bash
docker ps
```
- my_php → PHP http://localhost:8080
- my_db → MySQL port 3307
- my_phpmyadmin → phpMyAdmin http://localhost:8081

## 5️⃣ รัน Docker Containers
เข้าเว็บ PHP
    - http://localhost:8080

เข้า phpMyAdmin
    - http://localhost:8081
    - Username: user
    - Password: password

### รัน docker
```bash
cd D:\my-project
D:\my-project docker-compose up -d
```
### ตรวจสอบ container:
```bash
docker ps
```
### ติดตั้ง phpmyadmin
```bash
docker exec -it my_phpmyadmin bash
```
- my_php → PHP http://localhost:8080
- my_db → MySQL port 3307
- my_phpmyadmin → phpMyAdmin http://localhost:8081

## 7️⃣ คำสั่ง Docker Compose ที่สำคัญ
| คำสั่ง                        | ใช้ทำอะไร                             |
| ----------------------------- | ------------------------------------- |
| `docker-compose up -d`        | รัน container แบบ background          |
| `docker-compose down`         | หยุดและลบ container (volume ไม่ถูกลบ) |
| `docker-compose logs -f`      | ดู log ของ container แบบ real-time    |
| `docker exec -it my_php bash` | เข้า shell ของ container PHP          |
| `docker exec -it my_db bash`  | เข้า shell ของ container MySQL        |
