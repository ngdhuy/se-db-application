# Advance Store Procedure

## 1 - Config database for Website BabyShop

* Import SQL Script file ***db_babyshop.sql*** to create databse ***db_babyshop***

## 2 - Advance Store Procedure

* Get all produces

```sql
CREATE PROCEDURE sp_LayDanhSachSanPham()
BEGIN
    SELECT s.*, l.TenLoaiSanPham, h.TenHangSanXuat
    FROM SanPham s LEFT JOIN LoaiSanPham l ON s.MaLoaiSanPham = l.MaLoaiSanPham LEFT JOIN HangSanXuat h ON s.MaHangSanXuat = h.MaHangSanXuat
    WHERE s.BiXoa = 0
    ORDER BY s.MaSanPham;
END;

-- execute store procedure --
CALL sp_LayDanhSachSanPham;
```

* Get 5 newest products

```sql
CREATE PROCEDURE sp_Lay_05_SachSanPham_MoiNhat()
BEGIN
    SELECT s.*, l.TenLoaiSanPham, h.TenHangSanXuat
    FROM SanPham s LEFT JOIN LoaiSanPham l ON s.MaLoaiSanPham = l.MaLoaiSanPham LEFT JOIN HangSanXuat h ON s.MaHangSanXuat = h.MaHangSanXuat
    WHERE s.BiXoa = 0
    ORDER BY s.NgayNhap DESC
    LIMIT 5;
END;

-- execute store procedure --
CALL sp_Lay_05_SachSanPham_MoiNhat;
```

* Get 5 best sale products

```sql
CREATE PROCEDURE sp_Lay_05_SachSanPham_BanNhieu()
BEGIN
    SELECT s.*, l.TenLoaiSanPham, h.TenHangSanXuat
    FROM SanPham s LEFT JOIN LoaiSanPham l ON s.MaLoaiSanPham = l.MaLoaiSanPham LEFT JOIN HangSanXuat h ON s.MaHangSanXuat = h.MaHangSanXuat
    WHERE s.BiXoa = 0
    ORDER BY s.SoLuongBan DESC
    LIMIT 5;
END;

-- execute store procedure --
CALL sp_Lay_05_SachSanPham_BanNhieu;
```


* Get 5 hottest products

```sql
CREATE PROCEDURE sp_Lay_05_SachSanPham_QuanTam()
BEGIN
    SELECT s.*, l.TenLoaiSanPham, h.TenHangSanXuat
    FROM SanPham s LEFT JOIN LoaiSanPham l ON s.MaLoaiSanPham = l.MaLoaiSanPham LEFT JOIN HangSanXuat h ON s.MaHangSanXuat = h.MaHangSanXuat
    WHERE s.BiXoa = 0
    ORDER BY s.SoLuocXem DESC
    LIMIT 5;
END;

-- execute store procedure --
CALL sp_Lay_05_SachSanPham_QuanTam;
```

* Get product by ID

```sql
CREATE PROCEDURE sp_LayThongTinSanPhamTheoMa(
    IN MaSanPham INT
)
BEGIN
    SELECT s.*, l.TenLoaiSanPham, h.TenHangSanXuat
    FROM SanPham s LEFT JOIN LoaiSanPham l ON s.MaLoaiSanPham = l.MaLoaiSanPham LEFT JOIN HangSanXuat h ON s.MaHangSanXuat = h.MaHangSanXuat
    WHERE s.MaSanPham = MaSanPham AND s.BiXoa = 0;;
END;

-- execute store procedure --
CALL sp_LayThongTinSanPhamTheoMa(5);
```

* Get product by categories

```sql
CREATE PROCEDURE sp_LayDanhSachSanPhamTheoLoai(
    IN MaLoaiSanPham INT
)
BEGIN
    SELECT s.*, l.TenLoaiSanPham, h.TenHangSanXuat
    FROM SanPham s LEFT JOIN LoaiSanPham l ON s.MaLoaiSanPham = l.MaLoaiSanPham LEFT JOIN HangSanXuat h ON s.MaHangSanXuat = h.MaHangSanXuat
    WHERE s.MaLoaiSanPham = MaLoaiSanPham AND s.BiXoa = 0;
END;

-- execute store procedure --
CALL sp_LayDanhSachSanPhamTheoLoai(1);
```

* Get product by brand

```sql
CREATE PROCEDURE sp_LayDanhSachSanPhamTheoHang(
    IN MaHangSanXuat INT
)
BEGIN
    SELECT s.*, l.TenLoaiSanPham, h.TenHangSanXuat
    FROM SanPham s LEFT JOIN LoaiSanPham l ON s.MaLoaiSanPham = l.MaLoaiSanPham LEFT JOIN HangSanXuat h ON s.MaHangSanXuat = h.MaHangSanXuat
    WHERE s.MaHangSanXuat = MaHangSanXuat AND s.BiXoa = 0;
END;

-- execute store procedure --
CALL sp_LayDanhSachSanPhamTheoHang(1);
```

