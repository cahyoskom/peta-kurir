# Peta Kurir - Dokumentasi Teknis

## Spesifikasi Teknis

### Stack
- **HTML5** - Struktur
- **CSS3** - Styling (dengan CSS variables untuk dark mode)
- **Vanilla JavaScript (ES6+)** - Logic
- **Leaflet.js** (CDN) - Peta interaktif
- **OpenStreetMap** (gratis) - Tile peta
- **IndexedDB** - Local database
- **Web Speech API** - Voice input
- **Geolocation API** - GPS
- **Service Worker** - PWA & offline

**Zero dependencies** kecuali Leaflet (dari CDN).

### Ukuran File
- HTML file: ~80KB
- Runtime: minimal (~2MB dengan Leaflet cache)
- Database: tergantung jumlah alamat (1-10KB untuk 100 alamat)

### Browser Support
- Chrome 90+
- Firefox 88+
- Opera 76+
- Samsung Internet 14+
- Edge 90+

## Deployment

### Opsi 1: Local Testing (Paling Cepat)
```bash
# Desktop / Laptop
- Buka file peta-kurir.html di browser Chrome/Firefox
- Atau gunakan live server (VS Code extension)

# Android
- Transfer file ke HP via USB / email / cloud
- Buka dengan browser
- Atau install sebagai PWA
```

### Opsi 2: Web Server
```bash
# Copy ke web server
- Upload peta-kurir.html ke hosting (Vercel, GitHub Pages, dll)
- Akses via URL
- Akan bekerja offline setelah first load (PWA)
```

### Opsi 3: Production PWA
Jika ingin distribute ke kurir:
1. Host di HTTPS domain (wajib untuk PWA)
2. File akan di-cache otomatis oleh service worker
3. User bisa install dari home screen
4. Data tetap lokal di setiap HP

## Struktur Database (IndexedDB)

### Object Store: `addresses`
```javascript
{
  id: "addr_1695619200000",  // auto-generated, unique
  recipient_name: "Pak Budi",
  latitude: -6.2088,
  longitude: 106.8650,
  gang: "Gang 4",
  house_number: "27",
  address: "Jl. Raya No. 10",
  landmark: "Pagar hitam, sebelah warung biru",
  description: "Catatan tambahan",
  payment_status: "cod-transfer",  // belum|cod|cod-transfer|transfer|other
  location_type: "jelas",  // jelas|nitip|sulit|patokan
  problem_tags: ["Sulit dihubungi", "COD bermasalah"],
  raw_voice_note: "Pak Budi Gang 4 nomor 27 ...",  // transcription mentah
  visit_count: 3,
  created_at: "2026-09-25T10:30:00Z",
  updated_at: "2026-09-25T14:30:00Z",
  last_visit_at: "2026-09-25T14:30:00Z",
  history: [
    {
      date: "2026-09-25T10:30:00Z",
      status: "berhasil",
      payment_status: "cod",
      landmark: "Pagar hitam",
      notes: "Pembayaran normal"
    },
    {
      date: "2026-09-26T09:00:00Z",
      status: "berhasil",
      payment_status: "cod-transfer",
      landmark: "Pagar hitam",
      notes: "Minta transfer"
    }
  ]
}
```

### Indexes
- `recipient_name` - untuk search nama
- `created_at` - untuk timeline
- `location_type` - untuk filter
- `payment_status` - untuk filter

## Fitur Detail

### 1. Voice Input
```javascript
- SpeechRecognition API (Chrome, Firefox Android)
- Language: id-ID
- Fallback: manual form jika tidak tersedia
- Simpan: raw transcript untuk audit
```

**Parse Voice:**
- Ekstrak nama, gang, nomor, patokan
- Detect pembayaran (COD, transfer)
- Simple regex-based, tidak ML

### 2. Anti-Duplikat
```javascript
- Threshold: 30 meter (0.0003 lat/lon)
- Cek saat save
- Opsi: gabung atau simpan baru
- Merge: preservasi history, increment visit_count
```

### 3. GPS Tracking & Location Adjustment
```javascript
- watchPosition() - continuous update di map utama
- getCurrentPosition() - single location saat catat
- Location adjustment: marker draggable di mini-map
- User bisa tap untuk set lokasi manual
- Accuracy: ~5-20 meter (tergantung signal)
- Dapat di-adjust lebih presisi jika GPS tidak akurat
```

### 3a. Location Adjustment Flow
```
1. Tap ✓ Catat Selesai
2. GPS auto-ambil lokasi
3. Mini map muncul dengan marker di posisi saat ini
4. User drag marker atau tap untuk adjust lokasi
5. Koordinat update real-time
6. Setelah puas dengan lokasi, isi data alamat
7. Simpan
```

**Keuntungan:**
- Lebih akurat (GPS sering menyimpang 10-20 meter)
- User bisa adjust jika tahu lokasi yang tepat
- Visual feedback sebelum simpan
- Kurir bisa verify lokasi di map sebelum catat

