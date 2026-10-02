# Atur Dana (PWA offline + sinkron)

## 1. Siapkan database sinkron (Supabase, gratis)
1. Buat akun dan proyek baru di supabase.com.
2. Buka **SQL Editor**, jalankan:

```sql
create table finance_state (
  user_id uuid primary key references auth.users on delete cascade,
  data jsonb not null,
  updated_at timestamptz default now()
);
alter table finance_state enable row level security;
create policy "milik sendiri" on finance_state for all
  using (auth.uid() = user_id) with check (auth.uid() = user_id);
```
3. Di **Authentication > Providers > Email**, matikan "Confirm email" agar bisa langsung masuk setelah daftar.
4. Di **Project Settings > API**, salin **Project URL** dan **anon public key**.
5. Buka `index.html`, cari `const CFG={url:'',key:''}` lalu isi kedua nilai tadi. Kunci anon aman ditaruh di sini karena data dilindungi aturan RLS di atas.

## 2. Taruh di internet (wajib HTTPS agar bisa di-install)
Unggah seluruh isi folder ini ke hosting statis gratis, misalnya Netlify Drop (seret folder), Cloudflare Pages, atau GitHub Pages.

## 3. Install
- **Laptop (Chrome/Edge):** klik ikon install di kolom alamat.
- **Android:** menu Chrome > Install app / Add to Home screen.
- **iPhone:** Safari > Bagikan > Add to Home Screen.

Buka sekali saat online supaya file tersimpan, setelah itu aplikasi jalan offline. Di tiap perangkat, daftar atau masuk dengan akun yang sama; sinkron berjalan otomatis saat ada internet.

Kalau kamu mengubah `index.html` setelah dipasang, naikkan nomor `atur-dana-v1` di `sw.js` supaya perangkat mengambil versi baru.
