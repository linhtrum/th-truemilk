# 01 — Domain Glossary

## Hệ thống Quản lý Xuất khẩu KDQT (B2B Export Management System)

**Phiên bản:** 0.1  
**Trạng thái:** Living Draft — Canonical Vocabulary  
**Ngày cập nhật:** 2026-08-21  
**Baseline:** `docs/00-project-scope.md`  
**Nguồn nghiệp vụ:** `website_modules_analysis.md`, tài liệu PPTX yêu cầu gốc, workbook Excel nghiệp vụ gốc và các quyết định làm rõ `GLOSSARY-01` → `GLOSSARY-30`.

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

### 2.1. Canonical term

Mỗi khái niệm nghiệp vụ nên có **một canonical term** dùng xuyên suốt domain model và code.

Ví dụ:

- `Product` là canonical term; `SKU`, `Item`, `Goods`, `Mã hàng` là alias/ngữ cảnh sử dụng nếu cùng chỉ một đối tượng.
- `WorkItem` là canonical term; `Task` là alias phổ thông/UI.
- `Incoterm` là canonical term; `Trade Term` là alias nghiệp vụ/UI.
- `Carrier` là canonical term tổng quát; `Shipping Line` là carrier trong vận tải biển.

### 2.2. Business identifier và technical identifier

Business identifier không được đồng nhất với technical identifier.

Ví dụ:

```text
Product.Id              // technical identifier
Product.ProductionCode  // business identifier

Booking.Id              // technical identifier
Booking.BookingCode     // internal business identifier
```

### 2.3. Internal reference và external reference

Các mã nội bộ của hệ thống phải được phân biệt với mã do Customer/Carrier/đối tác cung cấp.

Ví dụ:

```text
OrderNumber               // hệ thống sinh
CustomerPoNumber           // Customer cung cấp
BookingCode                // hệ thống sinh
CarrierBookingNumber       // Carrier/Forwarder cung cấp
ShippingOrderNumber        // external logistics reference
BillOfLadingNumber         // external logistics reference
```

### 2.4. Snapshot commercial data

Các điều kiện thương mại đã áp dụng cho giao dịch phải được hiểu là dữ liệu của giao dịch đó, không phụ thuộc động vào master data sau này.

Ví dụ:

- `Customer.DefaultIncoterm` → default;
- `Order.Incoterm` → snapshot áp dụng cho Order;
- `Customer.DefaultPaymentTerm` → default;
- `Order.PaymentTerm` → snapshot áp dụng cho Order;
- `ProductPrice` → giá cơ sở theo hiệu lực;
- `OrderLine.UnitPrice` → giá thực tế đã chốt cho OrderLine.

### 2.5. Glossary không thay thế Domain Model

Các quan hệ trong tài liệu này thể hiện **ngữ nghĩa nghiệp vụ**, không mặc định quyết định cấu trúc table/entity cuối cùng nếu chưa được khóa trong tài liệu domain/data tương ứng.

---

# 3. Actors, Organizations & Identity

## 3.1. KDQT

**Tên đầy đủ:** Phòng Kinh Doanh Quốc Tế.  
**English:** International Business / Export Sales function, tùy naming chính thức của tổ chức được xác nhận sau.

Nhóm người dùng nội bộ chịu trách nhiệm quản lý Customer, Product, Order, Booking, vận hành, chi phí, giao hàng và theo dõi thanh toán.

---

## 3.2. Customer — Khách hàng

Business entity đại diện cho tổ chức/doanh nghiệp B2B mua hàng.

Customer có thể sở hữu hoặc liên quan đến:

- Contract;
- Order;
- KPI Target;
- Incentive Program;
- default commercial terms;
- Document Requirements;
- Customer Account.

```text
Customer != User
Customer != CustomerAccount
```

---

## 3.3. Customer Account — Tài khoản khách hàng

Account truy cập Customer Portal của một Customer cụ thể.

Theo Project Scope hiện tại:

```text
Customer 1 ─── 1 CustomerAccount
```

Customer Account xác định data scope của Customer Portal, nhưng không phải business entity Customer.

---

## 3.4. Warehouse — Kho

Business entity đại diện cho kho/địa điểm thực hiện các hoạt động như:

- hold hàng;
- nhập/kiểm tra Batch;
- dán tem;
- đóng/loading hàng;
- bàn giao hàng ra khỏi kho.

Theo glossary hiện tại, mỗi Booking có **một Warehouse đóng hàng chính**.

---

## 3.5. Warehouse Account — Tài khoản kho

Account truy cập Warehouse Portal cho một Warehouse cụ thể.

Theo Project Scope hiện tại:

```text
Warehouse 1 ─── 1 WarehouseAccount
```

Warehouse Account chỉ được truy cập dữ liệu thuộc Warehouse tương ứng.

---

## 3.6. Finance User — Người dùng tài chính/kế toán

User truy cập Finance Portal để xem, lọc và download dữ liệu về Order/Booking, chi phí và công nợ.

Finance Portal hiện là read-only ở cấp nghiệp vụ.

