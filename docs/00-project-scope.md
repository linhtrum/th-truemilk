# 00 — Project Scope

## Hệ thống Quản lý Xuất khẩu KDQT (B2B Export Management System)

**Phiên bản:** 1.0  
**Trạng thái:** Baselined for Design  
**Ngày cập nhật:** 2026-08-21  
**Nguồn yêu cầu:** `website_modules_analysis.md` + các quyết định phạm vi đã được xác nhận trong phiên làm rõ yêu cầu  
**Mục đích tài liệu:** Xác định ranh giới phạm vi dự án ở cấp hệ thống làm baseline cho các tài liệu domain, workflow, phân quyền, dữ liệu, API, kiến trúc và triển khai tiếp theo.

---

## 1. Mục tiêu dự án

Xây dựng hệ thống quản lý nghiệp vụ xuất khẩu B2B cho phòng Kinh Doanh Quốc Tế (KDQT), phục vụ xuyên suốt quy trình:

**Khách hàng → Đơn hàng (PO/PI) → Booking → Vận hành → Giao hàng → Thanh toán**

Hệ thống cung cấp các khu vực truy cập theo vai trò nghiệp vụ:

1. **KDQT Portal** — nhập liệu, tạo đơn hàng, booking và vận hành nghiệp vụ chính.
2. **Customer Portal** — khách hàng theo dõi đơn hàng, booking, chứng từ, thanh toán và KPI của chính mình.
3. **Warehouse Portal** — kho xử lý batch, lịch đóng hàng và trạng thái giao hàng trong phạm vi kho được phân công.
4. **Finance Portal** — kế toán/tài chính xem đơn hàng, chi phí và công nợ; lọc và tải dữ liệu.
5. **System Administration** — quản trị user, role, permission, account nghiệp vụ, master data, cấu hình và audit.

Hệ thống phải bảo đảm dữ liệu và trạng thái được liên kết nhất quán giữa các portal theo đúng workflow nghiệp vụ.

---

## 2. Phạm vi triển khai

Toàn bộ **20 module nghiệp vụ** trong tài liệu nguồn nằm trong phạm vi dự án hiện tại; không chia MVP/Phase 1/Phase 2 ở cấp Project Scope.

| Khu vực | Số module nghiệp vụ | Phạm vi tổng quát |
|---|---:|---|
| KDQT Portal | 8 | CRUD nghiệp vụ, import, export, vận hành |
| Customer Portal | 6 | Read-only theo dữ liệu của chính Customer + download |
| Warehouse Portal | 3 | Read + cập nhật batch/trạng thái theo kho |
| Finance Portal | 3 | Read-only + filter + download |
| **Tổng** | **20** | |

**System Administration** là năng lực quản trị hệ thống bổ sung, không tính vào 20 module nghiệp vụ nêu trên.

Việc triển khai kỹ thuật có thể chia sprint/release nội bộ nhưng không làm thay đổi ranh giới chức năng của dự án.

---

## 3. Phạm vi người dùng, account và portal

### 3.1. KDQT Portal

**Đối tượng:** Nhân viên phòng Kinh Doanh Quốc Tế.

**In Scope:**

- Quản lý khách hàng, hợp đồng, sản phẩm và giá.
- Tạo và quản lý Order PO/PI.
- Tạo và quản lý Booking.
- Quản lý các công việc vận hành theo PIC/deadline/trạng thái.
- Nhập và theo dõi chi phí.
- Quản lý lịch giao hàng.
- Theo dõi thanh toán và công nợ.
- Import Excel, export/download dữ liệu và xuất chứng từ theo quyền.

Chi tiết permission theo action/resource được xác định trong tài liệu Role & Permission Matrix.

### 3.2. Customer Portal

**Đối tượng:** Khách hàng B2B.

**Account model:**

- Mỗi **Customer chỉ có một account đăng nhập**.
- Account Customer chỉ được truy cập dữ liệu của chính Customer đó.
- Không được xem dữ liệu của Customer khác.

**In Scope:**

