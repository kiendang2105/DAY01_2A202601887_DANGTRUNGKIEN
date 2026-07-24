# K3 — Ngày 1: Bài Tập & Phản Ánh
## Khám Phá LLM API | Phiếu Thực Hành

**Thời lượng:** 9h00–13h00
**Cách làm:** Trả lời từng câu ngay sau khi hoàn thành block tương ứng —
đừng để dồn hết về cuối buổi. Thay dòng bằng câu
trả lời thật (chấm tự động sẽ đếm số câu đã trả lời).

---

## Block 1 — API Cơ Bản (trả lời sau Checkpoint 1)

### Câu 1.1 — Độ nhạy của temperature
Gọi `call_openai` với temperature 0.0, 0.5, 1.0 và 1.5 dùng prompt
**"Hãy kể cho tôi một sự thật thú vị về Việt Nam."**

**Bạn nhận thấy quy luật gì qua bốn phản hồi?** (2–3 câu)
> Khi temperature càng thấp (0.0), mô hình trả lời ổn định, ít sáng tạo và gần như cho kết quả giống nhau mỗi lần. Khi tăng lên 0.5 và 1.0, câu trả lời đa dạng và tự nhiên hơn. Ở mức 1.5, phản hồi sáng tạo hơn nhưng đôi khi lan man hoặc kém chính xác hơn.

### Câu 1.2 — Chọn temperature cho sản phẩm
**Bạn sẽ đặt temperature bao nhiêu cho chatbot hỗ trợ khách hàng, và tại sao?**
> Tôi sẽ chọn temperature khoảng 0.2–0.3 cho chatbot hỗ trợ khách hàng. Mức này giúp câu trả lời nhất quán, chính xác và hạn chế việc mô hình tự suy diễn, đồng thời vẫn giữ được ngôn ngữ tự nhiên khi giao tiếp với người dùng.

### Câu 1.3 — Đánh đổi chi phí
Kịch bản: 10.000 người dùng hoạt động mỗi ngày, mỗi người gọi API 3 lần,
mỗi lần trung bình ~350 token đầu ra.

**Ước tính GPT-4o đắt hơn GPT-4o-mini bao nhiêu lần cho workload này? Nêu một
trường hợp GPT-4o xứng đáng với chi phí và một trường hợp nên dùng mini:**
> Mỗi ngày có khoảng 30.000 lượt gọi API (10.000 người × 3 lần). Với cùng số lượng token, GPT-4o có chi phí cao hơn GPT-4o-mini khoảng 15–20 lần (tùy bảng giá tại thời điểm sử dụng). GPT-4o phù hợp với các tác vụ yêu cầu suy luận phức tạp như phân tích tài liệu hoặc viết báo cáo chuyên sâu, trong khi GPT-4o-mini phù hợp cho chatbot chăm sóc khách hàng, hỏi đáp thông thường hoặc các ứng dụng có lưu lượng lớn cần tối ưu chi phí.

---

## Block 2 — System Prompt & Token (trả lời sau Checkpoint 2)

### Câu 2.1 — Sức mạnh của persona
Gọi `chat_with_system_prompt` hai lần với cùng câu hỏi
**"Giải thích blockchain là gì?"** nhưng hai system prompt khác nhau:
- "Bạn là giáo viên tiểu học, giải thích thật đơn giản cho trẻ 8 tuổi."
- "Bạn là chuyên gia tài chính, trả lời chuyên sâu bằng thuật ngữ kỹ thuật."

**Hai phản hồi khác nhau như thế nào (độ dài, từ vựng, ví dụ)? System prompt
ảnh hưởng đến hành vi model ra sao?** (3–4 câu)
> Với persona là giáo viên tiểu học, câu trả lời ngắn gọn, sử dụng từ ngữ đơn giản và có ví dụ gần gũi để trẻ em dễ hiểu. Với persona là chuyên gia tài chính, câu trả lời dài hơn, sử dụng nhiều thuật ngữ như "distributed ledger", "consensus mechanism", "cryptography" và phân tích sâu hơn về công nghệ. Điều này cho thấy system prompt định hướng rất mạnh phong cách, mức độ chi tiết và đối tượng mà mô hình hướng tới.

### Câu 2.2 — tiktoken vs đếm từ
Chọn một đoạn văn tiếng Việt ~100 từ. So sánh số token theo `count_tokens`
(tiktoken) với ước lượng `số từ / 0.75` mà Part 1 đã dùng.

