## 🏛️ Bối cảnh: Tôi là ai?
Tôi là **Trung**, AI Engineer tại **Vin Smart Future**. Nhóm chúng tôi được giao nhiệm vụ phối hợp với các khối Vận hành, Hậu mãi và Chuỗi cung ứng của **VinFast** để tìm kiếm các cơ hội tối ưu hóa bằng trí tuệ nhân tạo.

Thông qua khảo sát thực địa tại các Xưởng dịch vụ, Tổng đài CSKH và Khối Back-office, tôi nhận thấy các nút thắt chủ yếu nằm ở quy trình xử lý khối lượng lớn hồ sơ thủ công, tra cứu tài liệu kỹ thuật phức tạp và rò rỉ hiệu suất trong chuỗi cung ứng linh kiện.

### 📝 List bài toán của tôi:
| # | Subsidiary (VinFast/Xanh SM...) | Lens | Mô tả ngắn bài toán |
|---|----------------------------------|------|---------------------|
| 1 | **VinFast** |Tốn thời gian |Chuyên viên Hậu mãi duyệt thủ công hàng ngàn hồ sơ yêu cầu bồi thường bảo hành từ các đại lý (mất 15 - 20 phút/hồ sơ).    |
| 2 | **VinFast** |AI-upgrade | Tổng đài viên tra cứu thủ công tài liệu kỹ thuật (Manuals) để hướng dẫn khách hàng xử lý lỗi hiển thị trên xe (mất 5 - 8 phút/cuộc gọi). |
| 3 | **VinFast** |Stakeholder Pain | Khách hàng phải để xe lưu xưởng nhiều ngày do thiếu phụ tùng thay thế (việc dự báo đặt hàng tồn kho hiện tại làm thủ công, dựa vào cảm tính). |
| 4 | **Vinhomes** |AI-upgrade | Phân loại và điều phối (route) thủ công các khiếu nại của cư dân trên App Vinhomes Resident cho các phòng ban (CSKH hiện tại phản hồi rập khuôn, mất trung bình 12 - 24 tiếng để giải quyết triệt để). |
| 5 | **VinFast** |Tốn thời gian | Kỹ sư mất hàng giờ rà soát thủ công hàng triệu điểm dữ liệu biểu đồ xả sạc để chấm điểm và phân loại tình trạng (SOH) của các cell pin cũ thu hồi. |


# 🃏 Phase 2 — QUICK-ASSESS: 3 Quick Problem Cards (Cá nhân)

Chọn top 3 từ danh sách SCAN: **#1 (Vinfast xử lý bồi thường), #2 (Vinfast lỗi hiển thị), #3 (Vinfast chậm phụ tùng).**


```text
┌────────────────────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1                                                      │
│                                                                            │
│ Bài toán: Chuyên viên Hậu mãi duyệt thủ công hàng ngàn hồ sơ yêu cầu bồi   │
│ thường bảo hành từ các đại lý.                                             │
│ Công ty thành viên: [x] VinFast                                            │
│                                                                            │
│ Ai đang đau? Chuyên viên duyệt bảo hành (quá tải), Đại lý (chờ đợi lâu)    │
│                                                                            │
│ Workflow thủ công hiện tại (5 bước):                                       │
│   1. Đại lý gửi hồ sơ bồi thường (ảnh lỗi, text) lên hệ thống              │
│   → 2. Chuyên viên mở hồ sơ để rà soát thông tin                           │
│   → 3. Tra cứu thủ công chính sách bảo hành dài hàng trăm trang            │
│   → 4. Dùng mắt kiểm tra ảnh xem có đúng lỗi từ nhà sản xuất không         │
│   → 5. Ra quyết định Duyệt hoặc Từ chối trên hệ thống                      │
│                                                                            │
│ Bước nào tốn nhất? Bước 3-4 (⏱ 15-20 phút/hồ sơ)                           │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 3-4                             │
│ (Quét ảnh/text -> Đối chiếu rule chính sách -> Đề xuất Duyệt/Từ chối)      │
│                                                                            │
│ Đo thành công bằng gì (Metric có số)?                                      │
│ Giảm thời gian xử lý từ 15 phút ──> dưới 2 phút/hồ sơ.                     │
│                                                                            │
│ Quick Architecture: [x] Rule-based + LLM Vision (Trích xuất & Phân loại)   │
└────────────────────────────────────────────────────────────────────────────┘

```
---