- Order List.
- Planning.
- On The Way.
- Completed.
- Payment Schedule.
- KPI theo tháng/quý/năm.
- Xem/tải chứng từ và dữ liệu được phép.

Customer Portal là read-only ở cấp nghiệp vụ hiện tại.

### 3.3. Warehouse Portal

**Account model:**

- Mỗi **Warehouse chỉ có một account đăng nhập**.
- Warehouse chỉ được xem và cập nhật Booking, Batch và Lịch đóng hàng được phân công cho chính Warehouse đó.
- Không được xem dữ liệu Warehouse khác.

**In Scope:**

- Xem booking/đơn hàng được phân công.
- Điền số batch.
- Cập nhật trạng thái batch.
- Cập nhật trạng thái đóng hàng.
- Cập nhật dữ liệu phục vụ giao hàng theo quyền.
- Download danh sách hàng theo SO.
- Màn hình lịch đóng hàng không hiển thị giá.

### 3.4. Finance Portal

Finance Portal là **read-only**.

**In Scope:**

- Xem thông tin đơn hàng/booking.
- Xem lượng hàng và giá theo phạm vi được cấp.
- Xem chi phí làm hàng.
- Xem công nợ/thanh toán.
- Tìm kiếm, lọc và download dữ liệu/báo cáo.

Finance Portal **không cập nhật** thanh toán, công nợ hoặc dữ liệu nghiệp vụ trong phạm vi hiện tại.

### 3.5. System Administration

System Administration nằm trong phạm vi dự án và bao gồm:

- User management.
- Role management.
- Permission management.
- Quản lý Customer account.
- Quản lý Warehouse account.
- Master data management.
- System settings.
- Audit log viewer.
- Import/export configuration.
- Document template management.

System Administrator **không mặc định thực hiện nghiệp vụ thay cho KDQT/Warehouse/Finance**; muốn thực hiện action nghiệp vụ phải được cấp permission tương ứng.

---

## 4. Authentication và truy cập

Hệ thống hỗ trợ đồng thời:

1. **Local account** do hệ thống quản lý.
2. **SSO**.

Nhà cung cấp SSO, mapping identity, session/token model, MFA, fallback login và các chi tiết bảo mật khác được xác định trong tài liệu Authentication/Security và ADR.

SSO là integration được xác nhận **In Scope** cho authentication; các tích hợp nghiệp vụ bên ngoài khác được xử lý ở Future Scope.

---

## 5. Phạm vi chức năng — KDQT Portal

### 5.1. Module 1 — Danh sách khách hàng

**In Scope:**

- Danh sách khách hàng; tìm kiếm, lọc, reset.
- Tạo mới và sửa.
- Import Excel.
- Theo dõi hợp đồng sắp hết hạn/đã hết hạn.
- Thị phần theo khách hàng/theo nước.
- Thống kê theo nhóm hàng, loại hàng, thương hiệu.
- Quản lý thông tin khách hàng, hợp đồng, trade term, payment term, currency, market, KPI, incentive và document requirements.
- `CustomerID` là giá trị duy nhất, không trùng.

Ngưỡng cảnh báo, soft delete, unique keys bổ sung và validation được thiết kế ở Business Rules/Data Dictionary.

### 5.2. Module 2 — Danh sách hàng hóa / sản phẩm và giá

**In Scope:**

- Danh sách sản phẩm; tìm kiếm, lọc, reset.
- Tạo mới và sửa.
- Import Excel.
- Cập nhật bảng tăng giá.
- Quản lý mã sản phẩm, barcode, HS Code, tên VN/EN, quy cách, giá, VAT, hạn sử dụng, Chill/Ambient, kích thước, trọng lượng, thể tích, pallet, ingredient list, nhiệt độ bảo quản và ngày hiệu lực giá.
- Thống kê top mặt hàng theo nước/khách hàng.
- Sản lượng theo khách hàng theo quý so với mục tiêu.
- Danh sách hàng sắp thay đổi/sắp ra mắt và lịch áp dụng.

