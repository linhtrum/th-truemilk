# 00 — Project Scope

## Hệ thống Quản lý Xuất khẩu KDQT (B2B Export Management System)

**Trạng thái:** Draft v0.1 — cần xác nhận các mục TBD trước khi khóa phạm vi  
**Nguồn yêu cầu:** `website_modules_analysis.md`  
**Mục đích tài liệu:** Xác định ranh giới phạm vi dự án ở cấp hệ thống trước khi thiết kế domain, cơ sở dữ liệu, API và triển khai mã nguồn.

---

## 1. Mục tiêu dự án

Xây dựng hệ thống quản lý nghiệp vụ xuất khẩu cho phòng Kinh Doanh Quốc Tế (KDQT), phục vụ nhiều nhóm người dùng với phạm vi truy cập khác nhau và liên kết xuyên suốt quy trình:

**Khách hàng → Đơn hàng (PO/PI) → Booking → Vận hành → Giao hàng → Thanh toán**

Hệ thống phải hỗ trợ 4 nhóm tài khoản/người dùng nghiệp vụ:

1. **KDQT** — nhập liệu đầu vào và vận hành nghiệp vụ chính.
2. **Khách hàng** — xem đơn hàng, booking, tracking, thanh toán và KPI của chính khách hàng đó.
3. **Kho** — xử lý batch, thông tin đóng hàng và trạng thái giao hàng theo phạm vi kho.
4. **Kế toán** — xem thông tin đơn hàng, chi phí và công nợ; tải báo cáo/dữ liệu được phép.

---

## 2. Kết quả mong đợi ở cấp hệ thống

Hệ thống sau khi hoàn thành phải cung cấp một nền tảng thống nhất để:

- Quản lý dữ liệu khách hàng, hợp đồng, sản phẩm và giá.
- Tạo và quản lý đơn hàng PO/PI.
- Tạo và theo dõi booking từ đơn hàng.
- Theo dõi quá trình vận hành đơn hàng.
- Ghi nhận và tổng hợp chi phí.
- Quản lý lịch giao hàng.
- Theo dõi thanh toán và công nợ.
- Cung cấp portal riêng cho khách hàng.
- Cung cấp chức năng nghiệp vụ dành cho kho.
- Cung cấp màn hình nghiệp vụ dành cho kế toán.
- Đồng bộ trạng thái và dữ liệu cần thiết giữa các nhóm người dùng theo luồng nghiệp vụ.
- Ghi nhận lịch sử thay đổi dữ liệu quan trọng.
- Hỗ trợ tìm kiếm, lọc, upload và export theo phạm vi được mô tả trong tài liệu nguồn.

---

## 3. Phạm vi người dùng và portal

### 3.1. KDQT Portal

**Đối tượng:** Nhân viên phòng Kinh Doanh Quốc Tế.

**Vai trò tổng quát theo tài liệu nguồn:**

- Nhập dữ liệu đầu vào.
- Tạo và cập nhật dữ liệu nghiệp vụ.
- Vận hành đơn hàng và booking.
- Theo dõi chi phí, giao hàng và thanh toán.
- Upload dữ liệu tại các module được yêu cầu.
- Export dữ liệu/biểu mẫu tại các module được yêu cầu.

**Số module xác định:** 8.

---

### 3.2. Customer Portal

**Đối tượng:** Khách hàng B2B.

**Vai trò tổng quát:**

- Xem dữ liệu đơn hàng và booking của chính khách hàng.
- Theo dõi booking theo trạng thái.
- Xem chứng từ được cung cấp.
- Xem lịch thanh toán.
- Xem KPI sản lượng/giá trị theo kỳ.
- Download dữ liệu tại các chức năng được phép.

**Ràng buộc phạm vi dữ liệu đã được xác định:**

> Một khách hàng chỉ được xem dữ liệu thuộc chính khách hàng đó, không được xem dữ liệu của khách hàng khác.

**Số module xác định:** 6.

---

### 3.3. Warehouse Portal

**Đối tượng:** Nhân sự kho.

**Vai trò tổng quát:**

