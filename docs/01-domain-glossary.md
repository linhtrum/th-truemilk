# 01 — Domain Glossary

## Hệ thống Quản lý Xuất khẩu KDQT (B2B Export Management System)

**Phiên bản:** 0.2  
**Trạng thái:** Living Draft — Canonical Vocabulary  
**Ngày cập nhật:** 2026-08-21  
**Baseline:** `docs/00-project-scope.md`  
**Nguồn nghiệp vụ:** `website_modules_analysis.md`, tài liệu PPTX yêu cầu gốc, workbook Excel nghiệp vụ gốc và các quyết định làm rõ `GLOSSARY-01` → `GLOSSARY-34`.

---

## 1. Mục đích

Tài liệu này định nghĩa **ngôn ngữ chung (ubiquitous language)** cho toàn bộ hệ thống quản lý xuất khẩu KDQT.

Mục tiêu là bảo đảm cùng một khái niệm nghiệp vụ được hiểu nhất quán trong:

- trao đổi nghiệp vụ;
- tài liệu domain và workflow;
- database/data dictionary;
- API contract;
- backend code;
- frontend/UI;
- import/export Excel;
- báo cáo, audit và vận hành.

`01-domain-glossary.md` là **living document**. Các thuật ngữ mới có thể được bổ sung trong quá trình xây dựng Domain Model, Workflow, Business Rules, Data Dictionary và API. Việc bổ sung thuật ngữ không tự động làm thay đổi Project Scope.

---

## 2. Nguyên tắc chuẩn hóa thuật ngữ

### 2.1. Một khái niệm — một canonical term

Mỗi khái niệm nghiệp vụ nên có một canonical term dùng xuyên suốt domain model và code.

Ví dụ:

- `Product` là canonical term; `SKU`, `Item`, `Goods`, `Mã hàng` là alias theo ngữ cảnh nếu cùng chỉ một đối tượng.
- `WorkItem` là canonical term; `Task` là alias phổ thông/UI.
- `Incoterm` là canonical term; `Trade Term` là alias nghiệp vụ/UI.
- `Carrier` là canonical term tổng quát; `Shipping Line` là Carrier trong vận tải biển.

### 2.2. Business identifier khác technical identifier

Ví dụ:

```text
Product.Id              // technical identifier
Product.ProductionCode  // business identifier

Booking.Id              // technical identifier
Booking.BookingCode     // internal business identifier
```

### 2.3. Internal reference khác external reference

Các mã nội bộ của hệ thống phải được phân biệt với mã do Customer/Carrier/đối tác cung cấp.

```text
OrderNumber               // hệ thống sinh
CustomerPoNumber           // Customer cung cấp nếu có
BookingCode                // hệ thống sinh
CarrierBookingNumber       // Carrier/đơn vị vận chuyển cung cấp
ShippingOrderNumber        // external logistics reference
BillOfLadingNumber         // external logistics reference
```

### 2.4. Dữ liệu giao dịch phải ổn định theo thời điểm giao dịch

Các điều kiện thương mại đã áp dụng cho Order/OrderLine không được hiểu là tham chiếu động vào master data hiện tại.

Ví dụ về ngữ nghĩa:

- Customer có default Incoterm; Order có Incoterm thực tế áp dụng.
- Customer có default Payment Term; Order có Payment Term thực tế áp dụng.
- ProductPrice có hiệu lực theo thời gian; OrderLine giữ giá đã chốt.

Chi tiết snapshot field, persistence và versioning thuộc Domain Model/Data Dictionary.

### 2.5. Glossary không thay thế Domain Model hoặc Business Rules

Glossary trả lời chủ yếu các câu hỏi:

- thuật ngữ này nghĩa là gì;
- khác thuật ngữ kia ở đâu;
- alias nào ánh xạ về canonical term nào;
- nguồn đang dùng tên gì.

Glossary **không phải nguồn chuẩn cho mọi công thức, schema, cardinality hoặc state transition**. Chỉ giữ các quan hệ đã được Project Scope hoặc phiên làm rõ chốt rõ và cần thiết để hiểu nghĩa của thuật ngữ. Công thức, validation, exact cardinality và state machine được khóa ở tài liệu downstream.

### 2.6. Source wording và normalized wording

Khi workbook/PPTX dùng tên chưa chuẩn hoặc nhập nhằng, glossary phải ghi rõ ánh xạ từ tên nguồn sang canonical term thay vì âm thầm thay nghĩa.

---

# 3. Actors, Organizations & Identity

## 3.1. KDQT

**Tên đầy đủ:** Phòng Kinh Doanh Quốc Tế.  
**English:** International Business / Export Sales function; tên tiếng Anh chính thức của đơn vị sẽ dùng theo naming tổ chức khi được xác nhận.

Nhóm người dùng nội bộ chịu trách nhiệm Customer, Product, Order, Booking, vận hành, chi phí, giao hàng và theo dõi thanh toán.

## 3.2. Customer — Khách hàng

Business entity đại diện cho tổ chức/doanh nghiệp B2B mua hàng.

Customer có thể liên quan đến Contract, Order, KPI Target, Incentive Program, commercial defaults, Document Requirement và Customer Account.

```text
Customer != User
Customer != CustomerAccount
```

## 3.3. Customer Account — Tài khoản khách hàng

Account truy cập Customer Portal của một Customer cụ thể.

Theo Project Scope hiện tại, mỗi Customer có đúng một Customer Account. Account xác định data scope của Customer Portal nhưng không phải business entity Customer.

## 3.4. Warehouse — Kho

Business entity đại diện cho kho/địa điểm thực hiện các hoạt động như hold hàng, batch, dán tem, đóng/loading hàng và bàn giao hàng.

Theo quyết định glossary hiện tại, một Booking có một Warehouse đóng hàng chính.

## 3.5. Warehouse Account — Tài khoản kho

Account truy cập Warehouse Portal của một Warehouse cụ thể.