Lịch sử giá, version ingredient list, unique rules và trạng thái sản phẩm được thiết kế ở tài liệu domain/data/business rules.

### 5.3. Module 3 — Tạo đơn hàng PO/PI

**In Scope:**

- Tạo Order PO/PI.
- Gợi ý số PO theo mẫu `TênKH/MãNước-Năm-SốTT`.
- Lấy thông tin Customer.
- Nhập nhiều dòng sản phẩm.
- Hỗ trợ số lượng, chiết khấu, phí tem, thuế, FOC và giá.
- Tính giá theo EXW hoặc FOB/DAT và VND/USD.
- In/xuất Invoice và Packing List theo template nghiệp vụ.
- Ghi nhận Incentive Quarter/Year.
- Ghi nhận Debit Note/Credit Note.
- Quản lý loại container, phương thức vận chuyển, ETD, ETA và ghi chú kho.

**Quyết định phạm vi:**

- Một **Order có thể có nhiều Booking** (`Order 1 → N Booking`).
- Order có **lifecycle/trạng thái riêng**.
- Order status phản ánh vòng đời/mức độ fulfillment của Order, không dùng trực tiếp Booking status làm Order status.
- Danh sách trạng thái cụ thể, transition, snapshot data/price, quyền sửa/hủy sau phát hành và quy tắc sinh số PO được thiết kế ở Workflow/Business Rules.

### 5.4. Module 4 — Tạo Booking

**In Scope:**

- Tạo Booking từ Order đã có.
- Một Order có thể sinh nhiều Booking.
- In/xuất Invoice, Packing List và Batch Information theo template nghiệp vụ.
- Booking có lifecycle riêng.
- Các trạng thái nguồn xác nhận: `Phát hành → Ongoing → Đang giao → Đã giao`.
- Cho phép bổ sung **Draft**, **Hold**, **Cancelled**.
- Khi Booking chuyển sang `Phát hành`, dữ liệu được đưa sang Lịch giao hàng và Customer Portal.

Tên trạng thái chuẩn hóa, transition matrix, permission và side effects được thiết kế trong `04-workflow-state-machines.md`.

### 5.5. Module 5 — Vận hành đơn hàng

Module Vận hành quản lý **work item/task riêng**, liên kết với Booking; không chỉ là các field trạng thái trên Booking.

**In Scope:**

- Một Booking có thể có nhiều work items.
- Work item có PIC, deadline và trạng thái phục vụ theo dõi Ongoing/Completed.
- Tìm kiếm, lọc, reset.
- Cập nhật thông tin vận hành.
- Hiển thị tỷ lệ công việc theo PIC.
- Hiển thị tỷ lệ công việc chậm deadline theo PIC.

Task entity, assignment, progress, dependency và workflow được thiết kế ở tài liệu domain/workflow sau.

### 5.6. Module 6 — Nhập chi phí

**In Scope:**

- Tìm kiếm, lọc, reset.
- Cập nhật chi phí từ Booking.
- Quản lý trạng thái chi phí theo nghiệp vụ.
- Thống kê tỷ lệ % chi phí theo khách hàng.
- Tổng chi phí hàng tháng.
- Hỗ trợ các danh mục chi phí trong tài liệu nguồn.

Mô hình chi phí, currency, approval rule và aggregation được thiết kế ở Domain/Business Rules.

### 5.7. Module 7 — Lịch giao hàng

**In Scope:**

- Hiển thị lịch giao hàng theo tuần/tháng.
- Tìm kiếm, lọc, reset.
- Ghi chú cho nhà máy.
- Quản lý booking, order, customer, nước, cảng, warehouse, địa điểm giao, lái xe, ngày xuất kho, ETD/ETA, container, số chì, hun trùng, lấy mẫu kiểm dịch và ghi chú.
- Khi Booking chuyển sang `Đang giao`, Lịch giao hàng chuyển trạng thái `Xong` theo yêu cầu nguồn và không còn xuất hiện trong danh sách hiện hành mặc định.

History/archive và nguồn cập nhật từng trường được xác định ở tài liệu functional/data.

