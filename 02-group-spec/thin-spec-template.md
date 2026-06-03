# Template — Thin SPEC Cuối Day 05

Thin SPEC không phải PRD đầy đủ. Đây là bản cam kết đủ rõ để sáng Day 06 nhóm build ngay.

## 1. Track, product/app và user

**Track:** A. Learning OS (VinAI Thực chiến)
**Product/app thật:** Website VinUni / Kênh hỗ trợ học viên VinAI Thực chiến
**User cụ thể:**
- Học viên mới onboard: tìm thông tin vận hành (deadline, thủ tục, setup môi trường)
- Ứng viên đang cân nhắc đăng ký: cần tư vấn cá nhân hóa (có phù hợp không?)

**Nhóm có phải user thật không?**
Có — Duy Bảo, Hữu Khoa, Anh Thư đều là học viên đang trải nghiệm workflow này trực tiếp.

## 2. Evidence summary

| Evidence | Nguồn | User/pain nói lên điều gì? | SPEC phải đổi gì? |
|---|---|---|---|
| Tìm "sinh viên năm cuối có được tham gia không?" trong Handbook PDF 20 trang bằng Ctrl+F | Self-use | User không đọc document dài; cần trích xuất thẳng câu trả lời + số trang | Automate FAQ từ Handbook kèm citation |
| Discord search "Lỗi setup môi trường" ra đống tin nhắn đứt đoạn, không biết đâu là chốt của Mentor | Self-use | Data Discord nhiễu → nguy cơ hallucination cao; cần fallback tag Mentor | Augment kỹ thuật + fallback khi confidence thấp |

## 3. Pain statement

```text
Học viên và ứng viên VinAI Thực chiến đang gặp khó
ở bước tìm thông tin hành chính và xử lý lỗi kỹ thuật khi làm lab,
vì thông tin nằm rải rác trong Handbook PDF dài và Discord nhiễu,
dẫn tới phải đợi Mentor online mới giải quyết được,
làm chậm tiến độ học và tăng tải cho đội ngũ hỗ trợ.
Bằng chứng chính là: học viên hỏi deadline trên Discord không tìm thấy;
học viên kẹt lỗi token limit kêu cứu công khai;
nhóm self-use phải Ctrl+F mỏi mắt mới ra kết quả ở trang 14 Handbook.
```

## 4. Build slice

```text
Cho học viên và ứng viên VinAI Thực chiến đang tìm câu trả lời
trên Handbook hoặc kẹt lỗi kỹ thuật khi làm lab,
prototype sẽ dùng AI RAG để:
  (1) Automate trả lời FAQ hành chính dựa trên Handbook — kèm trích dẫn số trang;
  (2) Augment câu hỏi kỹ thuật bằng cách tổng hợp Discord history
      thành gợi ý sửa lỗi nháp,
tạo ra câu trả lời có nguồn rõ ràng và lối thoát an toàn,
và xử lý failure mode AI không chắc / câu hỏi ngoài scope
bằng fallback nút "Báo cáo Mentor" + disclaimer rõ ràng.
```

## 5. Auto/Aug decision

Chọn một:

- [ ] **Augmentation:** AI gợi ý/draft/phân loại, user quyết cuối.
- [x] **Conditional automation:** AI tự làm trong case hẹp; case mơ hồ/rủi ro chuyển người.
- [ ] **Automation:** AI tự quyết và tự hành động.

**Lý do chọn:**
Phân luồng theo loại câu hỏi:
- FAQ hành chính (deadline, điều kiện, thủ tục) → Automate: câu trả lời xác định, nguồn rõ từ Handbook, rủi ro thấp.
- Câu hỏi kỹ thuật/lỗi code → Conditional: AI đưa gợi ý nháp từ Discord history; nếu confidence thấp hoặc user báo sai → fallback Mentor. Rủi ro AI bịa code sai gây học viên nản lòng quá cao để full-automate.

**Human role:** rescuer (Mentor nhận báo cáo khi AI bó tay) + trainer (vote Thumbs Up/Down để cải thiện)

## 6. Four paths

| Path | Prototype phải thể hiện gì? |
|---|---|
| Happy | User hỏi "Sinh viên năm cuối có được tham gia không?" → AI trả lời ngay kèm trích dẫn trang Handbook; user không cần rời chatbot |
| Low-confidence | User hỏi câu kỹ thuật mơ hồ → AI hiển thị gợi ý nháp kèm disclaimer "Thông tin tổng hợp từ Discord, chưa được Mentor xác nhận" + nút "Báo cáo Mentor" |
| Failure | User hỏi về nội dung không có trong Handbook và Discord → AI nói rõ giới hạn, không hallucination, tự động kích hoạt nút "Báo cáo Mentor" |
| Correction | User bấm Thumbs Down hoặc "Báo cáo Mentor" → correction được log lại; Mentor nhận notification kèm context câu hỏi gốc |

## 7. Failure mode nguy hiểm nhất

```text
Nếu học viên hỏi về lỗi code cụ thể (ví dụ: token limit, CUDA error),
AI có thể hallucinate đoạn code sửa lỗi trông có vẻ hợp lý nhưng sai,
hậu quả là học viên chạy theo code sai, lỗi lan rộng hơn, mất niềm tin vào tool và nản lòng.
Prototype sẽ xử lý bằng:
  - Hiển thị disclaimer rõ trên mọi câu trả lời kỹ thuật;
  - Nút "Báo cáo Mentor" luôn hiện kèm câu trả lời kỹ thuật;
  - Nếu AI không tìm thấy evidence từ Discord → không sinh code, chỉ tag Mentor;
  - Log correction khi user bấm Thumbs Down để Mentor review.
Owner kiểm thử path này là: Duy Bảo.
```

## 8. Owner plan cho sáng Day 06

| Thành viên | Việc phụ trách | Bằng chứng cần có trong repo |
|---|---|---|
| Anh Thư | Research / evidence — chuẩn hóa Handbook PDF và Discord Q&A thành data source cho RAG | evidence-pack.md + data folder |
| Hữu Khoa | SPEC — hoàn thiện thin-spec, viết 4 paths và failure mode chi tiết | thin-spec-final.md |
| Duy Bảo | Prototype — build chatbot RAG, phân luồng FAQ vs kỹ thuật, tích hợp fallback | prototype/ folder + README |
| Duy Bảo | Test / failure path — kiểm thử câu hỏi kỹ thuật mơ hồ và câu ngoài scope | test-cases.md |
| Cả nhóm | Demo script + ghi lại 3 case: happy / low-confidence / failure | demo-script.md |