Theo Project Scope hiện tại, mỗi Warehouse có đúng một Warehouse Account và account chỉ truy cập dữ liệu của Warehouse tương ứng.

## 3.6. Finance User — Người dùng tài chính/kế toán

User truy cập Finance Portal để xem, lọc và download dữ liệu Order/Booking, chi phí và công nợ.

Finance Portal hiện là read-only ở cấp nghiệp vụ.

## 3.7. System Administrator

User có quyền quản trị user, role, permission, account nghiệp vụ, master data, system settings, audit, import/export configuration và document templates.

System Administrator không mặc định có quyền thực hiện nghiệp vụ KDQT/Warehouse/Finance nếu không được cấp permission tương ứng.

## 3.8. User — Người dùng / Identity

Danh tính dùng cho authentication và authorization; có thể xác thực bằng local account hoặc SSO.

Không đặt credential, password hash, token hoặc role trực tiếp trong `Customer` hay `Warehouse`.

## 3.9. PIC — Person In Charge

Người chịu trách nhiệm chính cho một Work Item. Ở mức glossary hiện tại, WorkItem có một primary PIC; collaborator/multiple-assignee model nếu cần được xác định ở Domain Model.

---

# 4. Customer & Commercial Terms

## 4.1. Contract — Hợp đồng khách hàng

Hợp đồng thương mại của Customer. Một Customer có thể có nhiều Contract theo thời gian để giữ lịch sử.

Thông tin nguồn tối thiểu gồm Contract Number, Effective Date, Expiry Date và Status.

## 4.2. Contract Status

Lifecycle status của Contract.

Ở mức glossary tối thiểu phân biệt `Effective` và `Expired`.

`ExpiringSoon` được hiểu là **derived alert** từ Expiry Date + alert threshold, không phải bắt buộc là lifecycle state được lưu riêng.

> Lưu ý: workbook gốc có công thức mô tả trạng thái với dấu so sánh chưa nhất quán; logic chính xác phải được xác định trong Business Rules, không sao chép nguyên công thức workbook vào implementation.

## 4.3. Incoterm

Canonical domain term cho điều kiện thương mại; `Trade Term` là UI/business alias.

Nguồn hiện sử dụng EXW, FOB và DAT.

- Customer có default Incoterm.
- Order có Incoterm thực tế áp dụng cho giao dịch.
- ProductPrice được phân loại theo Incoterm.

Không tự động thay `DAT` bằng thuật ngữ Incoterms khác nếu nghiệp vụ chưa xác nhận thay đổi.

## 4.4. Payment Term — Điều khoản thanh toán

Quy tắc thương mại xác định khi nào khoản phải thu đến hạn.

`PaymentTerm` là rule/điều khoản; `DueDate` là ngày cụ thể của một Receivable.

## 4.5. Due Date — Ngày đến hạn

Ngày cụ thể mà một Receivable đến hạn thanh toán, được xác định từ Payment Term và mốc tham chiếu theo Business Rules.

## 4.6. Currency — Đồng tiền

Đơn vị tiền tệ áp dụng cho pricing, Order, financial adjustment, receivable và payment.

Nguồn hiện sử dụng tối thiểu VND và USD.

## 4.7. Market — Thị trường

Thị trường xuất khẩu/kinh doanh của Customer. Workbook nguồn có `Market_EN` và `Market_VN` riêng.

`Market` không đồng nhất với `Region`.

## 4.8. Region — Vùng phân luồng nội bộ

Khái niệm phân vùng nội bộ phục vụ phân luồng dữ liệu/Finance.

Workbook nguồn có field `REGION` với lựa chọn `Bắc/Nam` và ghi chú dùng để phân cho hai account FIN Bắc/Nam.

Exact Region taxonomy, permission và data-scope rule được xác định ở Master Data/Role & Permission Matrix; không gộp Region vào Market.

---

# 5. Product & Pricing

## 5.1. Product — Sản phẩm / Mã hàng

Canonical domain term cho một SKU/mã hàng cụ thể có thể xuất hiện trên OrderLine và BookingLine.

Nguồn chứa các thuộc tính như Production Code, tên VN/EN, HS Code, Product/Block/Carton barcode, packaging, shelf life, storage condition và physical specifications.

`SKU`, `Item`, `Goods`, `Hàng hóa`, `Mã hàng` có thể là alias theo ngữ cảnh nhưng không tạo domain entity song song nếu cùng chỉ một Product.

## 5.2. Production Code — Mã sản phẩm

Business identifier chính của Product.

`Mã hàng` trong giao tiếp nghiệp vụ mặc định ánh xạ về `Product.ProductionCode`, trừ khi màn hình/tài liệu ghi rõ đang nói đến barcode hoặc mã khác.

## 5.3. Barcode

Identifier bổ sung của Product/packaging level.

Nguồn có Product Barcode, Block Barcode và Carton Barcode. Barcode không thay thế Production Code làm canonical business identifier.

## 5.4. HS Code

Mã phân loại hàng hóa phục vụ nghiệp vụ hải quan/xuất khẩu; là attribute/reference của Product, không phải Product identifier chính.

## 5.5. Product Price — Giá sản phẩm

Khái niệm giá cơ sở của Product có hiệu lực theo thời gian và được phân biệt theo Incoterm/Currency.

Không overwrite lịch sử giá khi có giá mới; exact versioning/effective-date model được khóa trong Domain Model/Data Dictionary.

## 5.6. Base Product Price — Giá cơ sở

Giá cơ sở của Product theo Incoterm, Currency và thời gian hiệu lực.

Nguồn chưa xác nhận một bảng giá độc lập theo từng Customer, vì vậy `CustomerProductPrice` chưa là canonical concept.

## 5.7. Order Line Unit Price — Giá chốt trên dòng Order

Giá thực tế được áp dụng cho OrderLine tại giao dịch.

