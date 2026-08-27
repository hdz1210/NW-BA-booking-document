# TÀI LIỆU PHÂN TÍCH YÊU CẦU NGHIỆP VỤ (BUSINESS REQUIREMENTS DOCUMENT - BRD)
## HỆ THỐNG QUẢN TRỊ & VẬN HÀNH BOOKING BẤT ĐỘNG SẢN (NEWWAY BOOKING)
### CHUẨN HÓA TOÀN DIỆN THEO QUY TRÌNH 7 MỤC (UNTITLED.PDF) & CHI TIẾT SEQUENCE DIAGRAMS

---

## 1. TỔNG QUAN HỆ THỐNG (SYSTEM OVERVIEW)

### 1.1. Mục đích & Phạm vi
Tài liệu này đặc tả chi tiết toàn bộ kiến trúc nghiệp vụ, quy trình vận hành, luồng tương tác giữa các tác nhân và yêu cầu chức năng của **Hệ thống Quản lý Booking, Giỏ hàng độc quyền, Tiến độ Pháp lý ($T+n$) và Cơ chế Hoa hồng** (chuẩn hóa 100% theo sơ đồ quy trình 7 mục của `Untitled.pdf`).

Hệ thống số hóa toàn diện 3 phân hệ liên thông thời gian thực:
- **Phân hệ Sales Kinh doanh (4 Màn hình / Tab)**: Tạo booking wizard 4 bước, Quản lý danh sách booking (Lên cọc, Hoàn booking, Dồn căn), Bảng hàng Stacking Matrix (Lock Cọc 30p, Lock Thiện chí 24h, Xung đột 30p), Đề xuất hoa hồng 3 cấp.
- **Phân hệ Kế toán (Accounting Module - 5 Tab)**: Đối soát tiền vào theo 6 luồng giao dịch & banner cảnh báo khóa căn khẩn cấp, Gom chuyển CĐT, Quản lý hoàn tiền, Lịch sử sổ cái dòng tiền, Chi trả hoa hồng & Xác nhận Mã FT đi tiền cọc ôm.
- **Phân hệ Admin Vận hành (Admin Module - 8 Tab)**: Duyệt booking cấp phiếu giữ chỗ, Ráp căn CĐT (MATCHED), Hợp đồng & Cọc chính thức, Hoàn cọc / Đổi căn, Booking ôm nội bộ, Bảng hàng độc quyền & Lock cọc 30p/24h (Mục 4), Tiến độ pháp lý $T+n$ & Phạt chậm ký (Mục 5&6), Duyệt cơ chế hoa hồng đa cấp 4 bước (Mục 7).

---

## 2. ÁNH XẠ 7 MỤC QUY TRÌNH TRONG UNTITLED.PDF VÀO CÁC MÀN HÌNH

