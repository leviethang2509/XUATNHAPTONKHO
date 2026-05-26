# 📑 TÀI LIỆU CÔNG THỨC XỬ LÝ DỮ LIỆU TRÊN TỪNG FIELD
**Hệ thống:** CBT_QUANGNINH  
**Nghiệp vụ:** Báo cáo Xuất - Nhập - Tồn kho than theo tháng (Cấu trúc rút gọn các cột hiển thị)

---

## I. Nguyên Tắc Tính Xích-Ma (Bình Quân Gia Quyền)
Đối với các chỉ số chất lượng than (AK%, Vk%, Qk), hệ thống không dùng hàm trung bình cộng (AVG) đơn thuần mà bắt buộc áp dụng công thức **Bình quân gia quyền theo sản lượng ngày/phiếu** để đảm bảo tính cân đối chính xác tuyệt đối:

* **Công thức tổng quát:** Chỉ số bình quân = [Tổng_Tổng(Khối lượng phiếu * Chỉ số chất lượng phiếu)] / Tổng khối lượng kỳ báo cáo

---

## II. Bảng Tra Cứu Công Thức & Nguồn Dữ Liệu Trên Từng Field

| STT | Tên Field Hệ Thống | Mô Tả Giao Diện | Nguồn Dữ Liệu Gốc / Công Thức SQL | Ghi Chú |
| :---: | :--- | :--- | :--- | :--- |
| **1** | `TenLoaiThan` | Tên loại than | `dbo.DM_LOAITHAN.TenGoi` | Trục danh mục chủng loại than chính |
| **2** | `TonDauKy` | Tồn đầu kỳ (Tấn) | `dbo.BAOCAOXUATNHAPTON_THEOTHANG.TonCuoiKy` | Lấy từ dữ liệu chốt của **Tháng trước** |
| **3** | `TonDauKy_AK` | AK % đầu kỳ | `dbo.BAOCAOXUATNHAPTON_THEOTHANG.TonCuoiKy_AK` | Lấy từ dữ liệu chốt của **Tháng trước** |
| **4** | `TonDauKy_Vk` | Vk % đầu kỳ | `dbo.BAOCAOXUATNHAPTON_THEOTHANG.TonCuoiKy_Vk` | Lấy từ dữ liệu chốt của **Tháng trước** |
| **5** | `NhapMua` | Nhập mua | `SUM(ct.SoLuongThucNhap)` từ phiếu `01VT` | Chỉ lấy các phiếu thuộc kỳ báo cáo |
| **6** | `NhapMua_AK` | AK % Nhập mua | `SUM(SoLuongThucNhap * ChatLuongAk) / NhapMua` | Gia quyền từ chi tiết `01VT_CHITIET` |
| **7** | `NhapMua_Vk` | Vk % Nhập mua | `SUM(SoLuongThucNhap * V_PhanTram) / NhapMua` | Gia quyền từ chi tiết `01VT_CHITIET` |
| **8** | `NhapNoiBo` | Nhập nội bộ | `SUM(ct.SoLuongTheoAmThucTe)` từ phiếu `01VT` | Điều kiện: Có liên kết với `LENHNHAPKHOTHANNOIBO` |
| **9** | `NhapNoiBo_AK` | AK % Nhập nội bộ | `SUM(SoLuongTheoAmThucTe * ChatLuongAk) / NhapNoiBo` | Gia quyền theo lượng ẩm thực tế |
| **10** | `NhapPhaTron` | Nhập pha trộn | `SUM(ct.SoLuongThucNhap)` từ `PHIEUNHAPKHO` | Lọc theo cờ chế biến: `IsCheBien = 0` |
| **11** | `NhapPhaTron_AK` | AK % Nhập pha trộn | `SUM(SoLuongThucNhap * ChatLuongAK) / NhapPhaTron` | Lấy từ chi tiết `PHIEUNHAPKHO_HANGHOA` |
| **12** | `TongNhap` | **Tổng nhập tháng** | `= NhapMua + NhapNoiBo + NhapPhaTron` | Tổng khối lượng của 3 luồng nhập hiển thị |
| **13** | `TongNhap_AK` | **AK % Tổng nhập** | `= (NhapMua * NhapMua_AK + NhapNoiBo * NhapNoiBo_AK + NhapPhaTron * NhapPhaTron_AK) / TongNhap` | Trả về `0` nếu `TongNhap = 0` |
| **14** | `TongNhap_Vk` | **Vk % Tổng nhập** | `= (NhapMua * NhapMua_Vk) / TongNhap` | Cân đối chất bốc theo luồng nhập mua chính |
| **15** | `XuatBan` | Xuất bán | `SUM(ct.SoLuongThucXuat)` từ phiếu `02VT` | Quét từ chi tiết xuất kho tiêu thụ |
| **16** | `XuatBan_AK` | AK % Xuất bán | `SUM(SoLuongThucXuat * ChatLuongAk) / XuatBan` | Gia quyền chất lượng từ chi tiết phiếu `02VT` |
| **17** | `XuatBan_Qk` | Qk (Cal/g) Xuất bán| `SUM(SoLuongThucXuat * QKCal) / XuatBan` | Chỉ số nhiệt trị gia quyền của than tiêu thụ |
| **18** | `XuatBan_Vk` | Vk % Xuất bán | `SUM(SoLuongThucXuat * V_PhanTram) / XuatBan` | Chỉ số chất bốc gia quyền của than tiêu thụ |
| **19** | `XuatPhaTron` | Xuất pha trộn | `SUM(ct.SoLuongThucNhap)` từ `PHIEUXUATKHO` | Khối lượng than nền đưa vào máy pha trộn |
| **20** | `XuatPhaTron_AK` | AK % Xuất pha trộn | `SUM(SoLuongThucNhap * ChatLuongAK) / XuatPhaTron` | Lấy từ chi tiết `PHIEUXUATKHO_HANGHOA` |
| **21** | `TongXuat` | **Tổng xuất tháng** | `= XuatBan + XuatPhaTron` | Tổng khối lượng của 2 luồng xuất hiển thị |
| **22** | `TongXuat_AK` | **AK % Tổng xuất** | `= (XuatBan * XuatBan_AK + XuatPhaTron * XuatPhaTron_AK) / TongXuat` | Trả về `0` nếu `TongXuat = 0` |
| **23** | `TongXuat_Vk` | **Vk % Tổng xuất** | `= (XuatBan * XuatBan_Vk) / TongXuat` | Cân đối chất bốc theo luồng xuất tiêu thụ chính |
| **24** | `TonCuoiKy` | **Tồn cuối kỳ** | `= TonDauKy + TongNhap - TongXuat` | **Phương trình cân đối kho cốt lõi** |
| **25** | `TonCuoiKy_AK` | **AK % Tồn cuối kỳ** | `= (TonDauKy * TonDauKy_AK + TongNhap * TongNhap_AK - TongXuat * TongXuat_AK) / TonCuoiKy` | Trả về `0` nếu `TonCuoiKy = 0` |
| **26** | `TonCuoiKy_Vk` | **Vk % Tồn cuối kỳ** | `= (TonDauKy * TonDauKy_Vk + TongNhap * TongNhap_Vk - TongXuat * TongXuat_Vk) / TonCuoiKy` | Trả về `0` nếu `TonCuoiKy = 0` |

