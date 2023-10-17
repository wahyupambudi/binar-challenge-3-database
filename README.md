# Binar-Challenge-3-Database
Challenge Chapter 3 - RDBMS (Relational Database Management System) - Bootcamp Backend Binar Academy

## Delivery
- Analisa struktur data pada challenge 2
- Rancang Entity Relationship Diagram (ERD) sederhana yang mencakup tabel-tabel yang diperlukan
- Buat file SQL dengan nama create_and_insert.sql
- Tulis perintah SQL untuk membuat tabel-tabel sesuai dengan ERD yang Anda rancang
- Isi file SQL dengan perintah SQL untuk mengisi beberapa data awal ke dalam tabel-tabel

## Criteria
- Mampu merancang dan membuat ERD (40 points)
- Membuat Database & tabel menggunakan DDL (30 points)
- Melakukan operasi CRUD dengan DML (30 points)


## Identifikasi Entitas yang Terlibat:
- Nasabah
- Akun
- Transaksi

## Atribut-atribut yang Relevan:
- Nasabah:
  - id_nasabah (Primary Key)
  - nama
  - alamat
  - no_telp

- Akun:
  - id_akun (Primary Key)
  - id_nasabah (Foreign Key ke id_nasabah di tabel Nasabah)
  - jenis_akun (contoh: tabungan.)
  - saldo

- Transaksi:
  - id_transaksi (Primary Key)
  - id_akun (Foreign Key ke id_akun di tabel Akun)
  - jenis_transaksi (contoh: penyetoran,  transfer.)
  - jumlah
  - tanggal

## Relasi Antar Entitas:
- Setiap Nasabah dapat memiliki beberapa Akun (One-to-Many antara Nasabah dan Akun)
- Setiap Akun hanya dimiliki oleh satu Nasabah (Many-to-One antara Akun dan Nasabah)
- Satu Akun dapat memiliki banyak Transaksi (One-to-Many antara Akun dan Transaksi)
- Setiap Transaksi hanya terkait dengan satu Akun (Many-to-One antara Transaksi dan Akun)

## ERD (Entity-Relationship Diagram):
![App Screenshot](erd-binar-challenge-3-database.png)

## Buatkan SQL untuk mendefinisikan table
```bash
# membuat database
CREATE DATABASE create_and_insert;

# membuat table Nasabah
CREATE TABLE Nasabah (
    id_nasabah SERIAL PRIMARY KEY,
    nama VARCHAR(255) NOT NULL,
    alamat TEXT NOT NULL,
    nomor_telepon VARCHAR(20) NOT NULL
);

# membuat table Akun dan foreign key ke id_nasabah
CREATE TABLE Akun (
    id_akun SERIAL PRIMARY KEY,
    id_nasabah INT REFERENCES Nasabah(id_nasabah),
    jenis_akun VARCHAR(50) NOT NULL,
    saldo NUMERIC(15, 2) DEFAULT 0.00
);

# membuat table Transaksi
CREATE TABLE Transaksi (
    id_transaksi SERIAL PRIMARY KEY,
    id_akun INT REFERENCES Akun(id_akun),
    jenis_transaksi VARCHAR(50) NOT NULL,
    jumlah NUMERIC(15, 2) NOT NULL,
    tanggal DATE NOT NULL
);
```

## Buatkan SQL untuk operasi CRUD pada table yang ada
```bash
here
```