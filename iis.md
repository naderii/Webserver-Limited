<div dir="rtl">

## **🔐 1. محدودسازی نرخ درخواست (Rate Limiting)**
```bash
<system.webServer>
  <security>
    <requestFiltering>
      <requestLimits maxAllowedContentLength="30000000" />
    </requestFiltering>
  </security>
</system.webServer>
```
✅ **کاربرد**: کنترل حجم درخواست‌های ورودی برای جلوگیری از سوءاستفاده یا حملات DoS ساده

---

## **🔌 2. محدودسازی اتصالات همزمان**
```bash
<system.applicationHost>
  <limits maxConcurrentRequestsPerCPU="5000"
          maxConcurrentThreadsPerCPU="0"
          requestQueueLimit="5000" />
</system.applicationHost>
```
✅ **کاربرد**:  جلوگیری از استفاده بیش از حد منابع سرور از طریق درخواست‌های همزمان

---

## **🛡 3. فیلتر IP آدرس**
```bash
<system.webServer>
  <security>
    <ipSecurity allowUnlisted="false">
      <add ipAddress="192.168.1.0" subnetMask="255.255.255.0" allowed="true" />
      <add ipAddress="10.0.0.1" allowed="true" />
    </ipSecurity>
  </security>
</system.webServer>
```
✅ **کاربرد**:  محدود کردن دسترسی به منابع تنها برای IP‌های مجاز

---

## **🚫 4. بلاک کردن User-Agent های مخرب**
```bash
<system.webServer>
  <security>
    <requestFiltering>
      <filteringRules>
        <filteringRule name="BlockBadBots" scanUrl="false" scanQueryString="false">
          <scanHeaders>
            <add requestHeader="User-Agent" />
          </scanHeaders>
          <appliesTo>
            <add fileExtension="*" />
          </appliesTo>
          <denyStrings>
            <add string="wget" />
            <add string="curl" />
            <add string="nikto" />
          </denyStrings>
        </filteringRule>
      </filteringRules>
    </requestFiltering>
  </security>
</system.webServer>
```
✅ **کاربرد**:جلوگیری از دسترسی ابزارهای اسکن خودکار یا بات‌های مهاجم

---

## **📛 5. مسدودسازی فایل‌های حساس**
```bash
<system.webServer>
  <security>
    <requestFiltering>
      <fileExtensions>
        <add fileExtension=".env" allowed="false" />
        <add fileExtension=".log" allowed="false" />
        <add fileExtension=".htaccess" allowed="false" />
      </fileExtensions>
    </requestFiltering>
  </security>
</system.webServer>
```
✅ **کاربرد**: جلوگیری از دسترسی به فایل‌های حساس که ممکن است اطلاعات مهمی را فاش کنند

---

## **🛡 6. هدرهای امنیتی**
```bash
<system.webServer>
  <httpProtocol>
    <customHeaders>
      <add name="X-Frame-Options" value="SAMEORIGIN" />
      <add name="X-Content-Type-Options" value="nosniff" />
      <add name="X-XSS-Protection" value="1; mode=block" />
      <add name="Content-Security-Policy" value="default-src 'self'" />
    </customHeaders>
  </httpProtocol>
</system.webServer>
```
✅ **کاربرد**:  جلوگیری از حملات Clickjacking، XSS و MIME sniffing با ارسال هدرهای محافظتی

---

## **⚡ 7. محافظت در برابر Slowloris**
```bash
<system.webServer>
  <serverRuntime enabled="true" 
                maxRequestEntityAllowed="102400"
                uploadReadAheadSize="49152"
                appConcurrentRequestLimit="5000" />
</system.webServer>
```
✅ **کاربرد**: مقابله با حملات Slowloris که با ارسال آهسته درخواست‌ها باعث اشغال منابع سرور می‌شوند

---

## **🔒 8. محدودسازی متدهای HTTP**
```bash
<system.webServer>
  <security>
    <requestFiltering>
      <verbs allowUnlisted="false">
        <add verb="GET" allowed="true" />
        <add verb="POST" allowed="true" />
      </verbs>
    </requestFiltering>
  </security>
</system.webServer>
```
✅ **کاربرد**: فقط متدهای مورد نیاز HTTP را فعال کنید و متدهای غیرضروری را مسدود نمایید (مانند PUT، DELETE، OPTIONS و غیره)

---

## **💡 نکات نهایی**
1. تغییرات را در **ApplicationHost.config** یا **web.config** اعمال کنید
2. از **IIS Manager** برای مدیریت گرافیکی تنظیمات استفاده نمایید
3. ماژول **URL Rewrite** را برای قوانین پیشرفته نصب کنید
4. پس از تغییرات حتما IIS را ریستارت کنید

> **⚠️ هشدار**: تنظیمات را با دقت اعمال کنید چون IIS نسبت به Nginx/Apache حساسیت بیشتری دارد


</div>