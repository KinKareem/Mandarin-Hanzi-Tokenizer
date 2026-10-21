# Hanzi Overlay

Aplikasi desktop (Windows) yang membaca teks Mandarin dari layar dan menampilkannya dalam gelembung kecil berisi **pinyin**, **anotasi pinyin di atas tiap hanzi**, dan **terjemahan bahasa Indonesia**.

Ini adalah versi Mandarin dari proyek Kanji Overlay (Jepang). Hiragana diganti pinyin, sisanya bekerja dengan cara yang sama.

---

## Fitur

- **Baca teks yang diblok.** Blok teks Mandarin di aplikasi apa pun (browser, PDF, Word, dan lain-lain), lalu tekan `Ctrl+Alt+H`.
- **Baca teks dari gambar (OCR).** Tekan `Ctrl+Alt+J`, tarik kotak di area layar yang berisi tulisan Mandarin (misalnya gambar, video, atau game), lalu tekan Enter.
- **Pinyin per kata.** Teks dipecah menjadi kata dan tiap kata ditulis dengan pinyin bertanda nada. Huruf berbunyi ganda dibaca sesuai kata, misalnya 银行 menjadi *yínháng*, bukan *yínxíng*.
- **Anotasi pinyin.** Baris kedua menampilkan teks asli dengan pinyin kecil di atas setiap hanzi, seperti furigana.
- **Terjemahan.** Baris ketiga berisi terjemahan ke bahasa Indonesia (bisa diganti).
- **Widget melayang.** Tombol kecil di tepi layar yang bisa digeser ke mana saja, sebagai alternatif pintasan keyboard.
- Mendukung hanzi sederhana dan tradisional untuk pinyin. Untuk OCR, bahasa diatur lewat `OCR_LANG` (lihat bagian Pengaturan).

## Pintasan keyboard

| Pintasan | Fungsi |
|---|---|
| `Ctrl+Alt+H` | Baca teks yang sedang diblok |
| `Ctrl+Alt+J` | Pilih area gambar lalu baca dengan OCR |
| `Ctrl+Alt+W` | Tampilkan atau sembunyikan widget |
| `Ctrl+Alt+Q` | Keluar dari aplikasi |

Aplikasi tidak punya jendela utama. Setelah dibuka, yang tampil hanya widget kecil. Untuk menutup aplikasi, gunakan `Ctrl+Alt+Q`.

## Cara memakai

**Teks yang bisa diblok**
1. Blok teks Mandarin.
2. Tekan `Ctrl+Alt+H` (atau klik tombol **拼 Teks** di widget).
3. Gelembung muncul di dekat teks dan hilang sendiri setelah 12 detik. Klik gelembung untuk menutupnya lebih cepat.

**Teks di gambar**
1. Tekan `Ctrl+Alt+J` (atau klik tombol **▢ Gambar** di widget).
2. Tarik kotak di area tulisan. Kotak bisa digeser dan diubah ukurannya.
3. Tekan `Enter` atau klik dua kali di dalam kotak untuk membaca. `Esc` atau klik kanan untuk membatalkan.

## Cara kerja singkat

1. **Ambil teks.** Aplikasi menyalin teks yang diblok lewat simulasi `Ctrl+C`, membacanya, lalu mengembalikan isi clipboard seperti semula. Untuk gambar, layar difoto lalu area yang dipilih dibaca oleh Tesseract (OCR).
2. **Pecah kata.** Teks dipecah per kata dengan `Intl.Segmenter` bawaan Electron.
3. **Pinyin.** Library `pinyin-pro` mengubah tiap kata menjadi pinyin.
4. **Terjemahan.** Teks dikirim ke endpoint publik Google Translate (tidak resmi, tanpa API key). Kalau gagal atau tidak ada internet, gelembung tetap tampil tanpa terjemahan.
5. **Tampilkan.** Gelembung diletakkan di dekat area yang diblok.

Teks yang diproses dibatasi 300 karakter per sekali baca.

## Struktur file

| File | Fungsi |
|---|---|
| `main.js` | Proses utama: pintasan, baca teks, pinyin, OCR, terjemahan, penempatan jendela |
| `popup.html/css/js` | Tampilan gelembung hasil |
| `widget.html/css/js` | Widget melayang |
| `select.html/css/js` | Layar pemilih area untuk OCR |
| `preload.js`, `widget-preload.js`, `select-preload.js` | Jembatan aman antara jendela dan proses utama |
| `build/icon.ico`, `build/icon.png` | Ikon aplikasi |
| `.github/workflows/build.yml` | Build installer otomatis lewat GitHub Actions |

## Menjalankan dari kode (mode pengembangan)

Butuh Node.js (disarankan versi 20 atau lebih baru).

```
npm install
npm start
```

## Membuat installer (.exe)

```
npm run dist
```

Hasilnya ada di folder `dist/`:
- `Hanzi-Overlay-Setup-0.1.0.exe`: installer dengan pilihan folder, shortcut Desktop, dan shortcut Start Menu.
- `Hanzi-Overlay-Portable-0.1.0.exe`: versi tanpa instal.

Alternatif: unggah proyek ke GitHub, lalu jalankan workflow **Build installer** di tab Actions. Hasilnya bisa diunduh dari Artifacts, atau dari Releases kalau Anda push tag seperti `v0.1.0`.

Saat pertama kali dijalankan, Windows SmartScreen mungkin menampilkan peringatan "publisher tidak dikenal" karena installer belum ditandatangani. Klik **More info**, lalu **Run anyway**.

## Pengaturan

Ubah di bagian atas `main.js`:

| Pengaturan | Nilai | Keterangan |
|---|---|---|
| `TARGET_LANG` | `"id"`, `"en"`, dan lain-lain | Bahasa terjemahan |
| `OCR_LANG` | `"chi_sim"`, `"chi_tra"`, `"chi_sim+chi_tra"` | Bahasa OCR. Gabungan lebih lambat |
| `SHOW_KEY`, `CAPTURE_KEY`, `QUIT_KEY` | contoh `"Control+Alt+H"` | Pintasan keyboard |

## Lokasi data

- **Pengaturan dan data OCR:** `C:\Users\<nama>\AppData\Roaming\hanzi-overlay` (posisi widget dan data bahasa Mandarin untuk OCR, sekitar 20 MB).
- **Data OCR** diunduh otomatis saat pertama kali dipakai, jadi butuh internet satu kali.
- Menghapus folder itu hanya mengatur ulang posisi widget dan mengunduh ulang data OCR.

## Keterbatasan

- Hanya Windows yang diuji untuk membaca teks yang diblok. Di Mac dan Linux, fitur ini kurang lengkap.
- Terjemahan memakai endpoint tidak resmi, jadi bisa berhenti bekerja sewaktu-waktu.
- Akurasi OCR bergantung pada ukuran dan kejelasan tulisan. Teks kecil atau buram sering salah dibaca.
- Pemecahan kata memakai ICU, yang cukup baik tetapi tidak sekuat kamus khusus. Kalau ada kata yang salah dipecah, bisa diganti ke `jieba`.

## Teknologi

[Electron](https://www.electronjs.org/), [pinyin-pro](https://github.com/zh-lx/pinyin-pro), [Tesseract.js](https://github.com/naptha/tesseract.js), [electron-builder](https://www.electron.build/).