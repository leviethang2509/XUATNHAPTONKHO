# Báo cáo công thức `sp_BAOCAOXUATNHAPTON_THEOTHANG_GetListPaging`

## Điều kiện lọc chung

| Nhóm dữ liệu | Điều kiện |
|---|---|
| Đơn vị | `DonViId = @iDonViId` với phiếu kho than `01VT`, `02VT` |
| Phòng ban | Nếu `@iPhongBanId = 0x0` thì lấy tất cả, ngược lại lọc theo `PhongBanId` hoặc `PhanXuongId` |
| Kỳ báo cáo | Dữ liệu trong kỳ: `>= @iTuNgay` và `<= @iDenNgay` |
| Tồn đầu kỳ | Dữ liệu đã chốt sổ trước `@iTuNgay` |
| Phiếu hợp lệ | `IsDeleted = 0` |

## Bảng field và công thức

| STT | Field | Công thức / Nguồn tính | Ghi chú |
|---:|---|---|---|
| 1 | `LoaiThanId` | `DM_LOAITHAN.Id` | Lấy theo loại than phát sinh trong kỳ |
| 2 | `Id` | `NULL` | Store không lấy từ bảng báo cáo đã lưu |
| 3 | `TuNgay` | `@iTuNgay` | Ngày bắt đầu kỳ |
| 4 | `DenNgay` | `@iDenNgay` | Ngày kết thúc kỳ |
| 5 | `DonViId` | `@iDonViId` | Đơn vị lọc |
| 6 | `PhongBanId` | `@iPhongBanId` | Phòng ban/phân xưởng lọc |
| 7 | `TenLoaiThan` | `DM_LOAITHAN.TenGoi` | Tên loại than |
| 8 | `TonDauKy` | `KHOTHAN_TONKHO.SoLuong + NhapMua_TruocKy + NhapNoiBo_TruocKy + NhapPhaTron_TruocKy - XuatBan02_TruocKy - XuatPhaTron02_TruocKy - XuatBan04_TruocKy - XuatPhaTron04_TruocKy` | Tính trong CTE `TONDAUKY` |
| 9 | `TonDauKy_AK` | `NhapMua_AK_TruocKy + NhapNoiBo_AK_TruocKy + NhapPhaTron_AK_TruocKy - XuatBan_AK02_TruocKy - XuatPhaTron_AK02_TruocKy - XuatBan_AK04_TruocKy - XuatPhaTron_AK04_TruocKy` | Tương đương `TonCuoiKy_AK` kỳ trước theo dữ liệu đã chốt |
| 10 | `Vk_DauKy` | `0 + 0 - Vk_XuatBan02_TruocKy` | Tương đương `TonCuoiKy_VK` kỳ trước theo dữ liệu đã chốt |
| 11 | `NhapMua` | `SUM(PHIEUNHAPKHOTHAN_01VT_CHITIET.SoLuongThucNhap)` | Phiếu `PHIEUNHAPKHOTHAN_01VT` trong kỳ |
| 12 | `NhapMua_AK` | `SUM(PHIEUNHAPKHOTHAN_01VT_CHITIET.ChatLuongAk)` | AK tương ứng nhập mua |
| 13 | `Vk_NhapMua` | `0` | Store hiện chưa có nguồn VK nhập mua |
| 14 | `NhapNoiBo` | `SUM(PHIEUNHAPKHOTHAN_01VT_CHITIET.SoLuongTheoAmThucTe)` khi tồn tại `LENHNHAPKHOTHANNOIBO` | Nhập nội bộ trong kỳ |
| 15 | `NhapNoiBo_AK` | `SUM(PHIEUNHAPKHOTHAN_01VT_CHITIET.ChatLuongAK)` khi tồn tại `LENHNHAPKHOTHANNOIBO` | AK tương ứng nhập nội bộ |
| 16 | `NhapPhaTron` | `SUM(PHIEUNHAPKHO_HANGHOA.SoLuongThucNhap)` khi `PHIEUNHAPKHO.IsCheBien = 0` | Phiếu nhập kho thường trong kỳ |
| 17 | `NhapPhaTron_AK` | `SUM(PHIEUNHAPKHO_HANGHOA.ChatLuongAK)` khi `PHIEUNHAPKHO.IsCheBien = 0` | AK tương ứng nhập pha trộn |
| 18 | `TongNhap` | `NhapMua + NhapNoiBo + NhapPhaTron` | Tổng nhập trong kỳ |
| 19 | `BinhQuanNgay_AK` | `NhapMua_AK + NhapNoiBo_AK + NhapPhaTron_AK` | Store hiện đang cộng tổng AK, chưa chia bình quân có trọng số |
| 20 | `XuatBan` | `SUM(PHIEUXUATKHOTHAN_02VT_CHITIET.SoLuongThucXuat)` khi `LyDoXuatKhoId = '982A0756-555B-44B3-8F0C-CBF1E062AC21'` + `SUM(PHIEUXUATKHO_HANGHOA.SoLuongThucNhap)` khi `LyDoXuatId = '982A0756-555B-44B3-8F0C-CBF1E062AC21' OR IsCheBien = 1` | Gộp phiếu `02VT` và `04VT` |
| 21 | `XuatBan_AK` | `SUM(PHIEUXUATKHOTHAN_02VT_CHITIET.ChatLuongAk)` khi xuất bán + `SUM(PHIEUXUATKHO_HANGHOA.ChatLuongAK)` khi xuất bán hoặc chế biến | AK tương ứng xuất bán |
| 22 | `Qk_XuatBan` | `SUM(PHIEUXUATKHOTHAN_02VT_CHITIET.QKCal)` khi `LyDoXuatKhoId = '982A0756-555B-44B3-8F0C-CBF1E062AC21'` | Chỉ lấy từ phiếu `PHIEUXUATKHOTHAN_02VT` |
| 23 | `Vk_XuatBan` | `SUM(PHIEUXUATKHOTHAN_02VT_CHITIET.V_PhanTram)` khi `LyDoXuatKhoId = '982A0756-555B-44B3-8F0C-CBF1E062AC21'` | Chỉ lấy từ phiếu `PHIEUXUATKHOTHAN_02VT` |
| 24 | `XuatPhaTron` | `SUM(PHIEUXUATKHOTHAN_02VT_CHITIET.SoLuongThucXuat)` khi `LyDoXuatKhoId = 'B8FE5BE0-28CB-4EA8-8267-460A6EC7A66D'` + `SUM(PHIEUXUATKHO_HANGHOA.SoLuongThucNhap)` khi `LyDoXuatId = 'B8FE5BE0-28CB-4EA8-8267-460A6EC7A66D' OR IsCheBien = 1` | Gộp phiếu `02VT` và `04VT` |
| 25 | `XuatPhaTron_AK` | `SUM(PHIEUXUATKHOTHAN_02VT_CHITIET.ChatLuongAk)` khi xuất pha trộn + `SUM(PHIEUXUATKHO_HANGHOA.ChatLuongAK)` khi xuất pha trộn hoặc chế biến | AK tương ứng xuất pha trộn |
| 26 | `TongXuat` | `XuatBan + XuatPhaTron` | Tổng xuất trong kỳ |
| 27 | `BinhQuanThang_AK` | `XuatBan_AK + XuatPhaTron_AK` | Store hiện đang cộng tổng AK, chưa chia bình quân có trọng số |
| 28 | `TonCuoiKy` | `TonDauKy + TongNhap - TongXuat` | Tồn cuối kỳ số lượng |
| 29 | `TonCuoiKy_AK` | `TonDauKy_AK + BinhQuanNgay_AK - BinhQuanThang_AK` | Tồn cuối kỳ AK |
| 30 | `TonCuoiKy_VK` | `Vk_DauKy + Vk_NhapMua - Vk_XuatBan` | Tồn cuối kỳ VK |

