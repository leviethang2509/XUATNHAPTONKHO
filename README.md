# Báo Cáo Định Nghĩa Công Thức & Nguồn Dữ Liệu (Báo Cáo Xuất Nhập Tồn Theo Tháng)

Bảng dưới đây liệt kê chi tiết công thức tính toán và nguồn dữ liệu cho từng cột (field) hiển thị trên báo cáo Xuất Nhập Tồn, áp dụng trong khoảng thời gian từ `TuNgay` đến `DenNgay`.

### 1. Thông tin định danh cơ bản

| Tên Cột (Field) | Nguồn Dữ Liệu | Công Thức Tính / Mô Tả |
| :--- | :--- | :--- |
| **LoaiThanId** | `DM_LOAITHAN` | Khóa chính của bảng Danh mục Loại than (`Id`). |
| **TenLoaiThan** | `DM_LOAITHAN` | Tên gọi của loại than (`TenGoi`). |
| **TuNgay** | `Tham số Input` | Giá trị thời gian bắt đầu lấy báo cáo. |
| **DenNgay** | `Tham số Input` | Giá trị thời gian kết thúc lấy báo cáo. |

### 2. Các chỉ tiêu Số dư đầu kỳ

| Tên Cột (Field) | Nguồn Dữ Liệu | Công Thức Tính / Mô Tả |
| :--- | :--- | :--- |
| **TonDauKy** | Các phiếu Khởi tạo, Nhập, Xuất | `Tồn kho ban đầu` + Lũy kế (`Tổng Nhập` - `Tổng Xuất`) phát sinh **trước** `TuNgay`. |
| **TonDauKy_AK** | Các phiếu Nhập, Xuất | Lũy kế (`Tổng Nhập AK` - `Tổng Xuất AK`) phát sinh **trước** `TuNgay`. (Tương đương Tồn cuối kỳ AK của tháng trước). |
| **Vk_DauKy** | Phiếu xuất `02VT` | `0` - Lũy kế `Vk_XuatBan` phát sinh **trước** `TuNgay`. (Tương đương Tồn cuối kỳ VK của tháng trước). |

### 3. Các chỉ tiêu Nhập trong kỳ

| Tên Cột (Field) | Nguồn Dữ Liệu | Công Thức Tính / Mô Tả |
| :--- | :--- | :--- |
| **NhapMua** | Phiếu `01VT` | Tổng `SoLuongThucNhap` của các phiếu nhập kho trong kỳ. |
| **NhapMua_AK** | Phiếu `01VT` | Tổng `ChatLuongAk` (Tính tương tự Nhập Mua nhưng trỏ vào field chứa chỉ số AK). |
| **Vk_NhapMua** | N/A | Khởi tạo mặc định = `0` (Chưa áp dụng công thức). |
| **NhapNoiBo** | Phiếu `01VT` & `LENHNHAPKHOTHANNOIBO` | Tổng `SoLuongThucTe` (01VT) với điều kiện `LenhNhapKhoThanId` **tồn tại** trong lệnh nhập nội bộ (`IsDeleted = 0`). |
| **NhapNoiBo_AK** | Phiếu `01VT` & `LENHNHAPKHOTHANNOIBO` | Tổng `ChatLuongAK` (01VT) với điều kiện `LenhNhapKhoThanId` **tồn tại** trong lệnh nhập nội bộ (`IsDeleted = 0`). |
| **NhapPhaTron** | Phiếu `03VT` | Tổng `SoLuongThucNhap` với điều kiện phiếu **không phải** là chế biến (`IsCheBien = 0`). |
| **NhapPhaTron_AK**| Phiếu `03VT` | Tổng `ChatLuongAK` với điều kiện phiếu **không phải** là chế biến (`IsCheBien = 0`). |
| **TongNhap** | Khối Nhập | `NhapMua` + `NhapNoiBo` + `NhapPhaTron`. |
| **BinhQuanNgay_AK**| Khối Nhập | `NhapMua_AK` + `NhapNoiBo_AK` + `NhapPhaTron_AK`. |

### 4. Các chỉ tiêu Xuất trong kỳ

| Tên Cột (Field) | Nguồn Dữ Liệu | Công Thức Tính / Mô Tả |
| :--- | :--- | :--- |
| **XuatBan** | Phiếu `02VT` & `04VT` | Tổng `SoLuongThucXuat` (02VT) + `SoLuongThucNhap` (04VT) với điều kiện Lý do xuất là **Bán hàng**. |
| **XuatBan_AK** | Phiếu `02VT` & `04VT` | Tổng `ChatLuongAK` với điều kiện Lý do xuất là **Bán hàng**. |
| **Qk_XuatBan** | Phiếu `02VT` | Tổng `SoLuongThucTe` với điều kiện Lý do xuất là **Bán hàng**. |
| **Vk_XuatBan** | Phiếu `02VT` | Tổng `V_PhanTram` với điều kiện Lý do xuất là **Bán hàng**. |
| **XuatPhaTron** | Phiếu `02VT` & `04VT` | Tổng `SoLuongThucXuat` (02VT) + `SoLuongThucNhap` (04VT) với điều kiện Lý do xuất là **Pha trộn**. |
| **XuatPhaTron_AK**| Phiếu `02VT` & `04VT` | Tổng `ChatLuongAK` với điều kiện Lý do xuất là **Pha trộn**. |
| **TongXuat** | Khối Xuất | `XuatBan` + `XuatPhaTron`. |
| **BinhQuanThang_AK**| Khối Xuất | `XuatBan_AK` + `XuatPhaTron_AK`. |

### 5. Các chỉ tiêu Số dư cuối kỳ

| Tên Cột (Field) | Nguồn Dữ Liệu | Công Thức Tính / Mô Tả |
| :--- | :--- | :--- |
| **TonCuoiKy** | Tính toán truy vấn | Cân đối trong kỳ: `TonDauKy` + `TongNhap` - `TongXuat`. |
| **TonCuoiKy_AK** | Tính toán truy vấn | Cân đối chất lượng: `TonDauKy_AK` + `BinhQuanNgay_AK` - `BinhQuanThang_AK`. |
