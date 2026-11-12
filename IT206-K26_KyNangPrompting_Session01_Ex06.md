**BÀI LÀM: PHÂN TÍCH CƠ CHẾ TRÀN CỐC NGỮ CẢNH (TRUNCATION) TRONG LUỒNG TƯ VẤN CSKH**

**Phần 1: Giải thích hình tượng "Chiếc cốc nước" và nguyên nhân AI mất trí nhớ**
Để hiểu lý do tại sao AI đột nhiên quên thông tin khách hàng ở lượt chat 42, chúng ta có thể hình dung bộ nhớ ngắn hạn (Context Window / RAM) của mô hình AI như một **"chiếc cốc nước" có dung tích cố định** (ví dụ: giới hạn 32.000 tokens). Chiếc cốc này không thể tự động phình to ra.
*   **Quá trình rót nước (Tích lũy Token):** Mỗi tin nhắn khách hàng gửi đến và mỗi phản hồi của AI sinh ra đều giống như việc bạn đang rót thêm nước vào cốc.
*   **Lượt 1 (Đáy cốc):** Khi khách hàng cung cấp thông tin "Trần Văn B, mã đơn #MTX-9988", lượng thông tin này được rót vào đầu tiên và nằm yên dưới đáy cốc.
*   **Quá trình làm đầy:** Trong suốt 40 lượt chat tiếp theo, hai bên trao đổi dông dài về chi tiết các mặt hàng rau củ. Lượng từ vựng sinh ra quá lớn, liên tục được rót thêm vào khiến chiếc cốc dần bị lấp đầy tới mức 100%.
*   **Hiện tượng Tràn cốc (Truncation):** Ở lượt 42, khách hàng tiếp tục đặt câu hỏi mới. Để có không gian chứa lượng "nước mới" này, cơ chế mặc định của hệ thống AI là phải **gạt bỏ lớp "nước cũ" nhất nằm ở dưới đáy cốc**. 
*   **Kết quả:** Dữ liệu của Lượt 1 bị đẩy văng hoàn toàn ra khỏi bộ nhớ. Do không còn dữ kiện về định danh khách hàng, AI rơi vào trạng thái ảo giác (Hallucination), tự bịa ra cái tên "chị Mai" và buộc phải xin lại mã đơn.

**Phần 2: Phương án quản lý RAM bộ nhớ giúp tránh "Tràn cốc"**

**1. Phương án Thao tác người dùng (Dành cho Nhân viên CSKH / Người dùng trực tiếp):**
*   **Chủ động nhắc lại ngữ cảnh (Context Reinforcement):** Thay vì hỏi những câu cụt lủn ở cuối luồng chat dài, nhân viên cần mớm lại thông tin cốt lõi vào câu hỏi mới. 
    *   *Nên hỏi:* "Quay lại với đơn hàng #MTX-9988 của anh Trần Văn B, việc hoàn tiền cho đơn này có được miễn phí không?".
*   **Mở phiên chat mới (Tạo "cốc nước" mới):** Khi luồng tư vấn chuyển sang chủ đề khác, nên tóm tắt lại các thông tin quan trọng nhất, sau đó mở luồng New Chat.

**2. Phương án Thiết kế hệ thống (Dành cho Kỹ sư / Đội ngũ tích hợp AI):**
*   **Lưu trữ biến số tĩnh (State Variables) & Tiêm Prompt Hệ thống (System Prompt Injection):** Hệ thống tự động trích xuất các Thực thể cốt lõi (Core Entities) như Tên: Trần Văn B, Mã đơn: MTX-9988 ngay từ đầu.
*   Các thông tin này sẽ được tự động **"tiêm" ẩn vào phần System Instructions** ở mọi lượt gọi API. AI sẽ luôn luôn "nhớ" khách hàng là ai bất chấp đoạn hội thoại kéo dài.