---

## 3.7. System Administrator

User có quyền quản trị hệ thống như user, role, permission, account nghiệp vụ, master data, system settings, audit, import/export configuration và document templates.

System Administrator không mặc định có quyền thực hiện nghiệp vụ KDQT/Warehouse/Finance nếu không được cấp permission tương ứng.

---

## 3.8. User — Người dùng / Identity

Danh tính dùng cho authentication và authorization.

User có thể xác thực bằng local account hoặc SSO theo Project Scope.

```text
Business entity
    !=
Authentication identity
```

Không đặt credential, password hash, token hoặc role trực tiếp trong `Customer` hay `Warehouse`.

---

## 3.9. PIC — Person In Charge

Người chịu trách nhiệm chính cho một Work Item.

Ở cấp glossary hiện tại:

```text
WorkItem ─── 1 primary PIC
```

Collaborator/multiple-assignee model nếu cần sẽ được xác định ở Domain Model.

---

# 4. Customer & Commercial Terms

## 4.1. Contract — Hợp đồng khách hàng

Hợp đồng thương mại của Customer.

```text
Customer
└── 0..N Contract
```

Thuộc tính khái niệm tối thiểu:

- Contract Number;
- Effective Date;
- Expiry Date;
- Status.

Một Customer có thể có nhiều Contract theo thời gian để giữ lịch sử.

---

## 4.2. Contract Status

Lifecycle status cốt lõi của Contract.

Ở cấp glossary hiện tại tối thiểu phân biệt:

- `Effective`;
- `Expired`.

`ExpiringSoon` là **derived alert**, không phải lifecycle status cốt lõi.

```text
ExpiryDate + alert threshold
          ↓
     ExpiringSoon alert
```

---

## 4.3. Incoterm

Canonical domain term cho điều kiện thương mại; `Trade Term` là UI/business alias.

Nguồn hiện sử dụng các giá trị như:

- EXW;
- FOB;
- DAT.

Không tự động thay đổi `DAT` sang thuật ngữ Incoterms khác nếu nghiệp vụ chưa xác nhận.

```text
Customer.DefaultIncoterm
Order.Incoterm
ProductPrice.Incoterm
```

`Order.Incoterm` là snapshot của điều kiện thực tế áp dụng cho Order.

---

## 4.4. Payment Term — Điều khoản thanh toán

Quy tắc thương mại xác định khi nào khoản phải thu đến hạn.

Ví dụ nghiệp vụ có thể gồm:

- prepaid;
- T/T 30 days;
- T/T 45 days;
- N days after BL date.

```text
Customer.DefaultPaymentTerm
Order.PaymentTerm
```

`Order.PaymentTerm` là snapshot áp dụng cho Order.

---

## 4.5. Due Date — Ngày đến hạn

Ngày cụ thể mà một Receivable đến hạn thanh toán.

```text
PaymentTerm = rule
DueDate     = concrete date
```

Due Date được xác định từ Payment Term và mốc tham chiếu theo Business Rules.

---

## 4.6. Currency — Đồng tiền

Đơn vị tiền tệ áp dụng cho pricing, Order, adjustment, receivable và payment.

Nguồn hiện sử dụng tối thiểu:

- VND;
- USD.

---

# 5. Product & Pricing

## 5.1. Product — Sản phẩm / Mã hàng

Canonical domain term cho một SKU/mã hàng cụ thể có thể xuất hiện trên OrderLine và BookingLine.

```text
Product
├── Id                  // technical identifier
├── ProductionCode      // business identifier
├── NameVi
├── NameEn
├── HSCode
├── ProductBarcode
├── BlockBarcode
├── CartonBarcode
├── Packaging
├── ShelfLife
├── StorageCondition
└── PhysicalSpecifications
```

`SKU`, `Item`, `Goods`, `Hàng hóa`, `Mã hàng` có thể là alias theo ngữ cảnh nhưng không tạo domain entity song song nếu cùng chỉ một Product.

---

## 5.2. Production Code — Mã sản phẩm

Business identifier chính của Product.

```text
Mã hàng = Product.ProductionCode
```

trừ khi màn hình/tài liệu ghi rõ đang nói đến barcode hoặc mã khác.

---

## 5.3. Barcode

Identifier bổ sung của Product/packaging level.

Nguồn hiện có các loại:

- Product Barcode;
- Block Barcode;
- Carton Barcode.

Barcode không thay thế Production Code làm canonical business identifier của Product.

---

## 5.4. HS Code

Mã phân loại hàng hóa phục vụ nghiệp vụ hải quan/xuất khẩu.

HS Code là attribute/reference của Product, không phải Product identifier chính.

---

## 5.5. Product Price — Giá sản phẩm

Domain concept riêng, có hiệu lực theo thời gian.

```text
Product
└── 1..N ProductPrice
    ├── Incoterm
    ├── Currency
    ├── UnitPrice
    ├── EffectiveFrom
    └── EffectiveTo?
```

Không overwrite giá cũ khi có bảng tăng giá mới; giá cũ trở thành price history.

---

