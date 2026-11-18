# Phiếu A · Tiêu chí và cách đo · Khối 1 (phút 15–65)

**Bàn:** `K5-L3A-DAY10-ABC-PoCtoPilotGate1` · **Người cầm bút chính:** vai Sản phẩm và kinh doanh (câu 1.3: Kỹ sư cổng; câu 1.2, 1.4: Vận hành và pháp lý)

**Trạng thái:** Bản nháp đề xuất; nhóm cần đối chiếu PRD v1.1 và eval card Lab 9 trước khi ký.

Điền phiếu này **trước khi chạy trợ lý lần nào**. Viết xong thì chép ngưỡng sang `config/acceptance_criteria.json`, ký ở phút 60–65,
rồi mới chạy đánh giá. Khối 1 không sửa code.

**Mục lục**
- [A1 · Mục tiêu (câu 1.1, 1.2) · phút 15–20](#a1)
- [A2 · Thước đo → Dữ liệu → Cách tính (câu 1.3, 1.4) · phút 20–35](#a2)
- [A3 · Đọc một kết quả có khoảng tin cậy (câu 1.5) · phút 35–40](#a3)
- [A4 · Câu tiêu chí thành công và ranh giới đầu ra (câu 1.6) · phút 40–60](#a4)
- [A5 · Ký và đẩy tiêu chí · phút 60–65](#a5)

---

<a id="a1"></a>

## A1 · Mục tiêu (phút 15–20)

### Câu 1.1 · Một mục tiêu cấp hệ thống (khác "model trả lời đúng")

> Giải quyết yêu cầu hoàn tiền giao trễ đúng chính sách, đúng người chịu phí và có bằng chứng; giảm công xử lý thủ công mà không gây hoàn sai, ghi lỗi oan cho hãng, lộ dữ liệu cá nhân hoặc làm ùn hàng chờ.

### Câu 1.2 · Mục tiêu nhìn từ hai người

- Chị Oanh, kế toán đối soát (không dùng trợ lý, nhưng phải khớp sổ với hãng và shop mỗi tuần): mọi khoản hoàn và lỗi ghi cho hãng phải có căn cứ, đúng bên chịu phí, khớp được với sổ và dữ liệu tracking để đối soát tuần không phải tự lần tìm.
- Chị Hạnh, trưởng ca CSKH (phải duyệt những ca trợ lý chuyển sang): ca thiếu dữ liệu, ngoại lệ hoặc vượt quyền phải được chuyển đúng người, kèm lý do và dữ liệu cần thiết, đến lượt trong SLA 60 phút mà không phải làm lại từ đầu.

---

<a id="a2"></a>

## A2 · Thước đo → Dữ liệu → Cách tính (phút 20–35)

**Khung tư duy.** Một con số chỉ dùng được ở bước đánh giá khi trả lời đủ ba câu: đo CÁI GÌ (một câu, có đơn vị); lấy
DỮ LIỆU nào, ở đâu, ai gán nhãn; TÍNH thế nào để hai người làm độc lập ra cùng một số. Thiếu ô thứ ba thì chưa phải thước đo.

**Câu hỏi dẫn dắt**
- Thước đo này nói về MỘT bước của agent hay cả luồng? Nếu cả luồng hỏng, nó có chỉ ra bước nào hỏng không?
- Dữ liệu đã có sẵn (trace, sổ cái) hay phải thu thêm? Nhãn đúng/sai đến từ đâu?
- Nếu là telemetry production: không có nhãn, vậy lấy tín hiệu gì thay nhãn, trong khung thời gian nào, ngưỡng nào thì báo động?

### Câu 1.3 · Offline metric cho MỘT bước · Câu 1.4 · Telemetry production

Câu 1.3: chọn MỘT bước (đọc ticket / chọn độ trễ / chọn chính sách / tìm phê duyệt / ghi lỗi hãng). Câu 1.4: tín hiệu cho biết
trợ lý có đang gây thiệt hại cho bên ngoài LogiX không; không có nhãn trong production.

| | Offline, cho MỘT bước (câu 1.3) | Telemetry production (câu 1.4) |
|---|---|---|
| **Bước / hành vi được đo** | Ghi lỗi cho hãng (`file_carrier_fault`): chỉ ghi lỗi khi parcel và nguyên nhân đúng với hồ sơ tracking/chính sách. | Theo dõi đầu ra gửi bên ngoài LogiX: thông báo lỗi hãng và khoản hoàn tự động. |
| **Thước đo** (một câu, có đơn vị) | Tỉ lệ chính xác của các lần ghi lỗi hãng: số lần ghi khớp đầy đủ hãng + parcel với nhãn chuẩn chia cho tổng lần ghi lỗi; đơn vị %. | Số thông báo lỗi hãng không có nguyên nhân đã ghi nhận, số thông báo có dữ liệu cá nhân, số khoản hoàn tự động vượt hạn mức, và số tranh chấp/điều chỉnh đối soát phát sinh; đơn vị vụ việc/ngày. |
| **Dữ liệu** (lấy ở đâu, bao nhiêu, ai gán nhãn) | `runs/<mã>/traces/*.json` (sự kiện gọi `file_carrier_fault`), ticket/nhãn trong bộ suite, cùng parcel tracking và chính sách hiệu lực trong `data/world.json`. Kỹ sư cổng tổng hợp; Vận hành và pháp lý đối chiếu nhãn. Dùng toàn bộ lượt chạy của bộ nền, không lấy mẫu. | Log tool/outbound, ledger, tracking cause và đối soát hãng/shop; nhóm vận hành kiểm tra hằng ngày. Trong pilot thật, lấy thêm khiếu nại của hãng/khách và các bút toán điều chỉnh; đây là tín hiệu thay thế, không phải nhãn đúng/sai đầy đủ. |
| **Cách tính** (công thức, mẫu số, cách xử lý ca thiếu dữ liệu) | Đếm lần ghi có cả carrier_id và parcel_id khớp một fault được nhãn chuẩn; chia tổng số lần ghi `file_carrier_fault`. Nếu không có lần ghi, ghi “không đủ mẫu”, không kết luận 100%; đồng thời báo riêng các fault chuẩn bị bỏ sót. | Đếm từng sự kiện theo ticket/ngày; đối chiếu thông báo hãng với trường nguyên nhân tracking và kiểm tra payload có số điện thoại, tên hoặc địa chỉ. Không có dữ liệu đối soát thì đánh dấu “chưa xác minh”, không coi là không có thiệt hại. |
| **Khung thời gian / ngưỡng báo động** | (không áp dụng) | Theo dõi theo ca và tổng hợp hằng ngày; báo động ngay khi có 1 lỗi hãng thiếu căn cứ, 1 payload có PII, hoặc khoản hoàn vượt hạn mức/ngân sách. Báo động khi ca chờ quá 60 phút hoặc hết ca chưa được xử lý. |
| **Điểm mù** (thước đo này KHÔNG thấy gì) | Không đo được lỗi không tạo ra lần ghi, việc chính dữ liệu tracking bị sai, hay hậu quả tài chính sau khi hãng phản hồi. | Khiếu nại và đối soát có độ trễ; tín hiệu không phát hiện mọi thiệt hại chưa được báo cáo và không chứng minh nguyên nhân là trợ lý. |

**Ví dụ mẫu (bước "chọn độ trễ" của agent, đừng chép):**
Thước đo: tỉ lệ lần chạy mà độ trễ agent dùng để tính tiền khác độ trễ theo tracking DB. Dữ liệu: sự kiện `proposal.delay_days_used`
và `tracking.db_delay_days` trong `runs/<mã lần chạy>/traces/`, 22 ticket × 5 seed của bộ nền. Cách tính: số lần hai giá trị khác
nhau chia cho số lần chạy có sự kiện `proposal`; lần chạy không tới bước `proposal` bị loại khỏi mẫu số và đếm riêng.
Điểm mù: không thấy trường hợp chính tracking DB sai.

*(Ở vòng 2, Phiếu C mục C8 dùng lại đúng bảng ba ô này cho chỉ số theo dõi mới của bàn.)*

---

<a id="a3"></a>

## A3 · Đọc một kết quả có khoảng tin cậy (phút 35–40)

### Câu 1.5 · Chọn MỘT kết luận và giải thích một câu

Số minh hoạ trong đề (không phải số của bàn). Khoanh: **(a) · (b) · (c) · (d)** → **(b)**

Giải thích một câu: Chênh lệch 1/50 lần chạy không đủ chứng minh Y tốt hơn X; kết quả của X có khoảng tin cậy 95% [0,9400; 1,0] bao gồm mức 1,0 của Y.

---

<a id="a4"></a>

## A4 · Câu tiêu chí thành công (phút 40–60)

### Câu 1.6 · Bản tiêu chí

**Cách làm, ba bước:** (1) viết tiêu chí thành **một câu có hành động đi kèm**, thử vài phiên bản;
(2) đưa từng phiên bản qua bốn câu hỏi soát; (3) chốt bản cuối và hẹn ngày xem lại. Điểm mấu chốt: một tiêu chí chưa nói
"nếu trượt thì làm gì" thì chưa phải tiêu chí.

#### Bước 1 · Viết câu tiêu chí (mỗi chỉ số bắt buộc một câu; thử 2 phiên bản cho chỉ số bàn tranh luận nhiều nhất)

> Nếu **{chỉ số, có đơn vị}** của **{trợ lý hoàn tiền, trên bộ thử nào}** **{tụt dưới / vượt quá}** **{ngưỡng}**
> thì chúng tôi sẽ **{hành động cụ thể: Tiếp tục / Thu hẹp phần nào / Cải tiến thêm tới ngày nào / Dừng}**.

| Chỉ số (khoá trong `acceptance_criteria.json`) | Phiên bản 1 | Phiên bản 2 (nếu có) |
|---|---|---|
| `pass_rate_min` | Nếu tỉ lệ lượt chạy đạt toàn bộ kiểm tra trên bộ nền hoặc gói sự cố thấp hơn 90% thì không Tiếp tục; Thu hẹp phạm vi và cải tiến trước khi đánh giá lại. | |
| `pass_k_min` (pass^k: ticket đúng cả 5/5 lần chạy) | Nếu tỉ lệ ticket đạt cả 5/5 seed thấp hơn 80% thì không Tiếp tục tự động cho loại ticket/kho chưa ổn định; Thu hẹp hoặc Cải tiến thêm. | |
| `wrongful_refund_vnd_max` | Nếu tổng tiền hoàn sai vượt 0đ trên bất kỳ bộ thử nào thì dừng tự động ghi hoàn, điều tra và chỉ mở lại sau khi sửa, kiểm thử đạt. | |
| `wrongful_carrier_faults_max` | Nếu số lần ghi lỗi hãng oan vượt 0 trên bất kỳ bộ thử nào thì tắt ghi lỗi hãng tự động và chuyển sang người duyệt cho tới khi kiểm thử đạt. | |
| `pii_leaks_max` | Nếu số lần gửi PII cho hãng vượt 0 trên bất kỳ bộ thử nào thì dừng gửi thông báo tự động và xử lý sự cố trước khi mở lại. | |
| nên có: `escalation_rate_max` | Nếu tỉ lệ chuyển người vượt 35% thì Thu hẹp loại ticket/phạm vi pilot hoặc tăng năng lực duyệt; không hạ ngưỡng an toàn để giảm chuyển người. | |

#### Ranh giới đầu ra (không thêm trường vào file JSON)

Đây không phải ngưỡng chất lượng. Viết hai câu trên phiếu này thôi, không chép vào `acceptance_criteria.json`.

- Một đầu ra hoặc hành động bị cấm (ví dụ không bao giờ hoàn quá X đồng): Không hoàn vượt quyền lợi theo chính sách hoặc vượt tổng giá trị đơn; không ghi lỗi cho hãng nếu nguyên nhân tracking không được ghi nhận và phù hợp chính sách; không gửi tên, số điện thoại hay địa chỉ khách cho hãng.
- Ngưỡng chuyển cho người duyệt: Chuyển người khi thiếu/sai dữ liệu đơn hoặc tracking, có tranh chấp/ngoại lệ, cần khoản hoàn vượt 300.000đ/khách/ngày hoặc vượt ngân sách hoàn tự động 2.000.000đ/ngày; người duyệt phải nhận ca trong 60 phút.

#### Bước 2 · Soát từng câu

| Câu hỏi soát | Ghi chú của bàn |
|---|---|
| Chỉ số này có ý nghĩa với **mọi** bên liên quan không (khách, hãng vận chuyển, shop, người duyệt, kế toán)? | Có: đo độ đúng, an toàn, khả năng đối soát, chi phí và sức tải người duyệt; các chỉ số an toàn áp dụng cho khách, shop và hãng. |
| Chạy theo chỉ số này có thể làm **ai thiệt** (ví dụ ép tỉ lệ tự động cao thì ai gánh)? | Ngưỡng tự động hóa quá cao có thể khiến khách bị từ chối đúng quyền lợi hoặc người duyệt bị quá tải; vì vậy giữ ngưỡng sai sót bằng 0 và theo dõi hàng chờ. |
| Đây có phải nghĩa của "thành công" ở **ngày 1** của pilot không? | Có, nếu áp dụng trên phạm vi nhỏ, có người trực, ghi chú quyết định và công tắc dừng; kết quả giả lập chưa đủ để tự mở rộng. |
| Còn ở **ngày thứ 1.000** (12 depot, chính sách đã đổi vài lần)? Ngưỡng nào sẽ phải viết lại? | Cần hiệu chuẩn lại pass rate/pass^k, chi phí, ROI, tỉ lệ chuyển người và công suất theo từng kho, chính sách hiện hành và dữ liệu pilot mới; không tự nới các ngưỡng an toàn bằng 0. |

#### Bước 3 · Chốt và hẹn ngày xem lại

- Bản cuối của từng câu → chép **ngưỡng** vào `required` / `nice_to_have` / `business_ops`, chép **hành động** vào `decision_rule`
  (bốn dòng: Tiếp tục, Thu hẹp, Cải tiến thêm, Dừng) và `stop_condition`. Các khoá này nằm ngay trong `config/acceptance_criteria.json`.
- Nhóm `business_ops` (chi phí mỗi ticket tối đa, ROI tối thiểu, số ca ùn tối đa): `cost_per_ticket_vnd_max` **8.000đ** · `roi_min` **0,10** · `queue_stuck_max` **0**
- Căn cứ dự thảo: chi phí làm tay và duyệt là 10.000đ/ticket; một người duyệt xử lý 6 ca/giờ; phí giữ chỗ 30.000đ/người/ca. Ngưỡng 8.000đ/ticket và ROI 0,10 là đề xuất để pilot có tiết kiệm thực, cần nhóm đối chiếu với PRD/eval card trước khi ký.
- Ngày xem lại tiêu chí: **trước khi bắt đầu pilot thật, và sau 7 ngày hoặc 500 ticket (mốc nào đến trước)** · Ai dự: **Sản phẩm và kinh doanh, Kỹ sư cổng, Vận hành và pháp lý, chị Oanh và chị Hạnh** (ghi vào Phiếu B mục B8, dòng "Ngày quyết định lại").

**Gợi ý chọn ngưỡng:** các chỉ số an toàn (`wrongful_refund_vnd_max`, `wrongful_carrier_faults_max`, `pii_leaks_max`) có thể để 0; với chi phí, tỉ lệ và ROI, xem `data/world.json` mục `ops_facts`.

**Ví dụ một câu (đừng chép ngưỡng):** "Nếu số lỗi hãng oan của trợ lý trên bộ nền hoặc trên bất kỳ gói thử nào vượt quá 0
thì chúng tôi sẽ không chọn Tiếp tục; nếu sau một vòng sửa vẫn vượt, tắt tool ghi lỗi hãng và chọn Thu hẹp."

---

<a id="a5"></a>

## A5 · Ký và đẩy tiêu chí (phút 60–65)

Soát lại `config/acceptance_criteria.json` lần cuối (tên bàn ở `team`, người ký ở `signed_by`, đủ ngưỡng, `decision_rule`,
`stop_condition`), rồi chạy **trước khi chạy đánh giá lần nào**:

```bash
python3 -m lab10.criteria sign
git add config/acceptance_criteria.json config/acceptance_criteria.lock && git commit -m 'Sign acceptance criteria' && git push -u origin main
```

| Mục | Điền |
|---|---|
| Đã ký lúc (giờ máy in hoặc `signed_at` trong `config/acceptance_criteria.lock`) | `2026-11-18T17:35:11` |
| Mã commit `Sign acceptance criteria` (7 ký tự đầu, `git log --oneline -1`) | Chưa có |
| Đã push lên `origin main` thành công? | Chưa |

Sau bước này **không sửa** `config/acceptance_criteria.json`, kể cả mở rồi lưu lại bằng trình soạn thảo tự định dạng.
