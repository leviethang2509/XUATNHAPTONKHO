# Bảng công thức `sp_BAOCAOXUATNHAPTON_THEOTHANG_GetListPaging`

> **Nguồn chính**: 4 phiếu + `KHOTHAN_TONKHO`. Không lấy từ `BAOCAXUATNHAPTON_THEOTHANG`.

---

## Các field có công thức (từ 4 phiếu)

| STT | Field | Công thức / Nguồn | Ghi chú |
|--|--|--|--|
| 1 | `NhapMua` | `SUM(PHIEUNHAPKHOTHAN_01VT_CHITIET.SoLuongThucNhap)` | Qua phiếu 01VT, đã lọc `NgayThang` trong kỳ |
| 2 | `NhapCBTuThanNhapKhau` | `SUM(PHIEUNHAPKHO_HANGHOA.SoLuongThucNhap)` WHERE `IsCheBien = 1` | Qua phiếu 03VT |
| 3 | `NhapPhaTron` | `SUM(PHIEUNHAPKHO_HANGHOA.SoLuongThucNhap)` WHERE `IsCheBien = 0` | Qua phiếu 03VT |
| 4 | `XuatBan` | `SUM(PHIEUXUATKHOTHAN_02VT_CHITIET.SoLuongThucXuat)` WHERE `LyDoXuatKhoId = '982A0756-555B-44B3-8F0C-CBF1E062AC21'` + `SUM(PHIEUXUATKHO_HANGHOA.SoLuongThucNhap)` WHERE (`LyDoXuatId = '982A0756-555B-44B3-8F0C-CBF1E062AC21'` OR `IsCheBien = 1`) | 02VT + 04VT |
| 5 | `XuatCheBienTuThanNhapKhau` | `SUM(PHIEUXUATKHOTHAN_02VT_CHITIET.SoLuongThucXuat)` WHERE `LyDoXuatKhoId = '26d252c8-6edc-4c30-9d8c-2091e1d82430'` + `SUM(PHIEUXUATKHO_HANGHOA.SoLuongThucNhap)` WHERE (`LyDoXuatId = '26d252c8-6edc-4c30-9d8c-2091e1d82430'` OR `IsCheBien = 1`) | 02VT + 04VT |
| 6 | `XuatPhaTron` | `SUM(PHIEUXUATKHOTHAN_02VT_CHITIET.SoLuongThucXuat)` WHERE `LyDoXuatKhoId = 'B8FE5BE0-28CB-4EA8-8267-460A6EC7A66D'` + `SUM(PHIEUXUATKHO_HANGHOA.SoLuongThucNhap)` WHERE (`LyDoXuatId = 'B8FE5BE0-28CB-4EA8-8267-460A6EC7A66D'` OR `IsCheBien = 1`) | 02VT + 04VT |
| 7 | `TongNhap` | `NhapMua + NhapCBTuThanNhapKhau + NhapPhaTron` | Tổng hợp từ 3 cột nhập |
| 8 | `TongXuat` | `XuatBan + XuatCheBienTuThanNhapKhau + XuatPhaTron` | Tổng hợp từ 3 cột xuất |
| 9 | `TonDauKy` | `SoLuongTonKhoBanDau + NhapMua_TruocThang + NhapCBTuThanNhapKhau_TruocThang + NhapPhaTron_TruocThang - XuatBan_02VT_TruocThang - XuatCheBienTuThanNhapKhau_02VT_TruocThang - XuatPhaTron_02VT_TruocThang - XuatBan_04VT_TruocThang - XuatCheBienTuThanNhapKhau_04VT_TruocThang - XuatPhaTron_04VT_TruocThang` | CTE `TONDAUKY` |
| 10 | `TonCuoiKy` | `TonDauKy + TongNhap - TongXuat` | Tính trong SELECT cuối |
| 11 | `TonDauKy_AK` | `0` (gán cứng) | Cột AK không có nghiệp vụ |
| 12 | `TonCuoiKy_AK` | `0` (gán cứng) | Cột AK không có nghiệp vụ |
| 13 | `Id` | `NULL` | Trước đây lấy từ BAOCAO, nay không còn |
| 14 | `NgaySua` | `NULL` | Trước đây lấy từ BAOCAO, nay không còn |
| 15 | `DonViId` | `@iDonViId` | Truyền thẳng từ tham số |
| 16 | `PhongBanId` | `@iPhongBanId` | Truyền thẳng từ tham số |
| 17 | `IsXacNhan` | `0` (gán cứng) | Từng row không xác nhận riêng, chỉ có `@oIsXacNhanGrid` tổng thể |
| 18 | `TuNgay` | `@iTuNgay` | Truyền thẳng |
| 19 | `DenNgay` | `@iDenNgay` | Truyền thẳng |

