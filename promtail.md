عالیه! برای مانیتور کردن لاگ‌های NGINX با استفاده از **Grafana Loki**، معمولاً از ابزار **Promtail** برای جمع‌آوری لاگ‌ها استفاده می‌شه. در ادامه یه راهنمای کلی و کاربردی برات می‌نویسم:

---

### مراحل مانیتورینگ لاگ‌های NGINX با Grafana Loki

#### 1. **نصب Loki و Grafana**

تو گفتی که Loki و Grafana رو نصب کردی، پس این مرحله رو رد می‌کنیم (اگه نیاز به بررسی داشتی، بگو).

---

#### 2. **نصب و پیکربندی Promtail روی همون ماشینی که NGINX نصبه**

**نمونه فایل `promtail-config.yaml`:**

```yaml
server:
  http_listen_port: 9080
  grpc_listen_port: 0

positions:
  filename: /tmp/positions.yaml

clients:
  - url: http://<LOKI-HOST>:3100/loki/api/v1/push

scrape_configs:
  - job_name: nginx-logs
    static_configs:
      - targets:
          - localhost
        labels:
          job: nginx
          __path__: /var/log/nginx/access.log
      - targets:
          - localhost
        labels:
          job: nginx
          __path__: /var/log/nginx/error.log
```

**نکات مهم:**

* آدرس `LOKI-HOST` رو با آدرس لوکی خودت جایگزین کن.
* اگه لاگ‌ها جای دیگه‌ای هستن، مسیر `__path__` رو تنظیم کن.

---

#### 3. **اجرای Promtail**

مثلاً اگه از باینری استفاده می‌کنی:

```bash
./promtail -config.file=promtail-config.yaml
```

یا اگه با Docker می‌خوای:

```bash
docker run -d --name=promtail \
  -v /var/log/nginx:/var/log/nginx \
  -v $(pwd)/promtail-config.yaml:/etc/promtail/promtail-config.yaml \
  grafana/promtail:latest \
  -config.file=/etc/promtail/promtail-config.yaml
```

---

#### 4. **تنظیم داشبورد در Grafana**

1. وارد Grafana بشو.
2. منبع داده Loki رو اضافه کن.
3. یه داشبورد جدید بساز و توی قسمت Query اینو بزن:

   ```
   {job="nginx"}
   ```
4. می‌تونی با استفاده از فیلترهایی مثل `|~ "404"` درخواست‌های خاص رو پیدا کنی.

---

اگه خواستی برای Kubernetes راه‌اندازی کنی یا فایل‌های دقیق‌تر (مثل LogQL) بنویسیم، بگو تا برات تنظیم کنیم.
