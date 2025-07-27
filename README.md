# transaksi-pulsa-sql

-- MySQL Script untuk Sistem Pembelian Pulsa di Tokopedia
-- Drop database jika sudah ada (opsional)
-- DROP DATABASE IF EXISTS sistem_topup_pulsa;

-- Buat database
CREATE DATABASE IF NOT EXISTS sistem_topup_pulsa;
USE sistem_topup_pulsa;

-- Tabel user
CREATE TABLE user (
    user_id INT AUTO_INCREMENT PRIMARY KEY,
    nama VARCHAR(100) NOT NULL,
    nomor_hp VARCHAR(15) NOT NULL UNIQUE,
    email VARCHAR(100) NOT NULL UNIQUE,
    password VARCHAR(255) NOT NULL,
    saldo_wallet DECIMAL(15,2) DEFAULT 0.00,
    created_at DATETIME DEFAULT CURRENT_TIMESTAMP,
    updated_at DATETIME DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    INDEX idx_user_email (email),
    INDEX idx_user_nomor_hp (nomor_hp)
);

-- Tabel provider
CREATE TABLE provider (
    provider_id INT AUTO_INCREMENT PRIMARY KEY,
    nama_provider VARCHAR(50) NOT NULL,
    kode_provider VARCHAR(10) NOT NULL UNIQUE,
    logo_url VARCHAR(255),
    status_aktif BOOLEAN DEFAULT TRUE,
    created_at DATETIME DEFAULT CURRENT_TIMESTAMP,
    INDEX idx_provider_kode (kode_provider)
);

-- Tabel produk_pulsa
CREATE TABLE produk_pulsa (
    produk_id INT AUTO_INCREMENT PRIMARY KEY,
    provider_id INT NOT NULL,
    nominal INT NOT NULL,
    harga_normal DECIMAL(10,2) NOT NULL,
    harga_promo DECIMAL(10,2),
    masa_aktif_hari INT DEFAULT 30,
    status_promo BOOLEAN DEFAULT FALSE,
    persentase_diskon INT DEFAULT 0,
    deskripsi TEXT,
    status_aktif BOOLEAN DEFAULT TRUE,
    created_at DATETIME DEFAULT CURRENT_TIMESTAMP,
    updated_at DATETIME DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    FOREIGN KEY (provider_id) REFERENCES provider(provider_id) ON DELETE CASCADE,
    INDEX idx_produk_provider (provider_id),
    INDEX idx_produk_nominal (nominal),
    INDEX idx_produk_status (status_aktif)
);

-- Tabel metode_pembayaran
CREATE TABLE metode_pembayaran (
    metode_id INT AUTO_INCREMENT PRIMARY KEY,
    nama_metode VARCHAR(50) NOT NULL,
    jenis_metode VARCHAR(20) NOT NULL,
    kode_metode VARCHAR(10) NOT NULL UNIQUE,
    logo_url VARCHAR(255),
    biaya_admin DECIMAL(10,2) DEFAULT 0.00,
    status_aktif BOOLEAN DEFAULT TRUE,
    created_at DATETIME DEFAULT CURRENT_TIMESTAMP,
    INDEX idx_metode_kode (kode_metode),
    INDEX idx_metode_jenis (jenis_metode)
);

-- Tabel promo
CREATE TABLE promo (
    promo_id INT AUTO_INCREMENT PRIMARY KEY,
    kode_promo VARCHAR(20) NOT NULL UNIQUE,
    nama_promo VARCHAR(100) NOT NULL,
    jenis_promo VARCHAR(20) NOT NULL,
    nilai_diskon DECIMAL(10,2) DEFAULT 0.00,
    persentase_diskon INT DEFAULT 0,
    minimal_transaksi DECIMAL(10,2) DEFAULT 0.00,
    tanggal_mulai DATETIME NOT NULL,
    tanggal_berakhir DATETIME NOT NULL,
    status_aktif BOOLEAN DEFAULT TRUE,
    created_at DATETIME DEFAULT CURRENT_TIMESTAMP,
    INDEX idx_promo_kode (kode_promo),
    INDEX idx_promo_tanggal (tanggal_mulai, tanggal_berakhir),
    INDEX idx_promo_status (status_aktif)
);