---

## Các field **chưa có công thức** (đang gán `CAST(0 AS FLOAT)` / `CAST(0 AS BIT)` / chưa tính)

| STT | Field | Giá trị hiện tại | Lý do |
|--|--|--|--|
| 1 | `NhapMua_AK` | `CAST(0 AS FLOAT)` | Cột AK theo dõi riêng, chưa có logic |
| 2 | `NhapCBTuThanNhapKhau_AK` | `CAST(0 AS FLOAT)` | Cột AK theo dõi riêng, chưa có logic |
| 3 | `NhapPhaTron_AK` | `CAST(0 AS FLOAT)` | Cột AK theo dõi riêng, chưa có logic |
| 4 | `BinhQuanNgay_AK` | `CAST(0 AS FLOAT)` | Cột AK, chưa có logic |
| 5 | `XuatBan_AK` | `CAST(0 AS FLOAT)` | Cột AK, chưa có logic |
| 6 | `XuatCheBienTuThanNhapKhau_AK` | `CAST(0 AS FLOAT)` | Cột AK, chưa có logic |
| 7 | `XuatPhaTron_AK` | `CAST(0 AS FLOAT)` | Cột AK, chưa có logic |
| 8 | `NhapNoiBo` | `CAST(0 AS FLOAT)` | Chưa có phiếu nghiệp vụ nhập nội bộ |
| 9 | `NhapNoiBo_AK` | `CAST(0 AS FLOAT)` | Cột AK, chưa có logic |
| 10 | `NhapCBTuThanNhapKhau_ThuHoi` | `CAST(0 AS FLOAT)` | Chưa có phiếu nghiệp vụ thu hồi |
| 11 | `NhapCBTuSPNT` | `CAST(0 AS FLOAT)` | Chưa có phiếu nghiệp vụ |
| 12 | `NhapCBTuSPNT_ThuHoi` | `CAST(0 AS FLOAT)` | Chưa có phiếu nghiệp vụ |
| 13 | `NhapCBTuSPNT_AK` | `CAST(0 AS FLOAT)` | Cột AK, chưa có logic |
| 14 | `NhapCBTuThanSachBTP` | `CAST(0 AS FLOAT)` | Chưa có phiếu nghiệp vụ |
| 15 | `NhapCBTuThanSachBTP_ThuHoi` | `CAST(0 AS FLOAT)` | Chưa có phiếu nghiệp vụ |
| 16 | `NhapCBTuThanSachBTP_AK` | `CAST(0 AS FLOAT)` | Cột AK, chưa có logic |
| 17 | `NhapCBTuTNKNhapMua` | `CAST(0 AS FLOAT)` | Chưa có phiếu nghiệp vụ |
| 18 | `NhapCBTuTNKNhapMua_ThuHoi` | `CAST(0 AS FLOAT)` | Chưa có phiếu nghiệp vụ |
| 19 | `NhapCBTuTNKNhapMua_AK` | `CAST(0 AS FLOAT)` | Cột AK, chưa có logic |
| 20 | `XuatNoiBo` | `CAST(0 AS FLOAT)` | Chưa có phiếu nghiệp vụ xuất nội bộ |
| 21 | `XuatNoiBo_AK` | `CAST(0 AS FLOAT)` | Cột AK, chưa có logic |
| 22 | `XuatKhac` | `CAST(0 AS FLOAT)` | Chưa có phiếu nghiệp vụ xuất khác |
| 23 | `XuatKhac_AK` | `CAST(0 AS FLOAT)` | Cột AK, chưa có logic |
| 24 | `HaoHutTrongQuaTrinhXuatKho` | `CAST(0 AS FLOAT)` | Chưa có phiếu nghiệp vụ hao hụt |
| 25 | `HaoHutTrongQuaTrinhXuatKho_AK` | `CAST(0 AS FLOAT)` | Cột AK, chưa có logic |
| 26 | `BinhQuanThang_AK` | `CAST(0 AS FLOAT)` | Cột AK, chưa có logic |
| 27 | `ThuHoi` | `CAST(0 AS FLOAT)` | Chưa có phiếu nghiệp vụ |
| 28 | `ThuHoi_AK` | `CAST(0 AS FLOAT)` | Cột AK, chưa có logic |
| 29 | `HaoHutTrongQuaTrinhNhapKho` | `CAST(0 AS FLOAT)` | Chưa có phiếu nghiệp vụ |
| 30 | `HaoHutTrongQuaTrinhNhapKho_ThuHoi` | `CAST(0 AS FLOAT)` | Chưa có phiếu nghiệp vụ |
| 31 | `HaoHutTrongQuaTrinhNhapKho_AK` | `CAST(0 AS FLOAT)` | Cột AK, chưa có logic |
| 32 | `XuatCheBienTu_SPNT` | `CAST(0 AS FLOAT)` | Chưa có phiếu nghiệp vụ |
| 33 | `XuatCheBienTu_SPNT_AK` | `CAST(0 AS FLOAT)` | Cột AK, chưa có logic |
| 34 | `XuatCheBienTu_BTP` | `CAST(0 AS FLOAT)` | Chưa có phiếu nghiệp vụ |
| 35 | `XuatCheBienTu_BTP_AK` | `CAST(0 AS FLOAT)` | Cột AK, chưa có logic |
| 36 | `XuatCheBienTu_TNK_NhapMua` | `CAST(0 AS FLOAT)` | Chưa có phiếu nghiệp vụ |
| 37 | `XuatCheBienTu_TNK_NhapMua_AK` | `CAST(0 AS FLOAT)` | Cột AK, chưa có logic |
| 38 | `NhapKhac` | `CAST(0 AS FLOAT)` | Chưa có phiếu nghiệp vụ nhập khác |
| 39 | `IsXacNhan` (từng row) | `CAST(0 AS BIT)` | Chỉ có xác nhận tổng thể grid, không theo từng loại than |

