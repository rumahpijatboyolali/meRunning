# meRunning

meRunning adalah jurnal lari pribadi berbasis Google Apps Script.
Screenshot Huawei Health dibaca melalui OpenAI API. Angka hasil baca
ditampilkan untuk diperiksa sebelum masuk ke Google Sheets.

## Isi folder

- **Code.gs**: halaman, pembacaan screenshot, dan penyimpanan Sheet.
- **Index.html**: antarmuka, CSS, dan JavaScript dalam satu file.
- **appsscript.json**: manifest Apps Script.

Screenshot tidak disimpan ke Drive atau Google Sheets oleh aplikasi.
Data yang tersimpan hanya metrik yang sudah diperiksa pengguna.
Screenshot yang sangat panjang dipotong di browser menjadi beberapa bagian
agar teks metrik tetap terbaca, lalu dikirim bersama dalam satu permintaan AI.
meRunning mengambil ringkasan, split, pace dan zona detak jantung, kalori,
langkah, cadence, teknik lari, pemulihan, serta cuaca yang tampak pada screenshot.
Peta, nama akun, dan alamat diabaikan.

## Menyiapkan spreadsheet

1. Buat Google Sheet kosong dengan nama **meRunning**.
2. Dari Sheet, pilih **Extensions > Apps Script**.
3. Ganti isi **Code.gs** dengan file Code.gs dari folder ini.
4. Tambahkan file HTML bernama **Index**, lalu salin isi Index.html.
5. Aktifkan tampilan appsscript.json di Project Settings dan ganti
   isinya dengan file manifest dari folder ini.
6. Simpan proyek.
7. Pilih fungsi setupMeRunning di editor, lalu klik **Run** dan izinkan
   akses ke Google Sheet.

Fungsi setup membuat tab **Runs** dan **Settings**.

## Menambahkan OpenAI API

1. Buat API key di [OpenAI API Platform](https://platform.openai.com/api-keys).
2. Pastikan billing API OpenAI sudah aktif. Billing API terpisah dari
   langganan ChatGPT.
3. Di Apps Script, buka **Project Settings > Script properties**.
4. Tambahkan properti berikut:

   - **OPENAI_API_KEY**: secret key dari OpenAI API Platform.
   - **OPENAI_MODEL**: opsional; ID model vision yang mendukung JSON Schema.

Model awal di kode adalah **gpt-6-luna**. Model bisa diganti dari
Script Properties jika katalog atau kebutuhan berubah.

Key hanya dibaca oleh Code.gs; key tidak dikirim ke browser.
Screenshot dikirim ke OpenAI API untuk dibaca saat pengguna menekan
**Baca dengan AI**.

## Deploy sebagai Web App pribadi

1. Di Apps Script, pilih **Deploy > New deployment**.
2. Pilih jenis **Web app**.
3. Atur **Execute as** menjadi akun pemilik spreadsheet.
4. Atur akses menjadi **Only myself**.
5. Deploy, lalu buka URL Web App yang diberikan.

Setelah mengubah file, buka **Deploy > Manage deployments**, edit deployment,
pilih **New version**, lalu deploy lagi agar Web App memakai perubahan terbaru.

Atur akses pribadi karena versi awal tidak memiliki login per pengguna.
Jangan membagikan URL deployment ke publik.

## Alur penggunaan

1. Pilih screenshot JPG, PNG, atau WebP sampai 6 MB.
2. Tekan **Baca dengan AI**.
3. Periksa dan koreksi tanggal, jarak, durasi, pace, serta metrik lain.
4. Tambahkan RPE atau catatan kondisi tubuh bila diinginkan.
5. Tekan **Simpan aktivitas**.

Riwayat menyimpan rincian setiap aktivitas. Dashboard membandingkan pace
dengan lari terdahulu yang jaraknya serupa dan merangkum jarak per minggu.
Angka kosong tidak diisi dengan tebakan. Tab Runs yang sudah ada akan
mendapatkan kolom metrik tambahan di sisi kanan tanpa menghapus data lama.

## Fitur dashboard

- Ringkasan jarak, jumlah sesi, dan durasi bulan ini; perubahan jarak dibanding
  tanggal yang sama pada bulan sebelumnya.
- Rekor pribadi untuk jarak terjauh dan pace rata-rata tercepat.
- Grafik tren bulanan pace serta detak jantung rata-rata untuk enam bulan.
- Pencarian riwayat berdasarkan jenis aktivitas, catatan, atau tanggal, dengan
  filter jenis aktivitas dan rentang waktu.
- Edit dan hapus aktivitas langsung dari riwayat. Penghapusan meminta konfirmasi.
- Navigasi cepat ke ringkasan, perkembangan, input, dan riwayat.

Setelah menyalin perubahan ke Apps Script, deploy sebagai versi baru Web App
agar fitur edit dan hapus tersedia pada URL deployment.

## Landing page GitHub Pages

File `indexgithub.html` adalah landing page statis; `Index.html` tetap menjadi
antarmuka Apps Script. Landing page sudah memakai tautan Web App dashboard yang
dikonfigurasi di tombol **Buka dashboard**.

Untuk menerbitkannya, dorong repository ke GitHub, lalu buka **Settings > Pages**
dan pilih **Deploy from a branch**, branch utama, folder **/(root)**. Setelah
publikasi, landing dapat dibuka di `https://username.github.io/nama-repo/indexgithub.html`.
Halaman landing bersifat publik, sementara dashboard tetap meminta akses akun
Google yang diizinkan.