Giá này được hiểu là giá đã chốt của OrderLine; công thức lấy Base Product Price và các điều chỉnh cụ thể thuộc Business Rules.

## 5.8. Price Adjustment — Điều chỉnh giá

Khái niệm tổng quát cho discount, label fee, other fee hoặc điều chỉnh thương mại khi hình thành giá giao dịch.

Không đồng nhất `PriceAdjustment` với `FinancialAdjustment` dùng cho Debit/Credit Note.

---

# 6. Order Domain

## 6.1. Order — Đơn hàng

Canonical business entity đại diện cho một đơn hàng xuất khẩu của Customer.

Order chứa các điều kiện thương mại và nhiều OrderLine, đồng thời có thể được thực hiện qua nhiều Booking.

Order có lifecycle riêng; không dùng Booking status trực tiếp làm Order status.

## 6.2. Order Number — Mã đơn hàng nội bộ

Business identifier nội bộ do hệ thống sinh cho Order.

### Source-to-canonical mapping

Workbook/PPTX gốc dùng field **`Số PO`** và mô tả hệ thống tự gợi ý số theo pattern kiểu:

```text
Tên khách/Mã nước-Năm-Số thứ tự
```

Trong hệ thống mới, field nguồn **`Số PO` do hệ thống tự sinh** được chuẩn hóa thành:

```text
Order.OrderNumber
```

Quy tắc format chính xác thuộc Business Rules/Data Dictionary.

## 6.3. Customer PO Number — Số PO của khách hàng

External reference do Customer cung cấp **nếu nghiệp vụ có Customer PO riêng**.

```text
OrderNumber != CustomerPoNumber
```

`CustomerPoNumber` không thay thế mã nội bộ của Order.

## 6.4. Purchase Order (PO) — thuật ngữ/chứng từ PO

Để tránh nhập nhằng, glossary áp dụng quy tắc sau:

1. Khi source/UI cũ nói **`Số PO` được hệ thống tự sinh**, canonical meaning là `OrderNumber`, không phải một PurchaseOrder entity.
2. Khi nói **PO của Customer**, canonical reference là `CustomerPoNumber` hoặc file/chứng từ Customer PO nếu có.
3. Khi chức năng nguồn nói **“In ra PO theo form có sẵn”**, đó là một `BusinessDocument` loại `PurchaseOrder` được render từ dữ liệu Order theo template nghiệp vụ; nó không phải entity giao dịch thứ hai.

Không tạo `PurchaseOrder` entity song song với `Order` chỉ vì source dùng chữ PO ở nhiều ngữ cảnh.

## 6.5. Order Line — Dòng hàng đơn hàng

Một dòng hàng thương mại trong Order, gắn Product và các dữ liệu giao dịch như quantity, unit price, discount, tax, FOC và fee/adjustment liên quan.

Một OrderLine có thể được chia qua nhiều BookingLine.

## 6.6. Ordered Quantity

Số lượng Product được đặt trên OrderLine.

## 6.7. Booked Quantity

Số lượng của OrderLine đã được phân bổ sang các BookingLine.

Không khóa công thức chi tiết trong glossary; cách tính khi có FOC, cancellation, adjustment thuộc Business Rules.

## 6.8. Remaining Quantity

Số lượng OrderLine còn có thể phân bổ sang Booking theo rule nghiệp vụ.

## 6.9. FOC — Free of Charge

Hàng/số lượng cung cấp miễn phí theo điều kiện thương mại.

Cách FOC ảnh hưởng ordered quantity, booked quantity, invoice value và KPI thuộc Business Rules.

---

# 7. Booking & Logistics

## 7.1. Booking

Canonical domain entity đại diện cho một đợt/lần thực hiện vận chuyển toàn bộ hoặc một phần của Order.

Theo Project Scope:

```text
Order 1 ─── N Booking
```

Booking là đơn vị vận chuyển chính; chưa tạo entity `Shipment` riêng.

## 7.2. Shipment

Không phải canonical domain entity trong phạm vi hiện tại. Khi xuất hiện trong giao tiếp nghiệp vụ, `shipment` có thể được dùng như danh từ chung cho một lô vận chuyển nhưng canonical entity vẫn là `Booking`.

## 7.3. Delivery — Giao hàng

Quá trình/giai đoạn giao vận của Booking. `Delivery` hiện không phải domain entity độc lập; kế hoạch/trạng thái được thể hiện qua Booking, Delivery Schedule và workflow liên quan.

## 7.4. Booking Code

Mã nội bộ do hệ thống sinh cho Booking và tồn tại ngay khi Booking được tạo.

## 7.5. Carrier Booking Number

External booking reference do Carrier/đơn vị vận chuyển cung cấp. Có thể chưa tồn tại khi Booking vừa được tạo.

Workbook nguồn dùng `Số Booking` theo cách linh hoạt, thậm chí cho phép ghi tạm PO rồi thay bằng Booking/B/L. Hệ thống mới **không dùng một field duy nhất theo cách này** mà tách rõ các reference.

## 7.6. SO — working interpretation: Shipping Order

Nguồn gốc chỉ ghi `Số SO` và `Ngày SO`; không viết đầy đủ chữ viết tắt.

Trong dự án hiện tại, **working interpretation được chấp nhận là `Shipping Order`**, dựa trên ngữ cảnh SO gắn với Booking/Warehouse. Đây chưa được xem là bằng chứng rằng tài liệu nguồn đã định nghĩa chính thức chữ SO.

Nếu nghiệp vụ/upstream system sau này xác nhận SO có nghĩa khác, glossary phải cập nhật.

## 7.7. Shipping Order Number — Số SO

External logistics reference hiện được diễn giải theo working interpretation `Shipping Order`.

Warehouse dùng SO để nhận diện/download danh sách hàng theo nghiệp vụ nguồn.

## 7.8. Shipping Order Date — Ngày SO

