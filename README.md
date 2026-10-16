# Jejak — Demo Pelacak Aktivitas dengan Peta Gratis

Demo aplikasi pelacak aktivitas olahraga ala **Strava**, dibangun sebagai **satu file `index.html`**
dan memakai **stack peta 100% gratis tanpa API key**. Tidak ada build step, tidak ada server.

🔗 **Live demo:** https://albaehaki.github.io/geo-tracker-demo/

---

## Kenapa demo ini dibuat

Untuk teman yang mau bikin aplikasi bertipe Strava (rekam lari/gowes, lihat rute, statistik),
tapi belum mau bayar Google Maps API. Demo ini membuktikan bahwa versi gratisnya sudah
lebih dari cukup untuk MVP.

## Stack gratis yang dipakai

| Kebutuhan | Yang dipakai di demo | API key? | Catatan |
|---|---|---|---|
| Render peta (vektor) | **MapLibre GL JS** 4.x + tile **OpenFreeMap** | ❌ tidak | MapLibre = fork open-source Mapbox GL JS. OpenFreeMap benar-benar gratis tanpa batas & tanpa registrasi |
| Geometri rute contoh | **OSRM** (`router.project-osrm.org`) | ❌ tidak | 12,9 km rute asli Bandung (Dago → Punclut → Ciburial), dipakai sekali saat pembuatan demo lalu di-*bake* |
| Profil elevasi | **Open-Meteo Elevation API** | ❌ tidak | Gratis, 10.000 request/hari |
| Simpan aktivitas | `localStorage` browser | ❌ tidak | Untuk produksi ganti ke Supabase/Firebase free tier |
| Peta cadangan | raster **OpenStreetMap** | ❌ tidak | Otomatis dipakai kalau tile vektor gagal dimuat |

## Alternatif lain (kalau butuh)

| Layanan | Free tier | Catatan |
|---|---|---|
| **Mapbox GL JS v3** | 50.000 map loads / bulan | Butuh API key + kartu kredit |
| **MapTiler** | 100.000 map loads / bulan | Butuh API key, tile-nya paling rapi |
| **TomTom Maps** | 2.500 request / hari | Butuh API key |
| **Leaflet** + tile OSM | gratis | Paling ringan (raster), cocok kalau tidak butuh 3D/pitch |
| **OpenRouteService** | 2.000 request / hari | Routing + isochrone, butuh key gratis |
| **GraphHopper** | 500 request / hari | Routing |
| **Nominatim / Photon** | gratis | Geocoding (nama tempat → koordinat) |
| **Protomaps** | gratis (self-host .pmtiles) | Peta vektor satu file, cocok untuk offline |

## Fitur demo

- 🗺️ Peta vektor gelap dengan pitch 3D, kontrol zoom, skala, dan readout koordinat.
- 🚴 / 🏃 **Dua mode**: gowes (km/j) dan lari (pace min/km).
- 🎬 **Putar contoh rute** — animasi rute Bandung 12,9 km, garis diwarnai sesuai kecepatan,
  statistik dihitung live saat animasi jalan. Tidak perlu izin GPS.
- 📡 **Mulai rekam (GPS asli)** — memakai `navigator.geolocation.watchPosition`,
  dengan filter noise berbasis `accuracy` supaya garis tidak "kacang-kacangan".
- ⛰️ **Profil elevasi** — kanvas 2D, diambil dari Open-Meteo saat aktivitas disimpan.
- 📊 **Split per kilometer** — pace/kecepatan tiap km dengan bar.
- 💾 **Aktivitas tersimpan** di `localStorage`, klik untuk memuat ulang di peta.

## Catatan teknis

- `loadSample()` mensimulasikan waktu tempuh dari **gradien elevasi asli**: makin menanjak,
  makin lambat. Jadi warna garis dan split per km terlihat masuk akal, bukan acak.
- Layer peta disiapkan di event `style.load` (bukan `load`) supaya tetap jalan walau tile
  basemap lambat — `load` baru menyala setelah semua source selesai.
- Kalau 9 detik tile vektor belum siap, otomatis pindah ke style raster OpenStreetMap.

## Cara pakai

```bash
# cukup buka langsung, atau
python -m http.server 8099
# lalu buka http://127.0.0.1:8099
```

Rekam GPS butuh HTTPS (GitHub Pages sudah HTTPS) atau `localhost`.

## Kredit

Peta & data: © [OpenStreetMap contributors](https://www.openstreetmap.org/copyright) ·
Tile: [OpenFreeMap](https://openfreemap.org) ·
Routing: [OSRM](http://project-osrm.org) ·
Elevasi: [Open-Meteo](https://open-meteo.com) ·
Library: [MapLibre GL JS](https://maplibre.org)
