
> **Grafana + Loki + Prometheus**

اینجا تمرکز ما روی **Grafana Loki** هست — ابزاری که دقیقاً برای لاگینگ در معماری cloud-native طراحی شده.

---

## 🟨 Loki چیه؟ (و چرا با Grafana جوره)

**Grafana Loki** یک سیستم لاگ جمع‌کن (log aggregation system) هست که:

* توسط Grafana توسعه داده شده
* بسیار شبیه به **Prometheus** کار می‌کنه، ولی برای **لاگ‌ها** نه متریک‌ها
* بر خلاف ELK stack (Elasticsearch/Logstash/Kibana)، منابع کمتری مصرف می‌کنه

---

## 📦 ویژگی‌های اصلی Loki

| ویژگی                    | توضیح                                                                 |
| ------------------------ | --------------------------------------------------------------------- |
| **lightweight**          | نسبت به ELK بسیار سبک‌تره                                             |
| **native integration**   | با Grafana ادغام کامله، نیازی به پلاگین نداری                         |
| **label-based indexing** | لاگ‌ها رو مثل Prometheus با label ذخیره می‌کنه، نه full-text indexing |
| **Kubernetes-friendly**  | خیلی راحت لاگ‌های Podها، namespaceها و containerها رو جمع می‌کنه      |

---

## 🔧 معماری ساده Loki در Kubernetes

```
Pods (App/CI)  
   │
[ Promtail / Fluent Bit ]  ← جمع‌آوری لاگ‌ها
   │
[       Loki        ]       ← ذخیره‌سازی و ایندکس بر اساس Label
   │
[     Grafana       ]       ← نمایش و جستجو
```

---

## 🚀 برای مانیتور CI/CD و K8s چه کاری می‌تونه بکنه؟

| نیاز                                         | چی رو می‌بینی؟                                                    |
| -------------------------------------------- | ----------------------------------------------------------------- |
| لاگ pipeline‌ها (GitLab, ArgoCD, Jenkins...) | لاگ کامل مراحل build/deploy/test                                  |
| لاگ پادهای K8s                               | هر خطا، crash، خطاهای network، یا custom log                      |
| هشدار (alerting)                             | می‌تونی رو لاگ‌ها alert بذاری (مثلاً هر وقت `error` اومد)         |
| فیلتر با label                               | مثلا فقط لاگ‌های namespace `cicd` یا پاد `gitlab-runner` رو ببینی |

---

## 🛠️ نصب در Kubernetes (خلاصه)

1. نصب **Loki**:

   ```bash
   helm repo add grafana https://grafana.github.io/helm-charts
   helm upgrade --install loki grafana/loki-stack --namespace logging --create-namespace
   ```

2. نصب **Promtail** (جمع‌کننده لاگ‌ها):

   ```bash
   helm upgrade --install promtail grafana/promtail --namespace logging
   ```

3. نصب **Grafana**:

   ```bash
   helm upgrade --install grafana grafana/grafana --namespace logging
   ```

4. اتصال به Grafana و اضافه کردن Loki به عنوان **Data Source**.

---

## 🧪 جستجو توی لاگ‌ها (مثال)

توی Grafana، وقتی Loki رو Data Source کردی، می‌تونی با queryهای مثل زیر لاگ بگیری:

```logql
{namespace="cicd", pod=~"gitlab.*"} |= "error"
```

* لاگ‌هایی که از namespace=`cicd` و پادهایی که با `gitlab` شروع می‌شن میان
* فقط لاگ‌هایی که شامل "error" هستن

---

## 🎯 جمع‌بندی سریع

| ابزار        | نقش                              |
| ------------ | -------------------------------- |
| **Loki**     | جمع‌آوری و ذخیره لاگ‌ها          |
| **Promtail** | جمع‌کننده لاگ از پادها           |
| **Grafana**  | جستجو، داشبورد، هشدار (Alerting) |

---

ا