| STT | Mục trong `Untitled.pdf` | Màn hình đảm nhiệm trong Hệ thống | Đường dẫn tài liệu BA chi tiết |
| :---: | :--- | :--- | :--- |
| **1** | **Chuẩn bị (Preparation)** | - KT Tab 1 (Tạo STK, QR, 6 luồng tiền)<br>- Admin Tab 5 & KT Tab 5 (Chọn nhân sự HR, Cú pháp CK, Kế toán đi tiền cọc ôm) | 🔗 [KT Tab 1](./ke_toan/TAB_01_DOI_SOAT_VA_TRA_SOAT_TIEN_VAO.md)<br>🔗 [Admin Tab 5](./admin/TAB_05_QUAN_LY_BOOKING_OM_NOI_BO.md) |
| **2** | **Trong khi Booking** | - KT Tab 1 (Kiểm tra bill, Khớp mã FT, Nhận diện luồng cọc/lock)<br>- Admin Tab 1 (Thẩm định CCCD, Phê duyệt, Sinh Phiếu giữ chỗ PDF) | 🔗 [KT Tab 1](./ke_toan/TAB_01_DOI_SOAT_VA_TRA_SOAT_TIEN_VAO.md)<br>🔗 [Admin Tab 1](./admin/TAB_01_DUYET_BOOKING_VA_PHIEU_GIU_CHO.md) |
| **3** | **Hoàn thành Booking & Lên Cọc** | - Admin Tab 2 (Ráp căn CĐT: MATCHED)<br>- Sales Tab 2 (Bấm "Lên Cọc", Xuất Phiếu Đặt Cọc, Upload file ký)<br>- Admin Tab 3 (Duyệt Hợp đồng Cọc, Khóa căn ĐÃ CỌC)<br>- KT Tab 3 (Lệnh chi hoàn tiền cọc thiện chí / Xung đột 30p) | 🔗 [Admin Tab 2](./admin/TAB_02_GHEP_CAN_VA_DAY_GIU_CHO_CDT.md)<br>🔗 [Admin Tab 3](./admin/TAB_03_QUAN_LY_HOP_DONG_VA_COC_CHET.md)<br>🔗 [KT Tab 3](./ke_toan/TAB_03_QUAN_LY_HOAN_TIEN_REFUND.md) |
| **4** | **Giỏ hàng độc quyền & Lock căn** | - Sales Tab 3 & Admin Tab 6 (Bảng hàng độc quyền, Lock Cọc 30p, Lock Cọc Thiện Chí 24h, Xử lý xung đột 30p, Sync Google Sheets)<br>- KT Tab 1 (Banner cam ưu tiên đối soát FT trong 30p) | 🔗 [Admin Tab 6](./admin/TAB_05_BANG_HANG_DOC_QUYEN_VA_LOCK_CAN.md)<br>🔗 [Sales Tab 3](./sales/02_BA_SALES_LUONG_SAU_KHI_TAO_BOOKING.md) |
| **5** | **Các thủ tục ký (Tiến độ $T+n$)** | - Admin Tab 7 (Tính hạn $T+3, TTĐC+15$, Phân nhánh Tra soát CĐT $\leftrightarrow$ Ký CN TTĐC/HĐMB, Tiền phạt CĐT, PTG Rule) | 🔗 [Admin Tab 7](./admin/TAB_06_TIEN_DO_PHAP_LY_VA_TIEN_PHAT.md) |
| **6** | **Giỏ hàng chung / chéo** | - Admin Tab 7 (Quản lý tiến độ ký kết cho quỹ căn chung/chéo của CĐT & liên minh sàn) | 🔗 [Admin Tab 7](./admin/TAB_06_TIEN_DO_PHAP_LY_VA_TIEN_PHAT.md) |
| **7** | **Cơ chế Hoa hồng & Dồn tiền** | - Sales Tab 4 & Admin Tab 8 (Duyệt cơ chế 4 cấp: Sales $\rightarrow$ TP $\rightarrow$ BLĐ $\rightarrow$ Admin xác nhận, Tra soát dồn tiền sang căn khác)<br>- KT Tab 5 (Hạch toán chi trả hoa hồng qua Mã FT) | 🔗 [Admin Tab 8](./admin/TAB_07_CO_CHE_HOA_HONG_DA_CAP_VA_DON_TIEN.md)<br>🔗 [KT Tab 5](./ke_toan/TAB_05_CHI_TRA_HOA_HONG_VA_LENH_DI_TIEN_OM.md) |

---

## 3. CHI TIẾT CÁC SEQUENCE DIAGRAM THEO TỪNG LUỒNG NGHIỆP VỤ

Dưới đây là đặc tả trình tự tương tác (Sequence Diagrams) được phân tách chi tiết theo 10 kịch bản nghiệp vụ cốt lõi trong hệ thống:

---

### 🟢 LUỒNG 1: Thu Tiền Booking Thiện Chí & Xác Thực Dòng Tiền (Booking Flow)
*Áp dụng khi khách hàng nộp tiền đặt chỗ ưu tiên trong giai đoạn mở bán dự án.*

