# Template — Thin SPEC Cuối Day 05

Thin SPEC không phải PRD đầy đủ. Đây là bản cam kết đủ rõ để sáng Day 06 nhóm build ngay.

## 1. Track, product/app và user

**Track:** A. Learning OS (VinAI Thực chiến)
**Product/app thật:** Chatbot Kuter - Kênh hỗ trợ học viên VinAI Thực chiến (phiên bản nâng cấp từ Trợ lý Kute cũ)
**User cụ thể:**
- Học viên mới onboard: tìm thông tin vận hành (deadline, đổi nhóm, slide bài học, setup môi trường).
- Ứng viên đang cân nhắc đăng ký: cần tư vấn các quy định chung.

**Nhóm có phải user thật không?**
Có — Duy Bảo, Hữu Khoa, Anh Thư đều là học viên trực tiếp trải nghiệm và gặp các vấn đề này.

## 2. Evidence summary

| Evidence | Nguồn | User/pain nói lên điều gì? | SPEC phải đổi gì? |
|---|---|---|---|
| Thành viên Duy Bảo hỏi cách đổi nhóm đã được ghép cặp trên Discord của Trợ lý Kute cũ, bot báo lỗi và tag Mod | Self-use (`Figures/evidence_1.png`) | Thông tin vận hành lớp học linh hoạt (như đổi nhóm) không có trong Handbook PDF tĩnh nhưng đã được giải quyết trên Discord trước đó. Bot cũ không cập nhật được tri thức động này. | Nâng cấp RAG đa nguồn, kết hợp dữ liệu tĩnh (Handbook) và động (Discord Q&A). |
| Học viên Lê Bá Chiến xin link slide Day 4, bot cũ không chắc chắn trả lời và tiếp tục tag Mod | Discord (`Figures/evidence_2.png`) | Tài nguyên học tập được chia sẻ liên tục nhưng bot cũ không biết, gây trôi tin nhắn và quá tải kênh chung. | Tích hợp cơ chế trích xuất link tài nguyên từ lịch sử Discord và tự động cập nhật tri thức khi Mentor trả lời. |

## 3. Pain statement

```text
Học viên và ứng viên VinAI Thực chiến đang gặp khó
ở bước tìm kiếm tài nguyên học tập (slide) và giải quyết các vấn đề vận hành linh hoạt (đổi nhóm, lỗi setup),
vì thông tin nằm rải rác trong Handbook PDF 17 trang dài dòng và kênh Discord trôi tin nhắn rất nhanh,
dẫn tới học viên phải chờ đợi phản hồi thủ công từ Mentor/Admin,
làm chậm tiến độ học tập và gây quá tải tin nhắn hỗ trợ cho Ban tổ chức.
Bằng chứng cụ thể là: chatbot cũ (Trợ lý Kute) không thể trả lời, tag Mod khi Duy Bảo hỏi đổi nhóm và Lê Bá Chiến xin slide Day 4.
```

## 4. Build slice

```text
Cho học viên và ứng viên VinAI Thực chiến đang tìm kiếm tài nguyên hoặc thắc mắc vận hành,
prototype Kuter dùng AI RAG kết hợp đa nguồn để:
  (1) Automate trả lời 100% FAQ hành chính dựa trên Handbook PDF — kèm trích dẫn số trang;
  (2) Augment câu hỏi vận hành và kỹ thuật bằng cách trích xuất Discord Q&A thành gợi ý giải pháp nháp,
tạo ra câu trả lời có trích dẫn nguồn rõ ràng và lối thoát an toàn,
và xử lý failure mode AI không chắc / câu hỏi ngoài scope
bằng fallback tự động log câu hỏi chưa giải quyết vào new_issue.json và hướng dẫn người dùng liên hệ Mentor trên Discord.
```

## 5. Auto/Aug decision

Chọn một:

- [ ] **Augmentation:** AI gợi ý/draft/phân loại, user quyết cuối.
- [x] **Conditional automation:** AI tự làm trong case hẹp; case mơ hồ/rủi ro chuyển người.
- [ ] **Automation:** AI tự quyết và tự hành động.

**Lý do chọn:**
Phân luồng theo loại câu hỏi để kiểm soát rủi ro:
- FAQ hành chính từ Handbook -> Automate: dữ liệu chuẩn xác, cố định, rủi ro thấp.
- Câu hỏi vận hành/kỹ thuật từ Discord -> Augment: cung cấp giải pháp nháp kèm disclaimer. Nếu độ tin cậy thấp (cosine similarity < 0.78), tự động chuyển luồng (Fallback) sang tag Mentor và log ticket, tránh rủi ro AI hallucinate code sai gây học viên nản lòng.

**Human role:** Rescuer (Mentor nhận ticket khi AI không chắc) + Trainer (Mentor trả lời trên Discord để cập nhật tri thức bot thông qua Gemini paraphrase và cập nhật rulebase.json).

## 6. Four paths

| Path | Prototype phải thể hiện gì? |
|---|---|
| **Happy** | User hỏi "Điều kiện nhận chứng chỉ là gì?" -> AI truy xuất Handbook và trả lời chính xác kèm số trang trích dẫn. |
| **Low-confidence** | User hỏi câu kỹ thuật/vận hành phức tạp -> AI hiển thị giải pháp trích xuất từ Discord (Rule-base) kèm tag **`[Rule-base]`** để người dùng nhận biết nguồn. |
| **Failure** | User hỏi câu ngoài scope -> AI báo giới hạn, không bịa câu trả lời, trả về câu thoại fallback hướng dẫn qua Discord và log new_issue.json. |
| **Correction** | Mentor dùng tính năng Reply trên Discord để trả lời -> Hệ thống tự động paraphrase câu hỏi, lưu Q&A mới vào rulebase.json và cập nhật cache tức thì. |

## 7. Failure mode nguy hiểm nhất

```text
Nếu học viên hỏi về quy chế hoặc lỗi code, AI có thể hallucinate ra quy định sai lệch hoặc code lỗi trông có vẻ đúng,
hậu quả là học viên làm sai quy chế dẫn đến mất điểm hoặc chạy code lỗi nặng hơn gây ức chế và mất niềm tin vào tool.
Prototype sẽ xử lý bằng:
  - Gắn tag **`[Rule-base]`** hoặc nguồn Handbook (số trang) rõ ràng với mỗi câu trả lời.
  - Ngưỡng tin cậy (cosine similarity >= 0.78) để kích hoạt câu trả lời; dưới ngưỡng sẽ tự động fallback log new_issue.json và trả về câu thoại hướng dẫn.
  - Log câu hỏi ngoài scope vào new_issue.json chờ Mentor kiểm duyệt.
Owner kiểm thử path này là: Duy Bảo.
```

## 8. Owner plan cho sáng Day 06

| Thành viên | Việc phụ trách | Bằng chứng cần có trong repo |
|---|---|---|
| Anh Thư | Research / data preparation — chuẩn hóa dữ liệu Handbook PDF và Discord Q&A thành định dạng làm sạch cho RAG pipeline. | Handbook PDF + data folder (Discord Q&A) |
| Hữu Khoa | SPEC — hoàn thiện chi tiết thin-spec, kịch bản 4 paths và các lỗi failure mode. | thin-spec-final.md |
| Duy Bảo | Prototype — lập trình RAG đa nguồn (FAISS, Gemini 2.5 Flash), phân luồng FAQ/Vận hành và tích hợp dynamic rule-base. | codebase/ folder + README |
| Duy Bảo | Test / failure path — viết kịch bản kiểm thử và chạy test các luồng happy, low-confidence, fallback. | test-cases.md |
| Cả nhóm | Demo script — chuẩn bị script demo 3 kịch bản chính cho buổi thuyết trình. | demo-script.md |