## 5.6. Base Product Price — Giá cơ sở

Giá cơ sở của Product theo Incoterm, Currency và effective period.

Không tạo `CustomerProductPrice` như một canonical concept ở thời điểm hiện tại vì nguồn chưa xác nhận bảng giá độc lập cho từng Customer.

---

## 5.7. Order Line Unit Price — Giá chốt trên dòng Order

Giá thực tế được áp dụng cho OrderLine và được snapshot trên giao dịch.

Khái niệm tổng quát:

```text
Base Product Price
+/- commercial adjustments
          ↓
OrderLine.UnitPrice
```

Việc tính chính xác được định nghĩa trong Business Rules.

---

## 5.8. Price Adjustment — Điều chỉnh giá

Khái niệm tổng quát cho các thay đổi khi xác định giá giao dịch như discount, label fee hoặc điều chỉnh thương mại khác.

Không đồng nhất `PriceAdjustment` với `FinancialAdjustment` sau khi Order đã phát sinh nghĩa vụ tài chính.

---

# 6. Order Domain

## 6.1. Order — Đơn hàng

Canonical business entity đại diện cho một đơn hàng xuất khẩu của Customer.

Order chứa các điều kiện thương mại và nhiều OrderLine, đồng thời có thể được thực hiện qua nhiều Booking.

```text
Customer
└── Order
    ├── OrderLines
    └── 1..N Booking
```

Order có lifecycle riêng; không dùng Booking status trực tiếp làm Order status.

---

## 6.2. Order Number — Mã đơn hàng nội bộ

Business identifier nội bộ do hệ thống sinh cho Order.

Nguồn đề xuất pattern dạng:

```text
TênKH/MãNước-Năm-SốTT
```

Quy tắc chính xác được định nghĩa trong Business Rules/Data Dictionary.

---

## 6.3. Customer PO Number — Số PO của khách hàng

Số Purchase Order do Customer cung cấp.

```text
OrderNumber       != CustomerPoNumber
```

Một Order của hệ thống phải phân biệt mã nội bộ với PO reference của Customer.

---

## 6.4. Purchase Order (PO)

Đơn đặt hàng/chứng từ PO của Customer hoặc chứng từ PO theo ngữ cảnh nghiệp vụ.

Trong domain model:

- canonical transaction entity là `Order`;
- không tạo entity `PurchaseOrder` song song với Order chỉ vì UI/tài liệu gọi “PO”;
- `CustomerPoNumber` là external/business reference;
- nếu hệ thống render/export PO form, đó là một `GeneratedDocument` loại `PurchaseOrder`, không phải một Order thứ hai.

---

## 6.5. Order Line — Dòng hàng đơn hàng

Một dòng hàng thương mại trong Order.

Khái niệm có thể chứa:

- Product;
- ordered quantity;
- unit price;
- discount;
- tax;
- FOC;
- fee/adjustment liên quan.

Một OrderLine có thể được chia qua nhiều BookingLine.

---

## 6.6. Ordered Quantity

Số lượng Product được đặt trên OrderLine.

---

## 6.7. Booked Quantity

Tổng số lượng của OrderLine đã được phân bổ sang BookingLine.

```text
BookedQuantity
= SUM(BookingLine.Quantity for OrderLine)
```

---

## 6.8. Remaining Quantity

Số lượng của OrderLine còn có thể phân bổ sang Booking.

Khái niệm cơ bản:

```text
RemainingQuantity
= OrderedQuantity - BookedQuantity
```

FOC, cancellation, adjustment và các ngoại lệ được định nghĩa ở Business Rules.

---

## 6.9. FOC — Free of Charge

Số lượng/hàng hóa được cung cấp miễn phí theo điều kiện thương mại.

Cách FOC ảnh hưởng ordered quantity, booked quantity, invoice value và KPI được định nghĩa sau trong Business Rules.

---

# 7. Booking & Logistics

## 7.1. Booking

Canonical domain entity đại diện cho **một đợt/lần thực hiện vận chuyển** toàn bộ hoặc một phần của Order.

```text
Order 1 ─── N Booking
```

Booking là đơn vị vận chuyển chính của hệ thống; **không tạo entity `Shipment` riêng** ở thời điểm hiện tại.

---

## 7.2. Shipment

Không phải canonical domain entity trong phạm vi hiện tại.

Khi xuất hiện trong giao tiếp nghiệp vụ, `shipment` có thể được dùng như danh từ chung cho một lô vận chuyển, nhưng canonical entity vẫn là `Booking`.

---

## 7.3. Delivery — Giao hàng

Quá trình/giai đoạn giao vận của Booking.

`Delivery` hiện không phải domain entity độc lập; trạng thái và kế hoạch được thể hiện qua Booking/Delivery Schedule và các workflow liên quan.

---

## 7.4. Booking Code

Mã nội bộ do hệ thống sinh cho Booking và tồn tại ngay khi Booking được tạo.

```text
Booking.Id          // technical identifier
Booking.BookingCode // internal business identifier
```

---

## 7.5. Carrier Booking Number

