
# 🗺️ Service Map: [نام Feature]

> **نسخه:** 1.0.0  
> **تاریخ استخراج:** 2026-06-16  
> **محیط:** Stage  
> **وضعیت:** Draft | Reviewed | Approved  

---

## 📌 اطلاعات کلی

| فیلد | مقدار |
|------|-------|
| **Feature Name** | buy-now-pay-later |
| **Feature Flag** | BNPL_V2 |
| **traceId مرجع** | `abc123-def456-ghi789` |
| **تاریخ اجرای فلو** | 2026-06-16 |
| **محیط تست** | UAT / Stage |
| **نسخه PRD** | [لینک PRD](./PRD.md) |
| **User Story** | [لینک Story](./user-story.md) |

---

## 🔄 فلو اجرا شده

Service A --> Service B--> Service C 

---

## ✅ Feature-Aware Services

> این سرویس‌ها **منطق خاص feature** را دارند و **باید تست عمیق** شوند

### 1. `serviceA`

| فیلد | مقدار |
|------|-------|
| **نقش** | مدیریت اصلی فلو خرید |
| **Feature Flag بررسی می‌کند** | ✅ بله - `BNPL_V2` |
| **Endpoints درگیر** | `POST /a/b` |
| | `POST /c/d` |

| **Actions** | `INIT_PURCHASE`, `PAY`,  `DELIVER` |
| **صدا می‌زند * | service B,  service D |
| **صدا می‌شود از** | service F |
| **میانگین زمان پاسخ** | 145ms |

#### 🔴 تست‌های مورد نیاز:
- [ ] Unit Test برای منطق BNPL در init
- [ ] Integration Test با service F
- [ ] E2E کامل فلو init→pay→deliver
- [ ] تست وقتی feature flag غیرفعال است
- [ ] تست با creditId نامعتبر

---

### 2. `coupon-service`

| فیلد | مقدار |
|------|-------|
| **نقش** | اعمال تخفیف BNPL |
| **Feature Flag بررسی می‌کند** | ✅ بله - `BNPL_COUPON` |
| **Endpoints درگیر** | `POST /e/f` |
| **Actions** | `APPLY_COUPON` |
| **صدا می‌زند** | service G |
| **صدا می‌شود از** | Service L |
| **میانگین زمان پاسخ** | 89ms |

#### 🔴 تست‌های مورد نیاز:
- [ ] تست اعمال کوپن BNPL
- [ ] تست کوپن منقضی شده
- [ ] تست تداخل با کوپن‌های دیگر

---

## ⚪ Feature-Agnostic Services

> این سرویس‌ها **فقط داده پاس می‌دهند** و **تست سطحی** کافی است

### 1. `service U`

| فیلد | مقدار |
|------|-------|
| **نقش** | احراز هویت و Authorization |
| **Feature-specific منطق** | ❌ ندارد |
| **Endpoints درگیر** | `POST /a/b` |
| **Actions** | `AUTHENTICATE` |
| **صدا می‌شود از** | service A |

#### 🟡 تست‌های مورد نیاز:
- [ ] Smoke Test - توکن معتبر صادر می‌شود
- [ ] تست توکن منقضی شده


## 📊 خلاصه تست‌ها

| سرویس | نوع | تعداد تست | اولویت |
|-------|-----|-----------|--------|
| service  A | Feature-Aware 🔴 | 5 | High |
| service  B | Feature-Aware 🔴 | 3 | High |
| service  U | Agnostic ⚪ | 2 | Medium |
| service  F | Agnostic ⚪ | 2 | Low |
| service  G | Agnostic ⚪ | 2 | Low |