Ngày của reference SO tương ứng; nguồn VẬN HÀNH có `Ngày SO` link theo Booking.

## 7.9. Bill of Lading (BL) — Vận đơn

External logistics document/reference gắn với Booking.

Trong phạm vi nguồn hiện tại, hệ thống không được mô tả là đơn vị phát hành/generate BL; BL được lưu/tracking/upload khi có.

## 7.10. Bill of Lading Number — Số BL

External reference number của BL. Không đồng nhất với Booking Code, Carrier Booking Number hoặc SO Number.

## 7.11. Booking Line — Dòng hàng Booking

Phần số lượng của một OrderLine được phân bổ vào một Booking cụ thể.

Một OrderLine có thể được chia qua nhiều Booking.

## 7.12. Carrier — Đơn vị vận chuyển

Canonical term tổng quát cho tổ chức trực tiếp cung cấp dịch vụ vận chuyển.

Có thể gồm Ocean Carrier/Shipping Line, Air Carrier, Road Carrier và Rail Carrier.

## 7.13. Shipping Line — Hãng tàu

Carrier vận chuyển bằng đường biển. Không dùng `ShippingLine` làm canonical field cho mọi phương thức vận tải.

## 7.14. Forwarder

Đơn vị giao nhận/forwarder nếu nghiệp vụ có phát sinh.

Nguồn hiện chưa xác nhận field/entity Forwarder riêng, vì vậy Forwarder chưa là core domain concept bắt buộc. Nếu bổ sung sau, phải phân biệt với Carrier.

## 7.15. Transport Mode — Phương thức vận chuyển

Canonical concept cho loại hình vận tải chính của Booking.

Giá trị chuẩn hóa hiện tại: `Sea`, `Air`, `Road`, `Rail`.

Giá trị `Way` trong nguồn được chuẩn hóa thành `Road`; `Rainway/Railway` được chuẩn hóa thành `Rail`.

## 7.16. Shipment Load Type — Hình thức sử dụng tải/container

Khái niệm khác Equipment Type. Ví dụ: `FCL`, `LCL`.

`LCL` không phải container/equipment type.

## 7.17. Transport Equipment

Thiết bị vận chuyển cụ thể gắn với Booking. Một Booking có thể có nhiều equipment/container.

## 7.18. Equipment Type

Loại thiết bị vận chuyển, ví dụ nguồn có 20DC, 40DC, 40HC, 20RF, 40RF.

`EquipmentType` khác `ShipmentLoadType` và `TransportMode`.

## 7.19. Container Number — Số container

Identifier của container cụ thể, thuộc Transport Equipment.

## 7.20. Seal Number — Số chì / seal

Số seal/chì của container/equipment tương ứng.

## 7.21. Origin Location

Canonical domain term cho điểm đầu của chặng vận tải chính.

## 7.22. Destination Location

Canonical domain term cho điểm cuối của chặng vận tải chính.

## 7.23. POL — Port of Loading

Sea/logistics alias ánh xạ về Origin Location trong domain.

## 7.24. POD — Port of Discharge

Sea/logistics alias ánh xạ về Destination Location trong domain.

## 7.25. ETD — Estimated Time of Departure

Thời điểm dự kiến phương tiện vận tải chính khởi hành khỏi Origin Location.

Workbook nguồn gọi `Ngày đóng cont ETD`; hệ thống mới **không đồng nhất ETD với Loading Date**.

## 7.26. ETA — Estimated Time of Arrival

Thời điểm dự kiến phương tiện vận tải chính đến Destination Location.

## 7.27. ATD — Actual Time of Departure

Thời điểm thực tế phương tiện vận tải chính khởi hành.

Workbook nguồn link ATD khi Booking ở trạng thái `Đang giao`.

## 7.28. ATA — Actual Time of Arrival

Thời điểm thực tế phương tiện vận tải chính đến.

Workbook nguồn link ATA khi Booking ở trạng thái `Đã giao`.

## 7.29. Planned Loading Date

Ngày/thời điểm dự kiến đóng/loading hàng tại Warehouse; khác ETD.

## 7.30. Warehouse Release Date

Ngày/thời điểm hàng thực tế rời Warehouse; khác Planned Loading Date và ETD.

---

# 8. Warehouse & Batch

## 8.1. Warehouse Assignment

Quan hệ nghiệp vụ xác định Warehouse chịu trách nhiệm đóng hàng cho Booking.

Theo quyết định glossary hiện tại, một Booking có một Warehouse đóng hàng chính. Nếu một Order cần xuất hàng từ nhiều Warehouse, tạo nhiều Booking tương ứng.

## 8.2. Batch — Lô sản xuất

Lô sản xuất thực tế của một Product. Batch không thuộc độc quyền một Booking.

## 8.3. Batch Number — Số batch

Business identifier của Batch theo nghiệp vụ sản xuất/traceability.

Unique rule chính xác thuộc Data Dictionary vì có thể phụ thuộc Product/plant/time context.

## 8.4. Batch Allocation — Phân bổ batch

Quan hệ xác định phần số lượng BookingLine được lấy từ Batch nào.

Một BookingLine có thể sử dụng nhiều Batch; một Batch có thể được phân bổ cho nhiều BookingLine/Booking.

## 8.5. Loading Schedule — Lịch đóng hàng

Khái niệm mô tả kế hoạch và tiến độ xử lý hàng tại Warehouse trước khi Booking rời kho.

Nguồn Warehouse có các mốc/trạng thái như hold hàng, dán tem, đã giao hàng. Workflow chính xác thuộc state-machine document.

## 8.6. Delivery Schedule — Lịch giao hàng

Khái niệm mô tả lịch logistics tổng thể của Booking, gồm các mốc/thuộc tính như warehouse release, origin/destination, ETD/ETA, equipment/container, driver và notes.

Loading Schedule và Delivery Schedule là hai concept riêng nhưng cùng liên kết với Booking.

---

# 9. Operations