## Field xác nhận và output phụ

| Field / Output | Công thức / Nguồn tính | Ghi chú |
|---|---|---|
| `IsXacNhan` | `1` nếu không tồn tại phiếu chưa chốt của `01VT`, `02VT`, `03VT`, `04VT` theo từng `LoaiThanId`; ngược lại `0` | Được insert vào `@KETQUA`, nhưng SELECT cuối hiện chưa trả field này |
| `@oTotalRow` | `COUNT(1) FROM @KETQUA` | Tổng số dòng sau khi lọc |
| `@oIds` | Nối `Id` các phiếu `01VT`, `02VT`, `03VT`, `04VT` trong kỳ bằng dấu `;` | Dùng cho xác nhận/xóa danh sách |
| `@oIsXacNhanGrid` | `1` nếu tổng số phiếu trong kỳ > 0 và tất cả đều `IsChotSo = 1`; ngược lại `0` | Trạng thái xác nhận toàn bộ grid |

## CTE trung gian

| CTE | Mục đích |
|---|---|
| `SOLUONGTONKHOBANDAU` | Tổng tồn kho ban đầu theo `LoaiThanId` từ `KHOTHAN_TONKHO` |
| `NHAPMUA_TRUOCTHANG` | Tổng nhập mua, nhập nội bộ và AK trước `@iTuNgay` |
| `XUAT02_TRUOCTHANG` | Tổng xuất bán, xuất pha trộn, AK và VK từ phiếu `02VT` trước `@iTuNgay` |
| `NHAP03_TRUOCTHANG` | Tổng nhập pha trộn và AK từ phiếu nhập kho thường trước `@iTuNgay` |
| `XUAT04_TRUOCTHANG` | Tổng xuất bán, xuất pha trộn và AK từ phiếu xuất kho thường trước `@iTuNgay` |
| `TONDAUKY` | Tổng hợp tồn đầu kỳ theo số lượng, AK, VK |
| `DANHMUCLOAITHAN` | Gom phát sinh trong kỳ từ 4 nhóm phiếu bằng `UNION ALL` |
| `DANHMUCLOAITHAN_AGG` | Cộng tổng phát sinh theo `LoaiThanId`, `TenGoi` |
| `XACNHAN_THEO_LOAITHAN` | Kiểm tra trạng thái chốt sổ theo từng loại than |
| `ALL_TICKETS` | Gom danh sách phiếu trong kỳ để xuất `@oIds` và `@oIsXacNhanGrid` |