```mermaid
sequenceDiagram
    autonumber
    actor KH as Khách hàng
    actor Sales as Chuyên viên Sales
    participant App as Hệ thống NewWay Booking
    actor KT as Kế toán (Tab 1)
    actor Admin as Admin Vận hành (Tab 1)

    KH->>Sales: Cung cấp thông tin (CCCD, SĐT, Email) & Nguyện vọng căn
    Sales->>App: Tạo Booking (Wizard 4 Bước) & Tạo mã QR thanh toán
    KH->>App: Quét mã QR chuyển khoản tiền booking (VD: 100.000.000 đ)
    Sales->>App: Tải ảnh Bill chuyển khoản & Bấm "Gửi Booking"
    App-->>KT: Đẩy hồ sơ vào Hàng đợi Tab 1 (Luồng: "Booking Thiện Chí Mở Bán")

    rect rgb(240, 248, 255)
        note over KT,App: Kế toán đối soát sao kê ngân hàng
        KT->>App: Mở chi tiết giao dịch, kiểm tra số tiền & bill
        alt Tiền nổi đủ & đúng cú pháp
            KT->>App: Nhập Mã FT ngân hàng + Chọn "Đã nhận đủ tiền" -> Bấm "Xác nhận đối soát"
            App->>>Admin: Cập nhật trạng thái "ĐÃ ĐỐI SOÁT", chuyển sang Admin Tab 1
        else Tiền thiếu / Sai cú pháp / Chưa nổi
            KT->>App: Chọn "Cần tra soát" + Nhập nội dung cần làm rõ
            App-->>Sales: Thông báo cảnh báo tra soát trên Dashboard
        end
    end

    Admin->>App: Mở Tab 1 Admin -> Thẩm định thông tin CCCD & Pháp lý
    Admin->>App: Bấm "Phê duyệt Booking"
    App->>App: Tự động tạo Phiếu Đăng Ký Giữ Chỗ PDF (kèm mã QR định danh)
    App-->>Sales: Thông báo hồ sơ "ĐÃ BOOKING THÀNH CÔNG"
    Sales-->>KH: Gửi Phiếu Giữ Chỗ PDF có dấu mộc điện tử cho khách
```

---

### 🔵 LUỒNG 2: Gom Đợt Booking & Chuyển Tiền Cho Chủ Đầu Tư (CDT Aggregation Flow)
*Kế toán tổng hợp các booking đủ điều kiện để chuyển khoản sang CĐT theo đợt đóng giỏ hàng.*

```mermaid
sequenceDiagram
    autonumber
    actor Admin as Admin Vận hành
    actor KT as Kế toán (Tab 2)
    participant App as Hệ thống NewWay Booking
    actor Bank as Ngân hàng / CĐT

    Admin->>App: Chốt danh sách Booking đủ giấy tờ & bill hợp lệ
    KT->>App: Truy cập Tab 2 (Gom & Chuyển CĐT) -> Bấm "Tạo đợt gom mới"
    App->>App: Tự động tổng hợp số lượng căn & tổng số tiền (VD: 3 căn = 300.000.000 đ)
    KT->>App: Kiểm tra danh sách Booking trong đợt -> Bấm "Chốt danh sách đợt gom"
    
    KT->>Bank: Thực hiện lệnh chuyển khoản sang STK Chủ đầu tư
    Bank-->>KT: Trả về Biên lai / Ủy nhiệm chi (UNC) có Mã FT
    KT->>App: Tải file UNC + Nhập Mã FT chuyển tiền CĐT -> Bấm "Xác nhận đã chuyển CĐT"
    App->>App: Tự động sinh Email/Biên bản giao nhận đợt gom
    KT->>Bank: Gửi Email danh sách kèm UNC sang bộ phận Kế toán CĐT
    App-->>Admin: Cập nhật đợt gom: "HOÀN TẤT CHUYỂN TIỀN CĐT"
```

---

### 🟣 LUỒNG 3: Mở Bán, Ráp Căn & Chuyển Sang Cọc Chính Thức ("Lên Cọc")
*Quy trình khi CĐT công bố giỏ hàng, khách chọn được căn ưng ý và chuyển thành Đặt cọc chính thức.*

```mermaid
sequenceDiagram
    autonumber
    actor CDT as Chủ Đầu Tư
    actor Admin as Admin (Tab 2 & 3)
    participant App as Hệ thống NewWay Booking
    actor Sales as Chuyên viên Sales (Tab 2)
    actor KH as Khách hàng
    actor KT as Kế toán (Tab 1 & 5)

    CDT-->>Admin: Trả bảng danh sách căn phân bổ cho sàn
    Admin->>App: Tab 2 (Ráp căn): Ghép mã căn (VD: RP-12.08) vào Booking của khách
    App->>App: Cập nhật trạng thái Booking: "MATCHED (Đã khớp căn)"
    App-->>Sales: Thông báo: "Khách hàng Nguyễn Minh Anh đã khớp căn RP-12.08"

    Sales->>KH: Thông báo kết quả khớp căn & xác nhận chốt mua
    KH-->>Sales: Đồng ý chốt mua căn RP-12.08
    Sales->>App: Tab 2 (Sales): Bấm "LÊN CỌC"
    App->>App: Tự động xuất "Thỏa Thuận Đặt Cọc (PDF)" chuẩn pháp lý
    Sales->>KH: Đưa Thỏa thuận đặt cọc cho khách hàng ký
    KH-->>Sales: Ký tên hoàn tất thỏa thuận đặt cọc
    Sales->>App: Tải file PDF Scan Thỏa thuận cọc đã ký -> Bấm "Gửi duyệt cọc"

    Admin->>App: Tab 3 (Admin): Kiểm tra Checklist 4 bước (CCCD, Căn, Chữ ký HĐ, Cơ chế HH)
    Admin->>App: Nhập Số Hợp Đồng (Số HĐ) -> Bấm "Phê duyệt Hợp đồng cọc"
    App->>App: Khóa trạng thái căn hộ: "ĐÃ CỌC" (Locked Official) vĩnh viễn trên Bảng hàng

    App-->>KT: Đẩy giao dịch vào Tab 1 KT (Badge: "✅ Lên Cọc Chính Thức · RP-12.08")
    KT->>App: Xác nhận dòng tiền cọc chính thức vào Sổ cái doanh thu
    App-->>Sales: Mở khóa quyền gửi "ĐỀ XUẤT HOA HỒNG 3 CẤP"
```