**Hai con số chênh nhau bao nhiêu phần trăm? Vì sao tiếng Việt thường tốn
nhiều token hơn tiếng Anh cùng độ dài?**
> Khi so sánh, số token của tiktoken thường cao hơn khoảng 10–20% so với cách ước lượng bằng số từ/0.75. Nguyên nhân là tiếng Việt có nhiều dấu, khoảng trắng và các từ ghép khiến bộ mã hóa BPE phải chia thành nhiều token hơn. Vì vậy số token thực tế thường lớn hơn so với cách ước lượng đơn giản.

---

## Block 3 — Streaming & Độ Bền (trả lời sau Checkpoint 3)

### Câu 3.1 — Trải nghiệm người dùng với streaming
**Streaming quan trọng nhất trong trường hợp nào, và khi nào thì
non-streaming lại phù hợp hơn?** (1 đoạn văn)
> Streaming đặc biệt quan trọng đối với các câu trả lời dài hoặc khi mô hình cần nhiều thời gian để suy luận, vì người dùng có thể thấy phản hồi xuất hiện ngay lập tức thay vì phải chờ toàn bộ kết quả. Điều này cải thiện đáng kể trải nghiệm sử dụng và tạo cảm giác hệ thống phản hồi nhanh hơn. Ngược lại, với các tác vụ ngắn như phân loại, kiểm tra hoặc trả lời chỉ vài từ, non-streaming sẽ đơn giản hơn và không tạo khác biệt đáng kể.

### Câu 3.2 — Vì sao backoff theo cấp số nhân?
**So với delay cố định (ví dụ luôn chờ 1 giây), exponential backoff có lợi
thế gì khi API bị quá tải? Điều gì xảy ra nếu hàng nghìn client cùng retry
với delay cố định giống nhau?**
> Exponential backoff giúp giảm tải cho máy chủ khi API đang quá tải bằng cách tăng dần thời gian chờ sau mỗi lần thất bại. Nếu tất cả client đều retry với cùng một khoảng thời gian cố định, chúng có thể gửi yêu cầu đồng thời và tiếp tục gây quá tải cho máy chủ (hiện tượng "thundering herd"). Việc tăng dần thời gian chờ giúp phân tán các yêu cầu và tăng khả năng phục hồi của hệ thống.

---

## Block 4 — Mini-Project (trả lời sau Checkpoint 4)

### Câu 4.1 — Thiết kế persona
**Bạn chọn persona gì cho trợ lý của mình? Viết lại system prompt đó và giải
thích 1–2 lựa chọn từ ngữ quan trọng trong prompt (ví dụ: vì sao yêu cầu
"trả lời ngắn gọn", vì sao chỉ định ngôn ngữ...):**
> System Prompt:"Bạn là trợ lý AI chuyên hỗ trợ lập trình Python và AI. Luôn trả lời bằng tiếng Việt, giải thích rõ ràng từng bước, ưu tiên ví dụ thực tế, nếu có nhiều cách giải thì đề xuất cách đơn giản và hiệu quả nhất trước." Tôi yêu cầu "trả lời bằng tiếng Việt" để phù hợp với người dùng và "giải thích từng bước" nhằm giúp dễ học và dễ áp dụng. Việc ưu tiên cách làm đơn giản trước giúp tiết kiệm thời gian và phù hợp với người mới.

### Câu 4.2 — Hạn chế & cải thiện
**Trợ lý của bạn hiện có hạn chế lớn nhất là gì (ví dụ: history chỉ 3 lượt,
không có bộ nhớ dài hạn, không kiểm duyệt nội dung...)? Đề xuất một cải
thiện cụ thể và mô tả ngắn cách triển khai:**
> Hạn chế lớn nhất là trợ lý chỉ lưu được lịch sử của 3 lượt hội thoại gần nhất nên dễ quên ngữ cảnh của cuộc trò chuyện dài. Một cải thiện phù hợp là bổ sung bộ nhớ dài hạn bằng cách lưu lịch sử vào cơ sở dữ liệu hoặc sử dụng vector database để truy xuất các cuộc hội thoại liên quan. Khi người dùng đặt câu hỏi mới, hệ thống sẽ lấy những thông tin phù hợp từ bộ nhớ và bổ sung vào prompt trước khi gửi đến mô hình, giúp trợ lý ghi nhớ tốt hơn.

---

## Danh Sách Kiểm Tra Nộp Bài

- [ ] `python grade.py` — xem điểm tự động, mục tiêu ≥ 75/100
- [ ] Cả 4 checkpoint pytest đều pass
- [ ] Tất cả 9 câu trong file này đã được trả lời
- [ ] Đã copy bài làm vào folder `solution/` và zip theo hướng dẫn README
