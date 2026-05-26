# Bao cao du lieu test store xuat nhap ton theo thang

## Thong tin chung

| Noi dung | Gia tri |
|---|---|
| Store | `dbo.sp_BAOCAOXUATNHAPTON_THEOTHANG_GetListPaging` |
| DonViId | `06F58867-F9A1-4F3D-84A9-7C78352ED824` |
| PhongBanId / PhanXuongId | `12F42560-B952-4B52-9A7A-86A18E9C5FCC` |
| Marker | `CBT-XNT-TEST-20260526-DV06` |
| Thang test | 04/2026, 05/2026, 06/2026 |
| Loai than | 4 loai than co san trong `DM_LOAITHAN` |

## Logic store can bam

| Nhom chi tieu bao cao | Nguon trong store | Dieu kien loc |
|---|---|---|
| Ton dau ky | `BAOCAOXUATNHAPTON_THEOTHANG` thang lien truoc | Cung `DonViId`, `PhongBanId`, lay `TonCuoiKy`, `TonCuoiKy_AK`, `TonCuoiKy_Vk` |
| Nhap mua | `PHIEUNHAPKHOTHAN_01VT` + `PHIEUNHAPKHOTHAN_01VT_CHITIET` | Cong `SoLuongThucNhap`; binh quan AK/V theo san luong |
| Nhap noi bo | `PHIEUNHAPKHOTHAN_01VT` join `LENHNHAPKHOTHANNOIBO` | Co `LenhNhapKhoThanId`; cong `SoLuongTheoAmThucTe`; trong seed dat `SoLuongThucNhap = 0` de khong cong trung nhap mua |
| Nhap pha tron | `PHIEUNHAPKHO` + `PHIEUNHAPKHO_HANGHOA` | `PhanXuongId = @iPhongBanId`, `IsCheBien = 0` |
| Xuat ban | `PHIEUXUATKHOTHAN_02VT` + `PHIEUXUATKHOTHAN_02VT_CHITIET` | Cong `SoLuongThucXuat`; binh quan AK/QK/V theo san luong |
| Xuat pha tron | `PHIEUXUATKHO` + `PHIEUXUATKHO_HANGHOA` | `PhanXuongId = @iPhongBanId` |
| Trang thai grid | `ALL_TICKETS` va ton tai `BAOCAOXUATNHAPTON_THEOTHANG` ky hien tai | Seed dat tat ca phieu `IsChotSo = 1`, bao cao hien tai `NguoiLap_IsXacNhan = 1` |

## Loai than dung de test

| Ma test | LoaiThanId | Ma | Ten goi |
|---|---|---|---|
| LT-A | `B4262ED2-78A6-4DDC-B812-8B55518EDCA0` | `10` | Cam 1 |
| LT-B | `663ED966-D27F-4FE4-A446-2936318E628E` | `11` | Cam 2a.1 |
| LT-C | `F334EFE3-ECC1-40F3-AE3E-1ADD75C87929` | `12` | Cam 3a.1 |
| LT-D | `0EA85BA9-1B11-4F12-A65A-78DC5582B883` | `128` | Bun tuyen 3C |

## Du lieu bao cao thang truoc va hien tai se them

| Ky bao cao | Loai than | TonCuoiKy | TonCuoiKy_AK | TonCuoiKy_Vk | Muc dich |
|---|---|---:|---:|---:|---|
| 03/2026 | LT-A | 150 | 10 | 5 | Ton dau ky cho 04/2026 |
| 03/2026 | LT-B | 120 | 12 | 6 | Ton dau ky cho 04/2026 |
| 03/2026 | LT-C | 90 | 11 | 4.5 | Ton dau ky cho 04/2026 |
| 03/2026 | LT-D | 70 | 13 | 7 | Ton dau ky cho 04/2026 |
| 04/2026 | LT-A | 201 | 9.710945 | 4.411940 | Bao cao hien tai va ton dau ky cho 05/2026 |
| 04/2026 | LT-B | 158 | 12.241772 | 5.587975 | Bao cao hien tai va ton dau ky cho 05/2026 |
| 04/2026 | LT-C | 120 | 10.846667 | 4.172500 | Bao cao hien tai va ton dau ky cho 05/2026 |
| 04/2026 | LT-D | 93 | 13.209677 | 6.430108 | Bao cao hien tai va ton dau ky cho 05/2026 |
| 05/2026 | LT-A | 248 | 9.622984 | 4.091935 | Bao cao hien tai va ton dau ky cho 06/2026 |
| 05/2026 | LT-B | 198 | 12.337374 | 5.202525 | Bao cao hien tai va ton dau ky cho 06/2026 |
| 05/2026 | LT-C | 152 | 10.845395 | 4.012500 | Bao cao hien tai va ton dau ky cho 06/2026 |
| 05/2026 | LT-D | 118 | 13.300847 | 5.993220 | Bao cao hien tai va ton dau ky cho 06/2026 |
| 06/2026 | LT-A | 305 | 9.419344 | 3.788525 | Bao cao hien tai de store set `Id`, `IsXacNhanGrid` |
| 06/2026 | LT-B | 239 | 12.463180 | 4.960251 | Bao cao hien tai de store set `Id`, `IsXacNhanGrid` |
| 06/2026 | LT-C | 187 | 10.740107 | 3.808021 | Bao cao hien tai de store set `Id`, `IsXacNhanGrid` |
| 06/2026 | LT-D | 144 | 13.465972 | 5.741667 | Bao cao hien tai de store set `Id`, `IsXacNhanGrid` |

