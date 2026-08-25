# TÀI LIỆU PHÂN TÍCH YÊU CẦU NGHIỆP VỤ (BA SPECIFICATION)
# PHÂN HỆ KẾ TOÁN - TAB 3: QUẢN LÝ HOÀN TIỀN (REFUND)

**Dự án:** Hệ thống Quản trị & Vận hành Booking BĐS (NewWay Booking - Final Flow)  
**Phân hệ:** Vận hành Kế toán (Accounting Module)  
**Mã màn hình:** `ACC_TAB_03_REFUND_MANAGEMENT`

---

## 1. TỔNG QUAN (OVERVIEW)

### 1.1. Mục tiêu nghiệp vụ
1. **Tiếp nhận Yêu cầu Hoàn Tiền Đã Phê Duyệt**:
   - Tiếp nhận các yêu cầu hoàn cọc thiện chí từ Khách hàng không khớp được căn ưng ý tại lễ mở bán hoặc Khách hàng xin rút cọc đúng quy định (đã được Admin phê duyệt tại Tab 4 Admin).
2. **Chi Trả Hoàn Tiền Cho Khách Hàng**:
   - Kế toán kiểm tra số tiền hoàn, tài khoản thụ hưởng (Ngân hàng, Số tài khoản, Tên chủ tài khoản) $\rightarrow$ Thực hiện lệnh chuyển khoản hoàn tiền.
   - Nhập `Mã FT hoàn tiền`, ngày giờ hoàn và upload file Ủy Nhiệm Chi hoàn tiền.
3. **Cập nhật Trạng thái & Ghi nhận Sổ cái**:
   - Bấm **"Xác nhận đã hoàn tiền"** $\rightarrow$ Hệ thống đổi trạng thái sang `Đã hoàn booking`, ghi nhận bản ghi chi hoàn cọc vào **Tab 4 Lịch sử giao dịch**.

### 1.2. Sơ đồ Luồng Nghiệp vụ (Process Flow)

```mermaid
flowchart TD
    A["Admin duyệt Đề xuất Hoàn Cọc (Tab 4 Admin)"] --> B["Hồ sơ chuyển sang Tab 3 Kế toán (Trạng thái: Đã duyệt hoàn)"]
    B --> C["Kế toán chọn hồ sơ & kiểm tra thông tin STK thụ hưởng"]
    C --> D["Kế toán thực hiện Chuyển khoản hoàn tiền từ TK Công ty về TK Khách"]
    D --> E["Kế toán nhập Mã FT hoàn tiền & Upload file UNC"]
    E --> F["Bấm 'XÁC NHẬN ĐÃ HOÀN TIỀN'"]
    F --> G["Trạng thái cập nhật: 'ĐÃ HOÀN BOOKING'"]
    F --> H["Tự động tạo Log giao dịch hoàn tiền tại Tab 4 Lịch sử"]
```

---

## 2. ĐẶC TẢ CHỨC NĂNG & GIAO DIỆN (FUNCTIONAL SPECIFICATIONS & UI)

### 2.1. Giao diện Mockup Thực tế

![Quản Lý Hoàn Tiền Refund](../images/ke_toan/kt_tab3_hoan_booking.png)

### 2.2. Thành phần Giao diện & Tính năng Chi tiết
1. **Danh sách Hồ sơ Yêu cầu Hoàn Cọc (Left Table)**:
   - Hiển thị: Mã Booking (`CB-xxxx`), Khách hàng, Dự án, Số tiền hoàn (VNĐ), Lý do hoàn tiền (VD: *"Không khớp được căn 3PN tòa A"*), Người duyệt (Admin), Trạng thái (`Đã duyệt hoàn` / `Đã hoàn booking`).
2. **Cột Form Xử Lý Lệnh Chi Hoàn Tiền (Right Detail Panel)**:
   - **Thông tin thụ hưởng của Khách**: Ngân hàng thụ hưởng, Số tài khoản, Tên chủ tài khoản, Số tiền cọc cần hoàn.
   - **Phiếu đề xuất hoàn đính kèm**: Bấm xem trước file Scan Đơn đề nghị hoàn tiền của khách.
   - **Mã FT hoàn tiền**: Ô nhập mã giao dịch ngân hàng chuyển tiền hoàn.
   - **Thời gian hoàn tiền**: Ngày giờ thực hiện lệnh chi.
   - **Ghi chú đối soát**: Nhập nội dung tra soát sau hoàn (nếu có).
   - **Upload UNC hoàn tiền**: Khu vực đính kèm file scan UNC chuyển khoản trả khách.
   - **Nút Xác nhận đã hoàn tiền**: Nút hành động chính để đóng hồ sơ.

### 2.3. Danh mục trường dữ liệu (Data Dictionary)