---

### 🔥 LUỒNG 4: Lock Cọc Trực Tiếp 30 Phút Trên Bảng Hàng Matrix (Direct 30-Minute Lock)
*Áp dụng khi khách hàng chốt mua trực tiếp căn độc quyền trên Bảng hàng Stacking Matrix.*

```mermaid
sequenceDiagram
    autonumber
    actor KH as Khách hàng
    actor Sales as Chuyên viên Sales
    participant App as Hệ thống NewWay Booking
    actor KT as Kế toán (Tab 1)
    actor Admin as Admin Vận hành

    KH->>Sales: Yêu cầu mua trực tiếp căn độc quyền (VD: RP-12.08)
    Sales->>App: Tab 3 (Bảng hàng Matrix): Click vào ô căn RP-12.08
    Sales->>App: Chọn chế độ "Lock Cọc (30 phút)" -> Bấm "Xác nhận Lock Căn"
    
    rect rgb(255, 243, 230)
        note over App,KT: KÍCH HOẠT ĐẾM NGƯỢC 30 PHÚT KHẨN CẤP
        App->>App: Tạm khóa căn RP-12.08 trên toàn hệ thống sàn & đồng bộ Google Sheets
        App->>App: Hiển thị bộ đếm ngược 30:00 và sinh mã QR chuyển khoản cọc
        App-->>KT: Đẩy giao dịch lên ĐẦU hàng đợi Tab 1 (Banner cam khẩn cấp: "Còn 30p")
    end

    KH->>App: Quét QR chuyển khoản 100.000.000 đ tiền cọc
    Sales->>App: Tải ảnh UNC/Bill chuyển tiền của khách
    KT->>App: Mở Tab 1 KT (Thấy Banner cam: Còn 18 phút) -> Khớp mã FT -> Bấm "Xác nhận đã nhận tiền"
    Sales->>KH: In Thỏa thuận đặt cọc cho khách ký -> Tải file scan lên App
    Admin->>App: Duyệt hợp đồng cọc -> Căn chuyển thành "ĐÃ CỌC" vĩnh viễn
```

---

### ⏳ LUỒNG 5: Lock Cọc Thiện Chí 24 Giờ & Cơ Chế Xử Lý Xung Đột 30 Phút (Conflict Resolution)
*Kịch bản giải quyết xung đột khi Căn đang giữ thiện chí (Sales A) bị Sales B có khách vào Cọc trực tiếp.*