-- Tabel transaksi
CREATE TABLE transaksi (
    transaksi_id INT AUTO_INCREMENT PRIMARY KEY,
    user_id INT NOT NULL,
    produk_id INT NOT NULL,
    metode_pembayaran_id INT NOT NULL,
    nomor_tujuan VARCHAR(15) NOT NULL,
    subtotal DECIMAL(10,2) NOT NULL,
    biaya_admin DECIMAL(10,2) DEFAULT 0.00,
    total_pembayaran DECIMAL(10,2) NOT NULL,
    status_transaksi VARCHAR(20) NOT NULL DEFAULT 'PENDING',
    kode_transaksi VARCHAR(30) NOT NULL UNIQUE,
    catatan TEXT,
    tanggal_transaksi DATETIME DEFAULT CURRENT_TIMESTAMP,
    created_at DATETIME DEFAULT CURRENT_TIMESTAMP,
    updated_at DATETIME DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    FOREIGN KEY (user_id) REFERENCES user(user_id) ON DELETE CASCADE,
    FOREIGN KEY (produk_id) REFERENCES produk_pulsa(produk_id) ON DELETE CASCADE,
    FOREIGN KEY (metode_pembayaran_id) REFERENCES metode_pembayaran(metode_id) ON DELETE CASCADE,
    INDEX idx_transaksi_user (user_id),
    INDEX idx_transaksi_kode (kode_transaksi),
    INDEX idx_transaksi_status (status_transaksi),
    INDEX idx_transaksi_tanggal (tanggal_transaksi)
);

-- Tabel detail_pembayaran
CREATE TABLE detail_pembayaran (
    detail_id INT AUTO_INCREMENT PRIMARY KEY,
    transaksi_id INT NOT NULL UNIQUE,
    reference_number VARCHAR(50),
    payment_gateway_response TEXT,
    status_pembayaran VARCHAR(20) NOT NULL DEFAULT 'PENDING',
    tanggal_bayar DATETIME,
    expired_at DATETIME,
    keterangan TEXT,
    FOREIGN KEY (transaksi_id) REFERENCES transaksi(transaksi_id) ON DELETE CASCADE,
    INDEX idx_detail_transaksi (transaksi_id),
    INDEX idx_detail_status (status_pembayaran)
);

-- Tabel riwayat_saldo
CREATE TABLE riwayat_saldo (
    riwayat_id INT AUTO_INCREMENT PRIMARY KEY,
    user_id INT NOT NULL,
    transaksi_id INT,
    jenis_transaksi VARCHAR(20) NOT NULL,
    jumlah DECIMAL(15,2) NOT NULL,
    saldo_sebelum DECIMAL(15,2) NOT NULL,
    saldo_sesudah DECIMAL(15,2) NOT NULL,
    keterangan TEXT,
    created_at DATETIME DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (user_id) REFERENCES user(user_id) ON DELETE CASCADE,
    FOREIGN KEY (transaksi_id) REFERENCES transaksi(transaksi_id) ON DELETE SET NULL,
    INDEX idx_riwayat_user (user_id),
    INDEX idx_riwayat_jenis (jenis_transaksi),
    INDEX idx_riwayat_tanggal (created_at)
);

-- Tabel transaksi_promo
CREATE TABLE transaksi_promo (
    transaksi_promo_id INT AUTO_INCREMENT PRIMARY KEY,
    transaksi_id INT NOT NULL,
    promo_id INT NOT NULL,
    nilai_potongan DECIMAL(10,2) NOT NULL,
    created_at DATETIME DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (transaksi_id) REFERENCES transaksi(transaksi_id) ON DELETE CASCADE,
    FOREIGN KEY (promo_id) REFERENCES promo(promo_id) ON DELETE CASCADE,
    INDEX idx_transaksi_promo_transaksi (transaksi_id),
    INDEX idx_transaksi_promo_promo (promo_id)
);

-- Tabel notifikasi
CREATE TABLE notifikasi (
    notifikasi_id INT AUTO_INCREMENT PRIMARY KEY,
    user_id INT NOT NULL,
    transaksi_id INT,
    judul VARCHAR(100) NOT NULL,
    pesan TEXT NOT NULL,
    jenis_notifikasi VARCHAR(20) NOT NULL,
    is_read BOOLEAN DEFAULT FALSE,
    created_at DATETIME DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (user_id) REFERENCES user(user_id) ON DELETE CASCADE,
    FOREIGN KEY (transaksi_id) REFERENCES transaksi(transaksi_id) ON DELETE SET NULL,
    INDEX idx_notifikasi_user (user_id),
    INDEX idx_notifikasi_read (is_read),
    INDEX idx_notifikasi_jenis (jenis_notifikasi)
);



