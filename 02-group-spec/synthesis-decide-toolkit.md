# Toolkit — Từ Evidence Đến Build Slice

Dùng sau khi nhóm đã có evidence. Mục tiêu là chốt một build slice đủ nhỏ cho Day 06.

## 1. Gom evidence thành cụm

Gom theo **workflow/pain**, không gom theo tên feature.

**Cụm 1 — Tìm thông tin hành chính/vận hành tĩnh**
- Phải tìm thủ công thông tin điều kiện nhận chứng chỉ bằng cách Ctrl+F trong Handbook PDF 17 trang.
- Học viên lười đọc tài liệu dài và cần câu trả lời nhanh, kèm trích dẫn số trang chính xác để đối chiếu.

**Cụm 2 — Kẹt câu hỏi vận hành động & tài nguyên học tập trên Discord**
- Thành viên nhóm (Duy Bảo) hỏi cách đổi nhóm đã ghép trên Discord của Trợ lý Kute cũ, bot phản hồi lỗi và tag Mod.
- Học viên Lê Bá Chiến xin link slide bài giảng Day 4, bot trả lời không chắc chắn và tag Mod, dù link đã được post trước đó.
- Tin nhắn trên Discord bị trôi nhanh, học viên khó tìm kiếm và thường xuyên hỏi lặp.

**Cụm 3 — Sự thiếu tin cậy & quá tải của Mentor**
- Chatbot cũ không cập nhật được tri thức động từ các câu trả lời trước đó của Mentor.
- Mentor phải online liên tục để giải đáp thủ công các câu hỏi lặp đi lặp lại.
- Nếu chatbot tự động bịa câu trả lời kỹ thuật (Hallucination) sẽ gây hậu quả nghiêm trọng hơn cho học viên.

## 2. Viết insight

```text
Học viên và ứng viên VinAI Thực chiến không chỉ cần câu trả lời từ Handbook tĩnh.
Họ thực sự cần một "người hỗ trợ trực chiến" giải đáp nhanh chóng cả
quy chế tĩnh lẫn các thông tin vận hành động (slide, đổi nhóm) một cách tin cậy
với nguồn trích dẫn rõ ràng, vì evidence cho thấy chatbot cũ liên tục
đầu hàng và tag Mod trước các câu hỏi vận hành thực tế đã được giải quyết trên Discord.
```

## 3. Viết opportunity

```text
Cơ hội là dùng AI RAG kết hợp đa nguồn để:
(1) Automate 100% câu hỏi FAQ hành chính dựa trên Handbook PDF (kèm trích dẫn số trang);
(2) Augment mảng hỏi đáp vận hành và kỹ thuật bằng cách trích xuất Discord Q&A
    thành gợi ý nháp nhanh chóng — tự động log new_issue.json khi AI không chắc;
(3) Dynamic Update: tự động cập nhật tri thức mới khi Mentor trả lời câu hỏi trên Discord;
giúp học viên tự giải quyết vấn đề tức thì mà không phải chờ đợi, trong khi giảm tải tối đa cho ban tổ chức.
```

## 4. Chọn build slice

**Build slice được chọn:** Chatbot Kuter (Handbook tĩnh + Discord Q&A động), phân luồng câu hỏi FAQ (Automate) và câu hỏi kỹ thuật/vận hành (Augment + Dynamic Rule-base).

Kiểm tra 5 câu hỏi:

| Câu hỏi | Đạt khi | Kết quả |
|---|---|---|
| User cụ thể chưa? | Nói được ai dùng, trong bối cảnh nào. | Học viên và ứng viên đang tham gia/tìm hiểu lớp học VinAI Thực chiến. |
| Task đủ hẹp chưa? | Demo được trong 3-5 phút. | Chat 1 câu hỏi FAQ Handbook + 1 câu hỏi vận hành Discord. |
| AI decision rõ chưa? | AI gợi ý/tự làm một việc cụ thể. | Phân luồng: Automate (FAQ Handbook) vs Augment + Fallback (vận hành Discord) + Auto-update rule-base khi Mentor rep. |
| Failure path rõ chưa? | Có một case AI không chắc hoặc sai để test. | Hỏi câu ngoài phạm vi dữ liệu -> AI nhận diện độ tự tin thấp -> tự động log new_issue.json và trả về câu thoại fallback hướng dẫn qua Discord. |
| Có evidence không? | Có bằng chứng từ self-use/review/user/competitor. | Bằng chứng thực tế Duy Bảo hỏi đổi nhóm và Lê Bá Chiến xin slide Day 4 bị lỗi trên Trợ lý Kute cũ. |


## 5. Quyết định: giữ, giảm scope, hay đổi hướng?

| Tình huống | Quyết định | Áp dụng cho nhóm |
|---|---|---|
| Evidence yếu, user mơ hồ | Dừng build sâu; quay lại research 20 phút. | Không — bằng chứng lỗi thực tế rất rõ ràng và thuyết phục. |
| Ý tưởng quá rộng | Giữ domain, cắt xuống một flow. | Giữ domain chatbot, cắt xuống flow FAQ tĩnh + Q&A động Discord + cơ chế tự học khi Mentor rep. |
| AI không cần thiết | Dùng rule/manual prototype. | Không — RAG và paraphrase câu hỏi để match rule-base bắt buộc phải dùng LLM. |
| Rủi ro cao | Chọn augmentation hoặc conditional automation. | Chọn Conditional Automation: mảng vận hành dùng Augment + Fallback Mentor để kiểm soát rủi ro. |
| Không demo được trong 1 ngày | Đưa phần lớn vào backlog, giữ một path nhỏ. | Bỏ qua các tính năng nâng cao như giao diện giọng nói, dashboard phân tích. |

**Quyết định:** Giữ build slice và scope — Áp dụng Conditional Automation.

## 6. Câu chốt cuối

```text
Dựa trên bằng chứng chatbot Kute cũ bị lỗi khi xử lý câu hỏi đổi nhóm và slide học tập,
nhóm sẽ build chatbot Kuter (Handbook PDF + Discord Q&A),
cho học viên và ứng viên VinAI Thực chiến,
để giải quyết painpoint tìm kiếm tài nguyên chậm và nghẽn luồng hỗ trợ vận hành,
bằng cách AI automate trả lời FAQ Handbook (kèm số trang) và augment câu hỏi vận hành từ Discord (kèm tag [Rule-base]),
và sẽ test failure path: câu hỏi ngoài scope/mơ hồ -> độ tin cậy thấp -> tự động trả về câu thoại fallback và log new_issue.json.
```

---

## 7. Backlog

Những thứ **không build trong Day 06**:

- Giao diện giọng nói (Voice query).
- Dashboard quản trị toàn diện cho Admin/Mentor.
- Tính năng cá nhân hóa lộ trình học tập theo từng học viên.
- Phân tích cảm xúc (Sentiment Analysis) tin nhắn học viên.
- Tự động tag Mentor trực tiếp trên kênh Discord (chỉ log ticket cục bộ trong prototype).