- Xem booking/đơn hàng được phân luồng cho kho.
- Điền số batch.
- Cập nhật trạng thái batch.
- Cập nhật trạng thái đóng hàng.
- Cập nhật một số thông tin phục vụ giao hàng.
- Không hiển thị giá ở màn hình lịch đóng hàng theo yêu cầu nguồn.

**Số module xác định:** 3.

---

### 3.4. Finance Portal

**Đối tượng:** Nhân sự kế toán/tài chính.

**Vai trò tổng quát:**

- Xem thông tin đơn hàng/booking có lượng hàng và giá.
- Xem chi phí làm hàng do KDQT phát hành.
- Theo dõi công nợ.
- Lọc và download dữ liệu được phép.

**Số module xác định:** 3.

---

## 4. Phạm vi chức năng — KDQT Portal

### 4.1. Module 1 — Danh sách khách hàng

**Trong phạm vi:**

- Danh sách khách hàng.
- Tìm kiếm và lọc.
- Reset bộ lọc.
- Tạo mới.
- Sửa.
- Upload Excel.
- Theo dõi hợp đồng sắp hết hạn và đã hết hạn.
- Hiển thị thị phần theo khách hàng/theo nước.
- Thống kê theo nhóm hàng, loại hàng, thương hiệu.
- Quản lý các thông tin chính được mô tả trong tài liệu nguồn: mã khách hàng, tên, địa chỉ, mã số thuế, hợp đồng, trade term, payment term, currency, market, KPI, incentive và document requirements.

**Ràng buộc đã xác định:**

- `CustomerID` là giá trị duy nhất, không trùng.

**TBD:**

- Quy tắc xác định "sắp hết hạn".
- Có cho phép xóa khách hàng hay chỉ ngừng hoạt động/ẩn.
- Quy tắc duplicate ngoài `CustomerID`.

---

### 4.2. Module 2 — Danh sách hàng hóa / sản phẩm và giá

**Trong phạm vi:**

- Danh sách sản phẩm.
- Tìm kiếm, lọc, reset.
- Tạo mới, sửa.
- Upload Excel.
- Cập nhật bảng tăng giá.
- Quản lý thông tin mã sản phẩm, barcode, HS Code, tên VN/EN, quy cách, giá, VAT, hạn sử dụng, loại Chill/Ambient, kích thước/trọng lượng/thể tích, pallet, ingredient list, nhiệt độ bảo quản và ngày hiệu lực giá.
- Thống kê top mặt hàng bán nhiều theo nước/khách hàng.
- Sản lượng theo khách hàng theo quý so với mục tiêu.
- Danh sách hàng sắp thay đổi/sắp ra mắt và lịch áp dụng.

**TBD:**

- Cách lưu lịch sử thay đổi giá.
- Cách lưu lịch sử ingredient list cập nhật theo tháng.
- Sản phẩm có trạng thái ngừng kinh doanh hay không.
- Quy tắc duy nhất của mã sản phẩm/barcode.

---

### 4.3. Module 3 — Tạo đơn hàng PO/PI

**Trong phạm vi:**

- Tạo PO mới.
- Gợi ý số PO theo mẫu `TênKH/MãNước-Năm-SốTT`.
- Lấy thông tin khách hàng từ module khách hàng.
- Nhập nhiều dòng sản phẩm.
- Hỗ trợ số lượng, chiết khấu, phí tem, thuế, FOC và giá.
- Tính giá theo EXW hoặc FOB/DAT và theo VND/USD.
- In/xuất Invoice và Packing List theo biểu mẫu.
- Ghi nhận Incentive Quarter/Year.
- Ghi nhận Debit Note/Credit Note.
- Quản lý loại container, phương thức vận chuyển, ETD, ETA và ghi chú kho.

**TBD:**

- "PO/PI" là một đối tượng nghiệp vụ hay hai đối tượng riêng.
- Quy tắc chính xác tạo số PO và xử lý cạnh tranh khi nhiều người tạo đồng thời.
- Trạng thái/lifecycle của đơn hàng.
- Khi nào đơn hàng được coi là "đã phát hành".
- Một đơn hàng có thể có bao nhiêu booking.
- Quy tắc snapshot giá/sản phẩm tại thời điểm tạo hoặc phát hành đơn hàng.
- Quyền sửa đơn sau khi đã phát hành.
- Quy tắc hủy/xóa đơn hàng.

