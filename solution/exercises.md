# K4 — Ngày 1: Bài Tập & Phản Ánh
## Khám Phá LLM API | Phiếu Thực Hành

**Thời lượng:** 4 tiếng
**Cách làm:** Trả lời từng câu ngay sau khi hoàn thành block tương ứng —
đừng để dồn hết về cuối buổi. Thay dòng `*Câu trả lời của bạn*` bằng câu
trả lời thật (chấm tự động sẽ đếm số câu đã trả lời).

---

## Block 1 — API Cơ Bản (trả lời sau Checkpoint 1)

### Câu 1.1 — Độ nhạy của temperature
Gọi `call_openai` với temperature 0.0, 0.5, 1.0 và 1.5 dùng prompt
**"Hãy kể cho tôi một sự thật thú vị về Việt Nam."**

**Bạn nhận thấy quy luật gì qua bốn phản hồi?** (2–3 câu)
> Temperature = 1.0 cho kết quả là ngày giỗ tổ Hùng Vương còn 3 giá trị temperature còn lại cho kết quả về hang Sơn Đoòng. Với giá trị temperature 1.5 và 0.5, câu trả lời có kèm thêm các tính từ mô tả vẻ đẹp của hang, còn với temperature 0.0 thì chỉ có các thống kê về hang. Như vậy temperature cao cho kết quả đỡ cứng nhắc hơn.

### Câu 1.2 — Chọn temperature cho sản phẩm
**Bạn sẽ đặt temperature bao nhiêu cho chatbot hỗ trợ khách hàng, và tại sao?**
> Với chatbot hỗ trợ khách hàng chỉ cần thông tin chính xác chứ không cần độ sáng tạo quá nhiều nên temperature ở mức 0.5 đến 1.0 là lựa chọn của tôi.

### Câu 1.3 — Đánh đổi chi phí
Kịch bản: 10.000 người dùng hoạt động mỗi ngày, mỗi người gọi API 3 lần,
mỗi lần trung bình ~350 token đầu ra.

**Ước tính GPT-4o đắt hơn GPT-4o-mini bao nhiêu lần cho workload này? Nêu một
trường hợp GPT-4o xứng đáng với chi phí và một trường hợp nên dùng mini:**
> Vì số token input và output không đổi nên GPT-4o sẽ luôn đắt hơn GPT-4o-mini khoảng 16,7 lần bất kể độ dài prompt. Nên dùng GPT-4o khi các tác vụ đòi hỏi chính xác tuyệt đối hoặc các bước suy luận phức tạp. Nên dùng GPT-4o-mini khi các hoạt động cần dùng cơ bản như hỏi đáp cơ bản, trích xuất thông tin nhưng số lượng request mỗi ngày lớn.

---

## Block 2 — System Prompt & Token (trả lời sau Checkpoint 2)

### Câu 2.1 — Sức mạnh của persona
Gọi `chat_with_system_prompt` hai lần với cùng câu hỏi
**"Giải thích blockchain là gì?"** nhưng hai system prompt khác nhau:
- "Bạn là giáo viên tiểu học, giải thích thật đơn giản cho trẻ 8 tuổi."
- "Bạn là chuyên gia tài chính, trả lời chuyên sâu bằng thuật ngữ kỹ thuật."

**Hai phản hồi khác nhau như thế nào (độ dài, từ vựng, ví dụ)? System prompt
ảnh hưởng đến hành vi model ra sao?** (3–4 câu)
> Phản hồi số 1 ngắn hơn phản hồi số 2 rất nhiều và dùng những từ ngữ thông thường, không phải từ chuyên ngành. Phản hồi số 2 dài hơn, dùng những từ chuyên môn và có đi kèm từ tiếng Anh tương ứng. Như vậy, system prompt quyết định độ dài câu trả lời của model và cách model lựa chọn từ cho câu trả lời.

### Câu 2.2 — tiktoken vs đếm từ
Chọn một đoạn văn tiếng Việt ~100 từ. So sánh số token theo `count_tokens`
(tiktoken) với ước lượng `số từ / 0.75` mà Part 1 đã dùng.