* Get account which don't have transaction

```sql
CREATE PROCEDURE sp_LayDanhSachCacTaiKhoanChuaThucHienGiaoDich()
BEGIN
    SET @ds_taikhoan_cogiaodich := (SELECT t.MaTaiKhoan FROM TaiKhoan t INNER JOIN DonDatHang d ON t.MaTaiKhoan = d.MaTaiKhoan WHERE t.BiXoa = 0 GROUP BY t.MaTaiKhoan);
    SELECT * FROM TaiKhoan WHERE MaTaiKhoan NOT IN (@ds_taikhoan_cogiaodich);
END;

-- execute store procedure --
CALL sp_LayDanhSachCacTaiKhoanChuaThucHienGiaoDich;
```

## 3 - Trigger

Syntax of trigger

```sql
CREATE TRIGGER trigger_name trigger_time trigger_event
ON table_name
FOR EACH ROW
BEGIN
 ...
END
```

* ***trigger_name*** is name of trigger
* ***trigger_time*** is the time to active trigger
    * ***BEFORE*** set execute trigger before data changed
    * ***AFTER*** set execute trigger after data changed
* ***trigger_event*** is follow event to change data such as INSERT, UPDATE, DELETE
* Keyword ***OLD*** is reference data before changed.
* Keyword ***NEW*** is reference data after changed.

First example: create trigger to insert ***NgayLap, TongThanhTien, MaTinhTrang*** before insert data to table

```sql
DROP TRIGGER IF EXISTS `trg_after_insert_dondathang`;
CREATE TRIGGER `trg_after_insert_dondathang` BEFORE INSERT ON `DonDatHang`
FOR EACH ROW
BEGIN
    SET NEW.NgayLap = NOW();
    SET NEW.TongThanhTien = 0;
    SET NEW.MaTinhTrang = 1;
END;

-----------------------------------------------

SELECT * FROM DonDatHang;

INSERT INTO DonDatHang(MaDonDatHang, MaTaiKhoan) VALUES ('240911002', 1);

SELECT * FROM DonDatHang;
```

Second example: when insert product information to ***ChiTietDonHang***, using trigger to update ***TongThanhTien*** with ***TongThanhTien += GiaSanPham * SoLuong***

```sql
DROP TRIGGER IF EXISTS `trg_after_insert_chitietdondathang`;
CREATE TRIGGER `trg_after_insert_chitietdondathang` AFTER INSERT ON `ChiTietDonDatHang`
FOR EACH ROW
BEGIN
    UPDATE DonDatHang
    SET TongThanhTien = TongThanhTien + (NEW.GiaBan * NEW.SoLuong)
    WHERE MaDonDatHang = NEW.MaDonDatHang;
END;

-----------------------------------------------

SELECT * FROM DonDatHang;

INSERT INTO ChiTietDonDatHang(MaChiTietDonDatHang, MaDonDatHang, MaSanPham, SoLuong, GiaBan) VALUES ('24091100101', '240911001', 9, 2, 380000);

SELECT * FROM DonDatHang;

SELECT * FROM ChiTietDonDatHang;
```

> Note: INSERT event have only NEW data

Third example: when update ***SoLuong*** on ***ChiTietDonHang***, using trigger to update ***TongThanhTien***

```sql
DROP TRIGGER IF EXISTS `trg_after_update_chitietdondathang`;
CREATE TRIGGER `trg_after_update_chitietdondathang` AFTER UPDATE ON `ChiTietDonDatHang`
FOR EACH ROW
BEGIN
    UPDATE DonDatHang
    SET TongThanhTien = TongThanhTien + (NEW.GiaBan * (NEW.SoLuong - OLD.SoLuong))
    WHERE MaDonDatHang = NEW.MaDonDatHang;
END;

-----------------------------------------------

SELECT * FROM DonDatHang;

UPDATE ChiTietDonDatHang
SET SoLuong = 5
WHERE MaChiTietDonDatHang = '24091100101';

SELECT * FROM DonDatHang;
```

Four example: when delete ***ChiTietDonHang***, using trigger to update ***TongThanhTien***

```sql
DROP TRIGGER IF EXISTS `trg_after_delete_chitietdondathang`;
CREATE TRIGGER `trg_after_delete_chitietdondathang` AFTER DELETE ON `ChiTietDonDatHang`
FOR EACH ROW
BEGIN
    UPDATE DonDatHang
    SET TongThanhTien = TongThanhTien - (OLD.GiaBan * OLD.SoLuong)
    WHERE MaDonDatHang = OLD.MaDonDatHang;
END;

-----------------------------------------------

SELECT * FROM DonDatHang;

DELETE FROM ChiTietDonDatHang
WHERE MaChiTietDonDatHang = '24091100101';

SELECT * FROM DonDatHang;
```

> Note: DELETE event have only OLD data