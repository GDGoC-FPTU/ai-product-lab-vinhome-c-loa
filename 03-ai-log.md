# Nhật Ký Chiêm Nghiệm — Tương Tác Với AI Trong Buổi Workshop
- Đỗ Tuấn Đạt: 2A202600818
## AI Giúp Gì

Trong quá trình làm Lab, em dùng AI để brainstorm pain point trong hệ sinh thái Vin,
đưa nội dung thẻ bài toán vào LLM để nhận phản biện về quy trình và kiến trúc giải pháp,
đồng thời hỗ trợ debug code Python khi gặp lỗi logic.

---

## AI Sai Gì

Một ví dụ của AI làm sai đó là con số **$160/ngày bồi thường của VinFast**. AI đưa số này vào như thể đây là thực tế vận hành tại Việt Nam. Khi em hỏi lại nguồn, AI thừa nhận đây là chính sách aftersales công bố tại thị trường Bắc Mỹ năm 2023, không phải Việt Nam và không rõ còn hiệu lực không.

Đây không phải hallucination kiểu bịa số liệu hoàn toàn mà nguy hiểm hơn ở chỗ: số có thật, nguồn có thật, nhưng ngữ cảnh bị trượt.

---

## Sửa Đổi Ra Sao

Em thêm một điều kiện vào các prompt phía sau: chỉ dùng số liệu có nguồn gốc từ thị trường Việt Nam, hoặc đánh dấu rõ *[Quốc tế — cần xác nhận nội địa]* nếu không tìm được tương đương. Từ đó các phần sau AI tự thêm ghi chú nguồn thay vì đưa số như sự thật.

Thay đổi lớn hơn là em bắt đầu tách bước tìm kiếm thông tin ra khỏi bước điền template. Trước đây hay gộp chung một prompt, dẫn đến số liệu chưa kiểm tra đã được nhúng vào tài liệu. Tách ra tạo một checkpoint tự nhiên để đọc và xét duyệt trước khi mọi thứ đi vào bản chính thức.

---

## Chiêm Nghiệm Chung

Điều thay đổi nhiều nhất sau buổi hôm nay không phải hiểu biết kỹ thuật mà là nhận thức về vai trò của mình khi dùng AI. AI giỏi mở rộng nhanh và soạn thảo, nhưng việc xét duyệt độ chính xác và phù hợp ngữ cảnh vẫn là của người dùng. Đó cũng chính xác là lý do HITL phải có mặt trong mọi thiết kế hệ thống AI mà nhóm đang làm — không phải vì model chưa đủ giỏi, mà vì accountability không thể delegate cho model.
