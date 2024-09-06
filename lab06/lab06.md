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
    ORDER BY s.SoLuocXem DESC
    LIMIT 5;
END;

-- execute store procedure --
CALL sp_Lay_05_SachSanPham_QuanTam;
```