---

### 4.4. Module 4 — Tạo Booking

**Trong phạm vi:**

- Tạo booking từ đơn hàng đã có.
- In/xuất Invoice, Packing List và Batch Information theo biểu mẫu có sẵn.
- Luồng trạng thái được mô tả: `Phát hành → Ongoing → Đang giao → Đã giao`.
- Khi booking chuyển sang `Phát hành`, dữ liệu được đưa sang Lịch giao hàng và Account khách hàng.

**TBD:**

- Booking có trạng thái Draft trước "Phát hành" hay không.
- Có trạng thái hủy/tạm dừng hay không.
- Quan hệ Order–Booking chính xác là 1:1 hay 1:N.
- Điều kiện cho phép chuyển từng trạng thái.
- Ai có quyền thực hiện từng transition.
- Side effect đầy đủ của từng transition.

---

### 4.5. Module 5 — Vận hành đơn hàng

**Trong phạm vi:**

- Tìm kiếm, lọc, reset.
- Update thông tin vận hành.
- Chuyển trạng thái `Ongoing → Completed`.
- Hiển thị tỷ lệ công việc theo PIC với Ongoing/Completed.
- Hiển thị tỷ lệ công việc chậm deadline theo PIC.

**TBD:**

- Định nghĩa một "công việc" trong module vận hành.
- Cách gán PIC.
- Các trường dữ liệu vận hành cần cập nhật.
- Cách xác định deadline.
- Công thức tính tỷ lệ chậm deadline.
- Quan hệ trạng thái Operation với trạng thái Booking.

---

### 4.6. Module 6 — Nhập chi phí

**Trong phạm vi:**

- Tìm kiếm, lọc, reset.
- Update chi phí từ booking.
- Chuyển trạng thái chi phí theo nghiệp vụ.
- Thống kê tỷ lệ % chi phí theo khách hàng.
- Tổng chi phí hàng tháng.
- Hỗ trợ các danh mục chi phí được liệt kê trong tài liệu nguồn.

**TBD:**

- Lifecycle/trạng thái chi phí.
- Một booking có nhiều bản ghi chi phí theo từng loại hay một bảng tổng hợp.
- Currency của chi phí và quy tắc quy đổi.
- Có quy trình phê duyệt chi phí hay không.

---

### 4.7. Module 7 — Lịch giao hàng

**Trong phạm vi:**

- Hiển thị lịch giao hàng theo tuần/tháng.
- Tìm kiếm, lọc, reset.
- Ghi chú cho nhà máy.
- Quản lý các trường giao hàng được nêu trong tài liệu nguồn, bao gồm booking, đơn hàng, khách hàng, nước, cảng, kho, địa điểm giao, lái xe, ngày xuất kho, ETD/ETA, container, số chì, hun trùng, lấy mẫu kiểm dịch và ghi chú.
- Khi Booking chuyển sang `Đang giao`, lịch giao hàng chuyển trạng thái `Xong` và không còn xuất hiện trong danh sách hiện hành theo mô tả nguồn.

**TBD:**

- "Biến mất khỏi danh sách" là lọc khỏi màn hình mặc định hay chuyển sang archive/history.
- Có cho phép người dùng xem lịch sử đã hoàn thành hay không.
- Nguồn cập nhật các trường driver/container/seal.

---

### 4.8. Module 8 — Theo dõi thanh toán

**Trong phạm vi:**

- Hiển thị công nợ chưa thanh toán theo khách hàng.
- Hiển thị công nợ quá hạn theo khách hàng.
- Tìm kiếm, lọc, reset.
- Thêm ghi chú.
- Lấy dữ liệu liên quan từ Order và Booking.
- Tính ngày quá hạn theo ngày hiện tại và ngày đến hạn.
- Xác định trạng thái thanh toán theo nhóm `Quá hạn / Sắp đến hạn / Trong hạn`.