### 5.8. Module 8 — Theo dõi thanh toán

**In Scope:**

- Hiển thị công nợ chưa thanh toán và quá hạn theo khách hàng.
- Tìm kiếm, lọc, reset.
- Thêm ghi chú.
- Lấy dữ liệu từ Order và Booking.
- Tính ngày quá hạn và phân loại theo hạn thanh toán.
- Hỗ trợ **partial payment / nhiều lần thanh toán**.
- Theo dõi Amount Due, tổng đã thanh toán và Outstanding Balance.

Payment/Payment Transaction, liên kết Order/Booking, Debit/Credit Note, ngưỡng cảnh báo và công thức chi tiết được thiết kế ở Domain/Business Rules.

---

## 6. Phạm vi chức năng — Customer Portal

### 6.1. Order List
- Xem danh sách PO thuộc Customer hiện tại.
- Lọc dữ liệu.
- Download Excel.

### 6.2. Planning
- Xem Booking dự kiến đi ở trạng thái `Phát hành`.
- Xem chi tiết hàng hóa và chứng từ.

### 6.3. On The Way
- Xem Booking ở trạng thái `Đang giao`.
- Xem chi tiết hàng hóa và chứng từ.

### 6.4. Completed
- Xem Booking ở trạng thái `Đã giao`.
- Xem chi tiết hàng hóa.

### 6.5. Payment Schedule
- Xem Order/Booking chưa thanh toán.
- Xem chi tiết thanh toán và số dư công nợ.

### 6.6. KPI
- Xem sản lượng và giá trị theo tháng/quý/năm so với mục tiêu.
- Lọc theo kỳ.

### 6.7. Data Scope
- Mỗi Customer có một account.
- Customer chỉ đọc dữ liệu của chính mình.
- Không được truy cập dữ liệu Customer khác.

---

## 7. Phạm vi chức năng — Warehouse Portal

### 7.1. Danh sách đơn hàng kho
- Xem Booking kèm số SO theo Warehouse.
- Xem mã hàng, Chill/Ambient, số thùng, thể tích và gross weight.
- Download danh sách hàng theo SO.
- Điền số batch.
- Chuyển trạng thái theo quyền.

### 7.2. Batch hàng
- Xem Booking chưa có batch.
- Nhập/cập nhật batch.
- Theo dõi trạng thái batch.
- Cập nhật `Hoàn thành / Chưa hoàn thành` theo workflow.

### 7.3. Lịch đóng hàng kho
- Xem Booking được phân luồng cho chính Warehouse.
- Không hiển thị giá.
- Cập nhật `Đã hold hàng → Đã dán tem → Đã giao hàng`.
- Khi `Đã giao hàng`, cập nhật ngược sang Lịch giao hàng chung và Booking.

### 7.4. Data Scope
- Mỗi Warehouse có một account.
- Chỉ xem/cập nhật Booking, Batch và Lịch đóng hàng thuộc Warehouse đó.
- Không được xem dữ liệu Warehouse khác.

---

## 8. Phạm vi chức năng — Finance Portal

Finance Portal là **read-only**.

### 8.1. Thông tin đơn hàng
- Xem Booking/Order theo phạm vi được cấp.
- Xem lượng hàng và giá.
- Lọc và download.

### 8.2. Chi phí làm hàng
- Xem chi phí do KDQT phát hành.
- Lọc và download bảng chi phí.

### 8.3. Theo dõi công nợ
- Xem dữ liệu công nợ/thanh toán.
- Xem partial payments và outstanding balance khi có.
- Lọc và download theo khách hàng.

Finance không có quyền cập nhật payment/debt trong phạm vi hiện tại.

---

## 9. Import, export và chứng từ

### 9.1. Excel Import

`Upload Excel` là **import dữ liệu vào database**, không chỉ upload file để lưu trữ.

**In Scope:**

- Import tạo mới dữ liệu.
- Import cập nhật dữ liệu dựa trên khóa nghiệp vụ.
- Áp dụng validation và chống duplicate.
- Hỗ trợ **partial success**.
- Dòng hợp lệ được import.
- Dòng lỗi bị bỏ qua và được trả về trong báo cáo lỗi chi tiết.