---

## III. Các Tham Số Đầu Ra Hệ Thống (Output Parameters)

Mục tiêu phục vụ phân trang, hiển thị trạng thái động và bóc tách chuỗi ID trên giao diện Kendo Grid:

1. **`@oTotalRow` (Tổng số dòng):** Đếm số lượng các chủng loại than có phát sinh dữ liệu thực tế trong kỳ báo cáo để render phân trang grid.
2. **`@oIds` (Chuỗi ID tổng hợp):** Gom toàn bộ khóa chính `Id` của tất cả các phiếu nhập/xuất kho phát sinh trong tháng (`01VT`, `02VT`, `PHIEUNHAPKHO`, `PHIEUXUATKHO`) lại thành một chuỗi duy nhất, phân tách bằng dấu chấm phẩy `;`. Chuỗi này phục vụ nút chốt/hủy xác nhận đồng loạt dưới Client.
3. **`@oIsXacNhanGrid` (Cờ trạng thái tổng):** * Nếu **Tổng số lượng phiếu phát sinh trong tháng** bằng đúng **Tổng số lượng phiếu đã được chốt sổ (`IsChotSo = 1`)**, tham số trả về `1` (Giao diện hiển thị: `(DỮ LIỆU ĐÃ XÁC NHẬN)` màu xanh).
   * Chỉ cần tồn tại tối thiểu 1 phiếu chưa được chốt sổ, tham số trả về `0` (Giao diện hiển thị: `(DỮ LIỆU CHƯA XÁC NHẬN)` màu đỏ).
