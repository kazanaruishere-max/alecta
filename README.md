# 🎓 ALECTA - Smart School Management System

> **Platform Manajemen Sekolah Cerdas Berbasis AI dengan Sistem Terintegrasi**

Alecta adalah aplikasi web modern yang dirancang untuk mendigitalkan ekosistem sekolah secara menyeluruh. Dibangun dengan teknologi terkini, Alecta menghubungkan Siswa, Guru, Ketua Kelas, dan Administrator dalam satu platform yang efisien, transparan, dan cerdas.

---

## 🌟 Fitur Utama

### 🧠 Sistem AI Cerdas
- **AI Quiz Generator**: Guru dapat membuat kuis otomatis hanya dengan memasukkan topik.
- **AI Feedback**: Memberikan umpan balik otomatis pada tugas siswa untuk meningkatkan pemahaman.
- **Smart Analytics**: Analisis performa siswa dan kelas berbasis data.

### 👥 Peran & Akses Pengguna
1. **Siswa (Student)**
   - 📅 **Jadwal & Deadline**: Melihat jadwal pelajaran dan tenggat waktu tugas.
   - 📤 **Pengumpulan Tugas**: Upload tugas dan dapatkan feedback instan.
   - 📝 **Kuis Online**: Mengerjakan kuis dengan timer dan auto-grading.
   - 📊 **Progress Report**: Memantau nilai dan absensi sendiri.

2. **Ketua Kelas (Class Leader)**
   - 📢 **Pengumuman Kelas**: Membuat pengumuman untuk teman sekelas.
   - 📋 **Bantuan Absensi**: Membantu guru merekap kehadiran (opsional).
   - 🤝 **Penghubung**: Menjembatani komunikasi guru dan siswa.

3. **Guru (Teacher)**
   - 🏫 **Manajemen Kelas**: Mengatur materi, tugas, dan ujian.
   - 📥 **Sistem Penilaian**: Memberi nilai tugas dan kuis.
   - 📈 **Ekspor Excel**: Rekap nilai siap cetak/download dalam format Excel.
   - 🤖 **AI Assistant**: Membuat materi dan soal ujian dibantu AI.

4. **Super Admin**
   - 🛠️ **Manajemen Sistem**: Mengelola user, tahun ajaran, dan mata pelajaran.
   - 🔒 **Keamanan & Audit**: Memantau log aktivitas sistem.

---

## 🗺️ Roadmap & Alur Sistem

Berikut adalah alur lengkap sistem Alecta dari tahap pengembangan hingga deployment, beserta interaksi fitur di dalamnya.

