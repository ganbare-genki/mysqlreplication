# MySQL High Availability Lab - Configuration Files

MySQL Group Replication + HAProxy + Keepalived ile yüksek erişilebilirlik lab ortamının configuration dosyaları.

## 🏗️ Mimari

```
┌─────────────────┐
│   Uygulama      │ ← 192.168.1.250
└─────────┬───────┘
          │
┌─────────▼───────┐
│  Virtual IP     │ ← Keepalived
│ 192.168.1.250   │
└─────────┬───────┘
          │
    ┌─────┴─────┐
    ▼           ▼
┌─────────┐ ┌─────────┐
│ Router1 │ │ Router2 │ ← HAProxy + MySQL Router
│  .254   │ │  .13    │
└────┬────┘ └────┬────┘
     │           │
     └─────┬─────┘
           │
    ┌──────┼──────┐
    ▼      ▼      ▼
┌─────┐ ┌─────┐ ┌─────┐
│ DB1 │ │ DB2 │ │ DB3 │ ← MySQL Group Replication
│.209 │ │ .70 │ │.180 │
│ P   │ │ S   │ │ S   │
└─────┘ └─────┘ └─────┘
```

## 📁 Dosya Yapısı

```
mysql-ha-configs/
├── mysql-group-replication/
│   ├── primary-my.cnf           # Primary MySQL konfigürasyonu
│   ├── secondary-my.cnf         # Secondary MySQL konfigürasyonu
│   └── bootstrap-commands.sql   # Group Replication başlatma komutları
│
├── mysql-router/
│   ├── router1-mysqlrouter.conf # Router1 konfigürasyonu
│   └── router2-mysqlrouter.conf # Router2 konfigürasyonu
│
└── haproxy-keepalived/
    ├── router1-haproxy.cfg      # Router1 HAProxy konfigürasyonu
    ├── router1-keepalived.conf  # Router1 Keepalived konfigürasyonu (MASTER)
    ├── router2-haproxy.cfg      # Router2 HAProxy konfigürasyonu
    └── router2-keepalived.conf  # Router2 Keepalived konfigürasyonu (BACKUP)
```

## 💻 Sistem Bilgileri

- **OS:** Rocky Linux 9.5
- **MySQL:** 8.0.42
- **Toplam Makine:** 5 adet
- **Failover Süresi:** 6-10 saniye
- **Veri Kaybı:** Sıfır (senkron replikasyon)

## 🔧 IP Adresleri

| Component | IP Address | Role |
|-----------|------------|------|
| Virtual IP | 192.168.1.250 | Application entry point |
| Router1 | 192.168.1.254 | MASTER router |
| Router2 | 192.168.1.13 | BACKUP router |
| MySQL Primary | 192.168.1.209 | Read/Write |
| MySQL Secondary 1 | 192.168.1.70 | Read-Only |
| MySQL Secondary 2 | 192.168.1.180 | Read-Only |

## 📝 Notlar

- Lab ortamı için tasarlanmıştır
- Production kullanımı için ek güvenlik gerekir
- Config dosyalarındaki IP adreslerini kendi ortamınıza göre güncelleyin

---

*Bu config dosyaları, [LinkedIn makalesi](makale-linki) ile ilgili lab çalışmasından alınmıştır.*
