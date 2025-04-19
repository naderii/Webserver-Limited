<div dir="rtl">

## **🔐 1. محدودسازی نرخ درخواست (Rate Limiting)**
```bash
limit_req_zone $binary_remote_addr zone=req_zone:10m rate=10r/s;

server {
    location / {
        limit_req zone=req_zone burst=20 nodelay;
    }
}
```
✅ **کاربرد**: جلوگیری از حملات Brute Force و DDoS  
⚙ **پارامترها**:
- `rate=10r/s`: 10 درخواست بر ثانیه
- `burst=20`: تحمل 20 درخواست سریع
- `nodelay`: اعمال محدودیت فوری

---

## **🔌 2. محدودسازی اتصالات همزمان**
```bash
limit_conn_zone $binary_remote_addr zone=conn_zone:10m;

server {
    location / {
        limit_conn conn_zone 5;
    }
}
```
✅ **کاربرد**: جلوگیری از مصرف منابع توسط یک IP

---

## **🛡 3. فیلتر IP آدرس**
```bash
location /admin {
    allow 192.168.1.0/24;
    allow 10.0.0.1;
    deny all;
}
```
✅ **کاربرد**: محدود کردن دسترسی به بخش‌های حساس

---

## **🚫 4. بلاک کردن User-Agent های مخرب**
```bash
map $http_user_agent $bad_agent {
    default 0;
    ~*(wget|curl|nikto|sqlmap) 1;
}

server {
    if ($bad_agent) {
        return 403;
    }
}
```
✅ **کاربرد**: جلوگیری از دسترسی ابزارهای اسکن یا ربات‌های مهاجم با استفاده از User-Agent

---

## **📛 5. مسدودسازی فایل‌های حساس**
```bash
location ~* \.(env|log|htaccess|git|svn)$ {
    deny all;
    return 404;
}
```
✅ **کاربرد**: جلوگیری از دسترسی به فایل‌هایی که ممکن است شامل اطلاعات حساس باشند

---

## **🛡 6. هدرهای امنیتی**
```bash
add_header X-Frame-Options "SAMEORIGIN";
add_header X-Content-Type-Options "nosniff";
add_header X-XSS-Protection "1; mode=block";
add_header Content-Security-Policy "default-src 'self'";
```
✅ **کاربرد**:  جلوگیری از حملات XSS، Clickjacking و MIME sniffing
  ⚙ توضیح کوتاه:

 - X-Frame-Options: جلوگیری از نمایش سایت در iFrameهای دیگر 

 - X-XSS-Protection: فعال‌سازی محافظت مرورگر در برابر XSS

 - Content-Security-Policy: محدودسازی منابع به دامنه خود

---

## **⚡ 7. محافظت در برابر Slowloris**
```bash
client_body_timeout 5s;
client_header_timeout 5s;
client_max_body_size 100k;
keepalive_timeout 10s;
```
✅ **کاربرد**: جلوگیری از باز نگه داشتن طولانی اتصال توسط کلاینت‌های مخرب
 ⚙ توضیح:

 - client_body_timeout و client_header_timeout: تعیین زمان انتظار دریافت داده از کلاینت

 - client_max_body_size: محدودسازی حجم درخواست 

 - keepalive_timeout: بستن سریع‌تر اتصال‌های بلااستفاده



---

## **🔒 8. محدودسازی متدهای HTTP**
```bash
location /api {
    limit_except GET POST {
        deny all;
    }
}
```
✅ **کاربرد**:  جلوگیری از اجرای متدهای خطرناک مانند PUT, DELETE, TRACE
 ⚙ توضیح: تنها متدهای GET و POST مجاز هستند.

---

## **💡 نکات نهایی**
1. همیشه تغییرات را با `nginx -t` تست کنید
2. پس از اعمال تنظیمات `systemctl reload nginx` بزنید
3. از ابزارهایی مثل `fail2ban` برای امنیت بیشتر استفاده کنید

> **⚠️ هشدار**: تنظیمات را با توجه به ترافیک واقعی سرور اعمال کنید تا باعث اختلال در سرویس نشود.


</div>