---

## CTE trung gian

| CTE | Mục đích | Nguồn |
|--|--|--|
| `SOLUONGTONKHOBANDAU` | Tồn kho ban đầu | `KHOTHAN_TONKHO` |
| `NHAPMUA_TRUOCTHANG` | Nhập mua trước kỳ | 01VT, `dauThang -> @iTuNgay - 1` |
| `XUAT02_TRUOCTHANG` | Xuất 02VT trước kỳ | 02VT, `dauThang -> @iTuNgay - 1` |
| `NHAP03_TRUOCTHANG` | Nhập 03VT trước kỳ | 03VT (`PHIEUNHAPKHO`), `dauThang -> @iTuNgay - 1` |
| `XUAT04_TRUOCTHANG` | Xuất 04VT trước kỳ | 04VT (`PHIEUXUATKHO`), `dauThang -> @iTuNgay - 1` |
| `TONDAUKY` | Tồn đầu kỳ = tồn kho + nhập - xuất trước kỳ | Tổng hợp từ 5 CTE trên |
| `DANHMUCLOAITHAN` | Phát sinh trong kỳ từ 4 phiếu | 01VT, 02VT, 03VT, 04VT |
| `DANHMUCLOAITHAN_DISTINCT` | Dedup danh mục | `SELECT DISTINCT * FROM DANHMUCLOAITHAN` |

---

## Output parameters

| Parameter | Công thức | Nguồn |
|--|--|--|
| `@oTotalRow` | `COUNT(1) FROM @KETQUA` | Bảng tạm |
| `@oIds` | `STUFF(FOR XML PATH(';'))` từ `IDS_XACNHAN` | 4 phiếu đã xác nhận |
| `@oIsXacNhanGrid` | `1` nếu cả 4 nhóm `TotalCount = ConfirmCount`, `0` nếu ngược lại | 4 CTE `XACNHAN_01VT/02VT/03VT/04VT` |

---

## Trường hợp đặc biệt đang giữ nguyên

1. **`PHIEUXUATKHO.IsCheBien = 1`**: một phiếu xuất kho thường có IsCheBien = 1 sẽ được cộng đồng thời vào cả 3 cột:
   - `XuatBan`
   - `XuatCheBienTuThanNhapKhau`
   - `XuatPhaTron`
2. **`TongNhap`, `TongXuat`**: tính từ phát sinh 4 phiếu (không còn dùng ISNULL từ BAOCAO).
3. **`DANHMUCLOAITHAN_DISTINCT`**: dedup trước khi `GROUP BY` vào `@KETQUA`.
4. **Cột AK và các nghiệp vụ chưa có phiếu**: đang gán cứng `0`.

---

> ⚠️ Khi cần thêm logic cho các field đang gán `0` ở bảng "chưa có công thức" ở trên, cần xác định phiếu nguồn tương ứng và viết CTE/UNION ALL bổ sung.
