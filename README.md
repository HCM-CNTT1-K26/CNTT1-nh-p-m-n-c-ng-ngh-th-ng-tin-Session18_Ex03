# CNTT1-nh-p-m-n-c-ng-ngh-th-ng-tin-Session18_Ex03

### Phần 1. Định vị phần tử & Phân tích chi tiết Box Model

1. **Thao tác định vị (Chức năng 1):**
* Mở giao diện "Theo dõi lộ trình xe" trên trình duyệt (Chrome/Edge/CocCoc).
* Nhấn phím `F12` hoặc nhấn chuột phải vào khung **"Thông tin tài xế"** đang bị đè $\rightarrow$ Chọn **Inspect** (Kiểm tra).
* Quan sát cửa sổ **Elements** (cột bên trái của DevTools), tìm đến thẻ HTML đang được bôi sáng (thường là một thẻ `<div class="driver-info">`, `<div class="card">` hoặc `<section>`).


2. **Đọc sơ đồ Box Model chi tiết (Chức năng 2 - Tab Computed):**
* Ở cột bên phải của DevTools, chuyển sang tab **Computed** (nằm cạnh tab Styles).
* Cuộn xuống dưới cùng để nhìn thấy sơ đồ **Box Model** trực quan gồm 4 lớp từ trong ra ngoài:
* **Content:** Kích thước thực tế của vùng chứa chữ/ảnh (ví dụ: `300px × 150px`). Nếu ở đây có các thuộc tính tọa độ như `position: absolute` hoặc `top: 0px; left: 0px;`, đây chính là nguyên nhân làm nó bay ra khỏi luồng chuẩn và đè lên khung "Danh sách trạm".
* **Padding:** Khoảng đệm bên trong. Nếu giá trị là `0px`, chữ và nội dung tài xế sẽ dính sát mép viền trông rất lộn xộn.
* **Border:** Viền khung. Thường bị thiếu hoặc bằng `0px`, khiến ranh giới giữa hai khung bị nhòa.
* **Margin:** Khoảng cách bên ngoài. Nếu giá trị `margin-bottom` bằng `0px` hoặc nhận số âm, khung phía dưới ("Danh sách trạm") sẽ bị kéo ngược lên trên và bị đè lấp.





---

### Phần 2. Triển khai thông số CSS tối ưu (Tab Styles)

Sau khi đã xác định được thẻ HTML lỗi, quay lại tab **Styles** (cạnh tab Computed) để thêm trực tiếp các dòng lệnh sửa lỗi mà không cần đụng đến mã nguồn gốc:

1. **Thêm thuộc tính cố định vị trí (Fix Normal Flow):**
  css
position: relative !important;
top: auto !important;
left: auto !important;




*Giải thích:* Đưa phần tử trở lại luồng sắp xếp tự nhiên của trang web thay vì trôi nổi tự do (`absolute`), giúp nó xếp tuần tự phía trên khung "Danh sách trạm".
2. **Chuẩn hóa hộp kỹ thuật (Box-Sizing):**
  css
box-sizing: border-box;
width: 100%;



*Giải thích:* Đảm bảo khi bạn tăng `padding` hay `border`, tổng chiều rộng của khung không bị phình to vượt quá màn hình gây vỡ giao diện ngang.
3. **Tách biệt các lớp Content, Padding, Border, Margin:**
  css
/* Lớp Padding: Tạo khoảng thở bên trong cho nội dung tài xế dễ nhìn */
padding: 16px;

/* Lớp Border: Tạo đường viền mảnh phân tách rõ ràng */
border: 1px solid #dcdcdc;
border-radius: 8px;

/* Lớp Margin: Đẩy khung "Danh sách trạm" xuống dưới một khoảng an toàn */
margin-bottom: 24px !important;


### Phần 3. Kiểm thử sâu (Xử lý khi thay đổi Margin nhưng phần tử không dịch chuyển)

Nếu bạn đã gõ `margin-bottom` nhưng khung giao diện vẫn đứng yên hoặc tiếp tục đè lên nhau, hãy kiểm tra lần lượt 4 thủ phạm chính ở tầng Browser Engine:

1. **Bị ghi đè bởi quy tắc độ ưu tiên (Specificity):**
* Nhìn vào tab **Styles**, nếu dòng `margin-bottom` bạn vừa gõ bị **gạch ngang**, có nghĩa là một đoạn CSS khác của hệ thống có độ ưu tiên mạnh hơn (hoặc có chữ `!important`) đang lấn át.
* *Khắc phục:* Thêm chữ `!important` vào ngay sau giá trị (ví dụ: `margin-bottom: 24px !important;`).


2. **Bị ảnh hưởng bởi Layout cha (Flexbox hoặc Grid):**
* Nếu thẻ chứa bên ngoài (Parent Container) đang sử dụng `display: flex` hoặc `display: grid`, các thuộc tính `margin` thông thường giữa các phần tử con có thể bị vô hiệu hóa hoặc bị chi phối bởi khoảng cách chung.
* *Khắc phục:* Chọn thẻ cha ở cây DOM Tree, tại tab Styles tìm thuộc tính `gap` và đặt lại khoảng cách (ví dụ: `gap: 16px;` hoặc `row-gap: 16px;`).


3. **Bị dịch chuyển hệ tọa độ bởi `transform`:**
* Nếu trước đó lập trình viên dùng hiệu ứng chuyển động hoặc căn chỉnh bằng `transform: translateY(...)`, phần tử sẽ dịch chuyển hiển thị về mặt thị giác nhưng **vị trí thật** trong DOM vẫn giữ nguyên, khiến các phần tử xung quanh không nhận diện được khoảng cách.
* *Khắc phục:* Tìm và tắt (bỏ dấu tích) dòng `transform` trong tab Styles.
4. **Hiện tượng sụp đổ biên (Margin Collapsing):**
* Nếu hai khung nằm tiếp giáp nhau theo chiều dọc mà không có `border` hoặc `padding` ở giữa, đôi khi `margin` của chúng sẽ bị gộp làm một thay vì cộng dồn.
* *Khắc phục:* Đảm bảo khung chứa đã được thiết lập `border` hoặc `padding` như cấu hình ở Phần 2 để ngắt hiện tượng sụp đổ biên.
