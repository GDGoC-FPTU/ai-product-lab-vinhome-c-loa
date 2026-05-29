# 🏗️ Phase 3 — DEEP-DIVE (Nhóm)

## 3.1. Current-State Workflow


## 3.2. Problem Statement (6-field) — Vin Smart Future Standard

| Field | Nội dung |
|---|---|
| **1. Actor / Operator** | Điều phối viên (Dispatcher) thuộc Trung tâm Điều vận Xanh SM. |
| **2. Current Workflow** | Khi tài xế báo hết pin, điều phối viên tra cứu vị trí định vị trên bản đồ nội bộ, mở Dashboard trạm sạc VinFast để tìm trụ sạc trống gần nhất, viết tin nhắn chỉ dẫn/định vị gửi qua App tài xế, và gọi cứu hộ nếu pin dưới 5%. 5 bước, hoàn toàn thủ công, mất 15 phút/lượt. |
| **3. Bottleneck** | Bước 3 & 4 (mất 10 phút): Tra cứu thủ công trụ sạc trống phù hợp với dòng xe (VF5/VFe34/VF8) và soạn thảo tin nhắn hướng dẫn đường đi chi tiết bằng Tiếng Việt thân thiện. |
| **4. Business Impact** | Mỗi ngày có ~80 sự cố pin thực địa tại Hà Nội. Gây lãng phí 20 giờ làm việc/ngày của team điều vận. Tăng thời gian chờ đợi của tài xế, dẫn đến rò rỉ doanh thu ~15% do xe không thể đón khách và tài xế bị stress. |
| **5. Success Metric** | 1. Giảm tổng thời gian xử lý sự cố từ 15 phút xuống dưới 3 phút (Efficiency). 2. Tỉ lệ hướng dẫn đúng địa điểm và đúng loại trụ sạc phù hợp đạt 98% (Quality). |
| **6. Operational Boundary** | AI được phép truy xuất API định vị xe, API trạm sạc VinFast trống, tự động soạn thảo tin nhắn hướng dẫn dạng nháp (draft). **CẤM:** AI không được tự động gửi tin đi mà không có điều phối viên phê duyệt (Bắt buộc HITL); không được đề xuất trạm sạc không phù hợp với loại cổng sạc của xe. |

---

## 3.3. Future-State Flow & AI Fit

- **AI Fit:** Chọn **LLM Feature** (không cần Agent tự trị vì quy trình có cấu trúc cố định, rủi ro khi điều phối sai trạm sạc có thể khiến xe cạn kiệt pin giữa đường và gây tắc nghẽn giao thông).
- **Quy trình tương lai (Future-State):**

```text
┌──────────────┐     ┌──────────────┐     ┌──────────────┐     ┌──────────────┐
│ Bước 1       │     │ Bước 2       │     │ Bước 3       │     │ Bước 4       │
│ Nhận cuộc    │     │ 🔵 Auto-pull │     │ 🔵 AI draft  │     │ 🟢 Dispatch  │
│ gọi sự cố    │ ──→ │ vị trí &     │ ──→ │ SMS chỉ dẫn  │ ──→ │ click duyệt  │
│              │     │ trạm sạc trống│    │ & chỉ đường  │     │ & gửi tài xế │
└──────────────┘     └──────────────┘     └──────────────┘     └──────────────┘
                                                                      │
                                                                      ▼
                                                               ↩️ Fallback:
                                                               Nếu AI draft lỗi,
                                                               Dispatcher tự viết
                                                               tay lại như cũ.
```

---

# 🏁 Phase 5 — EVALUATE (Nhóm, 20 min)

### AI Readiness Checklist:

1. [x] **Chúng tôi có sẵn dữ liệu mẫu/logs sạch để test?**
   - Có. Trung tâm Điều vận Xanh SM có logs sự cố pin theo thời gian thực (biển số, toạ độ GPS, timestamp, loại xe). API trạm sạc VinFast đã được dùng nội bộ ở Dashboard hiện tại → dữ liệu đầu vào đủ điều kiện để xây dựng test set ngay.

2. [x] **Rủi ro khi AI sai có nằm trong tầm kiểm soát (qua HITL hoặc Fallback)?**
   - Có. Mọi draft SMS đều phải qua Dispatcher phê duyệt trước khi gửi (HITL bắt buộc). Fallback rõ ràng: nếu AI không tìm được trạm phù hợp hoặc draft lỗi, Dispatcher xử lý thủ công như quy trình cũ — không có single point of failure.

3. [x] **Stakeholders sẵn sàng thay đổi quy trình làm việc cũ?**
   - Có. Dispatcher là người hưởng lợi trực tiếp (giảm tải 10 phút/lượt × 80 lượt/ngày = 13 giờ/ngày). Workflow mới không xoá bỏ vai trò của họ mà chỉ loại bỏ tác vụ tra cứu lặp lại — mức kháng cự thay đổi dự kiến thấp.

---

### Quyết định cuối cùng của Ban Giám Đốc Vin Smart Future:

[x] **GO (Bắt đầu xây dựng Prototype):** Bắt đầu phát triển với scope hẹp.

[ ] **NOT YET (Cần tích lũy thêm dữ liệu/xác lập baseline):** Trì hoãn để chuẩn bị thêm.

[ ] **NO-GO (Không khả thi / Rule-based tốt hơn):** Hủy bỏ dự án AI này.

---

**Justification (Lý giải quyết định dựa trên bằng chứng kỹ thuật và chi phí):**

> **Quyết định: GO — Prototype với scope hẹp tại 1 thành phố (Hà Nội).**
>
> **Bằng chứng kỹ thuật:**
> Bài toán có cấu trúc rõ ràng (input/output xác định), dữ liệu đầu vào sạch và có sẵn (GPS logs + API trạm sạc), và tác vụ sinh ngôn ngữ tự nhiên (soạn SMS hướng dẫn tiếng Việt) là thế mạnh cốt lõi của LLM. Không cần model lớn — một LLM nhỏ fine-tuned hoặc prompt-engineered trên ~500 mẫu SMS thực tế là đủ để đạt ngưỡng 98% accuracy.
>
> **Bằng chứng chi phí:**
> Hiện tại: 80 sự cố/ngày × 10 phút bottleneck × 365 ngày = ~4.867 giờ nhân công/năm bị lãng phí ở Hà Nội đơn lẻ. Chi phí xây prototype (2–3 tuần kỹ sư) thấp hơn nhiều so với ROI giải phóng năng lực điều vận.
>
> **Rủi ro còn lại cần theo dõi:**
> - Độ trễ API trạm sạc VinFast (nếu API chậm >2s → trải nghiệm Dispatcher không cải thiện).
> - Tỷ lệ trạm sạc hiển thị "trống" nhưng thực tế đang bận (data staleness) → cần xác nhận SLA data freshness với team VinFast trước khi go-live.
>
> **Scope Prototype đề xuất:**
> Hà Nội, 2 tuần, đo trên 200 sự cố thực tế. Pass/Fail: thời gian xử lý trung bình < 3 phút và accuracy ≥ 95%.
