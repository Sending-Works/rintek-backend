# 📆 Rintek Backend

Backend API untuk aplikasi **RINTEK** (Ruang Interaktif Teknologi), dibangun dengan:
- Express.js + TypeScript
- Supabase (PostgreSQL) sebagai database
- bcrypt untuk enkripsi password
- express-validator untuk validasi
- Supabase JS Client untuk query ke database

---

## 🛠️ Setup Proyek

### 1. Clone Project

```bash
git clone https://github.com/Sending-Works/rintek-backend
cd rintek-backend
```

### 2. Install Dependencies

```bash
npm install
```

### 3. Buat File `.env`

```env
SUPABASE_URL=https://your-project.supabase.co
SUPABASE_KEY=your-supabase-service-role-key
PORT=3000
```

### 4. Jalankan Server Development (lokal)

```bash
npm run dev
```

---

## 📚 Struktur Folder

```
rintek-backend/
├── api/
│   └── index.ts          # Entry point untuk Vercel (serverless)
├── src/
│   ├── routes/           # Routing endpoint
│   ├── controllers/      # Logic untuk user, kategori, dll.
│   ├── services/         # Supabase client
│   ├── types/            # TypeScript types (user, kategori, general)
│   ├── middlewares/      # Middleware validasi dan error
│   ├── utils/            # Fungsi umum seperti sendResponse
│   └── server.ts         # Express app utama (tanpa listen)
├── vercel.json
├── tsconfig.json
├── package.json
└── .env
```

---

## 🧪 API Endpoint

### 🔐 Auth
| Method | Endpoint           | Deskripsi         |
|--------|--------------------|-------------------|
| POST   | /api/auth/register | Register user     |
| POST   | /api/auth/login    | Login user        |

### 👤 User
| Method | Endpoint         | Deskripsi               |
|--------|------------------|-------------------------|
| GET    | /api/users       | Ambil semua user        |
| GET    | /api/users/:id   | Ambil user berdasarkan ID |
| PATCH  | /api/users/:id/subscribe | Update tipe subscription user (`PRIBADI`/`KOMUNITAS`) |

### 🏷️ Kategori & Relasi
| Method | Endpoint                    | Deskripsi                                |
|--------|-----------------------------|------------------------------------------|
| GET    | /api/kategori               | Ambil semua kategori                     |
| GET    | /api/kategori/:id           | Ambil kategori berdasarkan ID            |
| POST   | /api/kategori               | Tambah kategori baru                     |
| PUT    | /api/kategori/:id           | Update kategori                          |
| DELETE | /api/kategori/:id           | Hapus kategori                           |
| POST   | /api/user-kategori          | Tambah user ke kategori (relasi)         |
| GET    | /api/user-kategori/:userId  | Ambil kategori yang diikuti oleh user    |

---

## 📎 Contoh Request Body

### 👭‍♂️ Register User
```json
{
  "name": "username_uniqe",
  "password": "secret321"
}
```

### 🔐 Login
```json
{
  "name": "username_exist",
  "password": "secret321"
}
```

### 🏷️ Tambah Kategori
#### KOMUNITAS:
```json
{
  "name": "Bisnis",
  "room_name": "Ruang Bisnis",
  "room_desc": "Diskusi seputar bisnis",
  "slug": "bisnis",
  "type": "KOMUNITAS"
}
```

#### PRIBADI:
```json
{
  "name": "Catatan Pribadi",
  "room_name": "Ruang Pribadi",
  "room_desc": "Refleksi dan pemikiran pribadi",
  "slug": "pribadi",
  "type": "PRIBADI"
}
```

### 🔗 Tambah User-Kategori
```json
{
  "user_id": "uuid-user",
  "kategori_id": "uuid-kategori"
}
```

---

## ⚙️ Fitur Tambahan

- Validasi input menggunakan `express-validator`
- Response konsisten via `utils/sendResponse.ts`
- Password dienkripsi dengan `bcrypt`
- Global error handling & middleware keamanan (`helmet`, `compression`, `cors`)

---

## 📌 Catatan Teknis

- Kolom `type` pada kategori menggunakan enum PostgreSQL: `PRIBADI`, `KOMUNITAS`
- Nilai `slug` pada kategori wajib unik
- Gunakan Supabase SQL Editor untuk setup awal skema database, misalnya:
```sql
CREATE TYPE tipeKategori AS ENUM ('PRIBADI', 'KOMUNITAS');
CREATE TABLE kategori (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  name TEXT NOT NULL,
  room_name TEXT,
  room_desc TEXT,
  slug TEXT UNIQUE,
  type tipeKategori NOT NULL,
  created_at TIMESTAMP DEFAULT now()
);
```

---

## 🚀 Deploy

- App ini siap deploy ke Vercel menggunakan struktur `api/index.ts` dan `vercel.json`
- Semua endpoint tersedia di:  
  👉 `https://rintek-backend.vercel.app`

---

## 👨‍💻 Kontributor

- Deris Firmansyah — [@sendingworks](https://github.com/Sending-Works)

---
