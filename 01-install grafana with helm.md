

# نصب Grafana با استفاده از Helm

زمانی که شما Grafana را با Helm نصب می‌کنید، مراحل زیر را انجام می‌دهید:

1. راه‌اندازی مخزن Helm مربوط به Grafana
2. استقرار (Deploy) Grafana با استفاده از Helm در یک Namespace مجزا
3. دسترسی به داشبورد Grafana



## 1. راه‌اندازی مخزن Helm مربوط به Grafana

برای راه‌اندازی مخزن Helm که شامل چارت‌های مربوط به Grafana است، مراحل زیر را انجام دهید:

### اضافه کردن مخزن Helm مربوط به Grafana:

```bash
helm repo add grafana https://grafana.github.io/helm-charts
````

### بررسی اینکه مخزن به درستی اضافه شده است:

```bash
helm repo list
```

خروجی مورد انتظار:

```
NAME    URL
grafana https://grafana.github.io/helm-charts
```

### به‌روزرسانی مخازن Helm برای دریافت آخرین نسخه چارت‌ها:

```bash
helm repo update
```

---

## 2. استقرار Grafana با استفاده از Helm

### ایجاد یک Namespace جدید به نام `monitoring`:

```bash
kubectl create namespace monitoring
```

خروجی مورد انتظار:

```
namespace/monitoring created
```

### جستجوی چارت رسمی Grafana:

```bash
helm search repo grafana/grafana
```

### نصب Grafana در Namespace مشخص شده:

```bash
helm install my-grafana grafana/grafana --namespace monitoring
```

**توضیح پارامترها:**

* `helm install`: دستور نصب چارت
* `my-grafana`: نام دلخواه برای نصب شما
* `grafana/grafana`: مسیر مخزن و بسته
* `--namespace monitoring`: نصب در Namespace مشخص

### بررسی وضعیت نصب:

```bash
helm list -n monitoring
```

خروجی نمونه:

```
NAME            NAMESPACE       REVISION        UPDATED                                 STATUS   CHART          APP VERSION
my-grafana      monitoring      1               2024-01-13 23:06:42.737989554 +0000 UTC deployed grafana-<ver>  <app-ver>
```

---

## 3. دسترسی به Grafana

برای دسترسی به داشبورد Grafana، می‌توانید دستور زیر را اجرا کنید تا اطلاعات دسترسی را مشاهده نمایید (مانند آدرس IP و پورت):

```bash
kubectl get svc -n monitoring
```

سپس با مرورگر خود وارد آدرس مربوطه شوید. اطلاعات پیش‌فرض ورود به صورت زیر است (مگر اینکه به صورت دیگری تعریف کرده باشید):

* **Username**: `admin`
* **Password**: در خروجی دستور زیر موجود است:

```bash
kubectl get secret --namespace monitoring my-grafana -o jsonpath="{.data.admin-password}" | base64 --decode ; echo
```

```

اگر مایل باشید، می‌توانم این راهنما را به صورت فایل Markdown (`.md`) برایتان آماده کنم.
```
