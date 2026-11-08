# Website EG Learning Centre

Company profile, pendaftaran tes TOEFL ITP dan TOEIC, pembayaran lewat DOKU Checkout, notifikasi email,
dan panel admin. Laravel 13, PHP 8.3+, MySQL, Tailwind CSS 4 lewat Vite.

## Menjalankan di komputer (Laragon)

```
composer install
npm install
npm run build
php artisan migrate
php artisan db:seed --class=ProgramSeeder
php artisan test
php artisan serve
```

- Akun admin dibuat dengan `php artisan eg:make-admin` (tidak ada akun bawaan).
- Selama mengubah tampilan, jalankan `npm run dev` supaya CSS langsung diperbarui.
- `ProgramSeeder` aman dijalankan ulang: mengisi harga dan deskripsi dari proposal ke program yang sudah ada.

## Pembayaran (DOKU Checkout)

- Selama `DOKU_CLIENT_ID` dan `DOKU_SECRET_KEY` kosong dan `APP_ENV=local`, tombol bayar membuka
  **simulator** lokal (ada pita kuning di bawah halaman). Simulator mengirim notifikasi berformat DOKU
  yang ditandatangani, jadi jalur verifikasinya sama dengan DOKU asli.
- Setelah dapat kredensial sandbox: isi `DOKU_CLIENT_ID` dan `DOKU_SECRET_KEY`, lalu daftarkan
  Notification URL di DOKU Back Office: `https://domain-anda/pembayaran/doku/notifikasi`.
- Production: `DOKU_BASE_URL=https://api.doku.com` dengan kredensial production.
- Status hanya berubah dari notifikasi DOKU yang signature-nya valid dan jumlahnya cocok.

Alur status: `Menunggu Pembayaran` → (DOKU lunas) → `Menunggu Verifikasi` → `Terverifikasi` → `Selesai`.
Program tanpa `test_price` langsung mulai di `Menunggu Verifikasi`.

## Email dan tugas terjadwal

- Email: pendaftaran diterima (peserta), pendaftar baru (admin, ke `EG_ADMIN_EMAIL`), pembayaran diterima
  (peserta), dan pendaftaran dibatalkan karena tidak dibayar (peserta).
- Email dikirim lewat antrean (`QUEUE_CONNECTION=database`). Di hosting cukup satu cron:

  ```
  * * * * * php /path/ke/project/artisan schedule:run >> /dev/null 2>&1
  ```

  Cron itu menjalankan pengiriman email dan `eg:batalkan-belum-bayar` tiap 15 menit, yang membatalkan
  pendaftaran belum dibayar setelah `EG_UNPAID_HOURS` jam (default 24), melepas kursinya, dan mengirim email.
- Dasbor admin memeriksa data baru otomatis tiap 10 detik (polling ke `/admin/dashboard/feed`).

## Arsitektur: kode dikelompokkan per modul

Setiap modul punya folder sendiri di `app/Modules/<Modul>/`:

```
app/Modules/
  CompanyProfile/  beranda, program, layanan, jadwal publik, tentang, FAQ, kontak, syarat, privasi
  Catalog/         data program, jadwal tes, sesi dan kuota, kelola jadwal di admin
  Registration/    formulir TOEFL ITP/TOEIC, nomor, kursi, dokumen, halaman status,
                   kelola pendaftaran di admin, email notifikasi, batal otomatis
  Payment/         DOKU Checkout, simulator lokal, invoice, notifikasi pembayaran
  Account/         login admin, dasbor admin, akun staf, perintah eg:make-admin
  Shared/          jam sistem, format nomor HP, header keamanan, controller dasar, @rupiah
```

Isi satu modul (hanya folder yang dibutuhkan modul itu):

```
Domain/          aturan bisnis murni PHP: enum, value object, exception, interface (port)
Application/     use case (RegisterForTest, StartPayment, ...)
Infrastructure/  implementasi port: repository Eloquent, DOKU, penyimpanan dokumen, email
Models/          model Eloquent
Http/            controller, FormRequest, middleware
Policies/        hak akses
Mail/, View/, Console/, Support/   sesuai kebutuhan modul
Resources/views/ tampilan modul, dipanggil dengan awalan, misalnya view('registration::public.toeic')
Routes/web.php   route modul, dimuat oleh routes/web.php
Providers/       binding interface, rate limiter, view, perintah artisan modul ini
```

Yang sengaja tetap di tempat bersama: `resources/views/components` (layout dan komponen
UI dipakai semua modul), `resources/views/errors`, `resources/css`, `resources/js`,
`database/` (migration, seeder, factory), `config/`, `lang/`, dan `app/Providers/AppServiceProvider.php`.

`tests/Unit/ArchitectureTest.php` menjaga agar Domain dan Application setiap modul tidak memakai
Laravel/Eloquent, dan agar folder lama (`app/Domain`, `app/Models`, `app/Http`, ...) tidak dipakai lagi.
Test lain dikelompokkan sama: `tests/Feature/<Modul>/` dan `tests/Unit/<Modul>/`.

## Sumber konten

Harga, deskripsi program, testimoni, mitra, alamat, dan kontak dari proposal resmi EG (22 Juli 2026):
`config/eg.php` dan `database/seeders/ProgramSeeder.php`. Arah desain di `DESIGN.md`.

Masih perlu dikonfirmasi ke perusahaan: kredensial DOKU, SMTP email, jadwal tes asli, dan bagian
bertanda `[...]` di halaman Syarat & Ketentuan dan Kebijakan Privasi.
