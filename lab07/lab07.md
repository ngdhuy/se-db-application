# View in MySQL

## 1 - Basic VIEW syntax

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

## 2 - Advance VIEW implementation

* Cannot create View with parameter because View is stored static data after execute query

* Example: create VIEW too count the number of Order

```sql
CREATE OR REPLACE VIEW v_DemTongDonDatHang AS
SELECT COUNT(MaDonDatHang) AS Total
FROM DonDatHang;

```

* SELECT first-time is 7 orders

```sql
SELECT * FROM v_DemTongDonDatHang;
```

* After that, we create new Orders

```sql
INSERT INTO DonDatHang(MaDonDatHang, NgayLap, TongThanhTien, MaTaiKhoan, MaTinhTrang) 
VALUES ('240916001', NOW(), 1000, 1, 1);
```

* And, select the second-time is 8 orders. So, the View is auto update when have request to select from view.

```sql
SELECT * FROM v_DemTongDonDatHang;
```

* Now, we can use View for caching common data such as:
    
    * Show top 5 product latest with view ***v_product_latest***
    * Shop top 5 best seller with view ***v_product_best_seller***
    * Shop top 5 hottest  with view ***v_product_hottest***

```sql
CREATE OR REPLACE VIEW v_product_latest AS
SELECT s.*
FROM SanPham s
ORDER BY s.NgayNhap DESC
LIMIT 5;

-------------------------------------------------------------------

CREATE OR REPLACE VIEW v_product_best_seller AS
SELECT s.*
FROM SanPham s
ORDER BY s.SoLuongBan DESC
LIMIT 5;

-------------------------------------------------------------------

CREATE OR REPLACE VIEW v_product_hottest AS
SELECT s.*
FROM SanPham s
ORDER BY s.SoLuocXem DESC
LIMIT 5;
```