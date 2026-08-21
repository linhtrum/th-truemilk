# Project Handoff — KDQT B2B Export Management System

**Ngày handoff:** 2026-08-21  
**Repository:** `linhtrum/th-truemilk`  
**Baseline branch:** `main`  
**Mục đích:** Cho phép một session mới tiếp tục phát triển bộ tài liệu dự án mà không cần phục dựng lại các quyết định từ lịch sử chat.

---

## 1. Bắt đầu session mới từ đâu

Đọc theo thứ tự:

1. `docs/00-project-scope.md` — baseline phạm vi dự án đã chốt.
2. `docs/01-domain-glossary.md` — canonical vocabulary / ubiquitous language hiện tại.
3. `docs/HANDOFF.md` — trạng thái, quy tắc làm việc, open items và next steps.

Nếu cần kiểm chứng sâu các yêu cầu gốc, dùng thêm:

- `website_modules_analysis.md`;
- PPTX yêu cầu nghiệp vụ gốc;
- workbook Excel nghiệp vụ gốc.

Hai file PPTX/XLSX là nguồn đặc biệt quan trọng cho các thuật ngữ, field và operational milestones. Nếu session mới không truy cập được attachment của session trước, cần cung cấp lại hai file này trước khi khẳng định các chi tiết chỉ tồn tại trong tài liệu gốc.

---

## 2. Trạng thái repository tại thời điểm handoff

### Đã merge

- PR #1 — `docs: baseline project scope`
  - Kết quả: `docs/00-project-scope.md`
  - Project Scope đã được baselined cho design.

- PR #2 — `docs: add domain glossary`
  - Kết quả: `docs/01-domain-glossary.md`
  - Đã independent review, sửa findings và merge bằng squash.
  - Merge commit: `e019e3cbc209d076e8da0ce79b6d0b328ad4e13d`.

### Handoff hiện tại

File này được tạo trên branch:

```text
docs/session-handoff
```

Nên merge handoff vào `main` để session sau có thể đọc trực tiếp từ repository.

---

## 3. Quy tắc làm việc đã thống nhất với nghiệp vụ

### 3.1. Không tự giả định khi requirement chưa rõ

Khi một quyết định ảnh hưởng scope/domain/workflow mà nguồn chưa đủ rõ:

1. nêu phần nguồn xác nhận;
2. nêu phần suy luận/đề xuất riêng;
3. đưa phương án A/B/C và recommendation;
4. trao đổi trong chat;
5. chỉ cập nhật tài liệu sau khi user chốt.

Không đưa một danh sách dài các `TBD` vào tài liệu rồi coi là hoàn thành.

### 3.2. Làm rõ trong chat trước, commit sau

Pattern đã áp dụng thành công:

```text
Source
  ↓
Identify ambiguity
  ↓
Discuss one decision at a time
  ↓
User approves
  ↓
Consolidate decisions
  ↓
Update Markdown on feature branch
  ↓
Independent PR review
  ↓
Fix findings
  ↓
Merge
```

### 3.3. Independent review trước merge

Các tài liệu nền tảng nên có một vòng review độc lập tập trung vào:

- mâu thuẫn với source;
- terminology không canonical;
- over-modeling;
- hidden assumptions;
- duplicated source of truth;
- missing domain concepts.

### 3.4. Glossary là living document

Không cần khóa toàn bộ thuật ngữ trước khi tiếp tục. Khi Domain Model, Business Rules, Workflow, Data Dictionary hoặc API phát hiện thuật ngữ mới/mơ hồ, cập nhật ngược `01-domain-glossary.md`.

---

## 4. Project Scope baseline quan trọng

Toàn bộ 20 business modules trong nguồn đều **In Scope**, không chia MVP ở cấp Project Scope.

Hệ thống gồm:

- KDQT Portal — 8 modules;
- Customer Portal — 6 modules;
- Warehouse Portal — 3 modules;
- Finance Portal — 3 modules;
- System Administration — capability bổ sung ngoài 20 business modules.

Các quyết định scope quan trọng:

- Backend direction: `.NET 10` / ASP.NET Core Web API.
- Hệ thống hiện vận hành standalone; business integrations bên ngoài là Future Scope.
- Authentication hỗ trợ **local account + SSO**.
- Mỗi Customer có đúng **1 Customer Account**.
- Mỗi Warehouse có đúng **1 Warehouse Account**.
- Finance Portal là **read-only**.
- `Order 1:N Booking`.
- Order có lifecycle riêng; exact states chưa khóa.
- Booking có lifecycle riêng; source-confirmed states gồm `Phát hành → Ongoing → Đang giao → Đã giao`; scope cho phép thêm `Draft`, `Hold`, `Cancelled` nhưng exact transition chưa khóa.
- Operations quản lý `WorkItem` riêng gắn với Booking.
- Hỗ trợ partial payment / multiple payment transactions.
- Excel import vào database, hỗ trợ create + update bằng business keys.
- Import dùng partial success: valid rows import, invalid rows bị skip và trả error report.
- Document generation dùng official templates khi loại chứng từ có khả năng sinh từ hệ thống.
- UI song ngữ Việt/Anh.
- Responsive web cho desktop/tablet/mobile; native mobile app Out of Scope.
- Warehouse account chỉ thấy/cập nhật dữ liệu thuộc Warehouse của mình.
- System Administration gồm user/role/permission/account/master data/settings/audit/import-export config/document template management.
- Có in-app notification; Email/SMS/push external channels là Future Scope.

Chi tiết xem `docs/00-project-scope.md`.

---

## 5. Canonical Domain Vocabulary đã chốt

Chi tiết đầy đủ xem `docs/01-domain-glossary.md`. Các điểm cần nhớ khi xây tài liệu tiếp theo:

### 5.1. Order

`Order` là canonical transaction entity.

Phân biệt:

```text
OrderNumber          // mã nội bộ hệ thống sinh
CustomerPoNumber     // PO reference do Customer cung cấp nếu có
```

Source cũ dùng `Số PO` cho mã do hệ thống tự sinh; trong hệ thống mới ánh xạ source field đó về `OrderNumber`.

PO được in theo form là một document representation của Order, không phải entity giao dịch thứ hai.

### 5.2. OrderLine / BookingLine

Một Order có nhiều OrderLine.

Một OrderLine có thể được chia qua nhiều BookingLine của nhiều Booking khác nhau.

Không khóa công thức booked/remaining quantity trong glossary; Business Rules sẽ xử lý FOC, cancellation, adjustment và các exception.

### 5.3. Booking

Booking là đơn vị shipping/fulfillment chính.

Hiện **không có Shipment entity riêng**.

Identifiers/reference phải tách:

```text
BookingCode
CarrierBookingNumber
ShippingOrderNumber
BillOfLadingNumber
```

`SO = Shipping Order` hiện chỉ là **working interpretation** vì source chỉ ghi `Số SO`/`Ngày SO`.

### 5.4. Warehouse

Một Booking có **một primary Warehouse**.

Nếu một Order cần xuất từ nhiều Warehouse, tạo nhiều Booking.

Loading Schedule và Delivery Schedule là hai concept riêng.

### 5.5. Batch

`Batch` là lô sản xuất của Product, không thuộc độc quyền một Booking.

`BatchAllocation` nối Batch với BookingLine và mang quantity phân bổ.

### 5.6. Transport

Canonical terms:

```text
Carrier
TransportMode      // Sea, Air, Road, Rail
ShipmentLoadType  // FCL, LCL
EquipmentType     // 20DC, 40DC, 40HC, 20RF, 40RF...
TransportEquipment
OriginLocation
DestinationLocation
```

`Shipping Line` chỉ là Sea Carrier.

POL/POD là Sea aliases của Origin/Destination.

Phân biệt rõ:

```text
PlannedLoadingDate
WarehouseReleaseDate
ETD
ATD
ETA
ATA
```

### 5.7. Product / Pricing

`Product` tương ứng một SKU/mã hàng cụ thể.

`ProductionCode` là business identifier chính.

`ProductPrice` là effective-dated price history theo Incoterm + Currency.

Không overwrite giá cũ khi có price update.

`OrderLine` giữ giá giao dịch đã chốt; exact snapshot/persistence rule thuộc Domain Model/Data Dictionary.

### 5.8. Customer / Warehouse vs Identity

Business entities và authentication identity tách nhau:

```text
Customer != CustomerAccount != User
Warehouse != WarehouseAccount != User
```

### 5.9. Contract / Incoterm / Payment Term

Customer có nhiều Contract theo thời gian.

`ExpiringSoon` là derived alert, không bắt buộc là lifecycle state.

`Incoterm` là canonical term; Customer có default, Order có giá trị áp dụng riêng.

`PaymentTerm` là rule; `DueDate` là ngày cụ thể của Receivable.

### 5.10. Receivable / Payment

`Receivable` là khoản phải thu.

Hỗ trợ nhiều `PaymentTransaction` cho một Receivable.

Không cần entity trung gian `Payment` nếu chưa phát sinh semantics riêng.