```mermaid
sequenceDiagram
    autonumber
    actor KHA as Khách hàng A (Giữ 24h)
    actor SalesA as Sales A
    actor KHB as Khách hàng B (Cọc ngay)
    actor SalesB as Sales B
    participant App as Hệ thống NewWay Booking
    actor Admin as Admin Vận hành
    actor KT as Kế toán (Tab 1 & 3)

    KHA->>SalesA: Nộp cọc thiện chí giữ căn RP-18.05 trong 24 giờ
    SalesA->>App: Chọn "Lock Thiện Chí (24h)" -> KT đối soát -> Căn hiển thị "Lock Thiện Chí (24h)"
    
    note over SalesB,App: XUẤT HIỆN KHÁCH HÀNG B MUỐN VÀO CỌC NGAY
    KHB->>SalesB: Quyết định vào Cọc chính thức ngay lập tức căn RP-18.05
    SalesB->>App: Click vào căn RP-18.05 -> Bấm "YÊU CẦU VÀO CỌC (KÍCH HOẠT XUNG ĐỘT)"
    
    rect rgb(255, 235, 235)
        note over App,SalesA: KÍCH HOẠT THỜI GIAN ÂN HẠN 30 PHÚT CHO SALES A
        App->>App: Khởi động đồng hồ đếm ngược Ân hạn 30:00
        App-->>SalesA: Gửi cảnh báo: "Căn RP-18.05 có khách B vào cọc. Bạn có 30p để nâng cấp cọc!"
        App-->>Admin: Hiển thị cảnh báo xung đột căn tại Tab 6 Admin
    end

    alt NHÁNH 1: Khách A đồng ý lên Cọc (Ưu tiên khách giữ chỗ)
        SalesA->>App: Bấm "NÂNG CẤP LÊN CỌC CHÍNH THỨC"
        KHA->>App: Nộp đủ tiền & Ký Thỏa thuận cọc -> Admin duyệt -> Khách A sở hữu căn
        App-->>SalesB: Thông báo: "Khách A đã vào cọc, căn đã bán"
    else NHÁNH 2: Khách A nhường quyền hoặc HẾT 30 PHÚT
        App->>App: Hết 30p hoặc Sales A bấm "Nhường quyền mua"
        App->>App: Chuyển quyền mua sang cho Sales B (Kích hoạt luồng Lock Cọc 30p cho Khách B)
        KHB->>App: Nộp tiền & ký cọc -> Căn thuộc về Khách B ("ĐÃ CỌC")
        App-->>KT: Tự động tạo lệnh hoàn tiền Khách A tại Tab 3 (Lý do: "Nhường quyền xung đột 30p")
        KT->>KHA: Thực hiện chuyển khoản hoàn trả 100% tiền cọc thiện chí cho Khách A
    end
```

---

### 💸 LUỒNG 6: Quy Trình Hoàn Tiền Booking / Rút Cọc (Refund Workflow)
*Áp dụng khi không ráp được căn, khách rút cọc thiện chí đúng hạn hoặc nhường quyền xung đột.*

```mermaid
sequenceDiagram
    autonumber
    actor KH as Khách hàng
    actor Sales as Chuyên viên Sales
    participant App as Hệ thống NewWay Booking
    actor Admin as Admin (Tab 4)
    actor KT as Kế toán (Tab 3)
    actor Bank as Ngân hàng

    KH->>Sales: Yêu cầu hoàn lại tiền booking / cọc thiện chí
    Sales->>App: Tab 2 (Sales): Chọn hồ sơ -> Bấm "Yêu Cầu Hoàn Booking"
    Sales->>App: Nhập thông tin tài khoản nhận hoàn (STK, Ngân hàng, Tên chủ tài khoản) + Lý do hoàn
    App->>App: Tự động xuất "Phiếu Đề Nghị Hoàn Booking (PDF)"
    KH->>Sales: Ký Phiếu Đề Nghị Hoàn Tiền (ký giấy hoặc ký điện tử)
    Sales->>App: Tải file scan Phiếu Hoàn đã ký -> Bấm "Gửi yêu cầu hoàn"

    Admin->>App: Tab 4 (Admin): Kiểm tra điều kiện hoàn tiền & thời hạn hợp lệ
    Admin->>App: Bấm "Phê duyệt lệnh hoàn"
    App-->>KT: Chuyển hồ sơ sang Tab 3 Kế toán (Trạng thái: "ĐÃ DUYỆT HOÀN")

    KT->>App: Tab 3 (KT): Mở chi tiết lệnh chi hoàn tiền
    KT->>Bank: Thực hiện lệnh chuyển tiền hoàn trả về STK khách hàng
    Bank-->>KT: Trả về UNC chuyển khoản hoàn tiền thành công
    KT->>App: Tải file UNC hoàn tiền + Nhập Mã FT hoàn tiền -> Bấm "Xác nhận đã hoàn booking"
    App->>App: Cập nhật trạng thái Booking: "ĐÃ HOÀN TIỀN" (Closed)
    App-->>KH: Gửi thông báo SMS/Email xác nhận đã hoàn trả tiền thành công
```

---

### 🔄 LUỒNG 7: Tra Soát Dồn Tiền / Đổi Căn Hộ (Exchange & Transfer Flow)
*Áp dụng khi khách chuyển dòng tiền cọc từ căn cũ sang căn mới cùng dự án hoặc khác dự án.*