**TBD:**

- Ngưỡng "sắp đến hạn".
- Có trạng thái `Đã thanh toán`/`Thanh toán một phần` hay không.
- Nguồn của "số tiền thực tế" và thời điểm ghi nhận.
- Có cho phép kế toán cập nhật thanh toán hay chỉ xem.
- Currency và xử lý nhiều lần thanh toán.

---

## 5. Phạm vi chức năng — Customer Portal

### 5.1. Order List

- Xem danh sách PO thuộc khách hàng hiện tại.
- Lọc dữ liệu.
- Download Excel.

### 5.2. Planning

- Xem booking dự kiến đi ở trạng thái `Phát hành`.
- Xem chi tiết hàng hóa và chứng từ.

### 5.3. On The Way

- Xem booking ở trạng thái `Đang giao`.
- Xem chi tiết hàng hóa và chứng từ.

### 5.4. Completed

- Xem booking ở trạng thái `Đã giao`.
- Xem chi tiết hàng hóa.

### 5.5. Payment Schedule

- Xem booking/đơn hàng chưa thanh toán.
- Xem chi tiết thanh toán.

### 5.6. KPI

- Xem sản lượng và giá trị theo tháng/quý/năm so với mục tiêu.
- Lọc theo kỳ.

### 5.7. Ràng buộc phạm vi dữ liệu Customer Portal

- Khách hàng chỉ được xem dữ liệu của chính mình.
- Không được truy cập dữ liệu của khách hàng khác.

**TBD cho toàn Customer Portal:**

- Khách hàng có được tải chứng từ ở tất cả các trạng thái hay không.
- Có thao tác xác nhận/feedback từ khách hàng hay hoàn toàn read-only.
- Một customer organization có một hay nhiều tài khoản đăng nhập.
- Cách liên kết tài khoản đăng nhập với Customer.

---

## 6. Phạm vi chức năng — Warehouse Portal

### 6.1. Danh sách đơn hàng kho

- Xem booking kèm số SO theo kho.
- Xem mã hàng, loại Chill/Ambient, số thùng, thể tích và tổng gross weight.
- Download danh sách hàng theo SO.
- Điền số batch.
- Chuyển trạng thái trong phạm vi được phép.

### 6.2. Batch hàng

- Phân biệt booking chưa có batch và đã có batch.
- Cập nhật trạng thái batch `Hoàn thành / Chưa hoàn thành`.
- Booking hoàn thành được chuyển sang nhóm riêng theo mô tả nguồn.

### 6.3. Lịch đóng hàng kho

- Xem booking được phân luồng cho từng kho.
- Chỉ hiển thị lượng hàng; không hiển thị giá.
- Cập nhật chuỗi trạng thái `Đã hold hàng → Đã dán tem → Đã giao hàng`.
- Khi `Đã giao hàng`, cập nhật ngược sang Lịch giao hàng chung và Booking.

**TBD cho Warehouse Portal:**

- Cách xác định/phân booking về từng kho.
- Một booking có thể thuộc nhiều kho hay không.
- Quan hệ giữa trạng thái Batch, lịch đóng hàng và Booking.
- Kho có được sửa batch sau khi hoàn thành hay không.
- Quyền truy cập giữa các kho: một kho có được thấy booking của kho khác hay không.

---

## 7. Phạm vi chức năng — Finance Portal

### 7.1. Thông tin đơn hàng

- Xem booking phân luồng theo miền.
- Xem lượng hàng và giá.
- Lọc dữ liệu.
- Download.

### 7.2. Chi phí làm hàng

- Xem chi phí do KDQT phát hành.
- Lọc dữ liệu.
- Download bảng chi phí.

### 7.3. Theo dõi công nợ

- Xem dữ liệu theo dõi công nợ tương ứng module thanh toán của KDQT.
- Lọc dữ liệu.
- Download theo khách hàng.

**TBD cho Finance Portal:**

- Finance là hoàn toàn read-only hay được cập nhật thông tin thanh toán/công nợ.
- Quy tắc "phân luồng theo miền".
- Phạm vi khách hàng/booking mà từng tài khoản finance được phép xem.