```mermaid
flowchart TD
    %% SUBGRAPH: FASE PENGEMBANGAN
    subgraph Dev_Phase [🛠️ FASE 1: DEVELOPMENT]
        Start((Mulai)) --> TechStack[Inisialisasi Tech Stack]
        TechStack -->|Next.js 15, Drizzle, Shadcn| SetupEnv[Setup Environment]
        SetupEnv --> DBInit[Inisialisasi Database Schema]
        
        DBInit --> Seed[Seeding Data Awal]
        Seed -->|User: Admin, Guru, Siswa| DevServer[Run Development Server]
        
        %% Fitur AI Integration
        DevServer --> AI_Config[Konfigurasi AI - OpenRouter]
        AI_Config -->|Siap Digunakan| AppReady{Aplikasi Siap}
    end

    %% SUBGRAPH: ALUR APLIKASI UTAMA
    subgraph App_Flow [💻 FASE 2: ALUR APLIKASI & FITUR]
        AppReady --> Login[Halaman Login]
        
        Login --> AuthCheck{Autentikasi}
        AuthCheck -->|Gagal| ErrMsg[Tampilkan Error]
        ErrMsg --> Login
        
        AuthCheck -->|Berhasil| RoleRoute{Cek Role User}
        
        %% --- ALUR ADMIN ---
        RoleRoute -->|SUPER_ADMIN| AdminDash[Admin Dashboard]
        AdminDash --> UM[Manajemen User]
        AdminDash --> MP[Manajemen Pelajaran]
        AdminDash --> Audit[Audit Log Sistem]

        %% --- ALUR GURU ---
        RoleRoute -->|TEACHER| TeachDash[Teacher Dashboard]
        
        %% Fitur Guru
        subgraph Teacher_Feat [Fitur Guru]
            TeachDash --> CreateMat[Upload Materi]
            TeachDash --> CreateQuiz[Buat Kuis]
            
            CreateQuiz -.->|Pilihan: AI Generated| AI_Gen[🤖 AI Quiz Generator]
            AI_Gen --> QuizReady[Kuis Siap]
            
            TeachDash --> Grading[Penilaian Tugas]
            Grading --> ExportExcel[📥 Ekspor Nilai ke Excel]
        end

        %% --- ALUR MURID ---
        RoleRoute -->|STUDENT| StudDash[Student Dashboard]
        
        %% Fitur Siswa
        subgraph Student_Feat [Fitur Siswa]
            StudDash --> ViewSched[Lihat Jadwal]
            StudDash --> DoQuiz[Kerjakan Kuis]
            DoQuiz -->|Auto Grade| ShowScore[Lihat Nilai]
            
            StudDash --> SubmitTask[Kumpulkan Tugas]
            SubmitTask -.->|Otomatis| AIFeedback[🤖 AI Feedback]
            AIFeedback --> StudReview[Review Hasil]
        end
        
        %% --- ALUR KETUA KELAS ---
        RoleRoute -->|CLASS_LEADER| LeaderDash[Leader Dashboard]
        LeaderDash --> Announce[Buat Pengumuman]
        LeaderDash --> AssistAttend[Rekap Absensi Harian]
        
        %% Interaksi Antar Fitur
        CreateMat -->|Notifikasi| StudDash
        CreateQuiz -->|Deadline Reminder| StudDash
    end

    %% SUBGRAPH: FASE DEPLOYMENT
    subgraph Deploy_Phase [🚀 FASE 3: DEPLOYMENT]
        AppReady -.->|Siap Rilis| Build[Build Production]
        Build --> Dockerize[Buat Docker Image]
        
        Dockerize --> Infra{Pilih Infrastruktur}
        
        Infra -->|Cloud VPS| DockerCompose[Docker Compose Up]
        DockerCompose --> ProdDB[Production Database]
        DockerCompose --> LiveApp[Alecta Live App]
        
        Infra -->|Serverless| Vercel[Deploy ke Vercel]
        Vercel --> ExtDB[Supabase / Neon DB]
        ExtDB --> LiveApp
    end

    %% Styling Nodes
    style AI_Gen fill:#f9f,stroke:#333,stroke-width:2px
    style AIFeedback fill:#f9f,stroke:#333,stroke-width:2px
    style ExportExcel fill:#9f9,stroke:#333,stroke-width:2px
    style Login fill:#ff9,stroke:#333,stroke-width:2px
```

---

## 🛠️ Teknologi & Pengaturan

### Tech Stack
- **Framework**: Next.js 15 (App Router)
- **Database**: PostgreSQL (via Supabase/Docker) & Drizzle ORM
- **Authentication**: Better Auth (Secure Email/Password)
- **UI/UX**: Tailwind CSS v4, Shadcn UI
- **AI Engine**: OpenRouter integration

### Konfigurasi Sistem
Sistem ini menggunakan konfigurasi berbasis file `.env` untuk keamanan maksimal.
- **Database URL**: Koneksi string ke PostgreSQL.
- **Auth Secret**: Kunci enkripsi untuk sesi login.
- **AI Keys**: API Key untuk fitur kecerdasan buatan.

---

## 📈 Skalabilitas & Masa Depan

Alecta dirancang untuk tumbuh bersama sekolah Anda. Arsitektur modular memungkinkan penambahan fitur di masa depan seperti:
- Integrasi Pembayaran SPP (Payment Gateway)
- Aplikasi Mobile (React Native)
- Perpustakaan Digital
- Pelacakan Bus Sekolah (GPS)

---

*Dibuat dengan ❤️ untuk Masa Depan Pendidikan Indonesia.*
