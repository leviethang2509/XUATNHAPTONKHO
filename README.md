# Bảng công thức `sp_BAOCAOXUATNHAPTON_THEOTHANG_GetListPaging`

> **Nguồn chính**: 4 phiếu + `KHOTHAN_TONKHO`. Không lấy từ `BAOCAXUATNHAPTON_THEOTHANG`.

---

## Các field có công thức (từ 4 phiếu)

| STT | Field | Công thức / Nguồn | Ghi chú |
|--|--|--|--|
| 1 | `NhapMua` | `SUM(PHIEUNHAPKHOTHAN_01VT_CHITIET.SoLuongThucNhap)` | Phiếu 01VT trong kỳ |
| 2 | `NhapMua_AK` | `SUM(PHIEUNHAPKHOTHAN_01VT_CHITIET.ChatLuongAk)` | Phiếu 01VT trong kỳ |
| 3 | `NhapCBTuThanNhapKhau` | `SUM(PHIEUNHAPKHO_HANGHOA.SoLuongThucNhap)` WHERE `IsCheBien = 1` | Phiếu 03VT trong kỳ |
| 4 | `NhapCBTuThanNhapKhau_AK` | `SUM(PHIEUNHAPKHO_HANGHOA.ChatLuongAK)` WHERE `IsCheBien = 1` | Phiếu 03VT trong kỳ |
| 5 | `NhapPhaTron` | `SUM(PHIEUNHAPKHO_HANGHOA.SoLuongThucNhap)` WHERE `IsCheBien = 0` | Phiếu 03VT trong kỳ |
| 6 | `NhapPhaTron_AK` | `SUM(PHIEUNHAPKHO_HANGHOA.ChatLuongAK)` WHERE `IsCheBien = 0` | Phiếu 03VT trong kỳ |
| 7 | `TongNhap` | `NhapMua + NhapCBTuThanNhapKhau + NhapPhaTron` | Tổng lượng nhập |
| 8 | `BinhQuanNgay_AK` | `NhapMua_AK + NhapCBTuThanNhapKhau_AK + NhapPhaTron_AK` | Tên field giữ nguyên theo store, thực chất là tổng AK nhập |
| 9 | `XuatBan` | `SUM(PHIEUXUATKHOTHAN_02VT_CHITIET.SoLuongThucXuat)` WHERE `LyDoXuatKhoId = '982A0756-555B-44B3-8F0C-CBF1E062AC21'` + `SUM(PHIEUXUATKHO_HANGHOA.SoLuongThucNhap)` WHERE (`LyDoXuatId = '982A0756-555B-44B3-8F0C-CBF1E062AC21'` OR `IsCheBien = 1`) | 02VT + 04VT |
| 10 | `XuatBan_AK` | `SUM(PHIEUXUATKHOTHAN_02VT_CHITIET.ChatLuongAk)` WHERE `LyDoXuatKhoId = '982A0756-555B-44B3-8F0C-CBF1E062AC21'` + `SUM(PHIEUXUATKHO_HANGHOA.ChatLuongAK)` WHERE (`LyDoXuatId = '982A0756-555B-44B3-8F0C-CBF1E062AC21'` OR `IsCheBien = 1`) | 02VT + 04VT |
| 11 | `XuatCheBienTuThanNhapKhau` | `SUM(PHIEUXUATKHOTHAN_02VT_CHITIET.SoLuongThucXuat)` WHERE `LyDoXuatKhoId = '26d252c8-6edc-4c30-9d8c-2091e1d82430'` + `SUM(PHIEUXUATKHO_HANGHOA.SoLuongThucNhap)` WHERE (`LyDoXuatId = '26d252c8-6edc-4c30-9d8c-2091e1d82430'` OR `IsCheBien = 1`) | 02VT + 04VT |
| 12 | `XuatCheBienTuThanNhapKhau_AK` | `SUM(PHIEUXUATKHOTHAN_02VT_CHITIET.ChatLuongAk)` WHERE `LyDoXuatKhoId = '26d252c8-6edc-4c30-9d8c-2091e1d82430'` + `SUM(PHIEUXUATKHO_HANGHOA.ChatLuongAK)` WHERE (`LyDoXuatId = '26d252c8-6edc-4c30-9d8c-2091e1d82430'` OR `IsCheBien = 1`) | 02VT + 04VT |
| 13 | `XuatPhaTron` | `SUM(PHIEUXUATKHOTHAN_02VT_CHITIET.SoLuongThucXuat)` WHERE `LyDoXuatKhoId = 'B8FE5BE0-28CB-4EA8-8267-460A6EC7A66D'` + `SUM(PHIEUXUATKHO_HANGHOA.SoLuongThucNhap)` WHERE (`LyDoXuatId = 'B8FE5BE0-28CB-4EA8-8267-460A6EC7A66D'` OR `IsCheBien = 1`) | 02VT + 04VT |
| 14 | `XuatPhaTron_AK` | `SUM(PHIEUXUATKHOTHAN_02VT_CHITIET.ChatLuongAk)` WHERE `LyDoXuatKhoId = 'B8FE5BE0-28CB-4EA8-8267-460A6EC7A66D'` + `SUM(PHIEUXUATKHO_HANGHOA.ChatLuongAK)` WHERE (`LyDoXuatId = 'B8FE5BE0-28CB-4EA8-8267-460A6EC7A66D'` OR `IsCheBien = 1`) | 02VT + 04VT |
| 15 | `TongXuat` | `XuatBan + XuatCheBienTuThanNhapKhau + XuatPhaTron` | Tổng lượng xuất |
| 16 | `BinhQuanThang_AK` | `XuatBan_AK + XuatCheBienTuThanNhapKhau_AK + XuatPhaTron_AK` | Tên field giữ nguyên theo store, thực chất là tổng AK xuất |
| 17 | `TonDauKy` | `SoLuongTonKhoBanDau + NhapMua_TruocThang + NhapCBTuThanNhapKhau_TruocThang + NhapPhaTron_TruocThang - XuatBan_02VT_TruocThang - XuatCheBienTuThanNhapKhau_02VT_TruocThang - XuatPhaTron_02VT_TruocThang - XuatBan_04VT_TruocThang - XuatCheBienTuThanNhapKhau_04VT_TruocThang - XuatPhaTron_04VT_TruocThang` | CTE `TONDAUKY` |
| 18 | `TonDauKy_AK` | `NhapMua_AK_TruocThang + NhapCBTuThanNhapKhau_AK_TruocThang + NhapPhaTron_AK_TruocThang - XuatBan_AK_02VT_TruocThang - XuatCheBienTuThanNhapKhau_AK_02VT_TruocThang - XuatPhaTron_AK_02VT_TruocThang - XuatBan_AK_04VT_TruocThang - XuatCheBienTuThanNhapKhau_AK_04VT_TruocThang - XuatPhaTron_AK_04VT_TruocThang` | Store hiện không cộng AK từ `KHOTHAN_TONKHO` |
| 19 | `TonCuoiKy` | `TonDauKy + TongNhap - TongXuat` | SELECT cuối |
| 20 | `TonCuoiKy_AK` | `TonDauKy_AK + BinhQuanNgay_AK - BinhQuanThang_AK` | SELECT cuối |
| 21 | `Id` | `NULL` | Không lấy từ bảng báo cáo |
| 22 | `NgaySua` | `NULL` | Không lấy từ bảng báo cáo |
| 23 | `DonViId` | `@iDonViId` | Truyền thẳng |
| 24 | `PhongBanId` | `@iPhongBanId` | Truyền thẳng |
| 25 | `IsXacNhan` | `0` (gán cứng) | Từng row không xác nhận riêng |
| 26 | `TuNgay` | `@iTuNgay` | Truyền thẳng |
| 27 | `DenNgay` | `@iDenNgay` | Truyền thẳng |

