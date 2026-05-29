Trong quá trình làm Lab, em dùng AI để:

* Brainstorm nhanh các ý tưởng về quy trình và pain point trong hệ sinh thái Vin.
* Hỗ trợ viết và refine prompt để kiểm tra khả năng chống prompt injection của hệ thống.
* Hỗ trợ debug và sửa lỗi code Python trong quá trình làm bài.

Tuy nhiên, AI đôi lúc đưa ra các giải pháp rule-based quá phức tạp hoặc đề xuất prompt có thể bypass ranh giới an toàn của hệ thống. Ngoài ra, một số câu trả lời cũng bị hallucination khi mô tả sai logic xử lý hoặc giả định chức năng không tồn tại.

Để khắc phục, em đã điều chỉnh prompt theo hướng:

* Giới hạn rõ vai trò và phạm vi trả lời của AI.
* Bổ sung các rule ưu tiên an toàn và từ chối override instruction.
* Chia nhỏ yêu cầu để AI trả lời từng bước thay vì sinh toàn bộ logic cùng lúc.