### 5.11. Debit Note / Credit Note

Quan trọng: workbook gốc dùng convention:

```text
Debit Note  = điều chỉnh trừ / số âm
Credit Note = điều chỉnh cộng / số dương
```

Đây là **source convention của workbook**, chưa phải kết luận về accounting perspective/issuer semantics.

Không tự áp quy ước kế toán chung để đảo nghĩa. Business Rules phải khóa cách adjustment tác động Final Amount/Receivable.

### 5.12. Documents

Canonical umbrella concept:

```text
BusinessDocument
├── SystemGenerated
└── ExternalUploaded
```

Source xác nhận rõ các loại có chức năng in/xuất theo form:

- Order: PO, PI, Quotation;
- Booking: IV/Commercial Invoice, Packing List, Batch Information.

Các loại chủ yếu external/uploaded/tracked trong scope hiện tại:

- Bill of Lading;
- Customs Declaration / TKHQ;
- Certificate of Origin / CO;
- Health Certificate / HC và subtype;
- certificates/documents bên ngoài khác.

Không mặc định hệ thống sinh mọi chứng từ xuất khẩu.

### 5.13. Market vs Region

Không gộp hai khái niệm:

- `Market` = thị trường kinh doanh/xuất khẩu của Customer;
- `Region` = vùng phân luồng nội bộ.

Workbook gốc có `REGION = Bắc/Nam`, ghi chú dùng cho Finance routing.

### 5.14. Operations

`WorkItem` = việc cần làm, có PIC/deadline/status.

`OperationalMilestone` = mốc thời gian/sự kiện để theo dõi tiến độ, khác WorkItem.

Known source milestones gồm các nhóm:

- production / packaging / label milestones;
- Cut-off Date;
- Select Container Date;
- Loading Date;
- ETD / ATD / ETA / ATA;
- Customs Clearance Date/Number;
- Invoice Issue Date;
- Packing List Issue Date;
- Sending Sample Date;
- HC Submit/Issue Date;
- CO Submit/Issue Date;
- CO Number;
- Docs Sent;
- Release Time.

Không mặc định toàn bộ là một enum. Domain Model/Workflow sẽ quyết định milestone nào là field riêng, record, WorkItem completion date hoặc Document metadata.

---

## 6. Các điểm intentionally chưa khóa

Không tự chốt các mục sau nếu chưa trao đổi thêm hoặc chưa có nguồn đủ mạnh:

- exact Order lifecycle states;
- exact Booking state machine/transitions/side effects;
- exact Contract lifecycle mở rộng;
- Forwarder model;
- Receivable-to-Order/Booking cardinality;
- payment bank reference/reconciliation model;
- accounting perspective và exact effect của Debit/Credit Note;
- detailed Cost taxonomy + approval + currency rules;
- exact Warehouse/loading status model;
- Batch unique key/traceability rules;
- exact KPI calculation;
- exact Incentive calculation;
- Health Certificate subtype taxonomy;
- document status/version/approval workflow;
- location hierarchy;
- Region taxonomy beyond current Bắc/Nam source;
- master-data ownership;
- OperationalMilestone persistence model;
- nghĩa chính thức của `SO` nếu upstream/business xác nhận khác Shipping Order;
- exact template engine/file format/mapping;
- loại chứng từ nào system-generated hay external nếu nguồn chưa xác nhận.

---

## 7. Source facts đáng chú ý từ workbook/PPTX

Các chi tiết sau đã giúp phát hiện/sửa assumptions trong glossary và nên được giữ làm reference khi thiết kế tiếp:

1. Warehouse module có `Download danh sách theo SO` và Booking kèm số SO theo từng kho.
2. Workbook Booking tách riêng `Số Booking`, `Số BL`, `Số SO`; Operations còn có `Ngày SO`.
3. Customer source có `Market_EN/VN`; workbook có thêm `REGION` Bắc/Nam cho Finance routing.
4. Source Order dùng `Số PO` do hệ thống tự gợi ý theo pattern; đây được normalize thành `OrderNumber`.
5. Workbook source cho Debit Note/Credit Note theo sign convention ngược với assumption kế toán phổ biến: Debit âm/trừ, Credit dương/cộng.
6. Document Requirement có nhiều chứng từ external và nhiều HC variants.
7. Operations workbook chứa nhiều milestone chuyên biệt, không chỉ WorkItem Ongoing/Completed.
8. Customer Portal chỉ xem dữ liệu của chính Customer.
9. Warehouse Portal chỉ xử lý quantity/logistics của Warehouse, không hiển thị price trong Loading Schedule.
10. Finance Portal là read-only theo scope đã chốt.

