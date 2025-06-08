# Ứng dụng Mạng Học Sâu trong Hỗ trợ Giới thiệu các Tác phẩm Hội họa

**Khóa luận tốt nghiệp đại học hệ chính quy Ngành Khoa học dữ liệu**  
**Trường Đại học Khoa học Tự nhiên Hà Nội, Đại học Quốc gia Hà Nội**

**Tác giả:** Phạm Hùng  
**Cán bộ hướng dẫn:** TS. Cao Văn Chung  
**Thời gian thực hiện:** Hà Nội - 2025  

**Lưu ý:** Mã nguồn triển khai các mô hình sinh mô tả **EffB0-Trans** và **YOLO-Trans** *không được công khai* trong repository này

## 1. Giới thiệu

Hội họa là một di sản văn hóa quan trọng, và việc diễn giải tác phẩm giúp người xem hiểu sâu sắc ý nghĩa của chúng. Tuy nhiên, với số lượng lớn các tác phẩm nghệ thuật được số hóa, việc tạo mô tả thủ công cho từng tác phẩm trở nên không khả thi. Đề tài khóa luận này tập trung vào việc **ứng dụng học sâu để sinh mô tả tự động cho các tác phẩm hội họa** nhằm hỗ trợ giới thiệu chúng.

## 2. Bài toán

Đề tài giải quyết hai bài toán chính:

- **Bài toán phân loại:** Phân loại trường phái nghệ thuật và chất liệu của tác phẩm. Thông tin này đóng vai trò quan trọng trong việc cung cấp giới thiệu kỹ thuật cho tác phẩm.
- **Bài toán sinh mô tả tranh:** Tự động tạo ra mô tả bằng ngôn ngữ (tiếng Anh) cho tác phẩm dựa trên nội dung hình ảnh. Mô tả này bổ sung yếu tố ngữ cảnh và nội dung cho tác phẩm.

## 3. Hướng tiếp cận và Mô hình nghiên cứu

Khóa luận tiếp cận bài toán bằng cách kết hợp các mô hình học sâu cho cả xử lý hình ảnh và ngôn ngữ.

- **Phân loại chất liệu và trường phái:** Sử dụng các mô hình Vision Transformer (ViT) và Swin Transformer.
- **Sinh mô tả tranh:** Sử dụng kiến trúc Encoder-Decoder. Khóa luận đã nghiên cứu và triển khai ba hướng tiếp cận cho phần Encoder kết hợp với Transformer Decoder:
  - **EfficientNetB0 - Transformer (EffB0-Trans):** Sử dụng EfficientNetB0 để trích xuất đặc trưng ảnh và Transformer để sinh mô tả.
  - **YOLOv4 - Transformer (YOLO-Trans):** Sử dụng YOLOv4 để trích xuất đặc trưng và thông tin đối tượng, sau đó kết hợp với Transformer để sinh mô tả.
  - **BLIP (Bootstrapping Language-Image Pre-training):** Một mô hình đa phương thức tiên tiến sử dụng Vision Transformer làm Encoder và Transformer Decoder. BLIP sử dụng cơ chế Bootstrapping (Captioning & Filtering) để học từ dữ liệu có/không nhãn.

Để nâng cao chất lượng mô tả sinh ra, khóa luận áp dụng:

- **Tiền xử lý dữ liệu mô tả:** Lọc bỏ thông tin bối cảnh (lịch sử, văn hóa, xã hội) trong mô tả gốc từ tập SemArt, chỉ giữ lại mô tả về hình thức và nội dung trực quan.
- **Xử lý kết quả:** Sử dụng kết quả phân loại chất liệu và trường phái để tạo câu mở đầu theo mẫu. Áp dụng hệ số phạt chống lặp từ cho EffB0-Trans và YOLO-Trans để cải thiện tính tự nhiên của câu sinh.

## 4. Bộ dữ liệu

- **Phân loại chất liệu:** Tập dữ liệu gồm 23.748 bức tranh với 6 nhãn chất liệu (fresco, tempera, lacquer, oil painting, watercolor, unknown). Dữ liệu được lấy từ SemArt, Watercolor2k, bộ tranh sơn mài tự thu thập, và WikiArt.
- **Phân loại trường phái:** Tập dữ liệu từ WikiArt gồm 42.500 ảnh với 13 trường phái nghệ thuật khác nhau.
- **Sinh mô tả:** Bộ dữ liệu SemArt gồm khoảng 21.000 bức tranh kèm mô tả chi tiết và metadata, được sử dụng để huấn luyện mô hình sinh mô tả. Trong thực nghiệm, chỉ sử dụng khoảng 10.000 ảnh và mô tả.

## 5. Kết quả thực nghiệm

Các mô hình được đánh giá trên các tập kiểm định.

- **Phân loại:**
  - Cả ViT-Base và Swin-Base đều đạt độ chính xác cao.
  - **Phân loại chất liệu:** Độ chính xác trên 91% (ViT-Base 92.90%, Swin-Base 91.91%). ViT-Base có lợi thế về tốc độ suy luận.
  - **Phân loại trường phái:** Độ chính xác trên 84% (ViT-Base 85.31%, Swin-Base 84.14%). ViT-Base đạt độ chính xác cao hơn và suy luận nhanh hơn.

- **Sinh mô tả:**
  - Chất lượng mô tả được đánh giá bằng chỉ số BLEU và phân tích thực thể có tên (NER).
  - **Điểm BLEU:** Nhìn chung, các mô hình có điểm BLEU ở mức thấp, đặc biệt là BLIP. EffB0-Trans cho điểm BLEU cao nhất trong ba mô hình.
  - **Phân tích NER:** Tỉ lệ câu mô tả sinh ra chứa tên riêng của người thấp hơn so với câu gốc. Quan sát thấy hiện tượng thiên kiến tên riêng (sinh ra tên phổ biến, bỏ sót tên ít gặp).
  - **Nguyên nhân điểm BLEU thấp:** Dữ liệu huấn luyện còn hạn chế (10.000 ảnh/mô tả) và cấu trúc câu mô tả nghệ thuật phức tạp, mang tính học thuật.

**Nhận xét chung:**

- Các mô hình phân loại hoạt động hiệu quả, cung cấp thông tin đáng tin cậy.
- Các mô hình sinh mô tả đã tạo ra câu mô tả mang phong cách nghệ thuật và dài hơn mô tả ảnh thông thường. Tuy nhiên, chất lượng theo điểm BLEU còn hạn chế, phản ánh thách thức của bài toán và đặc thù ngôn ngữ mô tả nghệ thuật. Việc xử lý tên riêng cũng là một thách thức. EffB0-Trans cho thấy kết quả khả quan nhất trong ba mô hình sinh mô tả đã thử nghiệm.