## 9.1. Work Item — Công việc vận hành

Canonical domain term cho một đơn vị công việc vận hành cần thực hiện cho Booking.

WorkItem có các khái niệm như Title, PIC, Deadline và Status. `Task` là alias phổ thông/UI.

## 9.2. Deadline

Thời hạn cần hoàn thành Work Item; dùng để xác định overdue/chậm tiến độ theo Business Rules.

## 9.3. Ongoing

Trạng thái nguồn xác nhận cho Work Item/công việc vận hành đang được xử lý.

## 9.4. Completed

Trạng thái nguồn xác nhận cho Work Item/công việc vận hành đã hoàn thành.

Không dùng `Completed` của WorkItem để suy diễn trực tiếp trạng thái Order/Booking.

## 9.5. Operational Milestone — Mốc vận hành

Một **mốc thời gian/sự kiện nghiệp vụ có ý nghĩa theo dõi tiến độ**, khác với WorkItem.

- `WorkItem` = việc cần làm, có PIC/deadline/status.
- `OperationalMilestone` = mốc/event đã lên kế hoạch hoặc đã xảy ra, thường có planned/actual date/time, reference hoặc completion metadata.

Workbook nguồn có nhiều milestone cần chuẩn hóa dần, ví dụ:

### Sản xuất / batch / nhãn

- Checking Batch Date;
- Get Batch Date;
- thông báo hoàn thành nhãn/gửi mẫu nguyên vật liệu;
- Sending to SPP / PP / Purchase Date;
- Raw Material Order Date;
- Raw Material Arrival Date;
- Production Date;
- Finished Production Date;
- Stick Sub-Label Date;
- Finished Stick Sub-Label Date.

### Booking / logistics

- Booking Logistic Date;
- Cut-off Date;
- Select Container Date;
- Loading Date;
- ETD;
- ATD;
- ETA;
- ATA.

### Hải quan / chứng từ

- Customs Clearance Date;
- Customs Clearance Number;
- Invoice Issue Date;
- Packing List Issue Date;
- Sending Sample Date;
- HC Submit Date;
- HC Issue Date;
- CO Submit Date;
- CO Issue Date;
- CO Number;
- Docs Sent;
- Release Time.

Danh sách trên **không mặc định là một enum cố định**. Tài liệu Domain Model/Workflow sẽ quyết định milestone nào là field chuyên biệt, milestone record, WorkItem completion date hoặc Document metadata.

## 9.6. Customs Clearance — Khai báo/thông quan hải quan

Khái niệm nghiệp vụ liên quan đến Tờ khai hải quan. Workbook nguồn theo dõi `Customs Clearance Date` và `Customs Clearance Number`.

Exact workflow, status và mapping với Customs Declaration thuộc Workflow/Documents/Data Dictionary.

---

# 10. Finance & Receivables

## 10.1. Receivable — Khoản phải thu

Nghĩa vụ tài chính Customer phải thanh toán.

Hệ thống cần theo dõi Amount Due, Due Date, Amount Paid, Outstanding Balance và các Payment Transaction. Exact relation với Order/Booking thuộc Domain Model/Business Rules.

## 10.2. Amount Due — Số tiền phải thu

Tổng số tiền phải thu của Receivable theo rule áp dụng. Công thức chính xác thuộc Business Rules.

## 10.3. Payment Transaction — Giao dịch thanh toán

Một lần ghi nhận khoản tiền thực tế Customer thanh toán. Hệ thống hỗ trợ partial payment/multiple transactions.

## 10.4. Payment

Thuật ngữ nghiệp vụ tổng quát cho hành vi thanh toán. Chưa tạo canonical entity `Payment` trung gian riêng nếu không có thêm ý nghĩa nghiệp vụ.

## 10.5. Amount Paid — Số tiền đã thanh toán

Tổng tiền đã ghi nhận thanh toán cho Receivable. Cách tính chính xác, refund/FX exception thuộc Business Rules.

## 10.6. Outstanding Balance — Dư nợ còn lại

Phần Amount Due chưa được thanh toán. Công thức chính xác và exception thuộc Business Rules.

## 10.7. Financial Adjustment — Điều chỉnh tài chính

Khái niệm cho điều chỉnh ảnh hưởng giá trị tài chính của Order/Receivable.

Các label nguồn đã xác nhận gồm `Debit Note` và `Credit Note`.

### Quy tắc quan trọng

Glossary **không tự áp dụng định nghĩa kế toán tổng quát về bên phát hành** để đảo nghĩa nguồn.

Workbook gốc của dự án dùng convention nhập liệu:

- `Debit note`: **điều chỉnh trừ, ghi số âm**;
- `Credit note`: **điều chỉnh cộng, ghi số dương**.

Đây là **source convention của dự án**, chưa phải kết luận về accounting perspective/issuer semantics. Business Rules phải xác nhận cách các adjustment này tác động Final Amount/Receivable trước khi implementation.

## 10.8. Debit Note

Một loại Financial Adjustment theo label nguồn.

Trong workbook gốc hiện tại, Debit Note được nhập theo convention **subtractive/negative**.

Không diễn giải thêm rằng Debit Note luôn “tăng nghĩa vụ khách hàng” nếu chưa xác nhận perspective của chứng từ.

## 10.9. Credit Note

Một loại Financial Adjustment theo label nguồn.

Trong workbook gốc hiện tại, Credit Note được nhập theo convention **additive/positive**.

Không diễn giải thêm rằng Credit Note luôn “giảm nghĩa vụ khách hàng” nếu chưa xác nhận perspective của chứng từ.

## 10.10. Cost — Chi phí làm hàng

Chi phí nghiệp vụ phát sinh liên quan đến Booking/logistics/export operation.

Nguồn có freight, handling fee, container fee, D/O fee, terminal fee, seal fee, inland trucking, packing/storage và các chi phí khác.