Business key, transaction strategy, dry-run/preview, retry và format error report được xác định ở tài liệu Import/Export và Data Dictionary.

### 9.2. Export/Download

Hệ thống hỗ trợ export/download theo các module đã nêu trong tài liệu nguồn, bao gồm Excel và các biểu mẫu nghiệp vụ.

### 9.3. Document Templates

- Invoice, Packing List, Batch Information và chứng từ khác được sinh theo **template chính thức do nghiệp vụ cung cấp**.
- Template có thể được cập nhật/thay thế mà không làm thay đổi logic nghiệp vụ cốt lõi.
- System Administration có chức năng quản lý template.

Versioning template, mapping field, rendering engine và storage implementation được xác định ở tài liệu chuyên biệt.

---

## 10. Notifications

Hệ thống có **in-app notification** trong phạm vi hiện tại.

Các nhóm thông báo tối thiểu:

- Hợp đồng sắp hết hạn.
- Hợp đồng hết hạn.
- Thanh toán sắp đến hạn.
- Thanh toán quá hạn.
- Work item vận hành quá deadline.
- Các sự kiện nghiệp vụ khác được xác định trong Functional Requirements.

**Email, SMS, push notification và các kênh bên ngoài hệ thống không nằm trong phạm vi hiện tại** và thuộc Future Scope.

---

## 11. Ngôn ngữ và thiết bị truy cập

### 11.1. Ngôn ngữ

Hệ thống hỗ trợ giao diện **song ngữ Việt/Anh**.

- UI có Tiếng Việt và Tiếng Anh.
- Dữ liệu nghiệp vụ có trường song ngữ tiếp tục lưu/hiển thị riêng VN/EN.
- i18n, fallback language và resource management được quyết định ở frontend/architecture.

### 11.2. Thiết bị

Website phải responsive và hỗ trợ đầy đủ trên:

- Desktop.
- Tablet.
- Mobile browser.

**Native iOS/Android application không nằm trong phạm vi hiện tại.**

---

## 12. Năng lực dùng chung trong phạm vi

### 12.1. Tìm kiếm, lọc và phân trang
Các module có nhu cầu phải hỗ trợ tìm kiếm, lọc và hiển thị dữ liệu phù hợp với quy mô sử dụng. Filter/sort/pagination cụ thể được xác định ở Functional Requirements/API Design.

### 12.2. CRUD và command nghiệp vụ
- KDQT có phạm vi nghiệp vụ rộng nhất nhưng vẫn chịu kiểm soát permission.
- Customer và Finance read-only theo scope đã xác nhận.
- Warehouse được update trong phạm vi dữ liệu của chính Warehouse.
- State transition quan trọng sẽ được mô hình hóa thành command nghiệp vụ, không chỉ sửa tự do field `Status`.

### 12.3. Chống trùng lặp
Hệ thống phải ngăn dữ liệu duplicate bằng validation, business key và database constraints phù hợp.

### 12.4. Audit Log
Hệ thống phải ghi lại lịch sử ai thay đổi dữ liệu gì. System Administration có Audit Log Viewer.

### 12.5. Authorization và Data Scope
- Customer chỉ thấy dữ liệu của chính Customer.
- Warehouse chỉ thấy/cập nhật dữ liệu của chính Warehouse được phân công.
- Finance read-only.
- System Admin không mặc định có quyền nghiệp vụ nếu chưa được cấp permission tương ứng.

### 12.6. Dashboard/KPI/Reporting
Các dashboard, KPI, thống kê và tổng hợp xuất hiện trong 20 module đều nằm trong phạm vi dự án. Formula, data source, filter và aggregation được đặc tả ở Reporting/Dashboard Specification.

---

## 13. Luồng nghiệp vụ xuyên hệ thống