## Bang du lieu phieu se them

Moi thang co 5 dau phieu/lenh:

| Bang | Mau so phieu | Vai tro trong store |
|---|---|---|
| `LENHNHAPKHOTHANNOIBO` | `CBT-XNT-TEST-20260526-DV06-yyyyMM-LENH-NB` | Kich hoat nhanh `NhapNoiBo` khi join voi 01VT |
| `PHIEUNHAPKHOTHAN_01VT` | `...-PN01-MUA` | Tao `NhapMua`, `NhapMua_AK`, `NhapMua_Vk` |
| `PHIEUNHAPKHOTHAN_01VT` | `...-PN01-NB` | Tao `NhapNoiBo`, `NhapNoiBo_AK` |
| `PHIEUNHAPKHO` | `...-PNPT` | Tao `NhapPhaTron`, `NhapPhaTron_AK` |
| `PHIEUXUATKHOTHAN_02VT` | `...-PX02` | Tao `XuatBan`, `XuatBan_AK`, `XuatBan_Qk`, `XuatBan_Vk` |
| `PHIEUXUATKHO` | `...-PXPT` | Tao `XuatPhaTron`, `XuatPhaTron_AK` |

| Ky | Loai than | NhapMua | NhapMua_AK | NhapMua_Vk | NhapNoiBo | NhapNoiBo_AK | NhapPhaTron | NhapPhaTron_AK | XuatBan | XuatBan_AK | XuatBan_Qk | XuatBan_Vk | XuatPhaTron | XuatPhaTron_AK |
|---|---|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|
| 04/2026 | LT-A | 60 | 8.5 | 4.2 | 18 | 10.8 | 12 | 9.1 | 32 | 9.4 | 5050 | 3.6 | 7 | 8.7 |
| 04/2026 | LT-B | 45 | 12.4 | 6.5 | 14 | 13.1 | 9 | 11.8 | 24 | 12 | 4900 | 5.4 | 6 | 10.9 |
| 04/2026 | LT-C | 35 | 10.2 | 5.1 | 10 | 11.4 | 7 | 10.6 | 18 | 10.8 | 4800 | 4.6 | 4 | 9.8 |
| 04/2026 | LT-D | 25 | 13.5 | 7.2 | 8 | 14 | 5 | 12.5 | 12 | 13.2 | 4700 | 6 | 3 | 11.7 |
| 05/2026 | LT-A | 55 | 8.8 | 4.4 | 16 | 10.2 | 14 | 9.4 | 30 | 9.1 | 5080 | 3.8 | 8 | 8.9 |
| 05/2026 | LT-B | 48 | 12.1 | 6.1 | 15 | 12.9 | 10 | 11.6 | 26 | 11.8 | 4920 | 5.6 | 7 | 10.7 |
| 05/2026 | LT-C | 38 | 10.5 | 5.4 | 11 | 11.2 | 8 | 10.9 | 20 | 10.6 | 4820 | 4.8 | 5 | 10.1 |
| 05/2026 | LT-D | 28 | 13.2 | 7 | 9 | 13.8 | 6 | 12.8 | 14 | 13 | 4720 | 6.2 | 4 | 11.9 |
| 06/2026 | LT-A | 65 | 8.2 | 4.1 | 20 | 10.6 | 15 | 9 | 34 | 9.3 | 5120 | 3.7 | 9 | 8.6 |
| 06/2026 | LT-B | 50 | 12.5 | 6.3 | 16 | 13 | 11 | 11.9 | 28 | 12.2 | 4950 | 5.7 | 8 | 10.8 |
| 06/2026 | LT-C | 42 | 10.1 | 5 | 12 | 11.6 | 9 | 10.7 | 22 | 10.9 | 4850 | 4.9 | 6 | 10 |
| 06/2026 | LT-D | 30 | 13.6 | 7.4 | 10 | 14.2 | 7 | 12.6 | 16 | 13.1 | 4750 | 6.4 | 5 | 11.8 |

