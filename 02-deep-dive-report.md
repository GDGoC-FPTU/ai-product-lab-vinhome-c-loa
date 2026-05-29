# 🏗️ Phase 3 — DEEP-DIVE: VinFast Warranty AI

---

## 3.1. Current-State Workflow Mapping

## 3.2. Problem Statement (6-field)

| Field | Nội dung chi tiết |
|---|---|
| **1. Actor / Operator** | Kỹ thuật viên (KTV) tại 400+ xưởng dịch vụ VinFast toàn quốc; Chuyên viên phê duyệt bảo hành tại Trung tâm HQ |
| **2. Current Workflow** | KTV kết nối OBD đọc DTC codes thủ công → điền form mô tả triệu chứng (Word/Excel/portal nội bộ) → gửi HQ qua email hoặc portal → chuyên viên HQ xếp hàng xét duyệt → phê duyệt hoặc yêu cầu bổ sung → KTV đặt phụ tùng → sửa chữa. 4 bước, hoàn toàn thủ công, mất 4–7 ngày/lượt. |
| **3. Bottleneck** | Bước 2 + 3: KTV mô tả triệu chứng tự do bằng ngôn ngữ không chuẩn hóa → HQ mất thêm thời gian giải mã; queue xét duyệt HQ tập trung không có SLA cứng → 2–4 ngày chờ với case rõ ràng lẫn case phức tạp |
| **4. Business Impact** | Bồi thường phát sinh từ ngày thứ 4 (theo chính sách aftersales VinFast quốc tế); ước tính 10–15% case vượt mốc 4 ngày. Ngoài chi phí trực tiếp: CSAT giảm, ảnh hưởng uy tín thương hiệu EV tại thị trường quốc tế nơi VinFast đang mở rộng |
| **5. Success Metric** | 1. "70% case bảo hành tiêu chuẩn được AI phân loại chính xác, KTV confirm trong <15 phút, tổng thời gian phê duyệt <4 giờ" 2. "Độ chính xác phân loại AI ≥ 90% so với quyết định chuyên viên HQ (đo trên 500 case test)" 3. "Giảm tỷ lệ form bị HQ trả về do mô tả thiếu từ ~30% xuống <5%" |
| **6. Operational Boundary** | ✅ AI được phép: đọc DTC codes từ OBD API, tra lịch sử sửa chữa xe, đối chiếu chính sách bảo hành, đề xuất phân loại (Trong BH / Ngoài BH / Cần khảo sát thêm) kèm confidence score, tạo draft form điền sẵn cho KTV. ❌ TUYỆT ĐỐI không: tự phê duyệt hoặc gửi form mà không có KTV xác nhận; truy cập thông tin tài chính/thanh toán; ra quyết định với case tai nạn, nghi ngờ gian lận, hoặc triệu chứng chưa có trong training data. ⚠️ Điểm HITL bắt buộc: KTV phải xác nhận mọi đề xuất AI trước khi gửi HQ; case confidence <80% phải escalate lên chuyên viên HQ theo luồng cũ |

---

## 3.3. Future-State Flow & AI Fit

**AI-Fit Matrix:** [x] LLM Feature
> Lý do chọn LLM Feature thay vì Agent: Tác vụ cốt lõi là structured reasoning trên dữ liệu
> có sẵn (DTC codes + lịch sử xe + policy bảo hành) với output xác định (phân loại + draft
> form). Không cần Agent vì không có multi-step tool call tự chủ — KTV luôn là người xác
> nhận trước mọi action. Agent sẽ phù hợp hơn ở phase sau khi hệ thống đã có đủ dữ liệu
> để nâng mức tự động hóa.

```text
[KTV tại xưởng]         [AI Tool]               [KTV tại xưởng]         [HQ / Hệ thống]
       │                     │                         │                       │
       ▼                     ▼                         ▼                       ▼
┌──────────────┐      ┌──────────────┐         ┌──────────────┐        ┌──────────────┐
│ Bước 1       │      │ Bước 2       │         │ Bước 3       │        │ Bước 4a      │
│ Kết nối OBD  │      │ 🔵 AI STEP   │         │ 🟢 HITL      │        │ ✅ Case rõ   │
│ → DTC codes  │ ──→  │ Phân tích    │ ──→     │ KTV review   │ ──→    │ (conf ≥80%)  │
│ tự động gửi  │      │ DTC + lịch   │         │ đề xuất AI   │        │ Auto-route   │
│ vào AI Tool  │      │ sử xe +      │         │ Chỉnh nếu    │        │ <4 giờ       │
│              │      │ policy BH    │         │ cần → Xác    │        │              │
│ ⏱ 15 phút    │      │ → Đề xuất    │         │ nhận → Gửi   │        ├──────────────┤
│ (giảm từ     │      │ phân loại +  │         │              │        │ Bước 4b      │
│ 30–60 phút)  │      │ confidence   │         │ ⏱ 10–15 phút │        │ ⚠️ Case phức │
│              │      │ + draft form │         │ (giảm từ     │        │ (conf <80%)  │
│              │      │              │         │ 30–45 phút)  │        │ ↩️ Fallback  │
│              │      │ ⏱ <60 giây   │         │              │        │ → Queue HQ   │
└──────────────┘      └──────────────┘         └──────────────┘        │ như cũ       │
                                                                        ├──────────────┤
                                                                        │ Bước 4c      │
                                                                        │ ❌ Tai nạn / │
                                                                        │ Gian lận     │
                                                                        │ ↩️ Fallback  │
                                                                        │ → Team điều  │
                                                                        │ tra, bypass  │
                                                                        │ AI hoàn toàn │
                                                                        └──────────────┘
```

