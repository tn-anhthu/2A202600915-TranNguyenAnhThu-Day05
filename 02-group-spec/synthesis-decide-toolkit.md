# Toolkit — Từ Evidence Đến Build Slice

Dùng sau khi nhóm đã có evidence. Mục tiêu là chốt một build slice đủ nhỏ cho Day 06.

## 1. Gom evidence thành cụm

Gom theo **workflow/pain**, không gom theo tên feature.

**Cụm 1 — Tìm thông tin hành chính/vận hành**
- Tìm trong Handbook PDF 17 trang bằng Ctrl+F mỏi mắt
- Hỏi deadline trên Discord group nhưng không thấy (tin nhắn bị trôi)
- Ứng viên hỏi điều kiện đầu vào cá nhân nhưng chỉ nhận lại điều kiện chung

**Cụm 2 — Kẹt lỗi kỹ thuật trong khi làm lab**
- Discord search ra tin nhắn đứt đoạn, không biết đâu là câu trả lời chốt của Mentor
- Học viên kẹt lỗi token limit phải đợi Mentor online
- AI có thể hallucination nếu context Discord quá nhiễu

**Cụm 3 — Thiếu trust vào câu trả lời AI**
- Chatbot rule-based cũ trả lời gò bó, hỏi lệch kịch bản là báo lỗi
- Không có cơ chế trích dẫn nguồn (Handbook page, Discord thread)
- Không có nút vote/feedback để học viên xác nhận câu trả lời đúng/sai

## 2. Viết insight

```text
Học viên và ứng viên VinAI Thực chiến không chỉ cần câu trả lời từ Handbook.
Họ thật ra cần một "người hỗ trợ trực chiến" có thể giải quyết nhanh
chênh lệch trình độ đầu vào và trả lời tin cậy với nguồn trích dẫn rõ ràng,
vì evidence cho thấy họ liên tục hỏi lại câu đã có trong Handbook
và thường xuyên bị kẹt lỗi kỹ thuật lặp lại trên Discord
mà phải đợi Mentor online mới giải quyết được.
```

## 3. Viết opportunity

```text
Cơ hội là dùng AI để:
(1) Automate 100% câu hỏi FAQ thủ tục/hành chính dựa trên Handbook
    kèm trích dẫn số trang — không cần Mentor trực;
(2) Augment mảng hỏi đáp kỹ thuật bằng cách tổng hợp Discord history
    thành gợi ý sửa lỗi nháp — có nút "Báo cáo Mentor" khi AI không chắc;
giúp học viên giải quyết vấn đề ngay lập tức mà không phải đợi,
trong khi vẫn kiểm soát rủi ro hallucination và code sai.
```

## 4. Chọn build slice

**Build slice được chọn:** Chatbot RAG tư vấn chương trình VinAI Thực chiến
— phân luồng câu hỏi hành chính (Automate / Handbook) vs kỹ thuật (Augment / Discord + Mentor fallback)

Kiểm tra 5 câu hỏi:

| Câu hỏi | Đạt khi | Kết quả |
|---|---|---|
| User cụ thể chưa? | Nói được ai dùng, trong bối cảnh nào. | ✅ Học viên đang cân nhắc đăng ký |
| Task đủ hẹp chưa? | Demo được trong 3-5 phút. | ✅ Chat 1 câu hỏi FAQ + 1 câu hỏi kỹ thuật |
| AI decision rõ chưa? | AI gợi ý/tự làm một việc cụ thể. | ✅ Phân luồng: Automate (FAQ) vs Augment + Fallback (kỹ thuật) |
| Failure path rõ chưa? | Có một case AI không chắc hoặc sai để test. | ✅ Câu hỏi kỹ thuật mơ hồ → trigger nút "Báo cáo Mentor" |
| Có evidence không? | Có bằng chứng từ self-use/review/user/competitor. | ✅ 5 observations từ Handbook, Discord, Facebook |


## 5. Quyết định: giữ, giảm scope, hay đổi hướng?

| Tình huống | Quyết định | Áp dụng cho nhóm |
|---|---|---|
| Evidence yếu, user mơ hồ | Dừng build sâu; quay lại research 20 phút. | Không — evidence đủ mạnh |
| Ý tưởng quá rộng | Giữ domain, cắt xuống một flow. | ✅ Giữ chatbot, cắt xuống 2 flow: FAQ + kỹ thuật |
| AI không cần thiết | Dùng rule/manual prototype. | Không — RAG cần AI để xử lý Handbook & Discord |
| Rủi ro cao | Chọn augmentation hoặc conditional automation. | ✅ Mảng kỹ thuật dùng Augment + Fallback Mentor |
| Không demo được trong 1 ngày | Đưa phần lớn vào backlog, giữ một path nhỏ. | ✅ Voice, multi-turn phức tạp → Backlog |

**Quyết định:** Giữ build slice, scope rõ — Conditional Automation.

## 6. Câu chốt cuối

```text
Dựa trên [evidence],
nhóm sẽ build [prototype slice],
cho [user],
để giải quyết [pain],
bằng cách AI [augment/automate task],
và sẽ test failure path [failure mode].
```

Dựa trên evidence từ Handbook, Discord và Facebook comment,
nhóm sẽ build Chatbot RAG phân luồng (FAQ Automate + Kỹ thuật Augment),
cho học viên và ứng viên đang cân nhắc đăng ký VinAI Thực chiến,
để giải quyết pain tìm thông tin chậm và kẹt lỗi kỹ thuật không có Mentor trực,
bằng cách AI automate trả lời FAQ dựa trên Handbook (kèm trích dẫn trang)
và augment câu hỏi kỹ thuật dựa trên Discord history (kèm nút Báo cáo Mentor),
và sẽ test failure path: câu hỏi kỹ thuật mơ hồ → AI không chắc → trigger fallback.
```

---

## 7. Backlog

Những thứ **không build trong Day 06**:

- Voice interface (query bằng giọng nói)
- Multi-turn conversation phức tạp (AI nhớ cả lịch sử session dài)
- Tích hợp calendar / deadline tracker tự động
- Admin dashboard để Mentor xem correction log
- Personalization theo track/level học viên 