## Uoc tinh ket qua khi store tong hop

| Ky | Loai than | TonDauKy | TonDauKy_AK | TonDauKy_Vk | NhapMua | NhapMua_AK | NhapMua_Vk | NhapNoiBo | NhapNoiBo_AK | NhapPhaTron | NhapPhaTron_AK | TongNhap | TongNhap_AK | XuatBan | XuatBan_AK | XuatBan_Qk | XuatBan_Vk | XuatPhaTron | XuatPhaTron_AK | TongXuat | TongXuat_AK | TonCuoiKy | TonCuoiKy_AK | TonCuoiKy_Vk |
|---|---|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|
| 04/2026 | LT-A | 150.000000 | 10.000000 | 5.000000 | 60.000000 | 8.500000 | 4.200000 | 18.000000 | 10.800000 | 12.000000 | 9.100000 | 90.000000 | 9.040000 | 32.000000 | 9.400000 | 5050.000000 | 3.600000 | 7.000000 | 8.700000 | 39.000000 | 9.274359 | 201.000000 | 9.710945 | 4.411940 |
| 04/2026 | LT-B | 120.000000 | 12.000000 | 6.000000 | 45.000000 | 12.400000 | 6.500000 | 14.000000 | 13.100000 | 9.000000 | 11.800000 | 68.000000 | 12.464706 | 24.000000 | 12.000000 | 4900.000000 | 5.400000 | 6.000000 | 10.900000 | 30.000000 | 11.780000 | 158.000000 | 12.241772 | 5.587975 |
| 04/2026 | LT-C | 90.000000 | 11.000000 | 4.500000 | 35.000000 | 10.200000 | 5.100000 | 10.000000 | 11.400000 | 7.000000 | 10.600000 | 52.000000 | 10.484615 | 18.000000 | 10.800000 | 4800.000000 | 4.600000 | 4.000000 | 9.800000 | 22.000000 | 10.618182 | 120.000000 | 10.846667 | 4.172500 |
| 04/2026 | LT-D | 70.000000 | 13.000000 | 7.000000 | 25.000000 | 13.500000 | 7.200000 | 8.000000 | 14.000000 | 5.000000 | 12.500000 | 38.000000 | 13.473684 | 12.000000 | 13.200000 | 4700.000000 | 6.000000 | 3.000000 | 11.700000 | 15.000000 | 12.900000 | 93.000000 | 13.209677 | 6.430108 |
| 05/2026 | LT-A | 201.000000 | 9.710945 | 4.411940 | 55.000000 | 8.800000 | 4.400000 | 16.000000 | 10.200000 | 14.000000 | 9.400000 | 85.000000 | 9.162353 | 30.000000 | 9.100000 | 5080.000000 | 3.800000 | 8.000000 | 8.900000 | 38.000000 | 9.057895 | 248.000000 | 9.622984 | 4.091935 |
| 05/2026 | LT-B | 158.000000 | 12.241772 | 5.587975 | 48.000000 | 12.100000 | 6.100000 | 15.000000 | 12.900000 | 10.000000 | 11.600000 | 73.000000 | 12.195890 | 26.000000 | 11.800000 | 4920.000000 | 5.600000 | 7.000000 | 10.700000 | 33.000000 | 11.566667 | 198.000000 | 12.337374 | 5.202525 |
| 05/2026 | LT-C | 120.000000 | 10.846667 | 4.172500 | 38.000000 | 10.500000 | 5.400000 | 11.000000 | 11.200000 | 8.000000 | 10.900000 | 57.000000 | 10.691228 | 20.000000 | 10.600000 | 4820.000000 | 4.800000 | 5.000000 | 10.100000 | 25.000000 | 10.500000 | 152.000000 | 10.845395 | 4.012500 |
| 05/2026 | LT-D | 93.000000 | 13.209677 | 6.430108 | 28.000000 | 13.200000 | 7.000000 | 9.000000 | 13.800000 | 6.000000 | 12.800000 | 43.000000 | 13.269767 | 14.000000 | 13.000000 | 4720.000000 | 6.200000 | 4.000000 | 11.900000 | 18.000000 | 12.755556 | 118.000000 | 13.300847 | 5.993220 |
| 06/2026 | LT-A | 248.000000 | 9.622984 | 4.091935 | 65.000000 | 8.200000 | 4.100000 | 20.000000 | 10.600000 | 15.000000 | 9.000000 | 100.000000 | 8.800000 | 34.000000 | 9.300000 | 5120.000000 | 3.700000 | 9.000000 | 8.600000 | 43.000000 | 9.153488 | 305.000000 | 9.419344 | 3.788525 |
| 06/2026 | LT-B | 198.000000 | 12.337374 | 5.202525 | 50.000000 | 12.500000 | 6.300000 | 16.000000 | 13.000000 | 11.000000 | 11.900000 | 77.000000 | 12.518182 | 28.000000 | 12.200000 | 4950.000000 | 5.700000 | 8.000000 | 10.800000 | 36.000000 | 11.888889 | 239.000000 | 12.463180 | 4.960251 |
| 06/2026 | LT-C | 152.000000 | 10.845395 | 4.012500 | 42.000000 | 10.100000 | 5.000000 | 12.000000 | 11.600000 | 9.000000 | 10.700000 | 63.000000 | 10.471429 | 22.000000 | 10.900000 | 4850.000000 | 4.900000 | 6.000000 | 10.000000 | 28.000000 | 10.707143 | 187.000000 | 10.740107 | 3.808021 |
| 06/2026 | LT-D | 118.000000 | 13.300847 | 5.993220 | 30.000000 | 13.600000 | 7.400000 | 10.000000 | 14.200000 | 7.000000 | 12.600000 | 47.000000 | 13.578723 | 16.000000 | 13.100000 | 4750.000000 | 6.400000 | 5.000000 | 11.800000 | 21.000000 | 12.790476 | 144.000000 | 13.465972 | 5.741667 |