```mermaid
sequenceDiagram
    autonumber
    actor KH as Khách hàng
    actor Sales as Chuyên viên Sales
    participant App as Hệ thống NewWay Booking
    actor Admin as Admin (Tab 4)
    actor KT as Kế toán (Tab 1 & 4)

    KH->>Sales: Yêu cầu đổi từ Căn Nguồn (RP-12.08) sang Căn Đích (RP-18.05)
    Sales->>App: Tab 2 (Sales): Chọn Booking -> Bấm "Tra Soát / Dồn Tiền"
    Sales->>App: Chọn Mã Căn Đích (RP-18.05) + Nhập chênh lệch tiền cọc (nếu có)
    App->>App: Hiển thị bảng so sánh đối soát 2 căn (Giá, Diện tích, Tiến độ)
    Sales->>App: Tải file Đơn xin chuyển nhượng / đổi căn có chữ ký khách -> Bấm "Gửi yêu cầu"

    Admin->>App: Tab 4 (Admin): Thẩm định quỹ căn đích và chấp thuận chuyển cọc
    Admin->>App: Bấm "Phê duyệt Đổi Căn"
    App->>App: Mở lại căn cũ RP-12.08 về trạng thái "TRỐNG", tạm khóa căn mới RP-18.05

    App-->>KT: Đẩy giao dịch vào Tab 1 KT (Luồng: "🔄 Tra Soát Dồn Căn: RP-12.08 ➔ RP-18.05")
    KT->>App: Kiểm tra bút toán điều chuyển dòng tiền -> Bấm "Khớp chuyển nguồn cọc"
    App->>App: Ghi nhận mã FT cũ liên kết với mã căn mới RP-18.05 trong Lịch sử giao dịch (Tab 4 KT)
    App-->>Sales: Thông báo: "Đổi căn thành công sang RP-18.05"
```

---

### 👥 LUỒNG 8: Quản Lý Quy Trình Booking Ôm Nội Bộ (Bulk Hold Flow)
*Sàn sử dụng nhân sự HR đứng tên để đặt cọc gom quỹ căn độc quyền từ Chủ đầu tư.*

```mermaid
sequenceDiagram
    autonumber
    actor BLD as Ban Lãnh Đạo Sàn
    actor Admin as Admin (Tab 5)
    actor HR as Nhân Sự HR Đứng Tên
    actor KT as Kế toán (Tab 5)
    participant App as Hệ thống NewWay Booking
    actor CDT as Chủ Đầu Tư

    BLD->>Admin: Phê duyệt kế hoạch ôm giỏ căn dự án NewWay Riverside
    Admin->>App: Tab 5 (Admin): Chọn Danh sách Nhân sự HR từ Master Data
    Admin->>App: Gán Nhân sự HR (VD: Vũ Minh Quân) + Gán Sales chăm sóc + Gán chỉ tiêu căn
    App->>App: Tự động sinh "Hợp Đồng Hợp Tác Booking Ôm Nội Bộ (PDF)"
    HR->>Admin: Ký xác nhận Hợp đồng hợp tác đứng tên cọc ôm
    Admin->>App: Tải file HĐ Ôm đã ký -> Chuyển sang Kế toán đi tiền

    KT->>App: Tab 5 (KT - Đi tiền ôm): Kiểm tra danh sách hợp đồng ôm
    KT->>CDT: Thực hiện lệnh chuyển tiền cọc ôm sang CĐT (VD: 100.000.000 đ/căn)
    CDT-->>KT: Trả về biên lai thu tiền / Phiếu giữ chỗ mang tên Nhân sự HR
    KT->>App: Tải UNC chuyển CĐT + Nhập Mã FT -> Bấm "Xác nhận đã đi tiền cọc ôm"
    
    App->>App: Đưa quỹ căn ôm vào "BẢNG HÀNG ĐỘC QUYỀN" (Admin Tab 6 & Sales Tab 3)
    App-->>BLD: Báo cáo: Quỹ căn ôm đã sẵn sàng mở bán độc quyền
```

---

### ⚖️ LUỒNG 9: Tiến Độ Pháp Lý $T+n$, Phạt Chậm Ký & PTG Rule (Legal $T+n$ Compliance)
*Theo dõi mốc thời gian ký TTĐC, HĐMB và điều kiện hoàn vốn trước khi chuyển nhượng Hợp đồng.*