```text
Customer + Product
        │
        ▼
     Order PO/PI
        │
        │ 1:N
        ▼
      Booking
        │
        ├──────────────► Customer Portal
        │
        ├──────────────► Warehouse Portal
        │
        ▼
 Operation Work Items
        │
        ▼
       Cost
        │
        ▼
 Delivery Schedule
        │
        ▼
 Payment / Receivables
        │
        ├──────────────► Customer Portal
        └──────────────► Finance Portal
```

Các liên kết nghiệp vụ đã xác nhận ở cấp scope:

1. `Order 1 → N Booking`.
2. Order có lifecycle riêng.
3. Booking có lifecycle riêng; ngoài chuỗi nghiệp vụ nguồn còn cho phép Draft/Hold/Cancelled.
4. Booking `Phát hành` → dữ liệu xuất hiện ở Lịch giao hàng và Customer Portal.
5. Booking `Đang giao` → Customer Portal `On The Way`; Lịch giao hàng chuyển `Xong` theo yêu cầu nguồn.
6. Booking `Đã giao` → Customer Portal `Completed`.
7. Warehouse `Đã giao hàng` → cập nhật ngược sang Lịch giao hàng và Booking.
8. Booking có thể có nhiều Operation Work Items.
9. Payment hỗ trợ nhiều lần thanh toán và outstanding balance.

Transactional boundary, transition chi tiết, consistency model và side effects được thiết kế ở Workflow/Architecture.

---

## 14. External Integrations

### 14.1. Trong phạm vi hiện tại
- SSO phục vụ authentication.

### 14.2. Future Scope / Out of Scope hiện tại

Các tích hợp nghiệp vụ bên ngoài có thể được bổ sung trong tương lai nhưng **không nằm trong phạm vi hiện tại**, bao gồm:

- ERP/SAP integration.
- CRM integration.
- Hệ thống kho bên ngoài.
- Hệ thống kế toán bên ngoài.
- E-invoice integration.
- Shipping line/carrier API.
- Payment gateway/online payment.
- Email/SMS/push provider.
- Các dịch vụ bên thứ ba khác chưa được yêu cầu cụ thể.

Khi có yêu cầu tích hợp mới, cần bổ sung scope và integration contract tương ứng.

---

## 15. Ngoài phạm vi hiện tại

- Native mobile application.
- Offline mode.
- ERP/SAP/CRM/accounting/warehouse external integrations, trừ SSO authentication.
- Shipping line/carrier API integration.
- Payment gateway/online payment.
- E-invoice integration.
- Email/SMS/push notification.
- OCR.
- Electronic signature/document signing nếu chưa có change request bổ sung.
- BI/Data Warehouse riêng biệt ngoài dashboard/reporting của ứng dụng.
- Public API cho hệ thống bên thứ ba nếu chưa có change request bổ sung.

Các hạng mục này thuộc Future Scope và phải qua change control nếu bổ sung.

---

## 16. Phạm vi kỹ thuật đã xác định

### 16.1. Backend
- **.NET 10**.
- **ASP.NET Core Web API**.

### 16.2. Những quyết định kỹ thuật không thuộc Project Scope

Các nội dung sau được quyết định trong Architecture/ADR, không phải điều kiện để xác định ranh giới nghiệp vụ:

- Database engine và database topology.
- Frontend framework cụ thể.
- Hosting/deployment topology.
- File storage implementation.
- Cache.
- Background processing/job engine.
- Observability stack.
- CI/CD.
- SSO provider cụ thể.

---

## 17. Tài liệu thiết kế tiếp theo

Project Scope này không quyết định chi tiết các nội dung sau; chúng được xử lý ở tài liệu chuyên biệt:

- `01-domain-glossary.md`
- `02-functional-requirements.md`
- `03-business-rules.md`
- `04-workflow-state-machines.md`
- `05-role-permission-matrix.md`
- `06-domain-model.md`
- `07-data-dictionary.md`
- `08-system-architecture.md`
- `09-api-design.md`
- `10-api-endpoints.md`
- Validation rules.
- Error handling.
- Audit schema.
- Import/export specification.
- File management.
- Background jobs/events.
- Notification rules.
- Concurrency/idempotency.
- Dashboard/reporting formulas.
- Testing strategy.
- Deployment/operations.