---

## Các field **chưa có công thức** (đang gán `CAST(0 AS FLOAT)` / `CAST(0 AS BIT)` / chưa tính)

| STT | Field | Giá trị hiện tại | Lý do |
|--|--|--|--|
| 1 | `NhapNoiBo` | `CAST(0 AS FLOAT)` | Chưa có phiếu nghiệp vụ nhập nội bộ |
| 2 | `NhapNoiBo_AK` | `CAST(0 AS FLOAT)` | Chưa có nguồn AK cho nghiệp vụ này |
| 3 | `NhapCBTuThanNhapKhau_ThuHoi` | `CAST(0 AS FLOAT)` | Chưa có phiếu nghiệp vụ thu hồi |
| 4 | `NhapCBTuSPNT` | `CAST(0 AS FLOAT)` | Chưa có phiếu nghiệp vụ |
| 5 | `NhapCBTuSPNT_ThuHoi` | `CAST(0 AS FLOAT)` | Chưa có phiếu nghiệp vụ |
| 6 | `NhapCBTuSPNT_AK` | `CAST(0 AS FLOAT)` | Chưa có nguồn AK tương ứng |
| 7 | `NhapCBTuThanSachBTP` | `CAST(0 AS FLOAT)` | Chưa có phiếu nghiệp vụ |
| 8 | `NhapCBTuThanSachBTP_ThuHoi` | `CAST(0 AS FLOAT)` | Chưa có phiếu nghiệp vụ |
| 9 | `NhapCBTuThanSachBTP_AK` | `CAST(0 AS FLOAT)` | Chưa có nguồn AK tương ứng |
| 10 | `NhapCBTuTNKNhapMua` | `CAST(0 AS FLOAT)` | Chưa có phiếu nghiệp vụ |
| 11 | `NhapCBTuTNKNhapMua_ThuHoi` | `CAST(0 AS FLOAT)` | Chưa có phiếu nghiệp vụ |
| 12 | `NhapCBTuTNKNhapMua_AK` | `CAST(0 AS FLOAT)` | Chưa có nguồn AK tương ứng |
| 13 | `XuatNoiBo` | `CAST(0 AS FLOAT)` | Chưa có phiếu nghiệp vụ xuất nội bộ |
| 14 | `XuatNoiBo_AK` | `CAST(0 AS FLOAT)` | Chưa có nguồn AK tương ứng |
| 15 | `XuatKhac` | `CAST(0 AS FLOAT)` | Chưa có phiếu nghiệp vụ xuất khác |
| 16 | `XuatKhac_AK` | `CAST(0 AS FLOAT)` | Chưa có nguồn AK tương ứng |
| 17 | `HaoHutTrongQuaTrinhXuatKho` | `CAST(0 AS FLOAT)` | Chưa có phiếu nghiệp vụ hao hụt |
| 18 | `HaoHutTrongQuaTrinhXuatKho_AK` | `CAST(0 AS FLOAT)` | Chưa có nguồn AK tương ứng |
| 19 | `ThuHoi` | `CAST(0 AS FLOAT)` | Chưa có phiếu nghiệp vụ |
| 20 | `ThuHoi_AK` | `CAST(0 AS FLOAT)` | Chưa có nguồn AK tương ứng |
| 21 | `HaoHutTrongQuaTrinhNhapKho` | `CAST(0 AS FLOAT)` | Chưa có phiếu nghiệp vụ |
| 22 | `HaoHutTrongQuaTrinhNhapKho_ThuHoi` | `CAST(0 AS FLOAT)` | Chưa có phiếu nghiệp vụ |
| 23 | `HaoHutTrongQuaTrinhNhapKho_AK` | `CAST(0 AS FLOAT)` | Chưa có nguồn AK tương ứng |
| 24 | `XuatCheBienTu_SPNT` | `CAST(0 AS FLOAT)` | Chưa có phiếu nghiệp vụ |
| 25 | `XuatCheBienTu_SPNT_AK` | `CAST(0 AS FLOAT)` | Chưa có nguồn AK tương ứng |
| 26 | `XuatCheBienTu_BTP` | `CAST(0 AS FLOAT)` | Chưa có phiếu nghiệp vụ |
| 27 | `XuatCheBienTu_BTP_AK` | `CAST(0 AS FLOAT)` | Chưa có nguồn AK tương ứng |
| 28 | `XuatCheBienTu_TNK_NhapMua` | `CAST(0 AS FLOAT)` | Chưa có phiếu nghiệp vụ |
| 29 | `XuatCheBienTu_TNK_NhapMua_AK` | `CAST(0 AS FLOAT)` | Chưa có nguồn AK tương ứng |
| 30 | `NhapKhac` | `CAST(0 AS FLOAT)` | Chưa có phiếu nghiệp vụ nhập khác |
| 31 | `IsXacNhan` (từng row) | `CAST(0 AS BIT)` | Chỉ có xác nhận tổng thể grid, không theo từng loại than |