```mermaid
sequenceDiagram
    autonumber
    actor Sales as Chuyên viên Sales
    actor KH as Khách hàng
    actor Admin as Admin (Tab 7)
    participant App as Hệ thống NewWay Booking
    actor CDT as Chủ Đầu Tư

    note over App,Admin: CĂN ĐÃ CỌC -> HỆ THỐNG TỰ ĐỘNG TÍNH HẠN PHÁP LÝ T+n
    App->>App: Thiết lập mốc: Hạn ký TTĐC = Ngày cọc + 3 ngày (T+3)
    App->>App: Thiết lập mốc: Hạn ký HĐMB = Ngày ký TTĐC + 15 ngày (TTĐC+15)

    alt Giao dịch Căn Khách Thường
        Admin->>App: Tab 7 (Admin): Theo dõi tiến độ ký kết TTĐC / HĐMB
        KH->>CDT: Ký Thỏa thuận đặt cọc / Hợp đồng mua bán đúng hạn
        Admin->>App: Tải file HĐMB đã ký + Cập nhật "HOÀN TẤT KÝ PHÁP LÝ"
    else Giao dịch Căn Ôm Nội Bộ (Áp dụng PTG Rule)
        Admin->>App: Kiểm tra điều kiện PTG Rule: "Khách hàng đã nộp đủ 100% tiền hoàn vốn cọc ôm?"
        alt CHƯA NỘP ĐỦ 100% TIỀN
            App->>Admin: KHÓA NÚT KÝ CHUYỂN NHƯỢNG (Cảnh báo: Phải thu đủ vốn ôm)
            Admin-->>Sales: Yêu cầu thu đủ tiền từ khách hàng
        else ĐÃ NỘP ĐỦ 100% TIỀN
            App->>Admin: MỞ KHÓA KÝ CHUYỂN NHƯỢNG
            Admin->>CDT: Nộp hồ sơ chuyển nhượng từ Nhân sự HR sang Khách hàng thật
            CDT-->>Admin: Xác nhận sang tên Hợp đồng thành công
        end
    end

    opt Trường hợp Khách chậm ký quá hạn quy định
        App->>Admin: Cảnh báo quá hạn T+n -> Tự động tính số ngày trễ
        Admin->>App: Nhập % hoặc số tiền phạt chậm ký CĐT (VD: 5.000.000 đ)
        App->>App: Ghi nhận tiền phạt vào Chi phí khấu trừ hoa hồng
    end
```

---

### 💰 LUỒNG 10: Đề Xuất & Chi Trả Hoa Hồng 3 Cấp (Commission Approval & Payout)
*Quy trình tính toán phân chia hoa hồng, phê duyệt 3 cấp và kế toán thực hiện chi trả.*

```mermaid
sequenceDiagram
    autonumber
    actor Sales as Chuyên viên Sales
    participant App as Hệ thống NewWay Booking
    actor TP as Trưởng Phòng Kinh Doanh
    actor BLD as Ban Lãnh Đạo Sàn
    actor Admin as Admin (Tab 8)
    actor KT as Kế toán (Tab 5)
    actor Bank as Ngân hàng

    note over Sales,App: ĐIỀU KIỆN: GIAO DỊCH ĐÃ CỌC HỢP LỆ & KẾ TOÁN XÁC NHẬN TIỀN
    Sales->>App: Tab 4 (Sales): Bấm "Tạo Đề Xuất Hoa Hồng"
    Sales->>App: Nhập cơ chế phân bổ: Sales chính (%), Co-broker (%), Thưởng nóng (VNĐ)
    App->>App: Tự động tính tổng tiền thực nhận sau thuế TNCN 10%
    Sales->>App: Bấm "Gửi phê duyệt Đề xuất hoa hồng"

    rect rgb(245, 245, 255)
        note over TP,Admin: LUỒNG DUYỆT 3 CẤP LIÊN THÔNG
        App-->>TP: Chuyển thông báo duyệt cấp 1 (Trưởng phòng)
        TP->>App: Kiểm tra doanh số & Bấm "Duyệt Đề xuất (Cấp 1)"
        
        App-->>BLD: Chuyển thông báo duyệt cấp 2 (Ban Lãnh Đạo)
        BLD->>App: Thẩm định chính sách thưởng & Bấm "Duyệt Đề xuất (Cấp 2)"
        
        App-->>Admin: Chuyển sang Admin Tab 8 xác thực hồ sơ pháp lý giao dịch
        Admin->>App: Bấm "Xác nhận hồ sơ đủ điều kiện chi trả hoa hồng"
    end

    App-->>KT: Đẩy lệnh chi sang Tab 5 Kế toán (Danh sách chờ chi hoa hồng)
    KT->>App: Tab 5 (KT): Kiểm tra danh sách người thụ hưởng & STK ngân hàng
    KT->>Bank: Thực hiện lệnh chuyển tiền hoa hồng vào tài khoản Sales / Co-broker
    Bank-->>KT: Báo chuyển khoản thành công có Mã FT
    KT->>App: Nhập Mã FT chi hoa hồng -> Bấm "Xác nhận đã chi trả"
    App->>App: Cập nhật trạng thái: "ĐÃ CHI TRẢ HOA HỒNG"
    App-->>Sales: Gửi thông báo & Phiếu lương hoa hồng điện tử
```