## Script tao du lieu

Chay file:

`seed_BAOCAOXUATNHAPTON_THEOTHANG_CBT_XNT_20260526.sql`

Sau khi insert, test store voi `DonViId = 06F58867-F9A1-4F3D-84A9-7C78352ED824`, `PhongBanId = 12F42560-B952-4B52-9A7A-86A18E9C5FCC`, cac ky:

- `2026-04-01` den `2026-04-30`
- `2026-05-01` den `2026-05-31`
- `2026-06-01` den `2026-06-30`



# 📑 BÁO CÁO PHÂN TÍCH LOGIC DỮ LIỆU & CÔNG THỨC STORED PROCEDURE
**Nghiệp vụ:** Báo cáo Xuất - Nhập - Tồn kho than theo tháng
**Thủ tục:** `sp_BAOCAOXUATNHAPTON_THEOTHANG_GetListPaging`

---

## I. Khối Thông Tin Chung & Đầu Kỳ (Kế Thừa Hệ Thống)
Các trường thông tin nền của chủng loại than và số liệu tồn kho được kế thừa từ danh mục gốc và bảng lưu trữ dữ liệu lịch sử chốt của tháng trước.

| Tên Field | Tên Cột Hiển Thị | Bảng Dữ Liệu Nguồn | Công Thức / Logic Xử Lý Trong SQL |
| :--- | :--- | :--- | :--- |
| `LoaiThanId` | Khóa chủng loại | `dbo.DM_LOAITHAN` | Khóa chính `Id` của loại than (Dùng để `JOIN` các nhánh dữ liệu). |
| `TenLoaiThan` | Tên loại than | `dbo.DM_LOAITHAN` | `lt.TenGoi` (Lọc theo `@iTextSearch` và sắp xếp mặc định). |
| `TonDauKy` | Tồn đầu kỳ (Tấn) | `dbo.BAOCAOXUATNHAPTON_THEOTHANG` | `ISNULL(tdk.TonCuoiKy, 0)` của tháng trước. |
| `TonDauKy_AK` | AK % đầu kỳ | `dbo.BAOCAOXUATNHAPTON_THEOTHANG` | `ISNULL(tdk.TonCuoiKy_AK, 0)` chất lượng tro chốt tháng trước. |
| `TonDauKy_Vk` | Vk % đầu kỳ | `dbo.BAOCAOXUATNHAPTON_THEOTHANG` | `ISNULL(tdk.TonCuoiKy_Vk, 0)` chất bốc chốt tháng trước. |