### 4. Service Worker
```javascript
- Cache-first strategy untuk assets
- Network-first untuk tile maps
- Skip cache untuk OpenStreetMap tiles
- Install: auto-cache core files
- Update: auto-cleanup old caches
```

### 5. Export/Import

**JSON Format:**
```json
{
  "version": "1.0",
  "exported_at": "2026-09-25T10:30:00Z",
  "addresses": [...]
}
```

**CSV Format:**
```csv
id,nama,gang,nomor,alamat,latitude,longitude,patokan,status_pembayaran,masalah,tipe_lokasi,visit_count,kunjungan_terakhir,catatan
```

### 6. Search
- Query: recipient_name, gang, house_number, landmark, address
- Case-insensitive
- Substring match
- Real-time (onChange)

### 7. Filter
- Semua
- Berhasil (no problem tags)
- Bermasalah (has problem tags)
- COD (payment status contains cod)
- Nitip (location_type === nitip)
- Sulit (location_type === sulit OR patokan)

### 8. Styling
```css
- CSS Variables untuk theming
- prefers-color-scheme: dark/light
- Responsive: 100% untuk mobile
- Safe area: env(safe-area-inset-*)
```

## Data Persistence

### Offline Capability
- ✓ IndexedDB untuk data (tidak online)
- ✓ Service Worker untuk assets (tidak online)
- ✗ Leaflet tiles (membutuhkan network first time)

### Data Durability
- IndexedDB: Persistent sampai user clear app data
- Backup: JSON export untuk long-term
- Android: Jika HP factory reset, data hilang (kecuali ada backup)

## Performance

### Target
- Load time: < 2 detik (pertama kali)
- Map interaction: < 100ms latency
- Database query: < 50ms

### Optimizations
- Single HTML file (no separate CSS/JS)
- Lazy loading untuk map tiles
- IndexedDB untuk instant search
- Minimal animations
- No external APIs (kecuali OpenStreetMap)

## Testing Checklist

### Functional
- [ ] App terbuka di Android
- [ ] GPS bekerja & map update
- [ ] Catat alamat berhasil
- [ ] Voice input working (jika support)
- [ ] Manual form as fallback
- [ ] Anti-duplikat detection
- [ ] Search & filter
- [ ] History preserved
- [ ] Export/Import
- [ ] Offline access

### Non-functional
- [ ] < 3 detik first load
- [ ] < 500ms untuk search 100+ addresses
- [ ] PWA installable
- [ ] Dark mode readable
- [ ] Safe area respected
- [ ] No console errors

## Maintenance

### Updates
1. Ubah source code
2. Increment version di service worker
3. Rebuild/redeploy
4. User cache akan auto-clear (old version)

### Backup Data
- User bertanggung jawab untuk backup
- Rekomendasi: weekly JSON export
- JSON bisa di-upload ke cloud

### Known Limitations
1. Voice recognition hanya di beberapa browser
2. Offline maps tidak fully cache (tiles memerlukan pre-load)
3. Export history tidak include semua detail (opsional untuk CSV)
4. GPS accuracy tergantung kondisi signal
5. Tidak ada sync antar device

## Extensibility

### Possible Features (Future)
- Cloud sync (Firebase, Supabase) - tambah backend
- Photo capture - tambah Camera API
- Voice reminders - tambah Notification API
- Offline maps - tambah tile pre-caching
- Analytics - tambah tracking
- Multiple users - tambah user table & auth

### Tidak Direkomendasikan
- Backend/database - lepas dari "offline-first"
- Complex UI - bikin lambat di field
- Heavy ML - kompleks & resource intensive

## API References

- Leaflet.js: https://leafletjs.com/
- IndexedDB: https://developer.mozilla.org/en-US/docs/Web/API/IndexedDB_API
- Web Speech API: https://developer.mozilla.org/en-US/docs/Web/API/Web_Speech_API
- Geolocation API: https://developer.mozilla.org/en-US/docs/Web/API/Geolocation_API
- Service Workers: https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API
- PWA: https://web.dev/progressive-web-apps/

## Troubleshooting

### GPS tidak bekerja
- Cek: GPS enabled di HP + browser permission
- Test: Buka Google Maps, lihat apakah GPS bekerja
- Debug: Check browser console (Ctrl+Shift+I)

### Map tidak muncul
- Cek: Internet connection
- Test: Zoom in/out untuk trigger tile load
- Offline: Peta tidak load tanpa cache sebelumnya

### Voice tidak recognize
- Cek: Microphone permission
- Test: Bicara jelas & santai
- Fallback: Gunakan manual form

### Data hilang
- Jika no backup: Tidak bisa recover
- Jika ada backup: Import dari JSON
- Rekomendasi: Daily backup untuk production use

### Performance issue
- Cek: Jumlah alamat (> 1000 might be slow)
- Debug: Open DevTools → Performance tab
- Solution: Archive old addresses ke separate file

---

**Version**: 1.0  
**Last Updated**: 2026-09-25

Untuk soalan teknis atau improvement, modifikasi source code sesuai kebutuhan!
