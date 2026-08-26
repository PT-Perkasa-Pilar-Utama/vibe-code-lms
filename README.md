# Vibe Coding LMS Presentation

Presentasi HTML interaktif **“Vibe Coding untuk Membuat LMS Sederhana”** beserta dokumen produk, engineering guard, workflow asset, dan media cover.

Deck ini dibuat dalam format 16:9 dengan viewer bergaya PowerPoint, mode present, navigasi slide, serta export ke PDF dan PPTX. Seluruh source utama sengaja dibuat mudah dibuka dan direview langsung dari browser.

**Status:** presentation artifact / MVP reference<br>
**Bahasa:** Indonesia<br>
**Canvas:** 1280 × 720 (16:9)

## Isi repository

```text
.
├── index.html
├── vibe-coding-lms-presentation.pdf
├── vibe-coding-lms-presentation.pptx
├── PRD.md
├── AGENT_BOOTSTRAP.md
├── ENGINEERING_GUARD.md
├── ANIMATED_LANDING_ASSET_GUIDE.md
├── COVER_SLIDE_RIGHT_VISUAL.mp4
├── COVER_SLIDE_EXPORT_MEDIA.js
├── COVER_SLIDE_GOOGLE_FLOW_PROMPT.md
├── COVER_SLIDE_IMAGE_PROMPT.md
├── COVER_SLIDE_VIDEO_FROM_IMAGE_PROMPT.md
├── .gitignore
└── README.md
```

## Ringkasan deck

Presentasi membahas cara mengubah ide LMS menjadi MVP yang usable melalui vibe coding, dengan fokus pada:

- alur learner dari landing page sampai lesson,
- state progress, completion, resume, dan revisit,
- prinsip visual dan sistem layout yang konsisten,
- guardrail engineering sebelum masuk fase production,
- validasi behavior melalui browser dan export artifact.

## Hasil render

Artifact siap pakai tersedia langsung di repository:

- [PDF presentation](./vibe-coding-lms-presentation.pdf) — 28 halaman, format 16:9.
- [PPTX presentation](./vibe-coding-lms-presentation.pptx) — deck PowerPoint dengan media cover.

## File utama

### `index.html`

Source utama deck presentasi.

Isinya mencakup:

- seluruh slide,
- styling dan layout 1280×720,
- thumbnail navigation,
- keyboard navigation,
- fullscreen presentation mode,
- cover video,
- export PDF,
- export PPTX,
- logic untuk menjaga fidelity hasil export.

Deck dapat dibuka langsung melalui `file://` atau disajikan melalui static HTTP server.

### `PRD.md`

Product Requirements Document untuk LMS yang dibahas di presentasi.

Dokumen ini juga memuat requirement kualitas implementasi, agent execution rules, serta mandatory Playwright E2E gate. Landing page memiliki dedicated motion E2E gate untuk scroll choreography, pinned/sticky scenes, responsive motion, media, WebGL/canvas, reverse scroll, dan intermediate animation states.

### `AGENT_BOOTSTRAP.md`

Panduan untuk menyiapkan agent/project context sebelum implementasi lebih jauh.

Fokusnya adalah bootstrap context dan workflow agent, bukan menjadi source presentasi.

### `ENGINEERING_GUARD.md`

Bootstrap contract untuk engineering phase setelah prototype/design selesai.

Dokumen ini mengatur antara lain:

- mandatory Tech Stack Contract,
- hard stop jika stack belum lengkap,
- repository inspection,
- migration dari prototype ke tech stack sebenarnya,
- architecture boundaries,
- local pre-commit guard,
- strict lint/type/build/test/security policy,
- DRY / SOLID / KISS / YAGNI / Clean Architecture,
- `AGENTS.md` dan `.agents/` planning,
- hard stop sebelum feature implementation.

Fase ini hanya menyiapkan **project bootstrap + architecture + plan + guard**. Eksekusi implementation plan dilakukan pada fase berikutnya.

### `ANIMATED_LANDING_ASSET_GUIDE.md`

Workflow untuk implementasi animated/cinematic landing page.

Aturan pentingnya:

- baca `AGENTS.md` dan pahami architecture/stack existing terlebih dahulu,
- user wajib memberikan **full landing-page reference image**,
- jika reference belum tersedia atau tidak lengkap, agent harus stop,
- visual tidak boleh dikarang dari template lain,
- resource gathering difokuskan pada cara mengimplementasikan reference ke stack existing,
- landing page dibangun lebih dulu menggunakan dummy image/video pada final media slot,
- image/video prompt harus mengikuti visual reference dari user,
- video memakai image-first workflow,
- final media idealnya hanya perlu mengganti file asset tanpa redesign layout.

## Cover media

### `COVER_SLIDE_RIGHT_VISUAL.mp4`

Video animasi yang digunakan pada visual kanan slide cover / slide 1.

### `COVER_SLIDE_EXPORT_MEDIA.js`

Helper export-only yang membawa poster frame dan byte MP4 dalam format yang dapat dibaca exporter ketika deck dibuka melalui `file://`.

File ini diperlukan karena browser dapat memutar local MP4 melalui `<video>`, tetapi JavaScript pada halaman `file://` tidak dapat melakukan `fetch()` terhadap sibling MP4 secara normal.

Behavior export:

- **PDF:** cover video dirender sebagai static poster/frame.
- **PPTX:** MP4 asli di-embed sebagai PowerPoint media object dan poster tetap digunakan sebagai preview frame.

Jika `COVER_SLIDE_RIGHT_VISUAL.mp4` diganti dengan video baru, helper ini harus diregenerasi supaya embedded media pada PPTX tetap menggunakan video terbaru.

## Prompt cover

### `COVER_SLIDE_GOOGLE_FLOW_PROMPT.md`

Prompt ringkas/workflow untuk pembuatan cover melalui Google Flow.

### `COVER_SLIDE_IMAGE_PROMPT.md`

Prompt untuk menghasilkan still image/reference frame cover.

### `COVER_SLIDE_VIDEO_FROM_IMAGE_PROMPT.md`

Prompt untuk menganimasikan still image cover menjadi video.

## Menjalankan presentasi

Cara paling sederhana:

```bash
xdg-open index.html
```

Atau gunakan static server, misalnya:

```bash
python3 -m http.server 8765 --bind 0.0.0.0
```

Lalu buka:

```text
http://localhost:8765/
```

Query slide dapat digunakan untuk membuka slide tertentu:

```text
?slide=1
?slide=8
```

## Navigasi presentasi

Deck mendukung:

- thumbnail navigation,
- tombol previous/next,
- arrow keys,
- Page Up / Page Down,
- Home / End,
- `F5` untuk present dari awal,
- `Shift + F5` untuk present dari slide aktif,
- fullscreen presentation mode.

## Export

Tombol export tersedia di topbar.

### PDF

Seluruh slide dirender ke high-resolution PNG terlebih dahulu kemudian dimasukkan ke PDF.

Cover video menjadi static frame agar PDF tetap deterministic.

### PPTX

Setiap slide menggunakan high-resolution rendered slide image untuk menjaga visual fidelity.

Khusus slide cover, MP4 asli juga di-embed ke PowerPoint sebagai media object sehingga animasi tetap tersedia pada hasil PPTX.

Export library dimuat secara lazy dari CDN, jadi koneksi internet dibutuhkan ketika library export belum tersedia di browser cache.

## Versioning

Rilis dan perubahan besar ditandai dengan annotated Git tag, misalnya `v0.1.0`. File media cover saat ini berukuran kecil dan disimpan langsung di repository; gunakan Git LFS jika ukuran asset berkembang signifikan.

## Catatan maintenance

- Jangan hapus `COVER_SLIDE_EXPORT_MEDIA.js` selama deck masih harus mendukung PPTX video export dari `file://`.
- Jika cover MP4 berubah, regenerasi export helper.
- Jangan menambah kembali static dummy cover lama; source slide 1 menggunakan video asli.
- Setelah perubahan signifikan ke deck/exporter, lakukan real-browser regression menggunakan Playwright dan validasi file hasil download, bukan hanya event download.
- Untuk PPTX, cek bahwa `.mp4` benar-benar ada di `ppt/media/` dan presentation package tetap valid.