Taxonomy, currency, approval và aggregation thuộc Domain/Data/Business Rules.

---

# 11. KPI & Incentive

## 11.1. KPI Target — Mục tiêu KPI

Mục tiêu sản lượng hoặc giá trị của Customer theo kỳ.

## 11.2. Incentive Program — Chính sách thưởng

Chính sách thưởng/ưu đãi của Customer theo kỳ và kết quả đạt được.

## 11.3. Incentive Tier — Mức thưởng

Một mức/ngưỡng trong Incentive Program.

`KPI Target` và `Incentive Program` là hai khái niệm khác nhau: KPI là mục tiêu; Incentive là chính sách thưởng.

---

# 12. Documents

## 12.1. Document Type — Loại chứng từ

Canonical classification của chứng từ.

Các loại hiện được nguồn/phiên làm rõ xác nhận hoặc chuẩn hóa gồm:

- Purchase Order;
- Quotation;
- Proforma Invoice;
- Commercial Invoice;
- Packing List;
- Batch Information;
- Bill of Lading;
- Certificate of Origin;
- Customs Declaration;
- Health Certificate.

Danh sách được phép mở rộng.

## 12.2. Document Requirement — Yêu cầu chứng từ

Quy định Customer yêu cầu những Document Type nào.

Nguồn Customer có `Document require` với các loại như Invoice, Packing List, TKHQ, BL, CO, HC tàu, HC Air, HC thú y, HC y tế và loại khác.

## 12.3. Document Template — Mẫu chứng từ

Template dùng để render/generate một số Document Type mà hệ thống được nghiệp vụ yêu cầu xuất theo form có sẵn.

Template có thể có Version, Effective From và template definition/file; chi tiết thuộc Document Design.

## 12.4. Business Document — Bản ghi chứng từ nghiệp vụ

Canonical umbrella concept cho **một chứng từ thực tế** được lưu/tracking trong hệ thống, bất kể nguồn của file là hệ thống generate hay người dùng upload.

Khái niệm có thể mang:

- Document Type;
- liên kết Order/Booking/Customer theo nghiệp vụ;
- Document Number nếu có;
- Issue/Received/Uploaded Date nếu có;
- status/tracking metadata;
- File Reference;
- Source (`SystemGenerated` hoặc `ExternalUploaded`).

Exact schema thuộc Domain Model/Data Dictionary.

## 12.5. Generated Document — Chứng từ hệ thống sinh

Business Document được hệ thống render từ dữ liệu nghiệp vụ + Document Template.

**Nguồn xác nhận rõ khả năng xuất/in theo form có sẵn:**

- ở Order: PO, PI, Quotation;
- ở Booking: IV/Commercial Invoice, Packing List, Batch Information.

Các loại trên là **ứng viên hệ thống sinh được** theo template; template/data mapping chi tiết phải được thiết kế riêng.

## 12.6. Uploaded / External Document — Chứng từ tải lên / bên ngoài

Business Document do người dùng upload hoặc nhận từ Carrier/cơ quan/đối tác bên ngoài; hệ thống chủ yếu lưu file, metadata và tiến độ/chứng từ liên quan.

**Phần lớn chứng từ xuất khẩu trong danh sách yêu cầu Customer có thể thuộc nhóm này**, ví dụ:

- Bill of Lading;
- Customs Declaration / TKHQ;
- Certificate of Origin;
- Health Certificate và các biến thể HC;
- các certificate/chứng từ khác do bên ngoài phát hành.

Không mặc định hệ thống generate các chứng từ này nếu nguồn không yêu cầu.

## 12.7. Quotation — Báo giá

Chứng từ báo giá được nguồn yêu cầu có thể in/xuất từ Order theo form có sẵn.

## 12.8. Proforma Invoice (PI) — Hóa đơn chiếu lệ

Chứng từ thương mại được tạo/phát hành từ dữ liệu Order theo workflow nghiệp vụ.

`PI` không phải Order và không đồng nhất Commercial Invoice.

## 12.9. Commercial Invoice — Hóa đơn thương mại

Chứng từ hóa đơn thương mại dùng cho lô hàng/Booking.

`IV` trong tài liệu nguồn được chuẩn hóa là alias/abbreviation của `CommercialInvoice`, không tạo Document Type `IV` riêng.

Nguồn Booking yêu cầu có thể in/xuất IV theo form có sẵn.

## 12.10. Packing List (PL) — Phiếu đóng gói

Chứng từ mô tả chi tiết hàng thực tế của Booking như Product, quantity, packaging, weight, volume và thông tin đóng gói.

Nguồn Booking yêu cầu có thể in/xuất PL theo form có sẵn.

## 12.11. Batch Information

Chứng từ thể hiện thông tin Batch/Batch Allocation của hàng trong Booking.

Nguồn Booking yêu cầu có thể in/xuất Batch Information theo form có sẵn.

## 12.12. Bill of Lading (BL)

External logistics document/reference. Trong scope nguồn hiện tại hệ thống lưu/tracking/upload BL; không có yêu cầu hệ thống phát hành BL.

## 12.13. Certificate of Origin (CO) — Chứng nhận xuất xứ

`CO` là canonical abbreviation cho Certificate of Origin.

Nguồn vận hành theo dõi CO Submit Date, CO Issue Date và CO No. Vì vậy CO phù hợp với mô hình Uploaded/External Document + Operational Milestone, trừ khi nghiệp vụ sau này yêu cầu integration/generation cụ thể.

## 12.14. Customs Declaration (TKHQ) — Tờ khai hải quan

`TKHQ` được xác nhận là **Tờ khai hải quan**; canonical English term là `CustomsDeclaration`.

Nguồn vận hành theo dõi Customs Clearance Date/Number. Đây chủ yếu là external/uploaded document/reference trong scope hiện tại.

## 12.15. Health Certificate (HC)

Canonical mapping hiện tại cho `HC` là Health Certificate.

