# Aplikasi Anotasi — cara menghosting

`index.html` adalah **satu berkas mandiri**: tidak ada backend, tidak ada
basis data, tidak ada CDN. Seluruh pekerjaan berlangsung di peramban anotator
dan tidak dikirim ke mana pun.

Karena itu ia bisa dijalankan dari mana saja. Pilih satu cara di bawah.

---

## Pilihan 1 — kirim berkasnya langsung (paling cepat, tanpa hosting)

Kirim `index.html` ke anotator lewat WhatsApp/email/Drive. Anotator klik dua
kali, peramban terbuka, selesai.

**Satu catatan:** saat dibuka lewat `file://`, sebagian peramban memblokir
penyimpanan lokal. Aplikasi tetap berjalan penuh — hanya penyimpanan otomatis
yang mati, jadi anotator harus menekan **Unduh Hasil** sebelum menutup tab.
Kalau anotasinya panjang, pakai pilihan 2 atau 3.

## Pilihan 2 — Cloudflare Pages atau Netlify (gratis, ~2 menit)

1. Buat folder berisi `index.html` saja.
2. Buka <https://pages.cloudflare.com> (atau <https://app.netlify.com/drop>).
3. Seret foldernya ke halaman unggah.
4. Salin URL yang diberikan, kirim ke anotator.

Tidak perlu server, tidak perlu konfigurasi. URL-nya permanen selama akunnya
ada.

## Pilihan 3 — GitHub Pages

Cocok bila ingin URL yang bisa dikutip di naskah.

```bash
mkdir anotasi-web && cd anotasi-web
cp /path/ke/public/anotasi/index.html .
git init && git add . && git commit -m "aplikasi anotasi"
git branch -M main
git remote add origin https://github.com/<akun>/anotasi-web.git
git push -u origin main
```

Lalu di GitHub: **Settings → Pages → Source: main / (root) → Save**.
URL-nya muncul dalam satu-dua menit: `https://<akun>.github.io/anotasi-web/`.

## Pilihan 4 — dari aplikasi Laravel ini

Sudah dilayani di `/anotasi` begitu aplikasi berjalan. Praktis saat anotator
berada di jaringan yang sama.

**Jangan mengekspos aplikasi Laravel ini ke internet hanya untuk itu.** Di
dalamnya ada panel admin, data analisis, dan data latih. Untuk anotator di luar
jaringan, pakai pilihan 1–3 yang hanya membagikan satu berkas statis.

---

## Yang perlu disiapkan sebelum anotator mulai

1. Golden dataset sudah **dibuat** lewat menu *Ambil Sampel dari Korpus*.
   Tidak perlu dibekukan dulu — pembekuan justru dilakukan **setelah** anotasi
   masuk, karena membekukan berarti mengunci isi dan label.
2. Berkas anotasi sudah diunduh dari halaman dataset:
   - **Unduh Berkas Anotasi** — tanpa prediksi model.
   - **Unduh + Prediksi Model** — dengan prediksi, tetapi aplikasi tetap
     menyembunyikannya sampai anotator memilih labelnya sendiri.
3. Anotator sudah membaca **Pedoman** di dalam aplikasi (tombol kanan atas).
   Versi lengkapnya di `docs/PEDOMAN_ANOTASI.md`.

## Setelah anotasi selesai

Anotator menekan **Unduh Hasil**, mengirimkan berkas `…-terisi.csv` kembali.
Unggah lewat halaman dataset → **Unggah Anotasi**. Pencocokan memakai kolom
`id`, jadi urutan baris boleh berubah dan teks yang tersunting tetap dikenali.

Baris bertanda "tidak jelas" pulang tanpa label. Buang lewat tombol
**Buang N baris tanpa label**, lalu **Bekukan Dataset**.
