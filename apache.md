<div dir="rtl">

## **🔐 1. محدودسازی نرخ درخواست (Rate Limiting)**
```bash
<IfModule mod_ratelimit.c>
    <Location "/">
        SetOutputFilter RATE_LIMIT
        SetEnv rate-limit 100
    </Location>
</IfModule>
```

✅ **کاربرد**: جلوگیری از حملات Brute Force و DDoS

---

## **🔌 2. محدودسازی اتصالات همزمان**
```bash
<IfModule mod_prefork.c>
    MaxClients 150
    MaxRequestsPerChild 1000
</IfModule>
```
✅ **کاربرد**: کنترل مصرف منابع سرور

---

## **🛡 3. فیلتر IP آدرس**
```bash
<Directory "/var/www/admin">
    Require ip 192.168.1.0/24
    Require ip 10.0.0.1
</Directory>
```
✅ **کاربرد**: محدود کردن دسترسی به بخش‌های حساس

---

## **🚫 4. بلاک کردن User-Agent های مخرب**
```bash
<IfModule mod_rewrite.c>
    RewriteEngine On
    RewriteCond %{HTTP_USER_AGENT} (wget|curl|nikto|sqlmap) [NC]
    RewriteRule ^ - [F]
</IfModule>
```
✅ **کاربرد**: جلوگیری از دسترسی ابزارهای مخرب و اتوماسیون

---

## **📛 5. مسدودسازی فایل‌های حساس**
```bash
<FilesMatch "\.(env|log|htaccess|git|svn)$">
    Require all denied
</FilesMatch>
```
✅ **کاربرد**: جلوگیری از دسترسی عمومی به فایل‌های پیکربندی

---

## **🛡 6. هدرهای امنیتی**
```bash
Header always set X-Frame-Options "SAMEORIGIN"
Header always set X-Content-Type-Options "nosniff"
Header always set X-XSS-Protection "1; mode=block"
Header always set Content-Security-Policy "default-src 'self'"
```
✅ **کاربرد**: محافظت در برابر حملات XSS، Clickjacking و MIME Sniffing

---

## **⚡ 7. محافظت در برابر Slowloris**
```bash
Timeout 30
KeepAlive On
MaxKeepAliveRequests 100
KeepAliveTimeout 5
LimitRequestBody 102400
```
✅ **کاربرد**: کاهش تاثیر حملات با اتصالات طولانی مدت

---

## **🔒 8. محدودسازی متدهای HTTP**
```bash
<LimitExcept GET POST>
    Require all denied
</LimitExcept>
```
✅ **کاربرد**: جلوگیری از سوء‌استفاده با متدهای غیرمجاز مثل PUT و DELETE

---

## **💡 نکات نهایی**
1. همیشه تغییرات را با `apachectl configtest` تست کنید
2. پس از اعمال تنظیمات `systemctl reload apache2` بزنید
3. از ماژول‌های امنیتی مثل mod_security استفاده کنید

> **⚠️ هشدار**: تنظیمات را با توجه به سخت‌افزار سرور و ترافیک واقعی اعمال کنید

</div>