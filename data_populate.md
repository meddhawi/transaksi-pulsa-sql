-- ========================================
-- INSERT DATA SAMPLE (5-10 RECORDS PER TABEL)
-- ========================================

```sql
-- 1. Insert data PROVIDER (5 records)
INSERT INTO provider (nama_provider, kode_provider, logo_url, status_aktif) VALUES
('Telkomsel', 'TSEL', 'https://example.com/tsel.png', TRUE),
('Indosat', 'ISAT', 'https://example.com/isat.png', TRUE),
('XL Axiata', 'XL', 'https://example.com/xl.png', TRUE),
('Tri', 'TRI', 'https://example.com/tri.png', TRUE),
('Smartfren', 'SMART', 'https://example.com/smart.png', TRUE);

-- 2. Insert data METODE_PEMBAYARAN (6 records)
INSERT INTO metode_pembayaran (nama_metode, jenis_metode, kode_metode, logo_url, biaya_admin, status_aktif) VALUES
('Saldo Wallet', 'WALLET', 'WALLET', 'https://example.com/wallet.png', 0.00, TRUE),
('Bank BCA', 'BANK', 'BCA', 'https://example.com/bca.png', 2500.00, TRUE),
('Bank Mandiri', 'BANK', 'MANDIRI', 'https://example.com/mandiri.png', 2500.00, TRUE),
('Bank BRI', 'BANK', 'BRI', 'https://example.com/bri.png', 2500.00, TRUE),
('OVO', 'E_WALLET', 'OVO', 'https://example.com/ovo.png', 1000.00, TRUE),
('GoPay', 'E_WALLET', 'GOPAY', 'https://example.com/gopay.png', 1000.00, TRUE);

-- 3. Insert data USER (8 records)
INSERT INTO user (nama, nomor_hp, email, password, saldo_wallet) VALUES
('Ahmad Rizki', '081234567890', 'ahmad.rizki@email.com', 'hashed_password_1', 250000.00),
('Siti Nurhaliza', '081234567891', 'siti.nurhaliza@email.com', 'hashed_password_2', 100000.00),
('Budi Santoso', '081234567892', 'budi.santoso@email.com', 'hashed_password_3', 75000.00),
('Dewi Lestari', '081234567893', 'dewi.lestari@email.com', 'hashed_password_4', 500000.00),
('Eko Prasetyo', '081234567894', 'eko.prasetyo@email.com', 'hashed_password_5', 0.00),
('Fitri Handayani', '081234567895', 'fitri.handayani@email.com', 'hashed_password_6', 150000.00),
('Galih Permana', '081234567896', 'galih.permana@email.com', 'hashed_password_7', 300000.00),
('Hani Rahmawati', '081234567897', 'hani.rahmawati@email.com', 'hashed_password_8', 25000.00);

-- 4. Insert data PRODUK_PULSA (10 records)
INSERT INTO produk_pulsa (provider_id, nominal, harga_normal, harga_promo, masa_aktif_hari, status_promo, persentase_diskon, deskripsi, status_aktif) VALUES
-- Telkomsel
(1, 5000, 6000.00, 5500.00, 30, TRUE, 8, 'Pulsa Telkomsel 5K', TRUE),
(1, 10000, 11000.00, 10500.00, 30, TRUE, 5, 'Pulsa Telkomsel 10K', TRUE),
(1, 25000, 26000.00, NULL, 30, FALSE, 0, 'Pulsa Telkomsel 25K', TRUE),
(1, 50000, 51000.00, NULL, 30, FALSE, 0, 'Pulsa Telkomsel 50K', TRUE),
-- Indosat
(2, 5000, 5800.00, 5500.00, 30, TRUE, 5, 'Pulsa Indosat 5K', TRUE),
(2, 10000, 10800.00, NULL, 30, FALSE, 0, 'Pulsa Indosat 10K', TRUE),
(2, 25000, 25500.00, 24000.00, 30, TRUE, 6, 'Pulsa Indosat 25K', TRUE),
-- XL Axiata
(3, 5000, 5900.00, NULL, 30, FALSE, 0, 'Pulsa XL 5K', TRUE),
(3, 10000, 10900.00, 10200.00, 30, TRUE, 6, 'Pulsa XL 10K', TRUE),
(3, 25000, 25800.00, NULL, 30, FALSE, 0, 'Pulsa XL 25K', TRUE);

-- 5. Insert data PROMO (7 records)
INSERT INTO promo (kode_promo, nama_promo, jenis_promo, nilai_diskon, persentase_diskon, minimal_transaksi, tanggal_mulai, tanggal_berakhir, status_aktif) VALUES
('NEWUSER10', 'Promo User Baru', 'PERSENTASE', 0.00, 10, 50000.00, '2024-01-01 00:00:00', '2024-12-31 23:59:59', TRUE),
('CASHBACK5K', 'Cashback 5K', 'NOMINAL', 5000.00, 0, 25000.00, '2024-06-01 00:00:00', '2024-12-31 23:59:59', TRUE),
('WEEKEND15', 'Weekend Special', 'PERSENTASE', 0.00, 15, 100000.00, '2024-01-01 00:00:00', '2024-12-31 23:59:59', TRUE),
('FLASH50', 'Flash Sale 50%', 'PERSENTASE', 0.00, 50, 10000.00, '2024-07-01 00:00:00', '2024-07-31 23:59:59', FALSE),
('TOPUP3K', 'Bonus Top Up', 'NOMINAL', 3000.00, 0, 50000.00, '2024-01-01 00:00:00', '2024-12-31 23:59:59', TRUE),
('LOYAL20', 'Member Loyal', 'PERSENTASE', 0.00, 20, 200000.00, '2024-01-01 00:00:00', '2024-12-31 23:59:59', TRUE),
('EXPIRED', 'Promo Kadaluarsa', 'NOMINAL', 10000.00, 0, 25000.00, '2023-01-01 00:00:00', '2023-12-31 23:59:59', FALSE);

-- 6. Insert data TRANSAKSI (9 records)
INSERT INTO transaksi (user_id, produk_id, metode_pembayaran_id, nomor_tujuan, subtotal, biaya_admin, total_pembayaran, status_transaksi, kode_transaksi, catatan, tanggal_transaksi) VALUES
(1, 1, 1, '081234567890', 5500.00, 0.00, 5500.00, 'SUCCESS', 'TRX202401001', 'Pembelian pulsa berhasil', '2024-01-15 10:30:00'),
(2, 3, 2, '081234567891', 26000.00, 2500.00, 28500.00, 'SUCCESS', 'TRX202401002', 'Transfer bank BCA', '2024-01-16 14:20:00'),
(3, 5, 3, '081234567892', 5500.00, 2500.00, 8000.00, 'PENDING', 'TRX202401003', 'Menunggu konfirmasi bank', '2024-01-17 09:15:00'),
(4, 2, 1, '081234567893', 10500.00, 0.00, 10500.00, 'SUCCESS', 'TRX202401004', 'Saldo wallet mencukupi', '2024-01-18 16:45:00'),
(1, 7, 5, '081234567890', 24000.00, 1000.00, 25000.00, 'SUCCESS', 'TRX202401005', 'Pembayaran OVO berhasil', '2024-01-19 11:30:00'),
(5, 1, 6, '081234567894', 5500.00, 1000.00, 6500.00, 'FAILED', 'TRX202401006', 'Saldo GoPay tidak mencukupi', '2024-01-20 13:20:00'),
(6, 9, 1, '081234567895', 10200.00, 0.00, 10200.00, 'SUCCESS', 'TRX202401007', 'Promo discount applied', '2024-01-21 08:10:00'),
(7, 4, 2, '081234567896', 51000.00, 2500.00, 53500.00, 'PENDING', 'TRX202401008', 'Transfer bank dalam proses', '2024-01-22 15:40:00'),
(8, 6, 1, '081234567897', 10800.00, 0.00, 10800.00, 'SUCCESS', 'TRX202401009', 'Top up pulsa Indosat', '2024-01-23 12:25:00');

-- 7. Insert data DETAIL_PEMBAYARAN (9 records - sesuai dengan transaksi)
INSERT INTO detail_pembayaran (transaksi_id, reference_number, payment_gateway_response, status_pembayaran, tanggal_bayar, expired_at, keterangan) VALUES
(1, 'REF001234567', '{"status": "success", "code": "00"}', 'PAID', '2024-01-15 10:31:00', NULL, 'Pembayaran wallet berhasil'),
(2, 'REF001234568', '{"status": "success", "code": "00"}', 'PAID', '2024-01-16 14:25:00', NULL, 'Transfer BCA berhasil'),
(3, 'REF001234569', '{"status": "pending", "code": "01"}', 'PENDING', NULL, '2024-01-17 21:15:00', 'Menunggu konfirmasi bank'),
(4, 'REF001234570', '{"status": "success", "code": "00"}', 'PAID', '2024-01-18 16:46:00', NULL, 'Pembayaran wallet berhasil'),
(5, 'REF001234571', '{"status": "success", "code": "00"}', 'PAID', '2024-01-19 11:32:00', NULL, 'Pembayaran OVO berhasil'),
(6, 'REF001234572', '{"status": "failed", "code": "05"}', 'FAILED', NULL, '2024-01-20 14:20:00', 'Saldo tidak mencukupi'),
(7, 'REF001234573', '{"status": "success", "code": "00"}', 'PAID', '2024-01-21 08:11:00', NULL, 'Pembayaran wallet berhasil'),
(8, 'REF001234574', '{"status": "pending", "code": "01"}', 'PENDING', NULL, '2024-01-23 03:40:00', 'Menunggu konfirmasi bank'),
(9, 'REF001234575', '{"status": "success", "code": "00"}', 'PAID', '2024-01-23 12:26:00', NULL, 'Pembayaran wallet berhasil');

-- 8. Insert data RIWAYAT_SALDO (6 records)
INSERT INTO riwayat_saldo (user_id, transaksi_id, jenis_transaksi, jumlah, saldo_sebelum, saldo_sesudah, keterangan) VALUES
(1, 1, 'DEBIT', -5500.00, 250000.00, 244500.00, 'Pembelian pulsa Telkomsel 5K'),
(1, NULL, 'KREDIT', 100000.00, 244500.00, 344500.00, 'Top up saldo dari bank'),
(4, 4, 'DEBIT', -10500.00, 500000.00, 489500.00, 'Pembelian pulsa Telkomsel 10K'),
(1, 5, 'DEBIT', -25000.00, 344500.00, 319500.00, 'Pembelian pulsa Indosat 25K via OVO'),
(6, 7, 'DEBIT', -10200.00, 150000.00, 139800.00, 'Pembelian pulsa XL 10K'),
(8, 9, 'DEBIT', -10800.00, 25000.00, 14200.00, 'Pembelian pulsa Indosat 10K');

-- 9. Insert data TRANSAKSI_PROMO (5 records)
INSERT INTO transaksi_promo (transaksi_id, promo_id, nilai_potongan) VALUES
(1, 1, 550.00),  -- NEWUSER10: 10% dari 5500
(5, 2, 5000.00), -- CASHBACK5K: cashback nominal
(7, 3, 1530.00), -- WEEKEND15: 15% dari 10200
(4, 5, 3000.00), -- TOPUP3K: bonus nominal
(2, 6, 5200.00); -- LOYAL20: 20% dari 26000

-- 10. Insert data NOTIFIKASI (8 records)
INSERT INTO notifikasi (user_id, transaksi_id, judul, pesan, jenis_notifikasi, is_read) VALUES
(1, 1, 'Transaksi Berhasil', 'Pulsa Telkomsel 5K telah berhasil dikirim ke 081234567890', 'TRANSAKSI', TRUE),
(2, 2, 'Transaksi Berhasil', 'Pulsa Telkomsel 25K telah berhasil dikirim ke 081234567891', 'TRANSAKSI', FALSE),
(3, 3, 'Transaksi Pending', 'Transaksi Anda sedang diproses, silakan tunggu konfirmasi', 'TRANSAKSI', FALSE),
(4, 4, 'Transaksi Berhasil', 'Pulsa Telkomsel 10K telah berhasil dikirim ke 081234567893', 'TRANSAKSI', TRUE),
(1, 5, 'Transaksi Berhasil', 'Pulsa Indosat 25K telah berhasil dikirim ke 081234567890', 'TRANSAKSI', TRUE),
(5, 6, 'Transaksi Gagal', 'Transaksi Anda gagal diproses. Saldo tidak mencukupi', 'TRANSAKSI', FALSE),
(6, 7, 'Transaksi Berhasil', 'Pulsa XL 10K telah berhasil dikirim ke 081234567895', 'TRANSAKSI', FALSE),
(1, NULL, 'Promo Tersedia', 'Dapatkan diskon 10% untuk pembelian pulsa pertama Anda!', 'PROMO', TRUE);

```