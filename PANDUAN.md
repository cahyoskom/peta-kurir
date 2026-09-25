# Peta Kurir - Panduan Penggunaan

## Mulai Cepat

1. **Buka file `peta-kurir.html` di browser Android** (Chrome, Firefox, atau browser lainnya)
2. **Install sebagai aplikasi** (tekan menu → "Install" atau "Add to Home Screen")
3. **Berikan izin lokasi GPS** saat diminta

## Fitur Utama

### 🗺️ Peta
- Tampilkan lokasi saat ini (titik biru)
- Tampilkan semua alamat tersimpan (pin)
- Klik pin untuk lihat detail

### ✓ Catat Selesai
1. Tekan tombol **✓** besar (FAB)
2. **Aplikasi akan menampilkan peta kecil dengan posisi GPS saat ini**
   - Pin marker bisa di-drag untuk sesuaikan lokasi
   - Atau tap di peta untuk set lokasi baru
   - Koordinat akan update otomatis
3. Setelah sesuaikan lokasi, pilih **Mulai Bicara** untuk voice input
   - Baca nama, gang, nomor, patokan, pembayaran
   - Contoh: "Pak Budi, Gang 4 nomor 27, pagar hitam, sebelah warung biru, COD minta transfer"
4. Atau **isi manual** jika voice tidak bekerja
5. Review data
6. Tekan **Simpan**

### Anti Duplikat
- Jika lokasi baru dekat dengan alamat lama (~30m), aplikasi akan menanyakan:
  - **Gabungkan**: Tambah kunjungan baru ke record lama
  - **Simpan Baru**: Buat record terpisah

### 📋 Daftar Alamat
- Lihat semua alamat dalam bentuk list
- **Cari** nama, gang, nomor, patokan
- **Filter**: Semua, Berhasil, Bermasalah, COD, Nitip, Sulit
- Klik item untuk lihat detail

### Detail Alamat
Setiap alamat menampilkan:
- Gang & nomor rumah
- Alamat lengkap
- Patokan/landmark
- Status pembayaran
- Tipe lokasi
- Masalah yang pernah terjadi
- **Riwayat kunjungan lengkap** (tidak dihapus)
- Statistik (jumlah kunjungan, terakhir dikunjungi)

**Tombol aksi:**
- ✏️ **Edit** - Ubah data
- 🧭 **Navigasi** - Buka di Google Maps / Aplikasi peta
- 📍 **Catat Kunjungan** - Tambah kunjungan baru
- 🗑️ **Hapus** - Hapus alamat

### 💾 Backup
Sangat penting untuk melindungi data!

**Export JSON**
- Download seluruh database sebagai `.json`
- Simpan di cloud atau tempat aman
- Gunakan untuk restore ke perangkat baru

**Export CSV**
- Download sebagai `.csv` untuk Excel/Google Sheets
- Kolom: id, nama, gang, nomor, alamat, GPS, patokan, dll

**Import JSON**
- Upload backup `.json`
- Pilih: **Gabungkan** (default) atau **Ganti Semua**
- Gabungkan = cek duplikat & merge data

**Import CSV**
- Upload file `.csv`
- Otomatis kenali kolom
- Jalankan anti-duplikat

**Hapus Data Demo**
- Hapus 3 data contoh (Pak Budi, Bu Siti, Pak Ahmad)

**Hapus Semua Data**
- ⚠️ Tidak bisa diundo!
- Yakinkan sudah backup sebelumnya

### ⚙️ Pengaturan
- Info aplikasi
- Fitur & teknologi
- Privasi

## Teknologi

- **Peta**: Leaflet.js + OpenStreetMap (gratis, offline-friendly)
- **Penyimpanan**: IndexedDB (lokal di HP, tidak ada server)
- **Voice**: Web Speech API (jika browser support)
- **Offline**: Service Worker (PWA)
- **GPS**: Geolocation API

## Data Anda

✓ **Sepenuhnya lokal** - Disimpan di perangkat, tidak pernah dikirim ke server  
✓ **Tidak ada login** - Tidak perlu akun  
✓ **Tidak ada biaya** - Semua fitur gratis  
✓ **Harus backup** - Jika HP reset/rusak, data bisa hilang

**Backup secara berkala!** Gunakan fitur Export JSON.

## Struktur Data Setiap Alamat

```
- ID (otomatis)
- Nama penerima
- Latitude & Longitude (GPS)
- Gang
- Nomor rumah
- Alamat lengkap
- Patokan/Landmark
- Status pembayaran (COD, transfer, dll)
- Tipe lokasi (jelas, nitip, sulit, patokan)
- Masalah pengantaran (checkbox: sulit hubungi, tidak ada, dll)
- Catatan tambahan
- Raw voice note (catatan voice asli)
- Visit count (jumlah kunjungan)
- Last visit (kunjungan terakhir)
- History (semua kunjungan, tidak dihapus)
- Created at & Updated at
```

## Masalah Pengantaran (Tag)

Pilih jika ada masalah:
- Sulit dihubungi
- Tidak ada di rumah
- Alamat tidak jelas
- Alamat nitip
- Sulit ditemukan
- Jalan buntu
- Sering FTD
- Sering retur
- COD bermasalah
- Lainnya

Semua tag bisa diedit & tidak dianggap permanen.

## Status Pembayaran

- Belum diketahui
- COD normal
- COD minta transfer
- Transfer
- Minta metode lain

## Offline

Aplikasi tetap bisa digunakan tanpa internet:
- ✓ Buka map (jika sudah pernah load)
- ✓ Lihat daftar alamat
- ✓ Search & filter
- ✓ Catat lokasi baru
- ✓ Export backup
- ✓ GPS tetap jalan

Peta mungkin tidak fully render offline, tapi semua fungsi data tetap kerja.

## PWA (Installable)

Aplikasi bisa di-install sebagai aplikasi standalone:
1. Buka di browser
2. Tekan menu (3 titik) → "Install app" / "Add to Home Screen"
3. Aplikasi akan muncul di home screen seperti aplikasi biasa
4. Bisa digunakan dalam mode fullscreen

## Tips Penggunaan

1. **Selalu backup**: Export JSON seminggu sekali
2. **Voice input**: Bicara dengan jelas dan santai
3. **Patokan penting**: Catat detail (warna pagar, tanda jalan, dll)
4. **Kunjungan ulang**: Gunakan "Catat Kunjungan" untuk track history
5. **Masalah**: Tandai masalah agar tidak terlupakan kunjungan berikutnya
6. **Map**: Peta bisa zoom/pan, klik pin untuk detail

## Browser Support

Direkomendasikan:
- ✓ Chrome Android
- ✓ Firefox Android
- ✓ Opera Android
- ✓ Samsung Internet

## File

- `peta-kurir.html` - Aplikasi lengkap (single file, 80KB)
- Tidak perlu file lainnya
- Cukup buka file ini di browser

## Troubleshooting

**GPS tidak bekerja**
- Izinkan lokasi di pengaturan browser
- Keluar ruangan / buka jendela
- Coba ulang

**Voice tidak bekerja**
- Gunakan manual input (tersedia fallback)
- Beberapa browser tidak support Web Speech API

**Data hilang**
- Backup & restore dari file `.json` yang sudah disimpan
- Jika tidak ada backup, data tidak bisa dikembalikan

**Map tidak muncul**
- Tunggu loading (internet diperlukan pertama kali)
- Zoom in/out untuk load tiles
- Offline: peta tidak fully load, tapi data tetap ada

---

**Versi**: 1.0  
**Dibuat**: 2026  
**License**: Free untuk penggunaan pribadi

Selamat gunakan Peta Kurir! 🗺️
