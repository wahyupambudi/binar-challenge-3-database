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
  - createdAt
  - updatedAt
  - deletedAt

- Akun:
  - id_akun (Primary Key)
  - id_nasabah (Foreign Key ke id_nasabah di tabel Nasabah)
  - jenis_akun (contoh: tabungan.)
  - saldo
  - createdAt
  - updatedAt
  - deletedAt

- Transaksi:
  - id_transaksi (Primary Key)
  - id_akun (Foreign Key ke id_akun di tabel Akun)
  - jenis_transaksi (contoh: penyetoran,  transfer.)
  - jumlah
  - createdAt
  - updatedAt
  - deletedAt

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
    id SERIAL PRIMARY KEY,
    nama VARCHAR(255) NOT NULL,
    alamat TEXT NOT NULL,
    no_telp VARCHAR(20) NOT NULL,
    createdAt TIMESTAMP NOT NULL DEFAULT NOW(),
    updatedAt TIMESTAMP DEFAULT NOW(),
    deletedAt TIMESTAMP
);

# membuat table Akun dan foreign key ke id_nasabah
CREATE TYPE JenisAkun AS ENUM ('tabungan', 'deposito');
CREATE TABLE Akun (
    id SERIAL PRIMARY KEY,
    id_nasabah INT REFERENCES Nasabah(id) ON DELETE CASCADE,
    nomor_rekening VARCHAR(20) NOT NULL,
    jenis_akun JenisAkun NOT NULL,
    saldo NUMERIC(12, 2) DEFAULT 0.00,
    createdAt TIMESTAMP NOT NULL DEFAULT NOW(),
    updatedAt TIMESTAMP DEFAULT NOW(),
    deletedAt TIMESTAMP
);

# membuat table Transaksi dan foreign key ke id_akun
CREATE TYPE JenisTransaksi AS ENUM ('penyetoran', 'penarikan', 'transfer');
CREATE TABLE Transaksi (
    id SERIAL PRIMARY KEY,
    id_akun INT REFERENCES Akun(id) ON DELETE CASCADE,
    nomor_transaksi VARCHAR(20) NOT NULL,
    jenis_transaksi JenisTransaksi NOT NULL,
    jumlah NUMERIC(12, 2) NOT NULL,
    createdAt TIMESTAMP NOT NULL DEFAULT NOW(),
    updatedAt TIMESTAMP DEFAULT NOW(),
    deletedAt TIMESTAMP
);
```

## Buatkan SQL untuk operasi CRUD pada table yang ada
### INSERT
```bash
# insert into table Nasabah
INSERT INTO Nasabah (nama, alamat, no_telp) VALUES 
('Wahyu P', 'Dusun 3, Jati Agung', '123-456-7890'),
('Pambudi', 'Bali Kutai', '234-567-8901'),
('John Doe', '123 Main St', '123-456-7890'),
('Jane Smith', '456 Elm St', '234-567-8901'),
('Alice Johnson', '789 Oak St', '345-678-9012'),
('Bob Williams', '101 Pine St', '456-789-0123'),
('Eva Davis', '202 Maple St', '567-890-1234'),
('Michael Brown', '303 Cedar St', '678-901-2345'),
('Sarah Lee', '404 Birch St', '789-012-3456'),
('David White', '505 Spruce St', '890-123-4567');

# insert into table Akun
INSERT INTO Akun (id_nasabah, nomor_rekening, jenis_akun, saldo) VALUES 
(1, '182123763','tabungan', 1000),
(1, '198234217', 'deposito', 5000),
(2, '091834651', 'tabungan', 2000),
(1, '1234567890', 'tabungan', 1000),
(2, '2345678901', 'deposito', 5000),
(1, '3456789012', 'tabungan', 200),
(3, '4567890123', 'deposito', 3000),
(2, '5678901234', 'tabungan', 1500),
(4, '6789012345', 'deposito', 7000),
(5, '7890123456', 'tabungan', 2500);

