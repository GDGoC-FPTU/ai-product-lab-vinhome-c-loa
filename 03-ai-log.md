# Nhật Ký Chiêm Nghiệm — Tương Tác Với AI Trong Buổi Workshop
- Đỗ Tuấn Đạt: 2A202600818
## AI Giúp Gì

Trong buổi học, tôi dùng AI chủ yếu để phản biện các bài toán mà bản thân đã nghĩ ra. Sau khi điền xong Quick Problem Card cho bài toán phê duyệt bảo hành VinFast, tôi đưa toàn bộ nội dung vào LLM và hỏi: *"Bước nào trong workflow này AI thực sự cần thiết, bước nào rule-based là đủ?"* AI chỉ ra rằng bước phân loại DTC codes có thể dùng rule-based nếu chỉ xét theo danh mục lỗi chuẩn OBD-II, nhưng phần diễn giải ngữ cảnh (xe chạy bao nhiêu km, lịch sử sửa chữa trước đó, mô tả của KTV) mới thực sự cần LLM. Nhận xét này giúp tôi làm rõ ranh giới kiến trúc thay vì để mọi thứ mơ hồ trong một ô "LLM xử lý".

Ngoài ra tôi dùng AI để hoàn thiện phần Future-State Flow và Phase 5 Evaluate — những phần cần viết nhiều nhưng cấu trúc đã rõ, đưa template vào là ra được bản nháp để chỉnh.

---

## AI Sai Gì

Điểm sai rõ nhất là con số **$160/ngày bồi thường của VinFast** xuất hiện trong Problem Card. AI đưa số này vào như thể đây là thực tế vận hành tại Việt Nam. Khi tôi hỏi lại nguồn, AI thừa nhận đây là chính sách aftersales công bố tại thị trường Bắc Mỹ năm 2023, không phải Việt Nam và không rõ còn hiệu lực không.

Đây không phải hallucination kiểu bịa số liệu hoàn toàn mà nguy hiểm hơn ở chỗ: số có thật, nguồn có thật, nhưng ngữ cảnh bị trượt.

---

## Sửa Đổi Ra Sao

Tôi thêm một điều kiện vào các prompt phía sau: chỉ dùng số liệu có nguồn gốc từ thị trường Việt Nam, hoặc đánh dấu rõ *[Quốc tế — cần xác nhận nội địa]* nếu không tìm được tương đương. Từ đó các phần sau AI tự thêm ghi chú nguồn thay vì đưa số như sự thật.

Thay đổi lớn hơn là tôi bắt đầu tách bước tìm kiếm thông tin ra khỏi bước điền template. Trước đây hay gộp chung một prompt, dẫn đến số liệu chưa kiểm tra đã được nhúng vào tài liệu. Tách ra tạo một checkpoint tự nhiên để đọc và xét duyệt trước khi mọi thứ đi vào bản chính thức.

---

## Chiêm Nghiệm Chung

Điều thay đổi nhiều nhất sau buổi hôm nay không phải hiểu biết kỹ thuật mà là nhận thức về vai trò của mình khi dùng AI. AI giỏi mở rộng nhanh và soạn thảo, nhưng việc xét duyệt độ chính xác và phù hợp ngữ cảnh vẫn là của người dùng. Đó cũng chính xác là lý do HITL phải có mặt trong mọi thiết kế hệ thống AI mà nhóm đang làm — không phải vì model chưa đủ giỏi, mà vì accountability không thể delegate cho model.