---

## II. Khối Nhập Trong Kỳ (Dữ Liệu Phát Sinh Đầu Vào)
Dữ liệu khối này được tổng hợp từ các nghiệp vụ Nhập kho thương mại (`01VT`) và Nhập kho thành phẩm chế biến pha trộn nội bộ.

### 1. Nhánh Nhập mua & Nhập nội bộ (Từ CTE `NHAP_01VT`)
* **Bảng nguồn chính:** `dbo.PHIEUNHAPKHOTHAN_01VT` (phieu) `INNER JOIN` `dbo.PHIEUNHAPKHOTHAN_01VT_CHITIET` (ct)
* **Bảng điều kiện liên kết nội bộ:** `LEFT JOIN dbo.LENHNHAPKHOTHANNOIBO` (lenhNoiBo)

| Tên Field | Tên Cột Hiển Thị | Công Thức SQL Cụ Thể (Xích-ma Gia Quyền) |
| :--- | :--- | :--- |
| `NhapMua` | Nhập mua | `SUM(ISNULL(ct.SoLuongThucNhap, 0))` |
| `NhapMua_AK` | AK % | `CASE WHEN SUM(ct.SoLuongThucNhap) <> 0 THEN SUM(ct.SoLuongThucNhap * ct.ChatLuongAk) / SUM(ct.SoLuongThucNhap) ELSE 0 END` |
| `NhapMua_Vk` | Vk % | `CASE WHEN SUM(ct.SoLuongThucNhap) <> 0 THEN SUM(ct.SoLuongThucNhap * ct.V_PhanTram) / SUM(ct.SoLuongThucNhap) ELSE 0 END` |
| `NhapNoiBo` | Nhập nội bộ | `SUM(CASE WHEN lenhNoiBo.Id IS NOT NULL THEN ISNULL(ct.SoLuongTheoAmThucTe, 0) ELSE 0 END)` |
| `NhapNoiBo_AK` | AK % | `CASE WHEN SUM(Sản_lượng_nội_bộ) <> 0 THEN SUM(Sản_lượng_nội_bộ * ct.ChatLuongAk) / SUM(Sản_lượng_nội_bộ) ELSE 0 END` |

### 2. Nhánh Nhập pha trộn (Từ CTE `NHAP_PHATRON`)
* **Bảng nguồn chính:** `dbo.PHIEUNHAPKHO` (phieu) `INNER JOIN` `dbo.PHIEUNHAPKHO_HANGHOA` (ct)
* **Điều kiện lọc nghiệp vụ:** Cờ chế biến `CAST(phieu.IsCheBien AS BIT) = 0` (Chỉ lấy các phiếu thuần pha trộn hằng ngày).

| Tên Field | Tên Cột Hiển Thị | Công Thức SQL Cụ Thể (Xích-ma Gia Quyền) |
| :--- | :--- | :--- |
| `NhapPhaTron` | Nhập pha trộn | `SUM(CASE WHEN phieu.IsCheBien = 0 THEN ISNULL(ct.SoLuongThucNhap, 0) ELSE 0 END)` |
| `NhapPhaTron_AK` | AK % | `CASE WHEN SUM(SL_PhaTron) <> 0 THEN SUM(SL_PhaTron * ct.ChatLuongAK) / SUM(SL_PhaTron) ELSE 0 END` |

---

## III. Khối Xuất Trong Kỳ (Dữ Liệu Tiêu Thụ Đầu Ra)

### 1. Nhánh Xuất bán tiêu thụ (Từ CTE `XUAT_02VT`)
* **Bảng nguồn chính:** `dbo.PHIEUXUATKHOTHAN_02VT` (phieu) `INNER JOIN` `dbo.PHIEUXUATKHOTHAN_02VT_CHITIET` (ct)

