# Team

## Thông tin nhóm

- Tên nhóm (`TenNhom`, không dấu, không khoảng trắng): ABC
- Lớp: `L3A`
- Repo của bàn: `https://github.com/khoii3110/K5-L3A-DAY10-ABC-PoCtoPilotGate`
- Thẻ sự cố đã bốc (S1–S8): `S1`

## Thành viên

Họ tên đầy đủ và MSSV của **mọi** thành viên; thêm dòng nếu cần. Vai: Kỹ sư cổng · Sản phẩm và kinh doanh · Vận hành và pháp lý (README mục 3, phút 0–15).

| STT | Họ và tên | MSSV | Vai | Trưởng nhóm |
| --: | --------- | ---- | --- | :---------: |
|   1 | Nguyễn Minh Khôi | 2A202603528 | Trưởng nhóm; Vận hành và pháp lý (PIC) | Có |
|   2 | Nguyễn Văn Đăng | 2A202603371 | Kỹ sư cổng; Sản phẩm và kinh doanh | |
|   3 | | | | |
|   4 | | | | |

Ghi `members` trong `submission/submission.json` theo dạng `"HoVaTen-MSSV"`, khớp bảng trên.

## Cá nhân

Mỗi thành viên tự viết một mục bên dưới (chép mẫu, đổi tiêu đề thành họ tên không dấu và MSSV, ví dụ `## NguyenVanAn-20241234`).

## NguyenMinhKhoi-2A202603528

### Phần việc phụ trách

Trưởng nhóm và PIC; đã xác nhận quyết định thu hẹp phạm vi để loại ticket không xác định kho cho tới khi xác minh, và xác nhận thẻ đã bốc là S1.

### Đóng góp cụ thể

Đã cung cấp thông tin nhóm/PIC và chốt quyết định v1 trong trao đổi. Có sử dụng Copilot AI để hỗ trợ rà soát hướng dẫn, sửa cổng phạm vi, cập nhật Phiếu B và chạy kiểm thử; cần tự rà soát, hiểu và xác nhận các thay đổi trước khi nộp.

### Bằng chứng

`R-base-after-20261118-181755-954705`; Phiếu B mục B7.4/B8.

### Exit ticket



## NguyenVanDang-2A202603371

### Phần việc phụ trách

Kỹ sư cổng kiêm Sản phẩm và kinh doanh: đối chiếu đề xuất hoàn/ghi lỗi hãng với nguồn dữ liệu, kiểm tra chi phí và phạm vi pilot theo tiêu chí đã ký. Vai vận hành và pháp lý do Nguyễn Minh Khôi giữ.

### Đóng góp cụ thể

Rà soát cổng sau bản chốt vòng 1 và bổ sung kiểm tra chung trong `lab10/guards.py`: mã kiện yêu cầu phải thuộc đơn; đơn đã có khoản hoàn trong sổ phải chuyển người đối soát; lệnh ghi lỗi hãng phải khớp kiện, hãng và tài xế trong hồ sơ tracking. Đối chiếu `T-002@55` và chạy lại bộ nền sau sửa; 13 test công khai đạt, 105/105 lượt trong phạm vi đạt, không phát sinh hoàn sai, lỗi hãng oan hoặc lộ PII. Rà soát phần kinh doanh: phạm vi hiện loại kho chưa xác định, chi phí 4.231đ/ticket và ROI 1,36 trên bộ nền sau sửa. Có dùng ChatGPT hỗ trợ rà soát code, diễn giải và kiểm thử; Đăng cần tự kiểm tra, hiểu và xác nhận nội dung trước khi nộp.

### Bằng chứng

`lab10/guards.py` (`write_gate`); trace `T-002@55`; `R-base-after-20261118-192901-38b3be`; `python -m unittest discover -s tests` (13/13 OK). Tiêu chí ký ở commit `e78c419` và bản chốt vòng 1 giữ nguyên.

### Exit ticket

Ở `T-002@55`, lời khách nói trễ 5 ngày nhưng tracking chỉ có 2 ngày; đề xuất hoàn 20.000đ được chuyển người vì quyền lợi tính từ dữ liệu gốc là 0đ. Kiểm tra trước lệnh ghi bảo vệ thêm cả trường hợp model chọn sai kiện/hãng hoặc hoàn lặp, nhưng vẫn phụ thuộc độ đúng và độ mới của tracking/sổ. Trước pilot thật cần đo sai lệch tracking, khoản hoàn lặp, chi phí và hàng chờ theo từng kho; khi có số vượt ngưỡng đã ký thì thu hẹp hoặc dừng thao tác tương ứng.