External booking reference do Carrier/đơn vị vận chuyển cung cấp.

Có thể chưa tồn tại khi Booking vừa được tạo.

```text
BookingCode != CarrierBookingNumber
```

---

## 7.6. Shipping Order (SO)

**SO = Shipping Order** theo cách hiểu nghiệp vụ đã được xác nhận cho dự án.

Shipping Order là chứng từ/mã tham chiếu logistics liên quan đến Booking.

```text
Booking
├── ShippingOrderNumber
└── ShippingOrderDate
```

Tài liệu nguồn không viết đầy đủ chữ “Shipping Order”; cách mở rộng chữ viết tắt này được xác nhận trong phiên làm rõ dựa trên ngữ cảnh SO gắn với Booking/Warehouse.

---

## 7.7. Shipping Order Number — Số SO

External logistics reference của Shipping Order.

Warehouse có thể dùng SO để nhận diện/download danh sách hàng theo nghiệp vụ.

---

## 7.8. Shipping Order Date — Ngày SO

Ngày của Shipping Order/reference tương ứng.

Có thể nullable trước khi nhận SO.

---

## 7.9. Bill of Lading (BL) — Vận đơn

External logistics document do Carrier/Shipping Line cung cấp cho Booking.

Trong scope hiện tại hệ thống **không generate BL**; hệ thống lưu metadata/reference/file nếu có.

---

## 7.10. Bill of Lading Number — Số BL

External reference number của Bill of Lading.

```text
Booking
└── BillOfLadingNumber
```

Không đồng nhất với Booking Code, Carrier Booking Number hoặc SO Number.

---

## 7.11. Booking Line — Dòng hàng Booking

Phần số lượng của một OrderLine được phân bổ vào một Booking cụ thể.

```text
OrderLine 1 ─── N BookingLine
Booking   1 ─── N BookingLine
```

Một OrderLine có thể được chia qua nhiều Booking.

---

## 7.12. Carrier — Đơn vị vận chuyển

Canonical term tổng quát cho tổ chức trực tiếp cung cấp dịch vụ vận chuyển.

```text
Carrier
├── Ocean Carrier / Shipping Line
├── Air Carrier
├── Road Carrier
└── Rail Carrier
```

---

## 7.13. Shipping Line — Hãng tàu

Carrier vận chuyển bằng đường biển.

```text
TransportMode = Sea
→ Carrier may be presented as Shipping Line
```

Không dùng `ShippingLine` làm canonical field cho mọi phương thức vận tải.

---

## 7.14. Forwarder

Đơn vị giao nhận/forwarder nếu nghiệp vụ có phát sinh.

Nguồn hiện chưa xác nhận một field/entity Forwarder riêng, vì vậy **Forwarder chưa là core domain concept bắt buộc**.

Nếu bổ sung sau, Forwarder phải được phân biệt với Carrier.

---

## 7.15. Transport Mode — Phương thức vận chuyển

Canonical concept cho loại hình vận tải chính của Booking.

Giá trị chuẩn hóa hiện tại:

- `Sea`;
- `Air`;
- `Road`;
- `Rail`.

Giá trị `Way` trong tài liệu nguồn được chuẩn hóa thành `Road` trong domain mới.

---

## 7.16. Shipment Load Type — Hình thức sử dụng tải/container

Khái niệm khác với Equipment Type.

Giá trị ví dụ:

- `FCL`;
- `LCL`.

`LCL` không phải container/equipment type.

---

## 7.17. Transport Equipment

Thiết bị vận chuyển cụ thể gắn với Booking.

```text
Booking
└── 0..N TransportEquipment
```

Một Booking có thể có nhiều equipment/container.

---

## 7.18. Equipment Type

Loại thiết bị vận chuyển.

Nguồn hiện có các loại như:

- 20DC;
- 40DC;
- 40HC;
- 20RF;
- 40RF.

`EquipmentType` khác với `ShipmentLoadType` và `TransportMode`.

---

## 7.19. Container Number — Số container

Identifier của container cụ thể.

Là thuộc tính của Transport Equipment, không phải một field duy nhất bắt buộc trên Booking.

---

## 7.20. Seal Number — Số chì / seal

Số seal/chì của container/equipment tương ứng.

---

## 7.21. Origin Location

Canonical domain term cho điểm đầu của chặng vận tải chính.

Không khóa domain vào thuật ngữ “Port” để hỗ trợ Sea/Air/Road/Rail.

---

## 7.22. Destination Location

Canonical domain term cho điểm cuối của chặng vận tải chính.

---

## 7.23. POL — Port of Loading

Alias/logistics term dùng chủ yếu cho Sea transport, ánh xạ về Origin Location trong domain.

---

## 7.24. POD — Port of Discharge

Alias/logistics term dùng chủ yếu cho Sea transport, ánh xạ về Destination Location trong domain.

---

## 7.25. ETD — Estimated Time of Departure

Thời điểm dự kiến phương tiện vận tải chính khởi hành khỏi Origin Location.

ETD **không đồng nhất** với ngày đóng container/hàng tại Warehouse.

