- Đỗ Tuấn Đạt: 2A202600818 
# 🔍 Phase 1 — SCAN (Cá nhân, 20 min)
### 📝 List bài toán của tôi:
| # | Subsidiary | Lens | Mô tả ngắn bài toán |
|---|------------|------|---------------------|
| 1 | Xanh SM | Stakeholder Pain | Tài xế & khách hàng phàn nàn ETA đón xe sai lệch nghiêm trọng (hiển thị "1 phút" trong 12 phút), do hệ thống không tích hợp traffic thực, SoC pin, hay mật độ đơn theo giờ → tỷ lệ hủy đơn 12–18%, ước tính 20–30 tỷ VND/tháng doanh thu bị mất |
| 2 | Xanh SM | Lặp lại | Tài xế tự quyết định thời điểm và trạm sạc thủ công, gây tắc nghẽn giờ cao điểm (11h–13h, 21h–23h), chờ 20–40 phút/lần sạc → mất 2–3 chuyến/ngày/tài xế |
| 3 | Vinhomes | Tốn thời gian | Ban quản lý xử lý 50–200 yêu cầu bảo trì/ngày/tòa thủ công qua nhiều kênh (hotline, app, Zalo), soạn phản hồi 1-sao từng cái một → 30–40% ticket trễ SLA, thời gian xử lý TB 4–8 giờ |
| 4 | VinFast | AI-upgrade | Kỹ thuật viên ghi nhận triệu chứng và gửi request bảo hành thủ công lên trung tâm — chờ phê duyệt 2–4 ngày → phát sinh bồi thường $160/ngày từ ngày thứ 4, áp lực trên 400 xưởng toàn quốc |
| 5 | Xanh SM | Lặp lại | Quy trình onboarding tài xế (xác minh bằng lái, lý lịch tư pháp, ký hợp đồng thuê xe) làm thủ công tại trung tâm, mất 3–5 ngày/hồ sơ → tài xế bỏ việc cao do thiếu minh bạch điều khoản phạt xe hỏng |

---

# 🃏 Phase 2 — QUICK-ASSESS (Cá nhân, 30 min)
```
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1                                       │
│                                                             │
│ Bài toán (1 câu): Hệ thống ETA đón xe Xanh SM sai lệch      │
│ nghiêm trọng khiến khách hủy đơn và chuyển sang đối thủ     │
│ Công ty thành viên: [ ] VinFast  [x] Xanh SM  [ ] Vinhomes  │
│                     [ ] Vinmec   [ ] Khác                   │
│                                                             │
│ Ai đang đau (Actor)? Khách hàng (chờ sai giờ, hủy đơn)      │
│                      Tài xế (mất chuyến do khách cancel)    │
│                                                             │
│ Workflow thủ công hiện tại (3-5 bước):                      │
│   1. Khách đặt xe ──> 2. Hệ thống gán tài xế gần nhất       │
│   ──> 3. ETA tính đơn giản theo khoảng cách thẳng           │
│   ──> 4. Tài xế di chuyển thực tế (kẹt xe, đang sạc pin)    │
│   ──> 5. Khách thấy ETA không đổi → hủy đơn                 │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 3 (sai lệch           │
│ trung bình 10–25 phút/lượt so với thực tế)                  │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 3 → Real-time    │
│ ETA model tích hợp GPS + lịch sử traffic + SoC pin          │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
│   "Giảm sai số ETA từ >10 phút xuống dưới 90 giây"          │
│   "Giảm tỷ lệ hủy đơn từ 12–18% xuống dưới 5%"              │
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [ ] LLM  [x] Agent │
└─────────────────────────────────────────────────────────────┘
```