Khi có mâu thuẫn giữa kiến thức chung và source dự án, **không âm thầm sửa source bằng kiến thức chung**; phải nêu mâu thuẫn và làm rõ nghiệp vụ.

---

## 8. Next recommended artifact: `02-domain-model.md`

Bước tiếp theo nên là xây **Domain Model**, không đi thẳng vào database schema.

### Mục tiêu

Xác định:

- domain areas / bounded contexts hoặc module ownership ở mức phù hợp;
- aggregate/entity/value-object candidates;
- ownership của dữ liệu;
- canonical relationships;
- business identifiers;
- lifecycle ownership;
- cross-domain references;
- những gì là derived data vs persisted business facts;
- document/operational/finance model ở mức domain.

### Không nên làm ngay trong Domain Model

Chưa cần khóa:

- SQL table names;
- EF Core mappings;
- API endpoint paths;
- exact UI DTOs;
- implementation framework details;
- workflow transition matrix đầy đủ.

### Quy trình khuyến nghị

Làm rõ từng cluster thay vì viết một ERD lớn ngay:

1. **Customer & Commercial** — Customer, Contract, KPI, Incentive, commercial defaults, Region/Market.
2. **Product & Pricing** — Product, ProductPrice, Batch.
3. **Order** — Order, OrderLine, commercial snapshot, adjustments.
4. **Booking & Logistics** — Booking, BookingLine, Carrier, equipment, Warehouse, schedules.
5. **Operations** — WorkItem, PIC, OperationalMilestone.
6. **Documents** — DocumentRequirement, DocumentTemplate, BusinessDocument, source/generated/uploaded model.
7. **Finance** — Cost, Receivable, PaymentTransaction, FinancialAdjustment.
8. **Identity/Admin references** — User/account relationship only ở mức domain boundary; security details sang tài liệu riêng.

Sau mỗi cluster:

- nêu source-supported facts;
- nêu assumptions/gaps;
- đưa recommendation;
- user approve;
- sau khi đủ cluster mới viết `02-domain-model.md` và mở PR.

---

## 9. Suggested documentation sequence sau Domain Model

Đây là **recommended sequence**, chưa phải baseline bắt buộc:

```text
00-project-scope.md              ✅ merged
01-domain-glossary.md            ✅ merged
02-domain-model.md               ← next
03-business-rules.md
04-workflow-state-machines.md
05-role-permission-matrix.md
06-data-dictionary.md
07-api-contract-guidelines.md
08-system-architecture.md
09-import-export-documents.md
10-audit-notification-design.md
```

Tên/số thứ tự có thể điều chỉnh khi repository phát triển; không coi sequence trên là requirement nguồn.

---

## 10. Prompt gợi ý để mở session mới

Có thể bắt đầu session mới bằng prompt:

> `@GitHub tiếp tục dự án linhtrum/th-truemilk. Đọc docs/00-project-scope.md, docs/01-domain-glossary.md và docs/HANDOFF.md. Bắt đầu làm rõ 02-domain-model.md theo quy trình trong handoff: không tự giả định requirement chưa rõ, trao đổi từng cluster trong chat trước khi tạo tài liệu/PR.`

Nếu PPTX/XLSX gốc chưa khả dụng trong session mới và cần kiểm chứng field/source-level detail, upload lại hai file nguồn trước khi chốt các quyết định phụ thuộc vào chúng.

---

## 11. Definition of done cho bước tiếp theo

`02-domain-model.md` chỉ nên merge khi:

- consistent với `00-project-scope.md`;
- canonical terms consistent với `01-domain-glossary.md`;
- không biến Domain Model thành SQL schema;
- assumptions được đánh dấu và làm rõ;
- các quan hệ quan trọng được business-approved;
- unresolved workflow/business rules được chuyển đúng tài liệu downstream;
- có independent review trước merge;
- mọi review blocker đã được xử lý.

---

## 12. Nguyên tắc cuối cùng

Ưu tiên **nhất quán nghiệp vụ và traceability về source** hơn việc cố hoàn thiện mọi chi tiết sớm.

Nếu phát hiện source mới làm thay đổi một quyết định đã merge:

1. xác định tài liệu baseline bị ảnh hưởng;
2. làm rõ với nghiệp vụ;
3. cập nhật glossary/scope nếu cần;
4. cập nhật downstream docs tương ứng;
5. tránh để hai tài liệu giữ hai định nghĩa khác nhau cho cùng concept.