---

## 7.26. ETA — Estimated Time of Arrival

Thời điểm dự kiến phương tiện vận tải chính đến Destination Location.

---

## 7.27. Planned Loading Date

Ngày/thời điểm dự kiến đóng/loading hàng tại Warehouse.

Khác với ETD.

---

## 7.28. Warehouse Release Date

Ngày/thời điểm hàng thực tế rời Warehouse.

Khác với Planned Loading Date và ETD.

---

# 8. Warehouse & Batch

## 8.1. Warehouse Assignment

Quan hệ nghiệp vụ xác định Warehouse chịu trách nhiệm đóng hàng cho Booking.

Ở cấp glossary hiện tại:

```text
Booking N ─── 1 Warehouse
```

Một Booking chỉ có một Warehouse đóng hàng chính.

Nếu cùng một Order cần xuất hàng từ nhiều Warehouse, tạo nhiều Booking tương ứng.

---

## 8.2. Batch — Lô sản xuất

Lô sản xuất thực tế của một Product.

```text
Product
└── 0..N Batch
```

Batch không thuộc độc quyền một Booking.

---

## 8.3. Batch Number — Số batch

Business identifier của Batch theo nghiệp vụ sản xuất/traceability.

Unique rule chính xác được định nghĩa trong Data Dictionary vì có thể phụ thuộc Product/plant/time context.

---

## 8.4. Batch Allocation — Phân bổ batch

Quan hệ xác định một phần số lượng BookingLine được lấy từ Batch nào.

```text
BookingLine
└── 0..N BatchAllocation
    ├── Batch
    └── Quantity
```

Một BookingLine có thể sử dụng nhiều Batch; một Batch có thể được phân bổ cho nhiều BookingLine/Booking.

---

## 8.5. Loading Schedule — Lịch đóng hàng

Domain concept riêng mô tả kế hoạch và tiến độ xử lý hàng tại Warehouse trước khi Booking rời kho.

Có thể bao gồm các mốc/trạng thái nghiệp vụ như:

- hold hàng;
- dán tem;
- giao hàng/rời kho.

Workflow chính xác được định nghĩa ở tài liệu state machine.

---

## 8.6. Delivery Schedule — Lịch giao hàng

Domain concept riêng mô tả kế hoạch logistics tổng thể của Booking.

Có thể bao gồm:

- Warehouse Release Date;
- Origin Location;
- Destination Location;
- ETD;
- ETA;
- equipment/container;
- driver/logistics information;
- notes.

```text
Booking
├── LoadingSchedule
└── DeliverySchedule
```

Hai khái niệm độc lập nhưng cùng liên kết với Booking.

---

# 9. Operations

## 9.1. Work Item — Công việc vận hành

Canonical domain term cho một đơn vị công việc vận hành cần thực hiện cho Booking.

```text
Booking
└── 0..N WorkItem
    ├── Title
    ├── PIC
    ├── Deadline
    └── Status
```

`Task` là alias phổ thông/UI, không tạo entity song song nếu cùng chỉ WorkItem.

---

## 9.2. Deadline

Thời hạn cần hoàn thành Work Item.

Deadline được dùng để xác định overdue/chậm tiến độ theo Business Rules.

---

## 9.3. Ongoing

Trạng thái nguồn xác nhận cho Work Item/công việc vận hành đang được xử lý.

---

## 9.4. Completed

Trạng thái nguồn xác nhận cho Work Item/công việc vận hành đã hoàn thành.

Không mặc định dùng `Completed` của WorkItem để diễn giải trạng thái Order hoặc Booking.

---

# 10. Finance & Receivables

## 10.1. Receivable — Khoản phải thu

Nghĩa vụ tài chính Customer phải thanh toán.

```text
Receivable
├── AmountDue
├── DueDate
├── AmountPaid
├── OutstandingBalance
├── Status
└── 0..N PaymentTransaction
```

Quan hệ chính xác với Order/Booking được khóa ở Domain Model/Business Rules.

---

## 10.2. Amount Due — Số tiền phải thu

Tổng số tiền Customer phải thanh toán cho Receivable trước/bao gồm các adjustment theo rule áp dụng.

Công thức chính xác được định nghĩa trong Business Rules.

---

## 10.3. Payment Transaction — Giao dịch thanh toán

Một lần ghi nhận khoản tiền thực tế Customer thanh toán.

```text
Receivable
└── 0..N PaymentTransaction
```

Hệ thống hỗ trợ partial payment/multiple transactions.

---

## 10.4. Payment

Thuật ngữ nghiệp vụ tổng quát cho hành vi thanh toán.

Chưa tạo canonical entity `Payment` trung gian riêng nếu không có thêm ý nghĩa nghiệp vụ.

---

## 10.5. Amount Paid — Số tiền đã thanh toán

Khái niệm tổng quát:

```text
AmountPaid
= SUM(PaymentTransaction.Amount)
```

---

## 10.6. Outstanding Balance — Dư nợ còn lại

Khái niệm tổng quát:

