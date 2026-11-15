# GoWithBae — Bộ yêu cầu G1

Mở [**GoWithBae_Requirements_G1_v1.1.md**](GoWithBae_Requirements_G1_v1.1.md) để đọc tài liệu chính mới nhất dành cho khách hàng và đội phát triển. [Bản v1.0](GoWithBae_Requirements_G1_v1.0.md) giữ để đối chiếu; các use case và báo cáo hiện theo v1.1.

| Tài liệu | Mục đích |
|---|---|
| [Requirement chính v1.1](GoWithBae_Requirements_G1_v1.1.md) | Mục tiêu, phạm vi, 35 FR, 30 RULE, dữ liệu, quyền, UI, NFR, AC, OQ và bàn giao; CR-001 cập nhật ba phản hồi khách |
| [Gap analysis](Gap_Analysis.md) | Hiện trạng/mục tiêu, 29 phát hiện và trạng thái đã làm rõ/còn mở |
| [Use case và nghiệm thu](../specs/README.md) | 27 UC, 58 AC Given/When/Then, sequence/state Mermaid, task từng UC |
| [Verification](Verification_Report.md) | Kết quả kiểm tra tài liệu, giới hạn và blocker cần PO chốt |
| [Backlog](Implementation_Backlog.md) | Thứ tự phụ thuộc và công việc triển khai tham chiếu spec |
| [Truy vết máy đọc](traceability.json) | UC → FR/RULE/AC/OQ, không chứa dữ liệu cá nhân |
| [Kiểm tra cấu trúc](validation-results.json) | Kết quả kiểm tra link, coverage, template và tính toàn vẹn nguồn |

**Trạng thái:** v1.1 ngày 15-11-2026. Web G1 + quản trị, G2 định hướng; CR-001 xác nhận có số lần đã đi, Bỏ qua lời nhắc chỉ đóng và xem link bằng khách không đăng nhập. Các chính sách PROP/TBD khác chưa được duyệt.

**Skill:** [srs-agent](https://github.com/phong-baruby/ba-skills/blob/f26e0874f5bcf6e31d26044134cc4a562cb4fadf/skills/srs-agent/SKILL.md) từ [phong-baruby/ba-skills](https://github.com/phong-baruby/ba-skills/tree/f26e0874f5bcf6e31d26044134cc4a562cb4fadf). Áp dụng nhánh phân tích tài liệu có sẵn; không cài vào cấu hình toàn cục. Phiên bản phương pháp được truy nguyên qua liên kết commit ở trên; mã nguồn skill không nằm trong repository này.

Công cụ sinh/kiểm tra được chạy cục bộ, không đưa lên repository. `validation-results.json` và `mermaid-validation.json` lưu kết quả kiểm tra tài liệu; chưa có kết quả chạy thử chức năng, hiệu năng hoặc bảo mật sản phẩm.
