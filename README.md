"# Laravel Production Deployment Guide

A curated collection of **Laravel production deployment tips, configurations, and real-world examples** for running Laravel securely and efficiently with **Nginx, PHP-FPM, reverse proxies, SSL/TLS, and Linux servers**.

This repository is intended for **backend engineers, DevOps engineers, and Laravel developers** who want battle-tested deployment practices in one place.

---

## 📌 Repository Purpose

This repo documents:

* Secure Nginx configurations for Laravel
* PHP-FPM production tuning
* Reverse proxy setups (HAProxy / Nginx / Load Balancers)
* SSL and TLS best practices
* Linux hardening tips
* Performance optimization techniques
* Zero-downtime deployment workflows
* Common pitfalls and troubleshooting guides

Think of this as a **Laravel production survival handbook**.

---

## 📂 Repository Structure

```
laravel-nginx-deployment/
│
├── secure-phpfpm-nginx-reverse-proxy.conf   # Hardened Nginx + PHP-FPM config
├── README.md                               # Documentation & deployment guide
└── docs/                                   # (Optional) Additional guides & scripts
```

---

## 🚀 Quick Start: Laravel Production Stack

Typical production stack:

* **Ubuntu / Debian / CentOS / RHEL**
* **Nginx** (Web server & reverse proxy)
* **PHP-FPM 8.x**
* **MySQL / PostgreSQL / Redis**
* **Supervisor / Queue workers**
* **Let's Encrypt or Enterprise SSL**
* **Git / CI/CD pipelines**

---

## 🔐 Secure Nginx + PHP-FPM Reverse Proxy Config

The file below contains a hardened configuration optimized for:

* Reverse proxy environments
* Cloud load balancers
* HTTP/2 & HTTPS
* Security headers
* Laravel-friendly routing

```
secure-phpfpm-nginx-reverse-proxy.conf
```

### Key Features

* X-Forwarded-For & Real IP handling
* FastCGI optimizations
* Static file caching
* Gzip compression
* Security headers (CSP, HSTS, XSS protection)
* Deny access to `.env`, `.git`, storage, vendor
* PHP-FPM socket security

---

## ⚙️ PHP-FPM Production Tuning

Recommended settings for `/etc/php/8.x/fpm/pool.d/www.conf`:

```ini
pm = dynamic
pm.max_children = 20
pm.start_servers = 5
pm.min_spare_servers = 5
pm.max_spare_servers = 10
pm.max_requests = 500
```

### Enable OPcache

```ini
opcache.enable=1
opcache.memory_consumption=256
opcache.max_accelerated_files=20000
opcache.validate_timestamps=0
```

---

## 🔒 Laravel Production Environment Settings

### `.env` Hardening

```env
APP_ENV=production
APP_DEBUG=false
APP_URL=https://yourdomain.com

LOG_CHANNEL=stack
LOG_LEVEL=warning

SESSION_SECURE_COOKIE=true
SESSION_HTTP_ONLY=true
SESSION_SAME_SITE=strict
```

---

## 🔐 SSL & TLS Best Practices

### Use Strong TLS

```nginx
ssl_protocols TLSv1.2 TLSv1.3;
ssl_ciphers HIGH:!aNULL:!MD5;
ssl_prefer_server_ciphers on;
```

### Enable HSTS

```nginx
add_header Strict-Transport-Security "max-age=63072000; includeSubDomains; preload" always;
```

---

## ⚡ Performance Optimization Tips

### Cache Everything

```bash
php artisan config:cache
php artisan route:cache
php artisan view:cache
```

### Queue Workers

Use Supervisor or systemd:

```bash
php artisan queue:work --daemon
```

### Redis & Horizon (Recommended)

* Use Redis for cache, queues, sessions
* Use Laravel Horizon for monitoring

---

## 🛡️ Security Hardening Checklist

* Disable directory listing
* Block `.env`, `.git`, `.htaccess`
* Use fail2ban for SSH and Nginx
* Disable PHP dangerous functions
* Run PHP-FPM under non-root user
* Enable firewall (UFW / firewalld)
* Use SSH keys only (disable password login)

---

## 🔄 Zero Downtime Deployment Strategy

### Recommended Workflow

1. CI builds artifacts
2. Upload to server
3. Use symlink-based releases
4. Run migrations
5. Reload PHP-FPM and Nginx

Tools:

* Envoyer
* Deployer
* GitHub Actions
* GitLab CI/CD

---

## 🧪 Health Checks & Monitoring

* Nginx status module
* PHP-FPM status page
* Laravel Telescope (non-production or restricted)
* Prometheus + Grafana
* Sentry / Bugsnag for errors

---

## 🧩 Common Production Issues & Fixes

### ❌ 502 Bad Gateway

* PHP-FPM not running
* Wrong socket path
* Permission issues on storage/cache

### ❌ SSL Untrusted Certificate

* Missing intermediate cert
* Wrong certificate chain
* Load balancer SSL termination misconfig

### ❌ High CPU / Memory Usage

* OPcache disabled
* Queue workers misconfigured
* N+1 queries
* Missing indexes

---

## 📚 Additional Resources

* Laravel Official Deployment Docs
* Nginx Hardening Guide
* PHP-FPM Tuning Guide
* OWASP Web Security Cheat Sheet
---

## 📜 License

MIT License — Use freely in production systems.

---

## ⭐ Author

**zuqongtech**
Laravel Backend Engineer | DevOps Enthusiast

---

If this repo helps you, give it a ⭐ and share with the Laravel community.