# insert into table Transaksi
INSERT INTO Transaksi (id_akun, nomor_transaksi, jenis_transaksi, jumlah) VALUES 
(1, 'TRX103','penyetoran', 500),
(1, 'TRX113', 'penarikan', 200),
(2, 'TRX133', 'transfer', 1000),
(3, 'TRX143', 'penyetoran', 300),
(1, 'TRX123', 'penyetoran', 500),
(2, 'TRX124', 'penarikan', 200),
(3, 'TRX125', 'transfer', 1000),
(1, 'TRX126', 'penyetoran', 300),
(2, 'TRX127', 'transfer', 700),
(4, 'TRX128', 'penarikan', 400);
```

### READ
```bash
# read with select
SELECT * FROM Nasabah;
SELECT * FROM Akun;
SELECT * FROM Transaksi;

# read with inner join Nasabah, Akun
SELECT Nasabah.nama, Nasabah.no_telp, 
Akun.nomor_rekening, Akun.jenis_akun, Akun.saldo 
FROM Nasabah INNER JOIN Akun 
ON Nasabah.id = Akun.id_nasabah;
```

### UPDATE
```bash
# update nasabah
UPDATE Nasabah 
SET nama = 'Wahyu P Updated', updatedAt = CURRENT_TIMESTAMP
WHERE id_nasabah = 1;

# update akun
UPDATE Akun
SET jenis_akun = 'Investasi', updatedAt = CURRENT_TIMESTAMP
WHERE id_akun = 2;

# update transaksi
UPDATE Transaksi
SET jenis_transaksi = 'Transfer Keluar', jumlah = 1500, updatedAt = CURRENT_TIMESTAMP
WHERE id_transaksi = 2;
```

### Membuat Indexing
```bash
# membuat indexing
CREATE INDEX indexNasabah ON "Nasabah" (nama);
CREATE INDEX indexAkun ON "Akun" (nomor_rekening);
CREATE INDEX indexTransaksi ON "Transaksi" (nomor_transaksi);
```

### Store Procedure
```bash
# store procedure tambah_nasabah
CREATE OR REPLACE PROCEDURE tambah_nasabah(
    IN p_nama VARCHAR(255),
    IN p_alamat TEXT,
    IN p_no_telp VARCHAR(20)
)
AS $$
BEGIN
    INSERT INTO Nasabah (nama, alamat, no_telp) VALUES (p_nama, p_alamat, p_no_telp);
END;
$$ LANGUAGE plpgsql;

# store procedure tambah_akun
CREATE OR REPLACE PROCEDURE tambah_akun(
    IN p_id_nasabah INT,
    IN p_nomor_rekening VARCHAR(20),
    IN p_jenis_akun JenisAkun,
    IN p_saldo NUMERIC(15, 2)
)
AS $$
BEGIN
    INSERT INTO Akun (id_nasabah, nomor_rekening, jenis_akun, saldo) VALUES (p_id_nasabah, p_nomor_rekening, p_jenis_akun, p_saldo);
END;
$$ LANGUAGE plpgsql;

# call store procedure
CALL tambah_nasabah('Udin', 'Kalimantan', '985-832-1234');
CALL tambah_akun(2, '091834651', 'tabungan', 2000);
```

### CTE (Common Table Expressions )
```bash
# CTE AccountInformation
WITH AccountInformation AS (
    SELECT n.nama AS nama_nasabah, a.nomor_rekening, a.jenis_akun, a.saldo
    FROM Nasabah n
    JOIN Akun a ON n.id = a.id_nasabah
)
SELECT * FROM AccountInformation;
```

### DELETE
```bash
# delete nasabah
DELETE FROM Nasabah WHERE id = 1;

# delete akun
DELETE FROM Akun WHERE id = 3;

# delete transaksi
DELETE FROM Transaksi WHERE id = 1;
```