# DESIGN.md: EG Learning Centre

Arah desain untuk website EG Learning Centre. File ini adalah sumber "rasa" desain.
`antislop/antislop.md` hanya filter di atasnya.

## Design Read

Dibaca sebagai: company profile + pendaftaran tes untuk lembaga kursus dan pusat tes bahasa Inggris
di Denpasar, untuk mahasiswa, pencari kerja, dan staf kampus/instansi, dengan gaya editorial yang
hangat memakai warna logo yang dilembutkan.

Dial: ENERGY 2 / RHYTHM 2 / MOTION 1

- ENERGY 2: cukup percaya diri untuk lembaga resmi, tidak heboh seperti landing page startup.
- RHYTHM 2: pola dasar konsisten, tapi tiap section punya komposisi sendiri sesuai isinya
  (panel besar untuk tes, daftar baris untuk kelas, garis waktu untuk cara daftar, kutipan untuk testimoni).
- MOTION 1: hanya transisi hover/fokus. Pengunjung datang untuk cek jadwal dan daftar, bukan menonton animasi.

## Identitas

- Sumber: logo EG Learning Centre dan proposal resmi (Juli 2026).
- Referensi struktur: jakartamandarin.com (urutan section lembaga kursus lokal), bukan untuk ditiru tampilannya.
- Motif identitas: **tiga titik** dari logo (biru, hijau, merah) dipakai sebagai penanda kecil yang
  berulang: penanda daftar fakta, simpul garis waktu "Cara daftar", dan pemisah di footer.
- Satu lengkungan lembut (swoosh) di bingkai foto hero, diambil dari sampul proposal. Hanya dipakai sekali.

## Warna

Warna logo (#00A8E8 biru, #80C020 hijau) terlalu terang untuk teks, jadi dilembutkan dan
digelapkan supaya lolos kontras WCAG AA.

| Token | Nilai | Dipakai untuk | Alasan |
| --- | --- | --- | --- |
| `--ink` | #17313B | teks utama, footer | biru-hijau tua, lebih lembut dari hitam, kontras 13.2:1 |
| `--ink-soft` | #4A6470 | teks sekunder | 6.1:1 di atas kertas |
| `--paper` | #FBFCF8 | latar halaman | putih hangat, material cetak EG juga berlatar putih |
| `--sky` | #166A99 | tombol utama, tautan | biru logo digelapkan, teks putih 5.9:1 |
| `--sky-mist` | #E8F4FA | latar section penekanan | versi sangat pudar dari biru logo |
| `--leaf` | #86BC3A | dekorasi saja (titik, garis) | hijau logo, tidak untuk teks |
| `--leaf-deep` | #3E7A1E | teks hijau (status lunas) | 5.1:1 |
| `--leaf-mist` | #F0F7E4 | latar section kedua | hijau logo sangat pudar |
| `--dot-red` | #E2463F | titik merah motif saja | aksen tunggal dari titik merah logo |
| `--danger` | #B42318 | pesan error | 6.4:1 |

Palet inti: biru + hijau. Aksen: titik merah (sangat hemat). Tidak ada gradien biru-ungu.

## Tipografi

- Judul: **Fraunces** (serif, sumbu SOFT=100). Alasan: wordmark "LEARNING CENTRE" di logo memakai huruf
  serif, jadi judul serif menyambung ke logo. Versi SOFT membuatnya ramah, cocok dengan permintaan "soft".
- Isi: **Plus Jakarta Sans**. Alasan: huruf buatan desainer Indonesia (Tokotype), terbaca jelas di layar HP,
  dan nyambung dengan konteks lokal.
- Keduanya di-host sendiri (`public/fonts`, lisensi OFL), tanpa request ke Google Fonts.
- Tidak ada label huruf kapital dengan spasi lebar.

## Bentuk & ruang

- Radius: 6px untuk input dan tombol, 14px untuk panel/foto. Hanya label status di admin yang membulat penuh.
- Bayangan: hanya satu tingkat elevasi untuk panel tes (yang bisa didaftar), supaya itu jadi fokus halaman.
- Spasi section tidak seragam: section utama lebih lega, section pendukung lebih rapat.

## Tema

Tema terang atau gelap mengikuti pengaturan perangkat secara otomatis, termasuk saat preferensi
perangkat berubah. Kedua mode tetap memakai palet biru-hijau EG dan kontras yang nyaman dibaca.

## Konten

- Semua fakta, harga, testimoni, dan mitra diambil dari proposal resmi EG (22 Juli 2026) dan dua Google Form pendaftaran.
- Tidak ada angka statistik karangan. Jadwal tes hanya tampil kalau admin sudah membukanya.
- Foto kelas asli dari proposal. Tidak memakai foto stok dari sampul proposal.