---

## 8. Năng lực dùng chung trong phạm vi

Các yêu cầu dùng chung được xác định trong tài liệu nguồn:

### 8.1. Tìm kiếm và lọc

- Các module có yêu cầu phải hỗ trợ tìm kiếm/lọc theo nhiều tiêu chí.
- Chi tiết tiêu chí cụ thể sẽ được xác định trong tài liệu functional requirements/API sau.

### 8.2. CRUD và cập nhật dữ liệu

- KDQT có quyền nghiệp vụ rộng nhất theo mô tả tổng hợp.
- Các portal khác có quyền giới hạn theo vai trò.
- Chi tiết Create/Read/Update/Delete theo từng resource **chưa được xem là đã khóa** cho đến khi có permission matrix.

### 8.3. Chống trùng lặp dữ liệu

- Hệ thống phải có cơ chế ngăn dữ liệu duplicate.
- Các khóa unique và quy tắc duplicate cụ thể ngoài `CustomerID` cần được xác định ở tài liệu business rules/data dictionary.

### 8.4. Quản lý trạng thái liên portal

- Trạng thái nghiệp vụ phải có khả năng tác động dữ liệu hiển thị/xử lý ở portal khác.
- Các transition cụ thể sẽ được khóa trong tài liệu workflow/state machines.

### 8.5. Audit Log

- Hệ thống phải ghi nhận lịch sử ai sửa và sửa gì.
- Mức độ chi tiết, retention, dữ liệu before/after và phạm vi entity cần audit chưa được xác định trong nguồn.

### 8.6. Phân quyền

- Hệ thống có nhiều nhóm người dùng với quyền khác nhau.
- Customer bắt buộc bị giới hạn theo phạm vi dữ liệu của chính khách hàng.
- Permission chi tiết cần tài liệu riêng.

### 8.7. Excel / biểu mẫu

- Có nhu cầu upload Excel ở một số module.
- Có nhu cầu export/download dữ liệu.
- Có nhu cầu xuất các biểu mẫu Invoice, Packing List, Batch Information.
- Format/template chính xác cần được xác nhận.

---

## 9. Luồng nghiệp vụ xuyên hệ thống trong phạm vi

Luồng nghiệp vụ cấp cao được xác định như sau:

```text
Customer + Product
        │
        ▼
     Order PO/PI
        │
        ▼
      Booking
        │
        ├──────────────► Customer Portal
        │
        ├──────────────► Warehouse Portal
        │
        ▼
     Operation
        │
        ▼
       Cost
        │
        ▼
 Delivery Schedule
        │
        ▼
 Payment Tracking
        │
        └──────────────► Finance Portal
```

Các liên kết trạng thái đã được nguồn mô tả rõ:

1. Booking `Phát hành` → dữ liệu sang Lịch giao hàng và Customer Portal.
2. Booking `Đang giao` → Customer Portal hiển thị trong `On The Way`; Lịch giao hàng chuyển `Xong` theo mô tả nguồn.
3. Booking `Đã giao` → Customer Portal hiển thị trong `Completed`.
4. Warehouse `Đã giao hàng` → cập nhật ngược sang Lịch giao hàng và Booking.
5. Dữ liệu Order/Booking được dùng để hình thành thông tin thanh toán/công nợ.

Chi tiết transactional boundary, thứ tự cập nhật và xử lý lỗi **không nằm trong phạm vi tài liệu Project Scope** và sẽ được xác định ở tài liệu workflow/architecture sau.

---

## 10. Phạm vi kỹ thuật đã xác định

### 10.1. Backend

- **.NET 10**.
- **ASP.NET Core Web API**.

### 10.2. Kiến trúc, database và hạ tầng

Chưa được khóa trong tài liệu nguồn và **không được giả định trong phiên bản này**.

Các quyết định còn mở:

- Database engine và nơi lưu trữ database.
- Hosting/deployment.
- Authentication mechanism.
- File storage.
- Frontend technology.
- Cơ chế background processing.
- Cache.
- Hệ thống notification.
- Tích hợp hệ thống ngoài.