---

## 18. Scope Decision Baseline

| ID | Quyết định | Kết quả |
|---|---|---|
| SCOPE-01 | Phạm vi triển khai | Toàn bộ 20 module nghiệp vụ In Scope |
| SCOPE-02 | System Administration | In Scope |
| SCOPE-03 | External business integrations | Future Scope, không thuộc phạm vi hiện tại |
| SCOPE-04 | Authentication | Local account + SSO |
| SCOPE-05 | Customer account model | Mỗi Customer một account |
| SCOPE-06 | Warehouse account model | Mỗi Warehouse một account |
| SCOPE-07 | Finance | Read-only |
| SCOPE-08 | Order → Booking | 1:N |
| SCOPE-09 | Order lifecycle | Có lifecycle riêng; chi tiết thiết kế sau |
| SCOPE-10 | Booking lifecycle | Có Draft/Hold/Cancelled ngoài các trạng thái nguồn; chi tiết thiết kế sau |
| SCOPE-11 | Order Operations | Quản lý work items/tasks riêng gắn với Booking |
| SCOPE-12 | Payment | Hỗ trợ partial payment / nhiều lần thanh toán |
| SCOPE-13 | Excel Upload | Import tạo mới/cập nhật dữ liệu vào database |
| SCOPE-14 | Import errors | Partial success + báo cáo lỗi |
| SCOPE-15 | Document templates | Template chính thức do nghiệp vụ cung cấp, có thể cập nhật/thay thế |
| SCOPE-16 | UI language | Song ngữ Việt/Anh |
| SCOPE-17 | Devices | Responsive desktop/tablet/mobile browser; không native app |
| SCOPE-18 | Warehouse data scope | Chỉ dữ liệu của chính Warehouse được phân công |
| SCOPE-19 | System Admin scope | Users/Roles/Permissions/Accounts/Master Data/Settings/Audit/Import-Export/Templates |
| SCOPE-20 | Notifications | In-app notifications In Scope; external channels Future Scope |

---

## 19. Tiêu chí baseline của Project Scope

- [x] 4 portal nghiệp vụ được xác nhận.
- [x] Toàn bộ 20 module nghiệp vụ được xác nhận In Scope.
- [x] System Administration được xác nhận In Scope.
- [x] Account model của Customer/Warehouse được xác nhận.
- [x] Finance read-only được xác nhận.
- [x] Quan hệ Order 1:N Booking được xác nhận.
- [x] Nguyên tắc lifecycle Order và Booking được xác nhận.
- [x] Operations work-item model được xác nhận ở cấp scope.
- [x] Partial payment được xác nhận.
- [x] Import/export và document template direction được xác nhận.
- [x] Authentication direction được xác nhận.
- [x] External integration boundary được xác nhận.
- [x] UI language và responsive scope được xác nhận.
- [x] In-app notification scope được xác nhận.

Các chi tiết còn lại thuộc thiết kế và **không còn là câu hỏi chặn Project Scope**.

---

## 20. Phê duyệt và quản lý thay đổi

Project Scope này là baseline cho giai đoạn thiết kế. Các thay đổi làm tăng/giảm phạm vi chức năng sau baseline phải được ghi nhận như scope change/change request và cập nhật version tài liệu.

Phê duyệt tổ chức chính thức, nếu quy trình dự án yêu cầu, được ghi nhận riêng theo vai trò Business Owner, Product/Project Owner và Technical Lead.

---

## 21. Lịch sử phiên bản

| Version | Date | Description |
|---|---|---|
| 0.1 | 2026-08-21 | Khởi tạo Project Scope từ `website_modules_analysis.md`; ghi nhận các điểm chưa rõ dưới dạng TBD/open questions. |
| 1.0 | 2026-08-21 | Baseline Project Scope sau phiên làm rõ; tích hợp SCOPE-01..SCOPE-20 và loại bỏ danh sách câu hỏi mở khỏi tài liệu baseline. |