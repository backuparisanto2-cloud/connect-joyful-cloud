# Impor proyek dari GitHub: connect-link-joy

Repo `backuparisanto2-cloud/connect-link-joy` adalah aplikasi Lovable (TanStack Start + Supabase) untuk manajemen kos/properti: denah lantai, kamar, tenant, inventaris/fasilitas, pendapatan, pengeluaran, jurnal, dan laporan. Proyek ini masih template kosong, jadi isinya akan disalin masuk.

## Yang akan dilakukan

1. Unduh isi repo (branch `main`) dan salin ke proyek ini:
   - `src/routes/*` (index, denah, kamar, tenant, fasilitas, pendapatan, pengeluaran, jurnal, kelola, laporan)
   - `src/components/*` termasuk komponen shadcn/ui yang belum ada
   - `src/lib/*`, `src/hooks/*`, `src/integrations/supabase/*`
   - `public/*` (ikon app, splash, gambar denah, manifest) dan `src/assets/*`
   - `src/styles.css`, `src/router.tsx`, `src/start.ts`, `src/server.ts`, `components.json`
2. Aktifkan Lovable Cloud (database, auth, storage) karena aplikasi memakai Supabase.
3. Jalankan ulang seluruh migrasi SQL dari `supabase/migrations/` (10 file) di urutan aslinya, sehingga tabel, RLS, grants, storage bucket, dan fungsi ikut terbentuk.
4. Pasang dependensi yang dibutuhkan sesuai `package.json` repo (mis. recharts, embla-carousel, react-hook-form, sonner, jspdf/xlsx bila ada, dsb.).
5. Regenerasi `routeTree.gen.ts` lewat dev server dan verifikasi build + typecheck.
6. Periksa halaman di preview (mobile 390px) untuk memastikan tampilan dan navigasi jalan.

## Catatan teknis

- File `.env` repo tidak disalin. Kredensial Cloud proyek ini dibuat otomatis saat Cloud diaktifkan, dan `src/integrations/supabase/client.ts` akan memakai nilai proyek baru.
- Data isi (tenant, kamar, transaksi) tidak ikut karena repo hanya memuat skema, bukan baris database. Jika perlu data contoh, saya bisa tambahkan seed setelah impor.
- Kunci Lovable AI dipakai oleh `expense-ai.functions.ts` dan `journal-format-ai.functions.ts`; keduanya akan bekerja lewat AI Gateway proyek ini.
- Jika ada fitur yang bergantung pada Storage bucket privat, kebijakan bucket akan dibuat ulang lewat migrasi tersebut.
