# Confluent Tiered Storage — Interactive Documentation

Dokumentasi interaktif untuk implementasi **Confluent Tiered Storage** on-premise dengan MinIO sebagai remote object store. Dibuat sebagai referensi teknis dan media presentasi.

---

## 📄 Halaman

| Halaman | File | Deskripsi |
|---|---|---|
| 🏠 **Home** | [`index.html`](index.html) | Landing page dengan penjelasan konsep utama dan link ke semua resource |
| 📋 **Summary** | [`project_summary.html`](project_summary.html) | Ringkasan arsitektur, problem statement, komponen kunci, dan lifecycle segment |
| ⚡ **Flow** | [`tiered_storage_flow.html`](tiered_storage_flow.html) | Visualisasi interaktif Write / Tiering / Fetch flow dengan animasi packet dan node yang bisa di-drag |
| ❓ **FAQ** | [`tiered_storage_faq.html`](tiered_storage_faq.html) | 24 pertanyaan mendalam: segment files, MinIO structure, failure recovery, RBAC, encryption |

---

## ✨ Fitur

- **Animated Flow Diagram** — Packet animasi yang bergerak mengikuti jalur data secara real-time
- **Draggable Nodes** — Node di flow diagram bisa di-drag & diposisikan ulang; arrows dan paket animasi ikut menyesuaikan secara dinamis
- **Arrow Border Snapping** — Panah terhubung tepat di border node, bukan di tengah
- **Mode Switching** — Toggle antara Write Flow, Tiering Flow, dan Fetch Flow
- **FAQ Search & Filter** — Pencarian full-text dan filter berdasarkan kategori
- **Responsive Design** — Mobile-friendly dengan hamburger navigation
- **Download Presentasi** — Link download file `.pptx` langsung dari halaman Summary

---

## 🏗️ Arsitektur Tiered Storage

```
Producer → Leader Replica → Follower Replicas
                ↓ (upload segment)
           Object Store (MinIO)
                ↑
         Historical Consumer (dedicated thread)
```

**Komponen kunci:**
- **Partition Leader** — Satu-satunya broker yang upload segment ke MinIO
- **`_confluent-tier-state`** — Internal topic yang menyimpan mapping offset ↔ object location
- **MinIO** — On-premise S3-compatible backend untuk cold tier
- **Tier Fetch Thread** — Thread pool terpisah untuk historical consumer (zero contention dengan RT)

---

## ⚙️ Konfigurasi Minimal (`server.properties`)

```properties
confluent.tier.enable=true
confluent.tier.backend=S3
confluent.tier.s3.bucket=kafka-tiered-storage
confluent.tier.s3.region=us-east-1
confluent.tier.s3.endpoint.override=http://minio.internal:9000
confluent.tier.local.hotset.ms=3600000
```

---

## 🚀 Cara Lihat

**Lokal:**
```bash
# Cukup buka langsung di browser
open index.html
```

**GitHub Pages:**  
Aktifkan di `Settings → Pages → Source: main branch` lalu akses:  
`https://<username>.github.io/<repo-name>/`

---

## 📦 File Structure

```
teired_storage/
├── index.html                    # Landing page
├── project_summary.html          # Project overview
├── tiered_storage_flow.html      # Interactive flow diagram
├── tiered_storage_faq.html       # FAQ 24 pertanyaan
└── tiered_storage_confluent.pptx # Presentasi PowerPoint
```

---

## 🔗 Referensi

- [Confluent Tiered Storage Documentation](https://docs.confluent.io/platform/current/kafka/tiered-storage.html)
- [KIP-405: Kafka Tiered Storage](https://cwiki.apache.org/confluence/display/KAFKA/KIP-405%3A+Kafka+Tiered+Storage)
- [MinIO Documentation](https://min.io/docs/minio/linux/index.html)
