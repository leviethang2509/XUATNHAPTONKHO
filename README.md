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
