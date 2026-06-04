# Template — Evidence Pack

Nộp kèm thin SPEC cuối Day 05.

## 1. Nhóm và track

**Tên nhóm:**  Duy Bảo, Hữu Khoa, Anh Thư (E402-Nhom69)

**Track:**  A. Learning OS (VinAI Thực chiến)

**Product/app đã chọn:**  Trợ lý Kute (phiên bản cũ) / Kênh hỗ trợ học viên VinAI Thực chiến

**Build slice đang nghĩ:**  Chatbot Kuter hỗ trợ học tập và vận hành VinAI Thực chiến (RAG kết hợp Handbook PDF tĩnh + Discord Q&A động trong Rule-base + Fallback tự động log new_issue.json và Mentor Reply).

## 2. Self-use evidence

| Observation | Screenshot/link | Path liên quan | Điều học được |
|---|---|---|---|
| Thành viên Duy Bảo hỏi Trợ lý Kute cũ cách xử lý khi muốn đổi nhóm đã được ghép. Bot không biết trả lời, lập tức tag Mod và đầu hàng vì câu hỏi này mang tính vận hành linh hoạt không có trong Handbook PDF. | ![Đổi nhóm bị lỗi](Figures/evidence_1.png) | Low-confidence / Failure | Tri thức động cực kỳ quan trọng. Các câu hỏi vận hành thực tế đã được Admin/Mentor giải quyết trên Discord nhưng bot cũ không truy cập được vì chỉ RAG Handbook tĩnh. |
| Mở Handbook PDF 17 trang ra để tìm xem "Sinh viên năm cuối có được tham gia không?", phải dùng Ctrl+F quét mới ra kết quả ở trang 13. | ![Handbook Search](Figures/handbook_search_manually.jpeg) | Happy | Học viên lười đọc tài liệu dài. Bot cần trích xuất trực tiếp câu trả lời ngắn kèm số trang trích dẫn thay vì quăng lại cả file PDF. |

## 3. User / review / social evidence

Nguồn có thể là phỏng vấn nhanh hoặc nguồn thực tế trên kênh Discord chung.

| Quote / review / observation | Nguồn | User là ai? | Pain/failure mode |
|---|---|---|---|
| Học viên Lê Bá Chiến tag Trợ lý Kute hỏi: "cho tao xin slide day 4". Bot trả lời không chắc chắn và tag Mod, dù link slide đã được post trên kênh Discord trước đó. | Discord (`Figures/evidence_2.png`) | Học viên lớp | Trôi tài nguyên học tập, bot không đồng bộ được các cập nhật mới nhất từ Mentor trên Discord, gây quá tải kênh chung. |
| "Mọi người có ai không làm được daily không ạ, hôm qua em làm bình thường nhưng hôm nay lại báo chỉ dùng được trong thread của team ạ" | Discord | Học viên lớp | Kẹt lỗi kỹ thuật, cần câu trả lời ngay lập tức nhưng phải đợi Mentor online để được hỗ trợ. |

## 4. Competitor / analog evidence

| App / mô hình tham khảo | Họ xử lý task này thế nào? | Pattern học được | Có áp dụng trong 1 ngày không? |
|---|---|---|---|
| Chatbot tư vấn tuyển sinh đại học (Rule-based) | Trả lời theo kịch bản cố định (nhấn phím 1, phím 2). Hỏi lệch kịch bản là báo lỗi hoặc xin số điện thoại. | Trải nghiệm rất gò bó. Nhưng cơ chế fallback "xin thông tin để gọi lại" rất an toàn để kiểm soát rủi ro. | Có. Nhóm dùng LLM để chat mượt mà (Augment) kết hợp cơ chế Fallback (tự động log new_issue.json và hướng dẫn liên hệ Mentor) khi AI không tự tin. |
| DataCamp / Coursera AI Assistant | Đưa ra gợi ý code, giải thích khái niệm hẹp ngay trong bài học. | Thu thập phản hồi khi người dùng báo cáo lỗi để đánh giá độ chính xác và cập nhật dữ liệu. | Có. Thiết kế cơ chế ghi nhận phản hồi lỗi để thu thập Learning Signal cho Mentor cập nhật tri thức của bot. |

## 5. Evidence -> Insight

```text
Evidence nổi bật nhất:
Học viên liên tục hỏi về tài nguyên (slide, link) và các thủ tục vận hành linh hoạt (đổi nhóm, lỗi setup) đã được giải đáp trước đó trên Discord, nhưng Trợ lý Kute cũ vẫn không trả lời được do chỉ dựa vào Handbook PDF tĩnh.

Insight:
Học viên cần một "người hỗ trợ trực chiến" phản hồi nhanh chóng và tin cậy cả thông tin tĩnh (Handbook) lẫn tri thức động phát sinh trong quá trình học (Discord Q&A). Họ cần sự tin tưởng rằng thông tin là chính xác nên cần có trích dẫn nguồn (số trang hoặc link tham chiếu).

Opportunity:
Dùng AI RAG đa nguồn (Handbook PDF + Discord Q&A) để:
1. Automate 100% các câu hỏi FAQ hành chính từ Handbook (kèm trích dẫn số trang).
2. Augment các câu hỏi kỹ thuật/vận hành bằng cách trích xuất Discord Q&A làm gợi ý nháp, đồng thời cung cấp lối thoát an toàn (Fallback) tự động log câu hỏi vào new_issue.json khi độ tự tin thấp (cosine similarity < 0.78 hoặc không tìm thấy thông tin).
```

## 6. Evidence đổi SPEC như thế nào?

- [ ] Đổi user chính.
- [ ] Đổi pain statement.
- [X] Đổi build slice.
- [X] Đổi Auto/Aug decision.
- [X] Đổi 4 paths.
- [X] Đổi failure mode.
- [ ] Đổi owner/test plan.

Ghi rõ 1-2 thay đổi quan trọng:

```text
Trước evidence, nhóm định: 
Làm một chatbot thông thường chỉ RAG trên tài liệu tĩnh (Handbook PDF) và trả lời tự động mọi câu hỏi (Full-Automation).

Sau evidence, nhóm đổi thành: 
Xây dựng Chatbot Kuter (kết hợp Handbook tĩnh + Discord Q&A động). Áp dụng quy tắc Phân luồng quyết định (Conditional Automation): câu hỏi hành chính rõ ràng từ Handbook thì Automate trả lời; câu hỏi vận hành/kỹ thuật thì Augment dạng gợi ý nháp trích xuất từ Rule-base kèm tag [Rule-base]. Đồng thời thêm cơ chế dynamic update: khi Mentor dùng tính năng Reply trên Discord để trả lời câu hỏi mới, bot tự động paraphrase và cập nhật tri thức vào rulebase.json.
```

