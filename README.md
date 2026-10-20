# Hanzi Overlay (Mandarin → Pinyin)

Versi Mandarin dari Kanji Overlay. Blok teks Mandarin di aplikasi apa pun, tekan **Ctrl+Alt+H**,
muncul gelembung berisi pinyin per kata, anotasi pinyin di atas tiap hanzi, dan terjemahan Indonesia.

## Menjalankan
    npm install
    npm start

## Pintasan
- Ctrl+Alt+H : baca teks yang diblok
- Ctrl+Alt+J : pilih area gambar lalu OCR
- Ctrl+Alt+W : tampil/sembunyikan widget
- Ctrl+Alt+Q : keluar

## Pengaturan (bagian atas main.js)
- TARGET_LANG : bahasa terjemahan ("id", "en", ...)
- OCR_LANG    : "chi_sim" (sederhana), "chi_tra" (tradisional), atau "chi_sim+chi_tra"

## Perbedaan dari versi Jepang
- kuroshiro + kuromoji (kamus ~17 MB) diganti `pinyin-pro` + `Intl.Segmenter` bawaan Electron.
- Hiragana diganti pinyin dengan tanda nada (银行 → yínháng; bacaan ganda dihitung per kata).
- OCR memakai data `chi_sim` (diunduh otomatis saat pertama kali, perlu internet).