```text
OutstandingBalance
= AmountDue - AmountPaid
```

Adjustment/refund/FX exception nếu có được định nghĩa ở Business Rules.

---

## 10.7. Financial Adjustment — Điều chỉnh tài chính

Điều chỉnh làm thay đổi nghĩa vụ tài chính liên quan đến Order/Receivable.

```text
Order
└── 0..N FinancialAdjustment
```

Các loại đã xác nhận:

- Debit Note;
- Credit Note.

---

## 10.8. Debit Note — Giấy báo nợ

Financial Adjustment làm tăng nghĩa vụ thanh toán của Customer theo rule nghiệp vụ.

---

## 10.9. Credit Note — Giấy báo có

Financial Adjustment làm giảm nghĩa vụ thanh toán của Customer theo rule nghiệp vụ.

---

## 10.10. Cost — Chi phí làm hàng

Chi phí nghiệp vụ phát sinh liên quan đến Booking/logistics/export operation.

Nguồn có các nhóm như freight, handling fee, container fee, D/O fee, terminal fee, seal fee, inland trucking, packing/storage và các chi phí khác.

Taxonomy, currency, approval và aggregation được định nghĩa ở Domain/Data/Business Rules.

---

# 11. KPI & Incentive

## 11.1. KPI Target — Mục tiêu KPI

Mục tiêu sản lượng hoặc giá trị của Customer theo kỳ.

```text
Customer
└── 0..N KpiTarget
    ├── Period
    ├── TargetQuantity
    └── TargetValue?
```

---

## 11.2. Incentive Program — Chính sách thưởng

Chính sách thưởng/ưu đãi của Customer theo kỳ và kết quả đạt được.

```text
Customer
└── 0..N IncentiveProgram
    ├── Period
    └── 1..N IncentiveTier
```

---

## 11.3. Incentive Tier — Mức thưởng

Một mức/ngưỡng trong Incentive Program.

Có thể chứa threshold và reward theo Business Rules.

```text
KPI Target != Incentive Program
```

KPI là mục tiêu; Incentive là chính sách thưởng.

---

## 11.4. Market — Thị trường

Khái niệm phân loại thị trường/quốc gia/khu vực của Customer phục vụ vận hành và reporting.

Cấu trúc master data cụ thể được xác định trong Data Dictionary.

---

# 12. Documents

## 12.1. Document Type — Loại chứng từ

Canonical classification của chứng từ.

Các loại hiện được xác nhận/chuẩn hóa gồm:

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

Danh sách được phép mở rộng trong quá trình xây dựng hệ thống.

---

## 12.2. Document Requirement — Yêu cầu chứng từ

Quy định Customer yêu cầu những Document Type nào.

```text
Customer
└── 0..N DocumentRequirement
    └── DocumentType
```

---

## 12.3. Document Template — Mẫu chứng từ

Template dùng để generate một Document Type.

Khái niệm có thể gồm:

- Document Type;
- Version;
- Effective From;
- template definition/file.

Template có thể được cập nhật/thay thế mà không thay đổi core business logic.

---

## 12.4. Generated Document — Chứng từ đã sinh

Một instance chứng từ thực tế được hệ thống tạo cho Order/Booking từ một template/version cụ thể.

```text
Order / Booking
└── 0..N GeneratedDocument
    ├── DocumentType
    ├── TemplateVersion
    ├── GeneratedAt
    └── FileReference
```

```text
DocumentRequirement
!= DocumentTemplate
!= GeneratedDocument
```

---

## 12.5. Quotation — Báo giá

Chứng từ báo giá trước hoặc trong quá trình hình thành giao dịch theo workflow nghiệp vụ.

---

## 12.6. Proforma Invoice (PI) — Hóa đơn chiếu lệ

Chứng từ thương mại được tạo/phát hành từ dữ liệu Order theo workflow nghiệp vụ.

`PI` không phải Order và không đồng nhất với Commercial Invoice.

---

## 12.7. Commercial Invoice — Hóa đơn thương mại

Chứng từ hóa đơn thương mại thực tế dùng cho lô hàng/Booking theo nghiệp vụ xuất khẩu.

`IV` trong tài liệu nguồn được chuẩn hóa là alias/abbreviation của `CommercialInvoice`, không tạo Document Type `IV` riêng.

---

## 12.8. Packing List (PL) — Phiếu đóng gói

Chứng từ mô tả chi tiết hàng thực tế của Booking như Product, quantity, packaging, weight, volume và thông tin đóng gói liên quan.

Hệ thống có thể generate PL.

---

## 12.9. Batch Information

Chứng từ thể hiện thông tin Batch/Batch Allocation của hàng trong Booking.

Nguồn dữ liệu khái niệm:

```text
Booking
└── BookingLine
    └── BatchAllocation
        └── Batch
```

Hệ thống có thể generate Batch Information.

---

## 12.10. Certificate of Origin (CO) — Chứng nhận xuất xứ

Canonical document term cho `CO`.

CO có thể là Document Requirement của Customer và có thể được lưu/tracking trong hệ thống.