| Tên Field | Tên Cột Hiển Thị | Công Thức SQL Cụ Thể (Xích-ma Gia Quyền) |
| :--- | :--- | :--- |
| `XuatBan` | Xuất bán | `SUM(ISNULL(ct.SoLuongThucXuat, 0))` |
| `XuatBan_AK` | AK % | `CASE WHEN SUM(ct.SoLuongThucXuat) <> 0 THEN SUM(ct.SoLuongThucXuat * ct.ChatLuongAk) / SUM(ct.SoLuongThucXuat) ELSE 0 END` |
| `XuatBan_Qk` | Qk (Cal/g) | `CASE WHEN SUM(ct.SoLuongThucXuat) <> 0 THEN SUM(ct.SoLuongThucXuat * ct.QKCal) / SUM(ct.SoLuongThucXuat) ELSE 0 END` |
| `XuatBan_Vk` | Vk % | `CASE WHEN SUM(ct.SoLuongThucXuat) <> 0 THEN SUM(ct.SoLuongThucXuat * ct.V_PhanTram) / SUM(ct.SoLuongThucXuat) ELSE 0 END` |

### 2. Nhánh Xuất nền pha trộn (Từ CTE `XUAT_PHATRON`)
* **Bảng nguồn chính:** `dbo.PHIEUXUATKHO` (phieu) `INNER JOIN` `dbo.PHIEUXUATKHO_HANGHOA` (ct) (Than nền xuất đưa vào máng trộn).

| Tên Field | Tên Cột Hiển Thị | Công Thức SQL Cụ Thể (Xích-ma Gia Quyền) |
| :--- | :--- | :--- |
| `XuatPhaTron` | Xuất pha trộn | `SUM(ISNULL(ct.SoLuongThucNhap, 0))` |
| `XuatPhaTron_AK` | AK % | `CASE WHEN SUM(ct.SoLuongThucNhap) <> 0 THEN SUM(ct.SoLuongThucNhap * ct.ChatLuongAK) / SUM(ct.SoLuongThucNhap) ELSE 0 END` |

---

## IV. Khối Tổng Hợp & Cân Đối Toàn Diện (Tính Toán Cuối)
Khối này không lấy dữ liệu trực tiếp từ bảng vật lý, mà thực hiện tính toán bắc cầu từ các khối CTE phía trên thông qua tập dữ liệu `TINHTOAN` và `TINHTOAN_AK`.

| Tên Field Hệ Thống | Mô Tả Ý Nghĩa | Công Thức SQL / Phương Trình Cân Đối Toán Học |
| :--- | :--- | :--- |
| **`TongNhap`** | **Tổng sản lượng nhập** | `= NhapMua + NhapNoiBo + NhapPhaTron` |
| **`TongNhap_AK`** | **AK% tổng khối nhập** | `= CASE WHEN TongNhap <> 0 THEN (NhapMua * NhapMua_AK + NhapNoiBo * NhapNoiBo_AK + NhapPhaTron * NhapPhaTron_AK) / TongNhap ELSE 0 END` |
| `TongNhap_Vk` | Vk% tổng khối nhập | `= CASE WHEN TongNhap <> 0 THEN (NhapMua * NhapMua_Vk) / TongNhap ELSE 0 END` |
| **`TongXuat`** | **Tổng sản lượng xuất** | `= XuatBan + XuatPhaTron` |
| **`TongXuat_AK`** | **AK% tổng khối xuất** | `= CASE WHEN TongXuat <> 0 THEN (XuatBan * XuatBan_AK + XuatPhaTron * XuatPhaTron_AK) / TongXuat ELSE 0 END` |
| `TongXuat_Vk` | Vk% tổng khối xuất | `= CASE WHEN TongXuat <> 0 THEN (XuatBan * XuatBan_Vk) / TongXuat ELSE 0 END` |
| **`TonCuoiKy`** | **Sản lượng tồn cuối kỳ**| `= TonDauKy + TongNhap - TongXuat` |
| **`TonCuoiKy_AK`**| **AK% tồn cuối kỳ** | `= CASE WHEN TonCuoiKy <> 0 THEN (TonDauKy * TonDauKy_AK + TongNhap * TongNhap_AK - TongXuat * TongXuat_AK) / TonCuoiKy ELSE 0 END` |
| **`TonCuoiKy_Vk`**| **Vk% tồn cuối kỳ** | `= CASE WHEN TonCuoiKy <> 0 THEN (TonDauKy * TonDauKy_Vk + TongNhap * TongNhap_Vk - TongXuat * TongXuat_Vk) / TonCuoiKy ELSE 0 END` |
