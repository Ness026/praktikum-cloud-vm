# Laporan Praktikum: Deployment VM & Penyimpanan Cloud

* **Nama:** Dodi Rohaedi
* **NIM:** 240131010
* **Mata Kuliah:** Komputasi Awan (Cloud Computing)
* **Lingkungan Praktikum:** Simulasi Infrastruktur Virtual (Oracle VirtualBox – KDE neon berbasis Ubuntu)

---

## 1. Pendaftaran & Persiapan Lingkungan VM
Praktikum dijalankan menggunakan lingkungan virtualisasi lokal **Oracle VirtualBox** sebagai simulasi infrastruktur IaaS (*Infrastructure as a Service*) karena keterbatasan metode verifikasi kartu pembayaran internasional pada pendaftaran akun *Free Tier* penyedia *cloud* publik. Seluruh tahapan inti mulai dari penyediaan VM Linux, penambahan *Block Storage*, konfigurasi *firewall* Port 22, hingga pengujian *Remote SSH* diterapkan secara penuh.

## 2. Penyediaan Instans VM
Instans mesin virtual (*Virtual Machine*) berbasis Linux dikonfigurasi dan diluncurkan dengan spesifikasi berikut:

| Parameter | Spesifikasi |
| :--- | :--- |
| **Nama Instans** | `KDE neon` |
| **Sistem Operasi** | KDE neon User Edition (berbasis Ubuntu 64-bit, Linux 7.0.0-30-generic x86_64) |
| **Alokasi Memori (RAM)** | `4096 MB` (4 GB) |
| **Prosesor (vCPU)** | `2 Processors` |
| **Penyimpanan Utama** | `30,00 GB` (`KDE neon.vdi` / `/dev/sda`) |
| **Jaringan & IP Instans** | NAT (`Intel PRO/1000 MT Desktop`), IP `10.0.2.15`, *Port Forwarding* Host `2222` → Guest `22` |
| **Status Instans** | `Running` |

## 3. Konfigurasi Penyimpanan Tambahan (Block Storage)
Volume penyimpanan tambahan (*Virtual Disk / Block Storage*) sebesar **8,00 GB** (`KDE neon_1.vdi`, format VDI) dibuat dan dipasang (*attach*) pada *SATA Port 1* di VirtualBox. Setelah terdeteksi sebagai `/dev/sdb` di dalam sistem operasi Linux, disk diformat ke sistem file `ext4` dan di-*mount* ke direktori `/mnt/storage-tambahan`:

```bash
# 1. Memeriksa daftar disk yang terpasang (disk tambahan 8G terdeteksi sebagai /dev/sdb)
lsblk

# 2. Memformat disk tambahan ke sistem file ext4
sudo mkfs -t ext4 /dev/sdb

# 3. Membuat direktori mount point
sudo mkdir -p /mnt/storage-tambahan

# 4. Memasang (mount) disk tambahan ke direktori tujuan
sudo mount /dev/sdb /mnt/storage-tambahan

# 5. Memverifikasi penyimpanan tambahan berhasil terpasang
df -h