| Ký hiệu | Mô tả |
|---------|-------|
| 🔵 AI Step | LLM đọc DTC codes qua OBD API + query lịch sử xe + đối chiếu policy → phân loại bảo hành + confidence score + draft form điền sẵn |
| 🟢 Human Step (HITL) | KTV review đề xuất, chỉnh sửa nếu cần, xác nhận trước khi gửi — không thể bỏ qua bước này |
| ↩️ Fallback #1 | Confidence < 80% → giữ nguyên luồng cũ, escalate HQ queue |
| ↩️ Fallback #2 | OBD không kết nối được / API lỗi → KTV điền form thủ công như hiện tại |
| ↩️ Fallback #3 | Case tai nạn / gian lận → bypass AI hoàn toàn, chuyển team chuyên trách |

---

# 🏁 Phase 5 — EVALUATE (Nhóm, 20 min)

### AI Readiness Checklist:

1. [x] **Chúng tôi có sẵn dữ liệu mẫu/logs sạch để test?**
   - Có. 400+ xưởng dịch vụ đang vận hành tạo ra hàng nghìn case/tháng với đầy đủ DTC
     codes, lịch sử sửa chữa, và kết quả phê duyệt HQ — đây là ground truth label sẵn có.
     Policy bảo hành VinFast đã được document hóa → đủ điều kiện xây dựng test set 500+
     case ngay mà không cần thu thập thêm.

2. [x] **Rủi ro khi AI sai có nằm trong tầm kiểm soát (qua HITL hoặc Fallback)?**
   - Có. 3 lớp bảo vệ: (1) HITL bắt buộc — KTV xác nhận trước khi gửi HQ; (2) Confidence
     threshold — case <80% tự động về queue HQ như cũ; (3) Hard bypass — case tai nạn/
     gian lận không bao giờ đi qua AI. Rủi ro tệ nhất: AI đề xuất sai nhưng KTV bắt được
     ở bước review → không có tác hại thực tế.

3. [x] **Stakeholders sẵn sàng thay đổi quy trình làm việc cũ?**
   - KTV: hưởng lợi trực tiếp (giảm 30–45 phút điền form/case), kháng cự thấp.
   - Chuyên viên HQ: giảm queue lặp lại, tập trung vào case phức tạp — cần change
     management nhẹ để tránh lo ngại bị thay thế.
   - Ban lãnh đạo VinFast: ROI rõ ràng qua giảm bồi thường và tăng CSAT.

---

### Quyết định cuối cùng của Ban Giám Đốc Vin Smart Future:

[x] **GO (Bắt đầu xây dựng Prototype):** Bắt đầu phát triển với scope hẹp.

[ ] **NOT YET (Cần tích lũy thêm dữ liệu/xác lập baseline):** Trì hoãn để chuẩn bị thêm.

[ ] **NO-GO (Không khả thi / Rule-based tốt hơn):** Hủy bỏ dự án AI này.

---

**Justification (Lý giải quyết định dựa trên bằng chứng kỹ thuật và chi phí):**

> **Quyết định: GO — Prototype tại 10 xưởng pilot (Hà Nội + TP.HCM), 4 tuần.**
>
> **Bằng chứng kỹ thuật:**
> Bài toán có cấu trúc lý tưởng cho LLM: input xác định (DTC codes chuẩn hóa theo chuẩn
> OBD-II + lịch sử xe dạng structured data), output có nhãn rõ ràng (Trong BH / Ngoài BH /
> Cần khảo sát), và ground truth label đã có sẵn từ hàng nghìn case lịch sử HQ đã duyệt.
> Không cần fine-tune model lớn — RAG trên policy bảo hành + few-shot prompting trên 200–
> 300 case mẫu là đủ để đạt ngưỡng 90% accuracy trong giai đoạn pilot.
>
> **Bằng chứng chi phí:**
> - Hiện tại: ước tính 10–15% trong tổng số case/tháng toàn quốc vượt mốc 4 ngày →
>   phát sinh bồi thường. Chi phí xây prototype (3–4 tuần kỹ sư) thấp hơn đáng kể so
>   với ROI kỳ vọng chỉ riêng khoản giảm bồi thường.
> - Lợi ích phụ đo được: giảm 30–45 phút KTV/case × số case/tháng = hàng trăm giờ
>   nhân công được giải phóng cho công việc chuyên môn cao hơn.
>
> **Rủi ro còn lại cần theo dõi trước go-live:**
> 1. Chất lượng OBD API: cần xác nhận tỷ lệ case OBD kết nối thất bại tại xưởng
>    (nếu >20% → cần fallback UI cho KTV nhập DTC thủ công vào tool).
> 2. Coverage của policy document: nếu policy bảo hành chưa được document hóa đầy
>    đủ dạng machine-readable → cần sprint riêng để chuẩn hóa trước khi build RAG.
> 3. Kháng cự từ chuyên viên HQ: cần framing đúng — AI không thay thế HQ mà giúp
>    HQ tập trung vào 30% case phức tạp thay vì 100% case như hiện tại.
>
> **Scope Prototype đề xuất:**
> 10 xưởng pilot tại Hà Nội + TP.HCM, 4 tuần, đo trên 300 case thực tế.
> Pass/Fail gate: accuracy ≥ 90% và thời gian phê duyệt trung bình <4 giờ với case
> confidence ≥ 80%. Nếu pass → mở rộng toàn quốc theo rolling deployment.
