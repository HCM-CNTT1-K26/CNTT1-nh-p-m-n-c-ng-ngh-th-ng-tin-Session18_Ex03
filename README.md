# CNTT1-nh-p-m-n-c-ng-ngh-th-ng-tin-Session18_Ex03
### Phần 1. Phân tích Box Model (Tab Computed)

Khi Inspect thẻ khung "Thông tin tài xế" và kiểm tra ở tab Computed, các thông số Box Model đang bị lệch chuẩn như sau:

Lớp **Content** hiện có kích thước khoảng 300px × 150px nhưng bị đè thuộc tính  position: absolute  (hoặc fixed), khiến phần tử bị rút khỏi luồng hiển thị tự nhiên của trang web và đè trực tiếp lên khung "Danh sách trạm". Lớp **Padding** và **Border** đều đang bằng 0px nên nội dung dính sát mép và không có đường viền phân tách. Lớp **Margin** bên ngoài cũng bằng 0px, dẫn đến việc không có khoảng trống cách biệt với khung bên dưới.

**Nguyên nhân chính:** Thẻ bị tràn vị trí do thuộc tính định vị tĩnh/tuyệt đối không chuẩn, kết hợp với việc thiếu  margin-bottom  và chưa thiết lập box-sizing: border-box.

---

### Phần 2. Chỉnh CSS trên Tab Styles

Inspect thẻ lỗi, chèn đoạn mã này vào tab Styles:

css


.driver-info {

  /* Trả về luồng hiển thị thường */
  
  position: relative !important;
  
  top: auto !important;

  /* Căn chỉnh lại 4 lớp Box Model */
  
  box-sizing: border-box;
  
  padding: 12px;
  
  border: 1px solid #ddd;
  
  margin-bottom: 20px !important;
  
}


### Phần 3. Kiểm thử lỗi Margin không chạy

Nếu chỉnh margin mà khung dưới vẫn đứng yên, check 3 chỗ này ở tab Styles:

1. **Bị gạch ngang (CSS bị đè):** Mã gốc có độ ưu tiên cao hơn hoặc dính !important. Fix: Thêm !important vào sau margin.
2. **Khung cha dùng Flex/Grid:** Thẻ cha gom 2 khung này đang bật display: flex hoặc grid. Fix: Đổi sang dùng gap: 20px ở thẻ cha.
3. **Bị dịch tọa độ bằng transform:** Thẻ dính transform: translateY(...). Fix: Bỏ tích chọn thuộc tính transform là xong.
