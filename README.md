# 24f_simgizi_penerima
📂 SISTEM INFORMASI MANAJEMEN GIZI - PENERIMA MBG SEKOLAH
 
Nama Aplikasi:  Simgizi_MBG_Sekolah 
Tujuan: Mengelola data siswa penerima manfaat pangan bergizi, memantau status gizi, pertumbuhan, dan penyaluran bantuan di lingkungan pendidikan.
🗄️ 1. STRUKTUR BASIS DATA
 
Simpan kode di bawah ini sebagai file  db_simgizi_sekolah.sql , lalu impor ke phpMyAdmin
buat data base
CREATE DATABASE IF NOT EXISTS db_simgizi_sekolah;
USE db_simgizi_sekolah;

-- Tabel Pengguna / Petugas
CREATE TABLE tb_pengguna (
  id_user INT NOT NULL AUTO_INCREMENT PRIMARY KEY,
  nama_lengkap VARCHAR(100) NOT NULL,
  username VARCHAR(50) NOT NULL UNIQUE,
  password VARCHAR(255) NOT NULL,
  level ENUM('ADMIN','PETUGAS GIZI','WALI KELAS','KEPALA SEKOLAH') NOT NULL,
  status ENUM('AKTIF','NONAKTIF') DEFAULT 'AKTIF'
);

-- Data Awal Login: admin / admin123
INSERT INTO tb_pengguna (nama_lengkap, username, password, level) VALUES
('Administrator Sekolah', 'admin', MD5('admin123'), 'ADMIN');

-- Tabel Data Siswa
CREATE TABLE tb_siswa (
  id_siswa VARCHAR(20) NOT NULL PRIMARY KEY COMMENT 'NIS / NISN',
  nik CHAR(16) UNIQUE,
  nama_lengkap VARCHAR(100) NOT NULL,
  jenis_kelamin ENUM('L','P') NOT NULL,
  tanggal_lahir DATE NOT NULL,
  kelas VARCHAR(20) NOT NULL COMMENT 'Contoh: 4-A, VII-B, X-APHP',
  jurusan VARCHAR(50) DEFAULT NULL COMMENT 'Khusus SMA/SMK',
  nama_ortu VARCHAR(100) NOT NULL,
  alamat TEXT NOT NULL,
  status_kelayakan ENUM('LAYAK','TIDAK LAYAK','PROSES VERIFIKASI') DEFAULT 'PROSES VERIFIKASI',
  status_penerima ENUM('AKTIF','LULUS','PINDAH','BERHENTI') DEFAULT 'AKTIF',
  tgl_daftar DATE DEFAULT CURRENT_DATE
);

-- Tabel Pemantauan Gizi & Antropometri
CREATE TABLE tb_pemantauan (
  id_pantau INT NOT NULL AUTO_INCREMENT PRIMARY KEY,
  id_siswa VARCHAR(20) NOT NULL,
  tanggal_ukur DATE NOT NULL,
  berat_badan DECIMAL(5,2) NOT NULL COMMENT 'kg',
  tinggi_badan DECIMAL(5,2) NOT NULL COMMENT 'cm',
  imt DECIMAL(5,2) DEFAULT NULL COMMENT 'Indeks Massa Tubuh',
  z_score DECIMAL(5,2) DEFAULT NULL,
  status_gizi VARCHAR(50) DEFAULT NULL COMMENT 'Sangat Kurus/Kurus/Normal/Gemuk',
  status_stunting ENUM('YA','TIDAK','BERISIKO') DEFAULT 'TIDAK',
  kondisi_kesehatan TEXT DEFAULT NULL COMMENT 'Catatan sakit/penyakit',
  petugas_ukur VARCHAR(100) NOT NULL,
  FOREIGN KEY (id_siswa) REFERENCES tb_siswa(id_siswa)
);

-- Tabel Penyaluran Bantuan
CREATE TABLE tb_penyaluran (
  id_terima INT NOT NULL AUTO_INCREMENT PRIMARY KEY,
  id_siswa VARCHAR(20) NOT NULL,
  periode VARCHAR(20) NOT NULL COMMENT 'Bulan & Tahun',
  jenis_bantuan VARCHAR(100) NOT NULL COMMENT 'Makan Siang / Paket Pangan / Susu',
  tanggal_terima DATE NOT NULL,
  status_terima ENUM('SUDAH DITERIMA','BELUM DITERIMA','IZIN','SAKIT') DEFAULT 'SUDAH DITERIMA',
  keterangan TEXT DEFAULT NULL,
  FOREIGN KEY (id_siswa) REFERENCES tb_siswa(id_siswa)
);
