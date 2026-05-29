### 📝 List bài toán của tôi:
| # | Subsidiary (VinFast/Xanh SM...) | Lens | Mô tả ngắn bài toán |
|---|----------------------------------|------|---------------------|
| 1 |Vinmec | Pain từ người khác| Bác sĩ phàn nàn mất nhiều thời gian đọc lịch sử khám dài hàng chục trang trước khi khám follow-up cho bệnh nhân mãn tính. Bác sĩ tự đọc EMR, scan PDF xét nghiệm và note cũ trước mỗi ca khám. 10–20 phút/bệnh nhân; với 80–120 ca follow-up/ngày/khoa → mất ~20–40 giờ bác sĩ/ngày.|
| 2 | Xanh SM | Lặp lại| QA team nghe thủ công recording cuộc gọi CSKH để audit thái độ tổng đài viên và phân loại lỗi dịch vụ. QA nghe random sample cuộc gọi rồi tag lỗi bằng spreadsheet/manual form 5–10 phút/call; vài nghìn call audit/tháng → tiêu tốn ~300–800 giờ QA/tháng.|
| 3 | VinFast | Lặp lại| Procurement team tổng hợp và đối chiếu báo giá linh kiện từ nhiều supplier để phát hiện chênh lệch giá, lead time hoặc nguy cơ thiếu hàng. Nhân viên export ERP/Excel, compare quotation thủ công Chậm procurement cycle 1–3 ngày; risk mua lệch giá 2–5%; thất thoát hàng trăm triệu đến vài tỷ VND/năm.|
| 4 | Vinmec| Pain từ người khác| Lễ tân và điều dưỡng bị quá tải vì phải trả lời lặp đi lặp lại các câu hỏi giống nhau về lịch khám, chuẩn bị xét nghiệm và quy trình nhập viện. Hotline/app chat/manual response theo FAQ nội bộ 30–50% inbound question là repetitive; overload giờ cao điểm, tăng waiting time 10–20 phút|
| 5 | Vinhomes| Tốn thời gian| Bộ phận kỹ thuật phải tổng hợp thủ công consumption report điện/nước từ nhiều tòa nhà để phát hiện bất thường vận hành hoặc thất thoát tiện ích. Export dữ liệu từ nhiều hệ thống BMS/Excel rồi check anomaly thủ công. Delay phát hiện bất thường 3–7 ngày; thất thoát utility 2–8%; tốn hàng trăm giờ vận hành mỗi tháng|

---

```
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD   #1                                     │
│                                                             │
│ Bài toán (1 câu):  AI tự động audit call CSKH và phân loại  │
│ lỗi dịch vụ thay cho QA nghe recording thủ công.            │
│ Công ty thành viên: [ ] VinFast  [x] Xanh SM  [ ] Vinhomes  │
│                     [ ] Vinmec   [ ] Khác (Ghi rõ)________  │
│                                                             │
│ Ai đang đau (Actor)? QA team / Customer Service Manager     │
│                                                             │
│ Workflow thủ công hiện tại (3-5 bước):                      │
│   1. Export recording call                                  │
│      ──> 2. QA nghe từng đoạn                               │
│      ──> 3. Ghi chú thái độ/lỗi                             │
│      ──> 4. Tổng hợp report Excel                           │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? 2 và 3 (⏱ 5-10 phút/lượt) │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Speech-to-text        |
|+ sentiment analysis + auto classify issue                   │
│ + auto QA scoring ngay sau khi call kết thúc.               │
│                                                             │
│ Đo thành công bằng gì (Metric có số)? _____________________ │
│   - Giảm QA review time từ 8 phút ──> dưới 1 phút/call      │
│   - Audit được 100% cuộc gọi thay vì sampling 5–10%         │
│   - Giảm manpower QA 50–70%                                 │
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM  [ ] Agent │
└─────────────────────────────────────────────────────────────┘
```

```
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2                                       │
│                                                             │
│ Bài toán (1 câu): AI hỗ trợ bác sĩ tóm tắt lịch sử khám dài │
│ để giảm thời gian chuẩn bị trước ca follow-up.              │
│                                                             │
│ Công ty thành viên: [ ] VinFast  [ ] Xanh SM  [ ] Vinhomes  │
│                     [x] Vinmec   [ ] Khác _________________ │
│                                                             │
│ Ai đang đau (Actor)? Bác sĩ nội trú / bác sĩ khám follow-up │
│                                                             │
│ Workflow thủ công hiện tại (3-5 bước):                      │
│   1. Mở EMR                                                 │
│      ──> 2. Đọc note khám cũ                                │
│      ──> 3. Xem xét nghiệm/chẩn đoán                        │
│      ──> 4. Tự tổng hợp tình trạng bệnh nhân                │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất?                            │
│ Đọc và tổng hợp lịch sử khám (⏱ 10–20 phút/bệnh nhân)      │
│                                                             │
│ AI có thể nhảy vào hỗ trợ ở bước nào?                       │
│ Sau khi mở EMR: AI auto summarize timeline bệnh án,         │
│ highlight medication, abnormal lab, previous diagnosis.     │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
│   - Giảm prep time từ 15 phút ──> dưới 3 phút              │
│   - Tăng số ca khám/ngày thêm 15–25%                        │
│   - Giảm bỏ sót thông tin quan trọng trong follow-up        │
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM  [x] Agent│
└─────────────────────────────────────────────────────────────┘
```

```
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #4                                      │
│                                                             │
│ Bài toán (1 câu): AI assistant tự động trả lời FAQ bệnh viện│
│ để giảm tải hotline và quầy tiếp nhận tại Vinmec.           │
│                                                             │
│ Công ty thành viên: [ ] VinFast  [ ] Xanh SM  [ ] Vinhomes │
│                     [x] Vinmec   [ ] Khác _________________ │
│                                                             │
│ Ai đang đau (Actor)?                                       │
│ Lễ tân, điều dưỡng, tổng đài CSKH và bệnh nhân              │
│                                                             │
│ Workflow thủ công hiện tại (3-5 bước):                      │
│   1. Bệnh nhân gọi hotline/chat app                         │
│      ──> 2. Nhân viên đọc câu hỏi                           │
│      ──> 3. Tra FAQ/quy trình nội bộ                        │
│      ──> 4. Trả lời thủ công                                │
│      ──> 5. Escalate bác sĩ/phòng ban nếu cần               │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất?                            │
│ Tra cứu và trả lời repetitive FAQ                           │
│ (⏱ 3–7 phút/request; peak hour có queue 10–20 phút)        │
│                                                             │
│ AI có thể nhảy vào hỗ trợ ở bước nào?                       │
│ Ngay sau khi bệnh nhân gửi câu hỏi:                         │
│ - LLM chatbot trả lời FAQ                                   │
│ - RAG search trên guideline nội bộ                          │
│ - Auto classify intent                                      │
│ - Escalate human nếu confidence thấp                        │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
│   - Auto-resolve 40–60% FAQ không cần người                 │
│   - Giảm waiting time từ 15 phút ──> dưới 2 phút            │
│   - Giảm workload hotline 30–50%                            │
│   - Tăng CSAT/NPS cho patient support                       │
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM  [x] Agent│
└─────────────────────────────────────────────────────────────┘
```