Workbook nguồn cho thấy Document Requirement có nhiều biến thể: HC tàu, HC Air, HC thú y, HC y tế. Vì vậy `HealthCertificate` là umbrella term; subtype/taxonomy cụ thể được bổ sung ở Data Dictionary/Documents khi nghiệp vụ khóa.

Nguồn vận hành theo dõi HC Submit Date và HC Issue Date; không mặc định hệ thống là đơn vị phát hành HC.

## 12.16. Document Source

Phân loại nguồn của Business Document:

- `SystemGenerated`: hệ thống render từ template + dữ liệu;
- `ExternalUploaded`: file/chứng từ được nhận từ bên ngoài hoặc người dùng upload.

Có thể bổ sung source khác sau nếu integration phát sinh; integration nghiệp vụ ngoài SSO hiện thuộc Future Scope.

---

# 13. System & Supporting Concepts

## 13.1. Role

Nhóm quyền/authorization role được gán cho User theo security model; không đồng nhất business organization như Customer/Warehouse.

## 13.2. Permission

Quyền cho phép thực hiện action trên resource/data scope cụ thể. Permission model chi tiết thuộc Role & Permission Matrix.

## 13.3. Master Data

Dữ liệu tham chiếu dùng chung được quản trị có kiểm soát, ví dụ location, transport mode, equipment type, Region hoặc business taxonomy tùy thiết kế cuối cùng.

## 13.4. Audit Log

Bản ghi lịch sử ai đã thay đổi dữ liệu nào, khi nào và nội dung thay đổi theo Audit Design.

## 13.5. Notification

Thông báo nội bộ về event cần chú ý như contract expiring soon, payment due soon/overdue, WorkItem overdue và business events khác.

Email/SMS/push external channels không thuộc scope hiện tại.

---

# 14. Canonical Relationship Summary

Phần này chỉ tóm tắt các quan hệ ngữ nghĩa quan trọng đã được chốt; exact schema/cardinality khác thuộc Domain Model.

```text
Customer
├── CustomerAccount                // 1:1 theo Project Scope
├── Contract(s)
├── KpiTarget(s)
├── IncentiveProgram(s)
├── DocumentRequirement(s)
└── Order(s)

Product
├── ProductPrice history
└── Batch(s)

Order
├── OrderLine(s)
└── Booking(s)                     // Order 1:N Booking đã chốt

OrderLine
└── BookingLine allocation(s)

Booking
├── one primary Warehouse          // đã chốt
├── BookingLine(s)
├── TransportEquipment(s)
├── WorkItem(s)
├── OperationalMilestone(s)
├── LoadingSchedule
├── DeliverySchedule
├── BookingCode
├── CarrierBookingNumber?
├── ShippingOrderNumber?
└── BillOfLadingNumber?

BookingLine
└── BatchAllocation(s)

Receivable
└── PaymentTransaction(s)

BusinessDocument
├── GeneratedDocument
└── Uploaded/ExternalDocument
```

---

# 15. Abbreviation & Source Mapping Reference

| Source term / abbreviation | Canonical meaning | Ghi chú |
|---|---|---|
| KDQT | Kinh Doanh Quốc Tế | Tên phòng nghiệp vụ |
| `Số PO` tự sinh | `OrderNumber` | Không dùng làm CustomerPoNumber |
| PO | Purchase Order | Document/Customer PO theo ngữ cảnh; không phải entity song song với Order |
| Customer PO | `CustomerPoNumber` | External reference nếu có |
| PI | Proforma Invoice | Khác Commercial Invoice |
| IV | Commercial Invoice | Alias từ tài liệu nguồn |
| PL | Packing List | Có thể system-generate theo form nguồn |
| SO | Shipping Order — working interpretation | Nguồn chỉ ghi `Số SO`; cần cập nhật nếu nghiệp vụ xác nhận nghĩa khác |
| BL | Bill of Lading | External/uploaded document/reference |
| CO | Certificate of Origin | External/uploaded/tracked |
| TKHQ | Tờ khai hải quan / Customs Declaration | Được nghiệp vụ xác nhận |
| HC | Health Certificate | Umbrella term; nguồn có nhiều HC subtype |
| PIC | Person In Charge | Primary owner của WorkItem |
| ETD | Estimated Time of Departure | Khác Loading Date |
| ATD | Actual Time of Departure | Actual departure milestone |
| ETA | Estimated Time of Arrival | Estimated arrival |
| ATA | Actual Time of Arrival | Actual arrival milestone |
| POL | Port of Loading | Sea alias của Origin Location |
| POD | Port of Discharge | Sea alias của Destination Location |
| FOC | Free of Charge | Hàng/số lượng miễn phí |
| FCL | Full Container Load | Shipment Load Type |
| LCL | Less than Container Load | Shipment Load Type, không phải Equipment Type |
| EXW | Ex Works | Incoterm theo nguồn |
| FOB | Free On Board | Incoterm theo nguồn |
| DAT | Delivered At Terminal | Giữ nguyên theo nguồn đến khi nghiệp vụ thay đổi |
| REGION | Region | Nguồn Bắc/Nam; khác Market |

---

# 16. Terms intentionally not locked yet

Các khái niệm dưới đây có thể được bổ sung/chi tiết hóa trong tài liệu sau:

- exact Order lifecycle states;
- exact Booking state machine/transitions;
- exact Contract lifecycle mở rộng;
- Forwarder model;
- Receivable-to-Order/Booking cardinality;
- Payment reference/bank reconciliation fields;
- exact Debit/Credit Note accounting perspective và effect on Final Amount/Receivable;
- detailed cost taxonomy;
- exact warehouse/loading status model;
- Product Batch unique rules;
- exact KPI calculation rules;
- detailed Incentive calculation;
- Health Certificate subtype taxonomy;
- document status/version/approval workflow;
- location hierarchy;
- Region taxonomy ngoài Bắc/Nam nếu phát sinh;
- master-data ownership;
- milestone storage model;
- additional export/logistics abbreviations phát sinh từ nghiệp vụ thực tế.

