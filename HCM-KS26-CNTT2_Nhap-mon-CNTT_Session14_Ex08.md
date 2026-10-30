Phần 1: Phân tích lỗ hổng bảo mật

1\. Xác định nguồn gốc rò rỉ dữ liệu khách hàng:

\-Dòng log gây rò rỉ trực tiếp: Dòng 1 (08:15 T2).



\-Chi tiết file: campuspay\_beta\_users.xlsx (chứa dữ liệu nhạy cảm gồm họ tên, số điện thoại của người tham gia).



Tổ hợp quyền \& phạm vi: Editor + Anyone with the link.



Cơ chế rò rỉ:



Phạm vi "Anyone with the link" biến tệp dữ liệu thành một tài nguyên công khai trên Internet, bất kỳ ai có đường dẫn URL đều có thể truy cập mà không cần xác thực tài khoản danh tính



Quyền "Editor" cho phép bất kỳ ai có đường dẫn không chỉ xem hoặc tải xuống, mà còn có toàn quyền chỉnh sửa, đổi tên, hoặc tiếp tục chia sẻ đường dẫn đó lên các nền tảng mạng xã hội công khai



2.Ba giả thuyết người lạ truy cập:

\-Email B chia sẻ tiếp (Resharing) hoặc cấu hình lại quyền:

Tài khoản Email B sau khi nhận quyền truy cập có thể đã vô tình hoặc chủ ý tạo một liên kết chia sẻ mới (hoặc thay đổi phạm vi thành công khai/cho phép người khác cùng xem), hoặc trực tiếp thêm địa chỉ email của người lạ vào danh sách chia sẻ của tệp.

\-Tài khoản của Email B bị xâm nhập (Account Compromise):

tài khoản Email B bị hack (ví dụ thông qua mật khẩu yếu, lộ thông tin đăng nhập). Do tài khoản đang được cấp quyền truy cập campuspay\_api\_keys.txt, kẻ tấn công đã dùng chính quyền hợp lệ đó để đột nhập vào tệp chứa khóa API.



\-Thay đổi quyền từ chủ sở hữu/Quản trị viên (Privilege Escalation / Admin Error):

Sau mốc 09:30 T2, Trưởng nhóm hoặc một thành viên có quyền quản trị cao hơn đã chỉnh sửa lại phạm vi chia sẻ của tệp campuspay\_api\_keys.txt (chuyển từ Restricted sang Anyone with the link hoặc thêm trực tiếp email người lạ vào danh sách được phép xem) trong quá trình bàn giao công việc mà quên kiểm tra lại tính bảo mật.



Phần 2:

3\.Viết lại bảng cấu hình quyền



|Tên file|Quyền đúng|Phạm vi đúng|
|-|-|-|
|campuspay\_beta\_users.xlsx|Viewer (hoặc Restricted hẳn nếu chỉ dev test)|Restricted (Chỉ chia sẻ nội bộ nhóm test/phát triển)|
|campuspay\_api\_keys.txt|Viewer|Restricted (Chỉ email dev/backend cụ thể)|
|slide\_pitch\_v8.pptx|Commenter hoặc Viewer|Anyone with the link với giới hạn quyền Commenter hoặc Viewer|



Giải thích ngắn gọn việc chọn:

\-campuspay\_beta\_users.xlsx chứa thông tin cá nhân,SĐT không dùng Anyone with the link \& Editor để tránh bị sửa đổi hoặc phát tán công khai

\-campuspay\_api\_keys.txt chứa khóa API hệ thống bảo mật. Chỉ cho phép các thành viên kỹ thuật trực tiếp sử dụng xem,không cấp quyền chỉnh sửa hay chia sẻ rộng rãi cho người ngoài.

\-slide\_pitch\_v8.pptx không chứa dữ liệu nhạy cảm cá nhân hay tài chính,gửi cho người chấm/người ngoài xem xét góp ý



4.Vô hiệu hóa link cũ \& Xử lý thời hạn (Expiration Date):

\-Tắt tính năng chia sẻ bằng đường liên kết (Turn off link sharing / Change access to Restricted) hoặc sử dụng tùy chọn "Reset link" / "Change link settings" trong phần quản lý chia sẻ nâng cao của tệp.

thao tác nhấn chuột phải vào file -> Share (Chia sẻ) -> Tại mục General access, chuyển từ Anyone with the link về Restricted (Hạn chế)

\-Nên dùng tính năng 'Set Expiration Date' khi

Làm việc với cộng tác viên hoặc bên thứ ba thời vụ

Dự án hoặc tài liệu có mốc thời gian kết thúc rõ ràng tránh việc quên thu hồi quyền sau khi sự kiện/vòng test kết thúc, tự động hóa khâu bảo mật và giảm thiểu rủi ro quên sót phân quyền thủ công