---

## 11. Ngoài phạm vi đã xác nhận / chưa được phép coi là In Scope

Tài liệu nguồn **không xác nhận** các chức năng sau. Vì vậy phiên bản Project Scope này không tự đưa chúng vào phạm vi triển khai:

- ERP/SAP integration.
- CRM integration.
- Shipping line/carrier API integration.
- Payment gateway/online payment.
- E-invoice integration.
- Email/SMS/Zalo/Teams notification.
- SSO.
- MFA.
- Mobile native application.
- Offline mode.
- Multi-company/multi-tenant operation ngoài yêu cầu phân tách dữ liệu từng khách hàng.
- Approval workflow ngoài các state transition đã mô tả.
- Document signing/e-signature.
- OCR.
- BI/Data Warehouse riêng.
- Public API cho hệ thống bên thứ ba.

> Các mục trên không được coi là "bị loại vĩnh viễn". Chúng chỉ chưa được xác nhận bởi nguồn hiện tại và cần quyết định riêng nếu có yêu cầu.

---

## 12. Các ràng buộc và nguyên tắc phạm vi đã xác định

1. Customer Portal phải bảo đảm cách ly dữ liệu giữa các khách hàng.
2. Warehouse Portal không hiển thị giá tại màn hình lịch đóng hàng theo mô tả nguồn.
3. Hệ thống phải có audit lịch sử chỉnh sửa.
4. Hệ thống phải chống dữ liệu duplicate.
5. Hệ thống phải hỗ trợ state propagation giữa các nhóm người dùng theo workflow nghiệp vụ.
6. Hệ thống phải hỗ trợ import/export ở các module đã được yêu cầu.
7. Các biểu mẫu Invoice, Packing List và Batch Information phải dựa trên form/template nghiệp vụ được cung cấp; template cụ thể hiện chưa được xác nhận trong nguồn.

---

## 13. Các nội dung không được quyết định trong tài liệu Project Scope

Các nội dung sau sẽ được xử lý ở tài liệu chuyên biệt tiếp theo:

- Domain entity và aggregate boundaries.
- Database schema/ERD.
- API endpoint cụ thể.
- DTO/request/response model.
- Permission chi tiết.
- Business rule chi tiết.
- State machine đầy đủ.
- Validation rule.
- Error handling.
- Audit schema.
- Import/export format chi tiết.
- File storage implementation.
- Background jobs/events.
- Caching.
- Logging/observability.
- Testing strategy.
- Deployment topology.

---

## 14. Tiêu chí hoàn tất phạm vi (Scope Completion Criteria)

Tài liệu `00-project-scope.md` chỉ được chuyển từ **Draft** sang **Approved/Baselined** khi:

- [ ] 4 nhóm người dùng/portal được xác nhận.
- [ ] Danh sách 20 module được xác nhận.
- [ ] In Scope của từng module được xác nhận.
- [ ] Các điểm TBD có ảnh hưởng đến ranh giới hệ thống được trả lời.
- [ ] Các hệ thống tích hợp ngoài (nếu có) được xác định.
- [ ] Phạm vi authentication/user management được xác định.
- [ ] Phạm vi import/export và template được xác định.
- [ ] Phạm vi ngôn ngữ và thiết bị truy cập được xác định.
- [ ] Các chức năng bị loại khỏi Phase 1 được xác nhận rõ.

---

## 15. Các câu hỏi cần làm rõ trước khi khóa Project Scope

### Nhóm A — Ranh giới hệ thống

1. **Hệ thống có tích hợp với hệ thống khác không?**  
   Ví dụ: SAP/ERP, CRM, hệ thống kho, kế toán, e-invoice, hãng vận chuyển, email/SMS hoặc dịch vụ bên thứ ba. Nếu có, cần liệt kê hệ thống và loại dữ liệu trao đổi.

2. **Phạm vi Phase 1 có đúng là toàn bộ 20 module trong tài liệu nguồn không?**  
   Hay cần chia giai đoạn/MVP?