**Hai con số chênh nhau bao nhiêu phần trăm? Vì sao tiếng Việt thường tốn
nhiều token hơn tiếng Anh cùng độ dài?**
> Xét 1 đoạn văn 100 từ tiếng việt. Số token theo công thức giả định /0.75 là 140 token. Với bộ đếm token chuẩn sẽ khoảng 180 token. Như vậy sẽ chênh khoảng 30%. Tiếng việt thường tốn token hơn so với tiếng anh vì tiếng việt có hệ thống dấu thanh, bộ mã hóa không tính 1 chữ có dấu là 1 token mà phải tách thành chữ cái và dấu riêng nên số token tăng lên.

---

## Block 3 — Streaming & Độ Bền (trả lời sau Checkpoint 3)

### Câu 3.1 — Trải nghiệm người dùng với streaming
**Streaming quan trọng nhất trong trường hợp nào, và khi nào thì
non-streaming lại phù hợp hơn?** (1 đoạn văn)
> Streaming quan trọng nhất trong các ứng dụng tương tác trực tiếp với người dùng (như chatbot) vì nó cho phép người dùng đọc ngay lập tức từng từ được sinh ra thay vì phải nhìn màn hình chờ lâu và thấy khó chịu. Ngược lại, non-streaming lại phù hợp hơn với các tác vụ chạy ngầm ở backend — chẳng hạn như trích xuất dữ liệu, sinh cấu trúc định dạng JSON nơi chỉ yêu cầu toàn bộ khối dữ liệu phải hoàn thiện và toàn vẹn để thực hiện ngay bước xử lý tiếp theo.

### Câu 3.2 — Vì sao backoff theo cấp số nhân?
**So với delay cố định (ví dụ luôn chờ 1 giây), exponential backoff có lợi
thế gì khi API bị quá tải? Điều gì xảy ra nếu hàng nghìn client cùng retry
với delay cố định giống nhau?**
> Exponential backoff có lợi thế là giúp giảm bớt áp lực liên tục lên máy chủ, cho phép hệ thống đang quá tải có đủ thời gian để phục hồi. Nếu hàng nghìn client cùng bị lỗi và đồng loạt thử lại với cùng một mức delay cố định 1 giây, toàn bộ lượng request đó sẽ lại cùng lúc dội thẳng vào máy chủ, tạo ra các đỉnh tải tuần hoàn khiến API vĩnh viễn không thể thoát khỏi trạng thái sập hoặc từ chối dịch vụ.

---

## Block 4 — Mini-Project (trả lời sau Checkpoint 4)

### Câu 4.1 — Thiết kế persona
**Bạn chọn persona gì cho trợ lý của mình? Viết lại system prompt đó và giải
thích 1–2 lựa chọn từ ngữ quan trọng trong prompt (ví dụ: vì sao yêu cầu
"trả lời ngắn gọn", vì sao chỉ định ngôn ngữ...):**
> Persona hiện tại: "Bạn là trợ giảng thân thiện của khóa AI, ""trả lời ngắn gọn bằng tiếng Việt.". Yêu cầu trả lời ngắn gọn để tiết kiệm token cho API thật. Yêu cần chỉ định ngôn ngữ để tránh trả lời bằng tiếng Anh hoặc các ngôn ngữ khác.

### Câu 4.2 — Hạn chế & cải thiện
**Trợ lý của bạn hiện có hạn chế lớn nhất là gì (ví dụ: history chỉ 3 lượt,
không có bộ nhớ dài hạn, không kiểm duyệt nội dung...)? Đề xuất một cải
thiện cụ thể và mô tả ngắn cách triển khai:**
> Hiện tại hạn chế lớn nhất là chỉ nhớ 3 đoạn hội thoại gần nhất nên chatbot sẽ rất nhanh quên ngữ cảnh. Để cải thiện, có thể chạy ngầm một lệnh gọi API để tóm tắt nội dung các tin nhắn cũ đó thành 1 đoạn văn ngắn. Bản tóm tắt này sau đó được tự động chèn thêm vào nội dung của system_prompt. Nhờ vậy, AI luôn nắm được bối cảnh tổng thể của toàn bộ cuộc trò chuyện mà vẫn giữ cho lượng token gửi đi ở mức thấp nhất.

---

## Danh Sách Kiểm Tra Nộp Bài

- [ ] `python grade.py` — xem điểm tự động, mục tiêu ≥ 75/100
- [ ] Cả 4 checkpoint pytest đều pass
- [ ] Tất cả 9 câu trong file này đã được trả lời
- [ ] Đã copy bài làm vào folder `solution/`, push lên fork và dán link trên trang bài Lab ở VLearn trước 23:59 ngày 11/09/2026
