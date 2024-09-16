# View in MySQL

* In SQL, a View is a virtual table based on the result-set of an SQL Statement. View contains rows and columns, just like a real table. The field in a view are fields from one or more real tables in the database.

* Create View Syntax

```sql
CREATE VIEW view_name AS
SELECT column1, column2, ...
FROM table_name
WHERE condition;
```

* Using View to show the full information of Product

```sql
CREATE 	VIEW HangSanXuat_Lamaze AS
SELECT s.MaSanPham, s.TenSanPham, s.HinhURL, s.GiaSanPham, s.NgayNhap, s.SoLuongTon, s.SoLuongBan, s.SoLuocXem, s.MoTa, s.BiXoa, s.MaLoaiSanPham, s.MaHangSanXuat, h.TenHangSanXuat
FROM SanPham s LEFT JOIN HangSanXuat h ON s.MaHangSanXuat = h.MaHangSanXuat
WHERE h.TenHangSanXuat = 'Lamaze';
-------------------------------------------------------------------
SELECT * FROM HangSanXuat_Lamaze;
```

* Edit view to get short information for products

```sql
CREATE OR REPLACE VIEW HangSanXuat_Lamaze AS
SELECT s.MaSanPham, s.TenSanPham, s.GiaSanPham, h.TenHangSanXuat, l.TenLoaiSanPham
FROM SanPham s INNER JOIN HangSanXuat h ON s.MaHangSanXuat = h.MaHangSanXuat INNER JOIN LoaiSanPham l ON s.MaLoaiSanPHam = l.MaLoaiSanPham
WHERE h.TenHangSanXuat = 'Lamaze';
-------------------------------------------------------------------
SELECT * FROM HangSanXuat_Lamaze;
```

* DROP a View

```sql
DROP VIEW HangSanXuat_Lamaze;
```