3. **Có cần một khu vực Admin riêng để quản lý user, role, permission, master data và cấu hình hệ thống không?**  
   Tài liệu nguồn mới mô tả 4 nhóm account nghiệp vụ nhưng chưa mô tả System Administrator.

### Nhóm B — Account và phân quyền

4. **Cơ chế đăng nhập mong muốn là gì?**  
   Email/password nội bộ, Microsoft/Google SSO, hoặc cơ chế khác?

5. **Một khách hàng có thể có nhiều tài khoản đăng nhập không?**

6. **Một kho có thể có nhiều tài khoản không, và tài khoản kho chỉ được xem dữ liệu của kho mình hay có thể xem nhiều kho?**

7. **Finance có hoàn toàn read-only không?**  
   Hay Finance được nhập/cập nhật thanh toán, công nợ hoặc xác nhận số tiền thực tế?

### Nhóm C — Workflow nghiệp vụ ảnh hưởng trực tiếp đến scope

8. **Một Order có thể tạo nhiều Booking không?**

9. **Order có lifecycle/trạng thái riêng không?**  
   Ví dụ Draft → Issued → Partially Booked → Completed → Cancelled.

10. **Booking có các trạng thái Draft/Cancelled/Hold ngoài `Phát hành → Ongoing → Đang giao → Đã giao` không?**

11. **Module Vận hành đơn hàng quản lý các task/công việc độc lập hay chỉ là một tập trường trạng thái của Booking?**

12. **Thanh toán có hỗ trợ trả nhiều lần/partial payment không?**

### Nhóm D — Import, export và chứng từ

13. **Các template IV/Invoice, PL/Packing List và Batch Information đã có file mẫu chính thức chưa?**

14. **Upload Excel cần "import tạo/cập nhật dữ liệu" hay chỉ upload file để lưu trữ?**

15. **Khi import có dòng lỗi, yêu cầu là all-or-nothing hay cho phép import các dòng hợp lệ và trả report các dòng lỗi?**

### Nhóm E — Ngôn ngữ và thiết bị

16. **Giao diện cần tiếng Việt, tiếng Anh hay song ngữ?**

17. **Website có bắt buộc responsive để sử dụng đầy đủ trên điện thoại/tablet không?**

18. **Có yêu cầu mobile app native không?**

### Nhóm F — Hạ tầng/triển khai

19. **Database dự kiến sử dụng là gì?**

20. **Triển khai trên server nội bộ, private cloud hay public cloud?**

21. **File/chứng từ cần lưu ở đâu?**  
   Local server, NAS/file server, object storage/cloud, hoặc chưa quyết định?

---

## 16. Trạng thái quyết định

| Hạng mục | Trạng thái |
|---|---|
| Mục tiêu hệ thống | Xác định |
| 4 nhóm account nghiệp vụ | Xác định |
| 20 module | Xác định |
| Luồng nghiệp vụ cấp cao | Xác định |
| Backend .NET 10 + ASP.NET Core Web API | Xác định |
| Database | TBD |
| Authentication | TBD |
| System Admin scope | TBD |
| Frontend stack | TBD |
| Hosting/deployment | TBD |
| File storage | TBD |
| External integrations | TBD |
| Excel/document templates | TBD |
| UI language | TBD |
| Mobile/responsive scope | TBD |
| Detailed workflow/state machines | TBD — tài liệu tiếp theo |
| Detailed permissions/data scope | TBD — tài liệu tiếp theo |

---

## 17. Phê duyệt tài liệu

| Vai trò | Người xác nhận | Trạng thái | Ngày |
|---|---|---|---|
| Business Owner / KDQT | TBD | Pending | TBD |
| Product/Project Owner | TBD | Pending | TBD |
| Technical Lead | TBD | Pending | TBD |

---

## 18. Lịch sử phiên bản

| Version | Date | Description |
|---|---|---|
| 0.1 | 2026-08-21 | Khởi tạo Project Scope từ `website_modules_analysis.md`; chỉ ghi nhận nội dung có căn cứ, đánh dấu TBD cho các yêu cầu chưa rõ. |