---

## 4. DANH MỤC CÁC TÀI LIỆU BA CHI TIẾT

### 🏛️ Phân hệ Kế toán (5 Tab):
1. 🔗 [TAB 1: Đối Soát & Tra Soát Tiền Vào](./ke_toan/TAB_01_DOI_SOAT_VA_TRA_SOAT_TIEN_VAO.md)
2. 🔗 [TAB 2: Gom Booking & Chuyển Tiền CĐT](./ke_toan/TAB_02_GOM_BOOKING_VA_CHUYEN_TIEN_CDT.md)
3. 🔗 [TAB 3: Quản Lý Hoàn Tiền (Refund)](./ke_toan/TAB_03_QUAN_LY_HOAN_TIEN_REFUND.md)
4. 🔗 [TAB 4: Lịch Sử Giao Dịch & Dòng Tiền](./ke_toan/TAB_04_LICH_SU_GIAO_DICH_VA_DONG_TIEN.md)
5. 🔗 [TAB 5: Chi Trả Hoa Hồng & Đi Tiền Cọc Ôm](./ke_toan/TAB_05_CHI_TRA_HOA_HONG_VA_LENH_DI_TIEN_OM.md)

### 🏢 Phân hệ Admin Vận hành (8 Tab):
1. 🔗 [TAB 1: Duyệt Booking & Sinh Phiếu Giữ Chỗ](./admin/TAB_01_DUYET_BOOKING_VA_PHIEU_GIU_CHO.md)
2. 🔗 [TAB 2: Ghép Căn & Đẩy Giữ Chỗ CĐT](./admin/TAB_02_GHEP_CAN_VA_DAY_GIU_CHO_CDT.md)
3. 🔗 [TAB 3: Quản Lý Hợp Đồng & Cọc](./admin/TAB_03_QUAN_LY_HOP_DONG_VA_COC_CHET.md)
4. 🔗 [TAB 4: Hoàn Booking & Đổi Căn Không Khớp](./admin/TAB_04_HOAN_BOOKING_VA_DOI_CAN.md)
5. 🔗 [TAB 5: Quản Lý Booking Ôm Nội Bộ](./admin/TAB_05_QUAN_LY_BOOKING_OM_NOI_BO.md)
6. 🔗 [TAB 6: Bảng Hàng Độc Quyền & Khóa Căn 30p/24h (Mục 4 PDF)](./admin/TAB_05_BANG_HANG_DOC_QUYEN_VA_LOCK_CAN.md)
7. 🔗 [TAB 7: Tiến Độ Pháp Lý $T+n$ & Phạt Chậm Ký CĐT (Mục 5&6 PDF)](./admin/TAB_06_TIEN_DO_PHAP_LY_VA_TIEN_PHAT.md)
8. 🔗 [TAB 8: Cơ Chế Hoa Hồng Đa Cấp & Dồn Tiền (Mục 7 PDF)](./admin/TAB_07_CO_CHE_HOA_HONG_DA_CAP_VA_DON_TIEN.md)

### 📱 Phân hệ Sales Kinh doanh (Chuyên viên tư vấn):
1. 🔗 [PHẦN 1: Quy trình Tạo Booking Khách Hàng (Wizard 4 Bước)](./sales/01_BA_SALES_LUONG_TAO_BOOKING.md)
2. 🔗 [PHẦN 2: Quy trình Hậu Booking, Khớp Căn, Lock Căn Độc Quyền & Hoa Hồng](./sales/02_BA_SALES_LUONG_SAU_KHI_TAO_BOOKING.md)

---
*Tài liệu BRD chuẩn hóa toàn diện - Phiên bản khớp 100% cấu trúc hệ thống NewWay Booking (Admin, Kế toán, Sales).*
