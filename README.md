# **Phần 1: Thiết kế Prompt 3 Thành phần Chuẩn hóa**
 - **1.** Phân tích nguyên nhân vì sao câu hỏi *'AI ơi phần mềm trạm sạc của tôi bị hỏng rồi'* lại khiến AI đưa ra câu trả lời sai lệch (hiện tượng AI Hallucination)?
  + thiếu ngữ cảnh : AI không biết bạn đang ở hệ điều hành và đang làm gì
  + thiếu chi tiết lỗi : không đưa ra các thông số lỗi
  + yêu cầu : không đưa rõ yêu cầu cho AI nên AI sẽ tự lắp đầy khoảng trống thông tin . Đưa ra thông tin mơ hồ AI sẽ đưa ra giải pháp về sự cố vật lý hoặc nguồn điện phần cứng thay vì lỗi phần mềm
 - **2.** Soạn thảo một câu Prompt 3 Thành phần hoàn chỉnh, chuyên nghiệp dựa trên đoạn nhật ký lỗi trên để gửi cho Trợ lý AI (phân tách rõ 3 phần: [Context], [Raw Error], [Instruction]).
  + Context : tôi là nhân viên vận hành hệ thống trạm sạc voltcharge tại bến xe miền đông . trụ sạc nhanh đột ngột ngừng cung cấp điện cho một chiếc xe taxi điện và terminal bật ra một chuỗi thông báo lỗi tiếng anh phức tạp
  + Raw Error :
    <table style="min-width: 25px;">
<colgroup><col style="min-width: 25px;"></colgroup><tbody><tr><td colspan="1" rowspan="1"><p>[TERMINAL ERROR LOG - VOLTCHARGE CHARGER #04]<br>2026-09-21 21:15:30 [FATAL] ControllerDaemon failed to start.<br>System.UnauthorizedAccessException: Access to the path 'C:\Program Files\VoltCharge\data\session.log' is denied.<br>&nbsp; at <a target="_blank" rel="noopener noreferrer nofollow" class="text-blue-500 hover:underline cursor-pointer" href="http://System.IO">System.IO</a>.FileStream.ValidateFileHandle(SafeFileHandle handle)<br>&nbsp; at VoltCharge.Core.SessionManager.Initialize()<br>InnerException: None. Process terminated with exit code 5 (0x00000005).<br>Suggestion: Run the application with elevated administrator privileges or verify NTFS file permissions.</p></td></tr></tbody>
</table>
  + Instruction : 
   * dịch và giải thích trên các dòng bị lỗi sang tiếng việt dễ hiểu đơn giản 
   * cung cấp quy trình 3 bước xử lý cụ thể trên hệ điều hành window
   * không đưa ra các câu lệnh xóa hết dữ liệu

# **Phần 2: Giải mã Lỗi & Kịch bản Khắc phục**

- **3.** Đóng vai Trợ lý AI, hãy cung cấp bản dịch và giải thích nguyên nhân gốc rễ của lỗi `UnauthorizedAccessException: Access to the path ... is denied` bằng tiếng Việt đời thường để một bạn kỹ thuật viên mới cũng có thể hiểu ngay. 
 + thông báo lỗi : C:\program\Files\Voltcharge\data\session.log
 + giải thích nguyên nhân gốc rễ : người dùng chưa đủ quyền quản trị viên để truy cập nên hệ thống sẽ chặn và không cho ghi file

- **4.** Đưa ra 3 bước xử lý cụ thể tại chỗ trên Windows 11 để kích hoạt quyền Quản trị viên (Run as Administrator) và phân quyền ghi file cho thư mục `data`, giúp trạm sạc hoạt động trở lại bình thường.
 + bước 1 : kích hoạt run as administrator
   * tìm file controllerdaemon.exe 
   * ấn chuột phải vào file và chọn run as administrator
   * nếu bảng hỏi user account control hiện lên thì chọn yes
 + Bước 2 : cấu hình tự động chạy quyền admin lâu dài
   * ấn chuột phải vào file controllerdaemon.exe -> properties
   * chuyển sang compatibility
   * chọn run this program as an administrator
   * nhấp apply -> ok
 - **5.** Bẫy an ninh: Giả sử trong đoạn mã lỗi có chứa chuỗi mã bí mật `API_KEY: volt_secret_token_889922`, kỹ thuật viên phải xử lý chuỗi này như thế nào trước khi dán vào khung chat của ChatGPT/Gemini? Giải thích lý do.
   Cách xử lý: Kỹ thuật viên phải xóa bỏ hoặc che (mask/redact) chuỗi API_KEY: volt_secret_token_889922 thành dạng giả lập (ví dụ: API_KEY: [REDACTED_API_KEY] hoặc API_KEY: xxx_secret_xxx) trước khi copy-paste vào khung chat AI.

Lý do bắt buộc:

Rủi ro lộ bảo mật: Dữ liệu gửi lên các nền tảng AI công cộng có thể được lưu trữ trên server bên thứ ba hoặc dùng để huấn luyện mô hình (training data).

Tránh bị tấn công: Kẻ xấu có thể lợi dụng API Key này để truy cập trái phép vào hệ thống VoltCharge, can thiệp luồng điện, lấy cắp dữ liệu khách hàng hoặc gây tổn thất tài chính cho trạm sạc.
  