---

## CTE trung gian

| CTE | Mục đích | Nguồn |
|--|--|--|
| `SOLUONGTONKHOBANDAU` | Tồn kho ban đầu | `KHOTHAN_TONKHO` |
| `NHAPMUA_TRUOCTHANG` | Nhập mua trước kỳ + AK trước kỳ | 01VT, `dauThang -> @iTuNgay - 1` |
| `XUAT02_TRUOCTHANG` | Xuất 02VT trước kỳ + AK trước kỳ | 02VT, `dauThang -> @iTuNgay - 1` |
| `NHAP03_TRUOCTHANG` | Nhập 03VT trước kỳ + AK trước kỳ | 03VT (`PHIEUNHAPKHO`), `dauThang -> @iTuNgay - 1` |
| `XUAT04_TRUOCTHANG` | Xuất 04VT trước kỳ + AK trước kỳ | 04VT (`PHIEUXUATKHO`), `dauThang -> @iTuNgay - 1` |
| `TONDAUKY` | Tồn đầu kỳ lượng thường + tồn đầu kỳ AK | Tổng hợp từ 5 CTE trên |
| `DANHMUCLOAITHAN` | Phát sinh trong kỳ từ 4 phiếu, gồm cả lượng thường và AK | 01VT, 02VT, 03VT, 04VT |
| `DANHMUCLOAITHAN_AGG` | Gộp tổng theo `Id, TenGoi, NhomId` sau `UNION ALL` | `SUM(...) GROUP BY ...` |