Việc hệ thống generate hay chỉ lưu external document được xác định theo từng workflow/document type.

---

## 12.11. Customs Declaration (TKHQ) — Tờ khai hải quan

`TKHQ` được xác nhận là **Tờ khai hải quan**.

Canonical English term trong glossary:

```text
CustomsDeclaration
```

---

## 12.12. Health Certificate (HC)

Canonical mapping hiện tại cho `HC` trong danh sách chứng từ yêu cầu.

Nếu nghiệp vụ sau này xác nhận HC có nghĩa khác hoặc cần phân loại certificate chi tiết hơn, glossary được cập nhật tương ứng.

---

# 13. System & Supporting Concepts

## 13.1. Role

Nhóm quyền/authorization role được gán cho User theo security model.

Role không đồng nhất với business organization như Customer/Warehouse.

---

## 13.2. Permission

Quyền cho phép thực hiện action trên resource/data scope cụ thể.

Permission model chi tiết được định nghĩa trong Role & Permission Matrix.

---

## 13.3. Master Data

Dữ liệu tham chiếu dùng chung được quản trị có kiểm soát, ví dụ các danh mục location, transport mode, equipment type hoặc business taxonomy tùy thiết kế cuối cùng.

---

## 13.4. Audit Log

Bản ghi lịch sử ai đã thay đổi dữ liệu nào, khi nào và nội dung thay đổi theo mức độ được định nghĩa trong Audit Design.

---

## 13.5. Notification

Thông báo nội bộ trong hệ thống về sự kiện cần chú ý như:

- contract expiring soon;
- payment due soon;
- payment overdue;
- Work Item overdue;
- business events khác.

Email/SMS/push external channels không thuộc scope hiện tại.

---

# 14. Canonical Relationship Summary

```text
Customer
├── 1 CustomerAccount
├── 0..N Contract
├── 0..N KpiTarget
├── 0..N IncentiveProgram
├── 0..N DocumentRequirement
└── 0..N Order

Product
├── 0..N ProductPrice
└── 0..N Batch

Order
├── 1..N OrderLine
├── 0..N FinancialAdjustment
└── 0..N Booking

OrderLine
└── 0..N BookingLine

Booking
├── 1 Warehouse
├── 1..N BookingLine
├── 0..N TransportEquipment
├── 0..N WorkItem
├── LoadingSchedule
├── DeliverySchedule
├── BookingCode
├── CarrierBookingNumber?
├── ShippingOrderNumber?
├── ShippingOrderDate?
└── BillOfLadingNumber?

BookingLine
└── 0..N BatchAllocation

BatchAllocation
└── 1 Batch

Receivable
└── 0..N PaymentTransaction
```

Cardinality chính xác cho các relation chưa được Project Scope/Glossary khóa sẽ được xác định trong Domain Model.

---

# 15. Abbreviation Reference

| Abbreviation | Canonical meaning | Ghi chú |
|---|---|---|
| KDQT | Kinh Doanh Quốc Tế | Tên phòng nghiệp vụ |
| PO | Purchase Order | Phân biệt Customer PO reference với internal Order Number |
| PI | Proforma Invoice | Không phải Commercial Invoice |
| IV | Commercial Invoice | Alias/abbreviation từ tài liệu nguồn |
| PL | Packing List | Chứng từ hệ thống có thể generate |
| SO | Shipping Order | Mapping được xác nhận theo ngữ cảnh nghiệp vụ dự án |
| BL | Bill of Lading | External logistics document/reference |
| CO | Certificate of Origin | Chứng nhận xuất xứ |
| TKHQ | Tờ khai hải quan / Customs Declaration | Được nghiệp vụ xác nhận |
| HC | Health Certificate | Mapping hiện tại; có thể cập nhật nếu nghiệp vụ xác nhận khác |
| PIC | Person In Charge | Người chịu trách nhiệm chính cho WorkItem |
| ETD | Estimated Time of Departure | Không phải ngày đóng hàng |
| ETA | Estimated Time of Arrival | Thời điểm dự kiến đến |
| POL | Port of Loading | Sea/UI alias của Origin Location |
| POD | Port of Discharge | Sea/UI alias của Destination Location |
| FOC | Free of Charge | Hàng/số lượng miễn phí |
| FCL | Full Container Load | Shipment Load Type |
| LCL | Less than Container Load | Shipment Load Type, không phải Equipment Type |
| EXW | Ex Works | Incoterm theo dữ liệu nguồn |
| FOB | Free On Board | Incoterm theo dữ liệu nguồn |
| DAT | Delivered At Terminal | Giữ nguyên theo dữ liệu nguồn cho đến khi nghiệp vụ thay đổi |

---

# 16. Terms intentionally not locked yet

Các khái niệm dưới đây có thể được bổ sung hoặc chi tiết hóa trong tài liệu sau mà không cần xem là thiếu sót của glossary hiện tại:

