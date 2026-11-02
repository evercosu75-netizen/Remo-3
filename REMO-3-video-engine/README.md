# REMO 3 — Video Explainer Engine

Fondasi produksi video explainer animasi vertikal 9:16 (Reels, Shorts, TikTok) dari kode. Template mencakup tokoh SVG, tipografi editorial, transisi, subtitle sinkron per kata, efek suara sintetis, pemeriksaan frame, dan render MP4 1080×1920 pada 30 fps.

Repository ini adalah mesin produksi yang netral merek. Atur nama channel dan tagline pada adegan penutup di `template/animatic.html` sebelum produksi.

## Struktur

| Folder / file | Fungsi |
|---|---|
| `template/animatic.html` | Adegan, animasi, transisi, subtitle, dan daftar efek suara. `render(t)` menggambar frame pada detik ke-t. |
| `template/karakter.js` | Generator SVG karakter: `host`, `nur`, `yohana`, `togar`, `lina`. |
| `template/words.js` | Timestamp mulai dan selesai tiap kata narasi. |
| `template/naskah-contoh.txt` | Naskah contoh untuk memahami format. |
| `template/frames.py` | Mengambil frame untuk pemeriksaan sebelum render penuh. |
| `template/render.py` | Render video per adegan lalu menggabungkannya dengan narasi. |
| `template/sfx.py` | Membuat efek suara sintetis dan mencampurnya dengan narasi. |
| `skill/produksi-video-explainer/SKILL.md` | Panduan workflow produksi REMO 3. |
| `LICENSE` | Lisensi kode sumber. |

## Kebutuhan

- Python 3.10+
- Node.js 18+
- `ffmpeg`
- Narasi MP3 (rekaman sendiri atau layanan text-to-speech)

## Instalasi

```bash
cd template
npm install
pip install -r requirements.txt
python -m playwright install chromium
```

## Uji template

```bash
cd template
python frames.py 4.5 26.3 36.4
NAME=contoh python render.py
python sfx.py
```

Buka `animatic.html` di browser untuk melihat animatic. Di console browser, jalankan `render(20)` untuk menampilkan frame pada detik ke-20.

## Workflow produksi

1. **Naskah:** buat naskah 570–750 karakter untuk durasi sekitar 45–60 detik. Satu paragraf untuk satu adegan.
2. **Narasi:** simpan audio sebagai `narasi.mp3` di folder `template/`.
3. **Timestamp:** buat `words.js` berisi `window.WORDS=[[mulai,selesai], ...]`, satu pasangan waktu untuk setiap kata.
4. **Animasi:** sesuaikan `animatic.html`. Gunakan indeks kata melalui `ws(i)` dan `we(i)`, `PARA` untuk awal paragraf, `CAPW` untuk subtitle, serta `fx()` untuk efek suara.
5. **Pemeriksaan:** jalankan `python frames.py <daftar detik>` dan periksa hasil di folder `cek/`.
6. **Render:** jalankan `NAME=judul python render.py narasi.mp3`.
7. **Sound design:** jalankan `python sfx.py narasi.mp3`, lalu gabungkan `judul-final.mp4` dan `mix.wav`:

```bash
ffmpeg -i judul-final.mp4 -i mix.wav -map 0:v -map 1:a -c:v copy -c:a aac -b:a 192k -shortest judul-final-sfx.mp4
```

## Pengaturan identitas channel

Di bagian adegan penutup (`s7`) dalam `template/animatic.html`, ganti teks:

- `NAMA CHANNEL`
- `TAGLINE CHANNEL ANDA`

Karakter utama default menggunakan key `host`. Ubah palet warna karakter di objek `LOOKS` dalam `template/karakter.js`. Font yang digunakan: Anton, Plus Jakarta Sans, Baloo 2, dan Caveat.

## Catatan lisensi dan atribusi

Kode sumber mengikuti lisensi MIT. File `LICENSE` dan pemberitahuan hak cipta asal harus tetap disertakan pada salinan dan distribusi kode. Font memiliki lisensi SIL Open Font License masing-masing. Aset narasi/audio tidak disertakan.

Contoh naskah dan data di repository ini hanya demonstrasi format. Verifikasi ulang seluruh fakta, angka, dan sumber sebelum menerbitkan video.