---

## Output parameters

| Parameter | Công thức | Nguồn |
|--|--|--|
| `@oTotalRow` | `COUNT(1) FROM @KETQUA` | Bảng tạm |
| `@oIds` | `STUFF(FOR XML PATH(';'))` từ `IDS_XACNHAN` | 4 phiếu đã xác nhận |
| `@oIsXacNhanGrid` | `1` nếu cả 4 nhóm `TotalCount = ConfirmCount`, `0` nếu ngược lại | 4 CTE `XACNHAN_01VT/02VT/03VT/04VT` |

---

## Trường hợp đặc biệt đang giữ nguyên

1. **`PHIEUXUATKHO.IsCheBien = 1`**: một phiếu xuất kho thường có `IsCheBien = 1` sẽ được cộng đồng thời vào cả 3 cột:
   - `XuatBan`
   - `XuatCheBienTuThanNhapKhau`
   - `XuatPhaTron`
   - và đồng thời cộng cả 3 cột `_AK` tương ứng từ `PHIEUXUATKHO_HANGHOA.ChatLuongAK`
2. **Các field `_AK` có công thức song song field thường**:
   - 01VT lấy từ `PHIEUNHAPKHOTHAN_01VT_CHITIET.ChatLuongAk`
   - 02VT lấy từ `PHIEUXUATKHOTHAN_02VT_CHITIET.ChatLuongAk`
   - 03VT lấy từ `PHIEUNHAPKHO_HANGHOA.ChatLuongAK`
   - 04VT lấy từ `PHIEUXUATKHO_HANGHOA.ChatLuongAK`
3. **`TongNhap`, `TongXuat`**: tính từ phát sinh 4 phiếu.
4. **`BinhQuanNgay_AK`, `BinhQuanThang_AK`**: store hiện dùng như tổng AK nhập / tổng AK xuất, tên field giữ nguyên theo thiết kế cũ.
5. **Các nghiệp vụ chưa có phiếu nguồn**: vẫn gán cứng `0`.

---

> ⚠️ Khi cần thêm logic cho các field đang gán `0` ở bảng "chưa có công thức" ở trên, cần xác định phiếu nguồn tương ứng và viết CTE/UNION ALL bổ sung.
