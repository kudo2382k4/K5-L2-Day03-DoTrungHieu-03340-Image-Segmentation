# Báo cáo thực hành Day 3

- Mã học viên theo lớp: 03340
- Ngày / CVAT local: 12/11/2026 / CVAT local
- Công cụ đã dùng: CVAT local (Polygon [BẠN ĐIỀN: thêm "và Brush" nếu có dùng]); tự chạy `scoring/score.py` trên máy sau khi coach phát ground truth. [không dùng SAM/Intelligent Scissors]

## 1. Bài đã nộp

| Task | File ZIP đúng tên | Hoàn thành mấy ảnh | Điểm tối đa (coach chấm sau) |
| --- | --- | ---: | ---: |
| easy_semantic | `submissions/easy_semantic.zip` | 3 / 3 | 20 |
| medium_instance | `submissions/medium_instance.zip` | 3 / 3 | 32 |
| hard_panoptic | `submissions/hard_panoptic.zip` | 2 / 2 | 30 |
| cp1_holes | chưa có | 0 / 1 | 3 |
| cp2_slice | chưa có | 0 / 1 | 3 |
| cp5_occlusion | chưa có | 0 / 1 | 3 |
| cp3_thin | chưa có | 0 / 1 | 3 |
| cp4_curb | chưa có | 0 / 1 | 3 |
| cp6_coverage | chưa có | 0 / 1 | 3 |
| **Tổng tối đa** | | | **100** |

Ba task chính đã Save và export đúng format (Easy: Segmentation mask 1.1; Medium và Hard: COCO 1.0). Sáu checkpoint chưa làm do hết thời gian. Bản `medium_instance.zip` trong `submissions/` là bản đã bổ sung sau khi xem reference (xem mục 3); bản đầu tiên là bản tự làm trước khi xem đáp án.

## 2. Một quyết định trước khi dùng gợi ý

- Ảnh, vị trí và object Medium đầu tiên tự vẽ: [ `000000458325.jpg`, người phụ nữ mặc áo dài ở giữa ảnh]
- Class và quy tắc tôi dùng để chọn biên: [ `person`, chỉ vẽ phần nhìn thấy, dừng ở mép xe máy che chân, không đoán phần khuất]
- Nếu dùng gợi ý sau đó: [BẠN ĐIỀN, hoặc ghi "không dùng"]

## 3. Một lỗi tôi tìm thấy và sửa

- Task/ảnh/vùng: `medium_instance`, cả ba ảnh (`000000181542`, `000000373353`, `000000458325`), chủ yếu các `person` và `car` nhỏ ở xa hoặc bị che.
- Lỗi thuộc loại: thiếu vật (bỏ sót instance).
- Bằng chứng tôi nhìn thấy: ở bản đầu, tôi chủ động bỏ qua vật nhỏ và mờ ở nền ảnh vì nghĩ không cần thiết. Sau khi coach phát ground truth và tôi chạy `score.py`, kết quả là nộp 39 instance so với 71 trong reference, R@0.5 = 0.46, mean matched IoU = 0.759, điểm Medium = 0.0 / 32. Chất lượng biên của các mask đã vẽ khá tốt, vấn đề nằm ở số vật bị thiếu.
- Quy tắc và hành động sửa: mỗi người/xe nhìn thấy là một instance riêng, kể cả vật nhỏ. Tôi mở lại 3 ảnh trong CVAT, tự nhìn ảnh và vẽ bổ sung các person/car nhỏ còn thiếu, không chép biên từ reference.
- Sau sửa đã Save và export lại chưa? Đã Save, export COCO 1.0 lại và thay `submissions/medium_instance.zip`.

Kết quả tự chấm (làm **sau khi xem reference**, không phải bằng chứng làm độc lập): Medium từ metric 0.353 (0.0 / 32) lên 0.566 (11.8 / 32). Scorecard ba tier hiện là 45.8 / 82 (Easy 17.7 / 20, Medium 11.8 / 32, Hard 16.3 / 30); đây không phải điểm cuối trên 100.

## 4. Ba ca chưa chắc hoặc đã cân nhắc

| Ảnh/vị trí | Hai cách hiểu có thể | Quy tắc/chứng cứ | Quyết định hoặc câu hỏi cho coach |
| --- | --- | --- | --- |
| Easy, ảnh cao tốc `7ee6d192-89e2408b.jpg`, hai sườn đồi hai bên đường | Đồi cỏ khô màu nâu là `vegetation`, hoặc là đất trống không thuộc class nào | Task chỉ có 5 class, không có `terrain`; ảnh không có ranh rõ giữa cỏ khô và đất | Tôi chỉ tô các dải cây bụi xanh sẫm là `vegetation` và để phần đồi nâu trống. Câu hỏi cho coach: reference có tính cỏ khô là `vegetation` không? |
| Medium, vật nhỏ và mờ ở nền ảnh (người, xe ở xa) | Bỏ qua vì quá nhỏ, hoặc vẽ thành instance riêng | Hướng dẫn: mỗi vật nhìn thấy là một instance; không ép đoán phần không thấy rõ | Ban đầu tôi bỏ qua, điểm Medium bằng 0 (R@0.5 = 0.46). Sau đó tôi vẽ bổ sung các vật còn nhìn thấy được, điểm lên 11.8 / 32. 
| Easy, ảnh phố có cây trụi lá, polygon `vegetation` ở góc trái trên | Cả tán cây (kể cả khoảng trời giữa các cành) là `vegetation`, hoặc chỉ phần cành và lá, khoét khoảng trời là `sky` | Cây trụi lá rất thưa nên polygon thô phủ cả pixel trời; vẽ sát từng cành là không khả thi trong thời gian | ["giữ polygon thô"] |