Khi thuật ngữ mới xuất hiện trong Domain Model, Workflow, Data Dictionary hoặc API mà có khả năng gây hiểu khác nhau, thuật ngữ đó phải được bổ sung trở lại glossary.

---

# 17. Decision Register

| ID | Quyết định đã chốt |
|---|---|
| GLOSSARY-01 | `Order` là canonical transaction entity; tách internal `OrderNumber` và `CustomerPoNumber`; PI là document |
| GLOSSARY-02 | Booking là đơn vị vận chuyển chính; không có Shipment entity riêng |
| GLOSSARY-03 | Một OrderLine có thể được chia qua nhiều BookingLine |
| GLOSSARY-04 | Batch được phân bổ ở cấp BookingLine; hỗ trợ nhiều Batch |
| GLOSSARY-05 | SO được hiểu theo working interpretation là Shipping Order |
| GLOSSARY-06 | Tách BookingCode, CarrierBookingNumber, ShippingOrderNumber và BillOfLadingNumber |
| GLOSSARY-07 | Carrier là canonical term; Shipping Line là Sea Carrier; Forwarder chưa bắt buộc |
| GLOSSARY-08 | Tách TransportMode, ShipmentLoadType và EquipmentType |
| GLOSSARY-09 | Domain dùng OriginLocation/DestinationLocation; POL/POD là alias theo ngữ cảnh Sea |
| GLOSSARY-10 | Một Booking có một Warehouse đóng hàng chính |
| GLOSSARY-11 | LoadingSchedule và DeliverySchedule là hai concept riêng |
| GLOSSARY-12 | ETD/ETA chuẩn logistics, tách khỏi loading/release dates |
| GLOSSARY-13 | Một Booking có thể có nhiều TransportEquipment/container |
| GLOSSARY-14 | Batch là traceability concept theo Product; BatchAllocation nối Batch với BookingLine |
| GLOSSARY-15 | Product tương ứng SKU/mã hàng; ProductionCode là business identifier chính |
| GLOSSARY-16 | ProductPrice là concept có effective period/history |
| GLOSSARY-17 | ProductPrice là base price; giá thực tế giữ ở OrderLine; chưa có CustomerProductPrice riêng |
| GLOSSARY-18 | Tách Customer, CustomerAccount và User |
| GLOSSARY-19 | Tách Warehouse, WarehouseAccount và User |
| GLOSSARY-20 | Incoterm là canonical term; Customer có default và Order giữ giá trị áp dụng riêng |
| GLOSSARY-21 | PaymentTerm là rule; DueDate là ngày cụ thể của Receivable |
| GLOSSARY-22 | Receivable hỗ trợ nhiều PaymentTransaction; Payment chưa cần entity trung gian riêng |
| GLOSSARY-23 | Debit Note/Credit Note là FinancialAdjustment; source convention hiện là Debit âm/trừ, Credit dương/cộng; accounting perspective để Business Rules khóa |
| GLOSSARY-24 | Tách KpiTarget và IncentiveProgram |
| GLOSSARY-25 | Customer có nhiều Contract theo thời gian; ExpiringSoon là derived alert |
| GLOSSARY-26 | WorkItem là canonical term; có primary PIC ở cấp glossary |
| GLOSSARY-27 | Tách DocumentRequirement, DocumentTemplate và document instance |
| GLOSSARY-28 | Tách Quotation, ProformaInvoice và CommercialInvoice; IV là alias CommercialInvoice |
| GLOSSARY-29 | PO/PI/Quotation và IV/PL/BatchInformation là các loại nguồn yêu cầu có thể render; BL là external document/reference |
| GLOSSARY-30 | CO = Certificate of Origin; TKHQ = Customs Declaration; HC = Health Certificate umbrella; glossary được bổ sung dần |
| GLOSSARY-31 | `BusinessDocument` là umbrella cho chứng từ thực tế; phân biệt `SystemGenerated` và `ExternalUploaded` |
| GLOSSARY-32 | Tách `Market` khỏi `Region`; nguồn Region hiện dùng Bắc/Nam cho Finance routing |
| GLOSSARY-33 | Bổ sung `OperationalMilestone` để chuẩn hóa các mốc VẬN HÀNH trong workbook |
| GLOSSARY-34 | SO giữ là working interpretation `Shipping Order`, không tuyên bố tài liệu nguồn đã định nghĩa chính thức |

---

# 18. Governance

1. Tài liệu này là nguồn canonical cho terminology của dự án.
2. Khi tài liệu downstream dùng thuật ngữ khác cho cùng concept, ưu tiên canonical term trong glossary hoặc cập nhật glossary bằng quyết định có chủ đích.
3. Không tạo entity/class/API resource mới chỉ vì có một alias nghiệp vụ khác.
4. Không tự thay nghĩa thuật ngữ nguồn khi chưa có bằng chứng hoặc xác nhận nghiệp vụ.
5. Phải phân biệt rõ **source fact**, **business clarification** và **working interpretation**.
6. Các thuật ngữ có thể được bổ sung khi xây dựng hệ thống; thay đổi ảnh hưởng Project Scope phải quay lại `00-project-scope.md`.
7. Thay đổi ảnh hưởng state transition phải được phản ánh trong workflow/state-machine document.
8. Thay đổi ảnh hưởng schema/identifier/validation phải được phản ánh trong Domain Model/Data Dictionary.
9. Công thức và accounting/business calculation phải được khóa trong Business Rules, không coi ví dụ trong glossary là nguồn triển khai cuối cùng.
10. Với chứng từ, mặc định phân biệt `DocumentRequirement`, `DocumentTemplate`, `BusinessDocument`, `GeneratedDocument` và `Uploaded/ExternalDocument`; không giả định mọi Document Type đều do hệ thống generate.