```
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2                                       │
│                                                             │
│ Bài toán (1 câu): Ban quản lý Vinhomes xử lý hàng trăm      │
│ ticket bảo trì và phản hồi 1-sao thủ công mỗi ngày,         │
│ gây trễ SLA và giảm hài lòng cư dân                         │
│ Công ty thành viên: [ ] VinFast  [ ] Xanh SM  [x] Vinhomes  │
│                     [ ] Vinmec   [ ] Khác                   │
│                                                             │
│ Ai đang đau (Actor)? Nhân viên CSKH (xử lý quá tải)         │
│                      Cư dân (chờ phản hồi 4–8 giờ)          │
│                                                             │
│ Workflow thủ công hiện tại (3-5 bước):                      │
│   1. Cư dân gửi yêu cầu (app/hotline/Zalo)                  │
│   ──> 2. CSKH đọc, phân loại thủ công (điện, nước, thang…)  │
│   ──> 3. CSKH soạn phản hồi xác nhận gửi cư dân             │
│   ──> 4. Forward thủ công cho đội kỹ thuật đúng chuyên môn  │
│   ──> 5. Kỹ thuật viên xử lý, CSKH cập nhật trạng thái      │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2 + 3 (8–15           │
│ phút/ticket, dễ forward sai đội kỹ thuật)                   │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2 → LLM phân     │
│ loại tự động; Bước 3 → LLM soạn draft phản hồi cho duyệt    │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
│   "Giảm thời gian xử lý ticket từ 4–8 giờ xuống dưới 1 giờ" │
│   "Giảm tỷ lệ forward sai đội kỹ thuật từ ~30% xuống <5%"   │
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [X] LLM  [] Agent  │
└─────────────────────────────────────────────────────────────┘
```

```
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #3                                       │
│                                                             │
│ Bài toán (1 câu): Quy trình phê duyệt bảo hành VinFast      │
│ mất 2–4 ngày thủ công, gây tắc nghẽn queue tại HQ và        │
│ làm giảm lòng tin khách hàng vào thương hiệu EV mới         │
│ Công ty thành viên: [x] VinFast  [ ] Xanh SM  [ ] Vinhomes  │
│                     [ ] Vinmec   [ ] Khác                   │
│                                                             │
│ Ai đang đau (Actor)? Khách hàng (xe nằm xưởng 2–4 ngày)     │
│                      Kỹ thuật viên (điền form thủ công,     │
│                      chờ duyệt không có SLA rõ ràng)        │
│                      VinFast (chi phí bồi thường phát sinh  │
│                      từ ngày thứ 4 — cần xác nhận mức       │
│                      áp dụng tại thị trường Việt Nam)       │
│                                                             │
│ Workflow thủ công hiện tại (3-5 bước):                      │
│   1. KTV kết nối OBD, đọc DTC codes thủ công (~30–60 phút)  │
│   ──> 2. KTV điền form mô tả triệu chứng tự do gửi HQ       │
│          (~30–45 phút, mô tả không chuẩn hóa dễ bị trả về)  │
│   ──> 3. Chuyên viên HQ xét duyệt theo queue                │
│          (2–4 ngày, không có SLA cứng)                      │
│   ──> 4. Phê duyệt → KTV đặt phụ tùng → sửa chữa            │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2 + 3 (2–4 ngày):     │
│ Form mô tả không chuẩn hóa khiến HQ mất thêm thời gian      │
│ giải mã; queue tập trung xử lý case rõ ràng lẫn phức tạp    │
│ như nhau, không có phân luồng                               │
│                                                             │
│ AI có thể nhảy vào hỗ trợ ở bước nào?                       │
│ Bước 1–2 → AI đọc DTC codes + lịch sử xe + đối chiếu        │
│ policy bảo hành → đề xuất phân loại kèm confidence score    │
│ + draft form điền sẵn → KTV confirm → case rõ ràng          │
│ (confidence ≥ 80%) bỏ qua queue HQ, xử lý trong <4 giờ      │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
│   "70% case tiêu chuẩn được phân loại trong <15 phút,       │
│    tổng thời gian phê duyệt <4 giờ"                         │
│   "Độ chính xác phân loại AI ≥ 90% so với quyết định HQ     │
│    (đo trên 500 case test)"                                 │
│   "Giảm tỷ lệ form bị HQ trả về từ ~30% xuống <5%"          │
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM  [ ] Agent │
└─────────────────────────────────────────────────────────────┘
```
