# Ekspor Riwayat, Pengingat Jatuh Tempo, dan Breadcrumb

## Catatan soal data lama (permintaan sebelumnya)

Database proyek asal masih bisa dibaca, tapi isinya hanya data awal yang sama dengan sekarang: 32 kamar, 288 barang kamar, 13 fasilitas utama, dan daftar kondisi/lokasi. Tabel tenant, pembayaran, pendapatan, pengeluaran, dan riwayat semuanya kosong (0 baris). Jadi tidak ada transaksi lama yang hilang — tidak ada yang perlu diimpor. Kalau Anda punya catatan lama di file Excel/CSV, kirimkan dan saya masukkan.

## 1. Tombol ekspor riwayat (PDF + Excel)

Dua tempat mendapat tombol **Ekspor PDF** dan **Ekspor Excel**:

- Halaman **Tenant & Pembayaran** → panel "Riwayat Perubahan Data": mengekspor seluruh log perubahan (tanggal, nama tenant, status lama → baru, kamar lama → baru, catatan).
- Halaman **Detail Tenant** → dua ekspor terpisah: riwayat pembayaran tenant tersebut (tanggal bayar, periode, masa berlaku, jumlah, metode, catatan, jumlah lampiran) dan riwayat perubahan data tenant tersebut.

Isi file mengikuti gaya laporan yang sudah ada: judul, tanggal cetak, ringkasan singkat (jumlah baris, total nominal untuk pembayaran), lalu tabel. Nama file otomatis, misalnya `riwayat-pembayaran-<nama-tenant>-<tanggal>.pdf`.

## 2. Pengingat otomatis sebelum jatuh tempo

Pengingat tampil di dalam aplikasi (tanpa email/WhatsApp otomatis, karena itu perlu layanan pengirim terpisah):

- Kartu **Pengingat Tagihan** di halaman Ringkasan dan di atas daftar tenant, berisi tenant yang jatuh tempo dalam 7 hari ke depan, jatuh tempo hari ini, dan yang sudah terlambat.
- Tiap baris menampilkan nama, kamar, tanggal jatuh tempo, sisa hari, status bukti pembayaran (ada lampiran atau belum), plus tombol cepat: buka detail tenang, catat pembayaran, dan hubungi via WhatsApp (jika nomor tersedia).
- Penanda "perlu diperiksa" untuk pembayaran yang tercatat tanpa lampiran bukti.
- Ambang hari (default 7) bisa diubah dan disimpan di perangkat.

## 3. Breadcrumb otomatis di AppShell

AppShell menampilkan jejak halaman di bawah header, misalnya:

```text
Ringkasan / Barang Inventaris / Inventaris Kamar
Ringkasan / Tenant & Pembayaran / Rani Putri
Ringkasan / Barang Inventaris / Inventaris Kamar / Kamar 004
```

Dibentuk otomatis dari rute yang aktif, dengan setiap bagian bisa diklik kecuali yang terakhir. Di layar kecil hanya dua bagian terakhir yang ditampilkan agar tidak penuh.

## Detail teknis

- Ekspor memakai helper yang sudah ada: `buildSimplePdf`/`downloadSimplePdf` (jspdf + autotable) dan `xlsx` yang sudah dipakai `report-export.ts`. Helper baru `src/lib/history-export.ts` untuk kedua dataset.
- Data pengingat dihitung di klien dari `tenants` + `tenant_payments` yang sudah di-query (`dueInfo` di `src/lib/tenants.ts`), tanpa perubahan skema database. Komponen baru `src/components/DueReminders.tsx`.
- Breadcrumb: peta label rute di `src/lib/breadcrumbs.ts` dengan `useMatches()` dari TanStack Router, dirender dengan komponen `ui/breadcrumb` yang sudah tersedia. AppShell tetap menerima `title`/`subtitle` seperti sekarang; label dinamis (nomor kamar, nama tenant) dikirim via prop opsional.