| Tên trường | Kiểu dữ liệu | Bắt buộc | Mô tả & Quy tắc |
| :--- | :---: | :---: | :--- |
| `id` | String | Có | Mã phiếu Booking yêu cầu hoàn (VD: `CB-2026-0008`). |
| `customerName` | String | Có | Họ tên khách hàng nhận tiền hoàn. |
| `refundAmount` | Currency | Có | Số tiền cọc thực tế hoàn trả cho khách. |
| `receivingBank` | String | Có | Tên ngân hàng thụ hưởng của khách hàng. |
| `receivingAccount` | String | Có | Số tài khoản ngân hàng của khách. |
| `accountHolder` | String | Có | Họ tên chủ tài khoản thụ hưởng. |
| `refundFtCode` | String | Có | Mã giao dịch ngân hàng lệnh chi hoàn tiền. |
| `refundTime` | DateTime | Có | Thời gian thực hiện chuyển tiền hoàn cọc. |
| `uncFile` | File/URL | Không | File đính kèm UNC lệnh chi hoàn tiền. |

---

## 3. QUY TẮC NGHIỆP VỤ (BUSINESS RULES & VALIDATIONS)

### 3.1. Các quy tắc xử lý nghiệp vụ (Business Rules - BR)
- **BR-ACC07 (Điều kiện chi hoàn tiền)**: Kế toán chỉ được phép thực hiện chi hoàn tiền đối với các hồ sơ đã có trạng thái `Đã duyệt hoàn` từ cấp Admin/Ban Giám đốc.
- **BR-ACC08 (Tính toàn vẹn của Mã FT)**: Mỗi lệnh hoàn tiền bắt buộc phải có `Mã FT hoàn` duy nhất để đối soát dòng tiền ra trên sao kê ngân hàng.

---

## 4. ĐẶC TẢ API & TÍCH HỢP (API SPECIFICATIONS)

### 4.1. Danh sách Endpoints

| STT | Method | Endpoint URI | Mô tả chức năng |
| :---: | :---: | :--- | :--- |
| 1 | `GET` | `/api/v1/accounting/refunds` | Lấy danh sách yêu cầu hoàn cọc cần kế toán chi trả. |
| 2 | `POST` | `/api/v1/accounting/refunds/{id}/execute` | Kế toán xác nhận đã chi hoàn tiền kèm mã FT và UNC. |

---

### 4.2. Chi tiết API & Schema Data

#### API 1: `GET /api/v1/accounting/refunds`
* **Mô tả**: Lấy danh sách yêu cầu hoàn tiền cọc thiện chí đã được Admin phê duyệt hoặc đã chi trả thành công.
* **Query Parameters**:
  * `status` (string, optional): `APPROVED_REFUND` (Đã duyệt hoàn - chờ chi), `COMPLETED` (Đã hoàn tiền).

* **Response Schema (200 OK)**:
```json
{
  "success": true,
  "statusCode": 200,
  "data": {
    "total": 1,
    "items": [
      {
        "id": "CB-2026-0008",
        "customerName": "Lê Hoài Nam",
        "project": "LUMIÈRE Riverside",
        "refundAmount": 100000000,
        "refundReason": "Không khớp được căn 3PN tòa West theo đúng nguyện vọng",
        "approvedBy": "Nguyễn Hoàng Nam (Admin)",
        "approvedAt": "2026-08-05 15:00",
        "bankInfo": {
          "bankName": "Techcombank",
          "accountNumber": "19034567890123",
          "accountHolder": "LE HOAI NAM"
        },
        "status": "APPROVED_REFUND",
        "statusBadge": "Đã duyệt hoàn",
        "refundFtCode": null,
        "refundTime": null
      }
    ]
  }
}
```

---

#### API 2: `POST /api/v1/accounting/refunds/{id}/execute`
* **Mô tả**: Kế toán xác nhận đã chuyển khoản hoàn tiền cọc cho khách hàng, lưu mã FT và file Scan UNC.
* **Path Parameters**:
  * `id` (string, required): Mã booking (VD: `CB-2026-0008`).
* **Request Body Schema**:
```json
{
  "refundFtCode": "string (Mã giao dịch ngân hàng lệnh chi hoàn tiền, required)",
  "refundTime": "string (Thời gian thực hiện chuyển tiền, ISO 8601 string)",
  "uncFileUrl": "string (URL tệp scan UNC hoàn tiền, optional)",
  "accountingNote": "string (Ghi chú đối soát sổ cái)",
  "executedBy": "string (Mã Kế toán)"
}
```

* **Request Example**:
```json
{
  "refundFtCode": "FT262170881144",
  "refundTime": "2026-08-05T16:00:00Z",
  "uncFileUrl": "https://storage.newway.vn/unc/2026/08/UNC_Hoan_100M_LeHoaiNam.pdf",
  "accountingNote": "Đã chuyển tiền hoàn cọc từ tài khoản Techcombank công ty sang Techcombank khách hàng.",
  "executedBy": "ACC-001"
}
```

* **Response Schema (200 OK)**:
```json
{
  "success": true,
  "statusCode": 200,
  "message": "Đã hoàn tiền thành công! Trạng thái booking chuyển sang Đã hoàn booking.",
  "data": {
    "bookingId": "CB-2026-0008",
    "status": "COMPLETED",
    "statusBadge": "Đã hoàn booking",
    "refundFtCode": "FT262170881144",
    "refundedAt": "2026-08-05T16:00:00Z",
    "loggedToTransactionHistory": true
  }
}
```

---
*Tài liệu BA chuẩn hóa theo hệ thống NewWay Booking Final.*