```text
┌────────────────────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2                                                      │
│                                                                            │
│ Bài toán: Tổng đài viên tra cứu thủ công tài liệu kỹ thuật (Manuals) để    │
│ hướng dẫn khách hàng xử lý lỗi hiển thị trên xe.                           │
│ Công ty thành viên: [x] VinFast                                            │
│                                                                            │
│ Ai đang đau? Tổng đài viên (áp lực tìm kiếm), Khách hàng (chờ máy lâu)     │
│                                                                            │
│ Workflow thủ công hiện tại (5 bước):                                       │
│   1. Khách hàng gọi báo lỗi đèn cảnh báo/màn hình trên xe                  │
│   → 2. Tổng đài viên tiếp nhận, nghe và gõ log ghi nhận lỗi                │
│   → 3. Hold máy khách, lục tìm file tài liệu/manuals kỹ thuật              │
│   → 4. Đọc, hiểu và chuyển đổi ngôn ngữ kỹ thuật sang kịch bản dễ hiểu     │
│   → 5. Hướng dẫn khách hàng thao tác xử lý hoặc gọi cứu hộ                 │
│                                                                            │
│ Bước nào tốn nhất? Bước 3-4 (⏱ 5-8 phút/cuộc gọi)                          │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 3-4                             │
│ (Đọc log NV gõ -> Rút trích tài liệu RAG -> Gợi ý kịch bản trả lời ngay)   │
│                                                                            │
│ Đo thành công bằng gì (Metric có số)?                                      │
│ Giảm thời gian xử lý cuộc gọi (AHT) từ 8 phút ──> dưới 3 phút/cuộc gọi.    │
│                                                                            │
│ Quick Architecture: [x] LLM (Tích hợp RAG để truy xuất Knowledge Base)     │
└────────────────────────────────────────────────────────────────────────────┘
```
---

```text
┌────────────────────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #3                                                      │
│                                                                            │
│ Bài toán: Khách hàng phải để xe lưu xưởng nhiều ngày do thiếu phụ tùng     │
│ thay thế vì việc dự báo đặt hàng tồn kho hiện tại làm thủ công, cảm tính.  │
│ Công ty thành viên: [x] VinFast                                            │
│                                                                            │
│ Ai đang đau? Khách hàng (bức xúc vì không có xe đi), Quản lý xưởng (áp lực)│
│                                                                            │
│ Workflow thủ công hiện tại (5 bước):                                       │
│   1. Quản lý xưởng xuất báo cáo tiêu hao phụ tùng của tháng trước          │
│   → 2. Ngồi ước lượng nhu cầu đặt hàng tháng tới dựa trên kinh nghiệm      │
│   → 3. Lập danh sách và chốt đơn đặt hàng (PO) gửi về nhà máy              │
│   → 4. Phụ tùng được giao xuống xưởng theo đơn                             │
│   → 5. Khách vào xưởng gặp lỗi đặc thù -> Thiếu linh kiện -> Lưu xe chờ    │
│                                                                            │
│ Bước nào tốn/lỗi nhất? Bước 1-2 (⏱ Lỗi dự báo -> Hậu quả: Lưu xe 3-5 ngày) │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 1-2                             │
│ (Phân tích dữ liệu lịch sử/lượng xe khu vực -> Tự động đề xuất list nhập)  │
│                                                                            │
│ Đo thành công bằng gì (Metric có số)?                                      │
│ Tăng tỷ lệ sẵn sàng phụ tùng (Fill rate) ──> trên 95%                      │
│ Giảm số ngày xe nằm xưởng chờ linh kiện từ 3-5 ngày ──> dưới 24h.          │
│                                                                            │
│ Quick Architecture: [x] Agent (Kết nối hệ thống ERP chạy Machine Learning) │
└────────────────────────────────────────────────────────────────────────────┘
```
---