- exact Order lifecycle states;
- exact Booking state machine/transitions;
- exact Contract lifecycle mở rộng;
- Forwarder model;
- Receivable-to-Order/Booking cardinality;
- Payment reference/bank reconciliation fields;
- detailed cost taxonomy;
- exact warehouse/loading status model;
- Product Batch unique rules;
- exact KPI calculation rules;
- detailed Incentive calculation;
- certificate/document sub-types;
- location hierarchy;
- master-data ownership;
- additional export/logistics abbreviations phát sinh từ nghiệp vụ thực tế.

Khi một thuật ngữ mới xuất hiện trong Domain Model, Workflow, Data Dictionary hoặc API mà có khả năng gây hiểu khác nhau, thuật ngữ đó phải được bổ sung trở lại tài liệu này.

---

# 17. Decision Register

| ID | Quyết định đã chốt |
|---|---|
| GLOSSARY-01 | `Order` là canonical transaction entity; tách internal `OrderNumber` và `CustomerPoNumber`; PI là document |
| GLOSSARY-02 | Booking là đơn vị vận chuyển chính; không có Shipment entity riêng |
| GLOSSARY-03 | Một OrderLine có thể được chia qua nhiều BookingLine |
| GLOSSARY-04 | Batch được phân bổ ở cấp BookingLine; hỗ trợ nhiều Batch |
| GLOSSARY-05 | SO được hiểu là Shipping Order |
| GLOSSARY-06 | Tách BookingCode, CarrierBookingNumber, ShippingOrderNumber và BillOfLadingNumber |
| GLOSSARY-07 | Carrier là canonical term; Shipping Line là Sea Carrier; Forwarder chưa bắt buộc |
| GLOSSARY-08 | Tách TransportMode, ShipmentLoadType và EquipmentType |
| GLOSSARY-09 | Domain dùng OriginLocation/DestinationLocation; POL/POD là alias theo ngữ cảnh Sea |
| GLOSSARY-10 | Một Booking có một Warehouse đóng hàng chính |
| GLOSSARY-11 | LoadingSchedule và DeliverySchedule là hai concept riêng |
| GLOSSARY-12 | ETD/ETA chuẩn logistics, tách khỏi loading/release dates |
| GLOSSARY-13 | Một Booking có thể có nhiều TransportEquipment/container |
| GLOSSARY-14 | Batch là entity traceability theo Product; BatchAllocation nối Batch với BookingLine |
| GLOSSARY-15 | Product tương ứng SKU/mã hàng; ProductionCode là business identifier chính |
| GLOSSARY-16 | ProductPrice là concept riêng có effective period/history |
| GLOSSARY-17 | ProductPrice là base price; giá thực tế snapshot trên OrderLine; chưa có CustomerProductPrice riêng |
| GLOSSARY-18 | Tách Customer, CustomerAccount và User |
| GLOSSARY-19 | Tách Warehouse, WarehouseAccount và User |
| GLOSSARY-20 | Incoterm là canonical term; Customer có default và Order snapshot riêng |
| GLOSSARY-21 | PaymentTerm là rule; DueDate là ngày cụ thể của Receivable |
| GLOSSARY-22 | Receivable có 0..N PaymentTransaction; Payment chưa cần entity trung gian riêng |
| GLOSSARY-23 | Debit Note/Credit Note là các FinancialAdjustment độc lập, có thể phát sinh nhiều lần |
| GLOSSARY-24 | Tách KpiTarget và IncentiveProgram |
| GLOSSARY-25 | Customer 1:N Contract; ExpiringSoon là derived alert |
| GLOSSARY-26 | WorkItem là canonical term; mỗi WorkItem có một primary PIC ở cấp glossary |
| GLOSSARY-27 | Tách DocumentRequirement, DocumentTemplate và GeneratedDocument |
| GLOSSARY-28 | Tách Quotation, ProformaInvoice và CommercialInvoice; IV là alias của CommercialInvoice |
| GLOSSARY-29 | PL/BatchInformation có thể generate; BL là external document/reference trong scope hiện tại |
| GLOSSARY-30 | CO = Certificate of Origin; TKHQ = Tờ khai hải quan/Customs Declaration; HC = Health Certificate; glossary được phép bổ sung thuật ngữ trong quá trình xây dựng |

---

# 18. Governance

1. Tài liệu này là nguồn canonical cho terminology của dự án.
2. Khi tài liệu downstream dùng một thuật ngữ khác cho cùng concept, ưu tiên canonical term trong glossary hoặc cập nhật glossary bằng quyết định có chủ đích.
3. Không tạo entity/class/API resource mới chỉ vì có một alias nghiệp vụ khác.
4. Không tự thay nghĩa thuật ngữ nguồn khi chưa có bằng chứng hoặc xác nhận nghiệp vụ.
5. Các thuật ngữ có thể được bổ sung khi xây dựng hệ thống; thay đổi làm ảnh hưởng scope phải quay lại `00-project-scope.md`.
6. Các thay đổi làm ảnh hưởng state transition phải được phản ánh trong workflow/state-machine document.
7. Các thay đổi làm ảnh hưởng schema/identifier/validation phải được phản ánh trong Domain Model/Data Dictionary.
