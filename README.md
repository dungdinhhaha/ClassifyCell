

```md
# 🧬 Phát hiện & Phân loại Tế bào Cổ tử cung

## 1. Giới thiệu

Dự án này xây dựng một **pipeline hoàn chỉnh để phát hiện (detection) và phân loại (classification) tế bào cổ tử cung** từ:

- Ảnh quang học thông thường  
- File ảnh bệnh lý số hoá chuẩn **SVS (Whole Slide Image)**  

Hệ thống sử dụng **TensorFlow / Keras** kết hợp với **OpenCV và OpenSlide**, có thể chạy trực tiếp trên **Google Colab** mà không cần cài đặt phức tạp.

Mục tiêu chính:
- Tự động phát hiện vùng chứa tế bào từ ảnh lớn
- Cắt (crop) từng tế bào riêng lẻ
- Phân loại tế bào vào **11 nhóm** theo mô học
- Có thể mở rộng thành hệ thống backend xử lý AI chạy ngầm (background processing)

---

## 2. Cấu trúc thư mục

```

## 2. Cấu trúc thư mục

```text
PhatHienTeBao/
│
├── SIMPLE_CELL_DETECTION.ipynb
│   └── Notebook chính:
│       - Đọc ảnh / file SVS
│       - Chia tile
│       - Lọc background
│       - Detect tế bào
│       - Crop & classify bằng model
│
├── TRAIN_CLASSIFIER_FROM_FOLDERS.ipynb
│   └── Notebook huấn luyện model phân loại
│       từ các thư mục ảnh đã được gán nhãn
│
├── best_cytology_model.keras
│   └── Model TensorFlow/Keras đã huấn luyện sẵn
│
├── sample.svs
│   └── File SVS mẫu để thử nghiệm
│
├── images/
│   ├── 1/    # Class 1
│   ├── 2/    # Class 2
│   ├── ...
│   └── 11/   # Class 11
│
└── README.md
    └── Tài liệu hướng dẫn sử dụng dự án


```

---

## 3. Hướng dẫn sử dụng nhanh trên Google Colab

### Bước 1: Mở notebook
Truy cập trực tiếp:
```

https://colab.research.google.com/github/dungdinhhaha/ClassifyCell/blob/master/SIMPLE_CELL_DETECTION.ipynb

```

---

### Bước 2: Kết nối Google Drive & clone source
- Chạy **Cell 1**
- Notebook sẽ:
  - Mount Google Drive
  - Clone repository từ GitHub

---

### Bước 3: Thêm file SVS
- Upload file `.svs` vào Google Drive
- Đổi tên file thành:
```

sample.svs

```

---

### Bước 4: Chạy pipeline
- Chạy lần lượt các cell còn lại
- Hệ thống sẽ tự động:
  - Đọc file SVS
  - Chia ảnh thành các tile nhỏ
  - Lọc background
  - Detect tế bào
  - Crop vùng tế bào
  - Phân loại bằng model đã huấn luyện

---

### Bước 5: Xem kết quả
- Ảnh tế bào đã detect
- Kết quả phân loại theo từng class
- File kết quả:
  - CSV
  - JSON  
  (nếu bật chế độ lưu)

---

## 4. Kiến trúc hệ thống (Định hướng triển khai thực tế)

### 4.1 Kiến trúc tổng quát

```

[ Client / Browser ]
|
| Upload ảnh / SVS
v
[ Backend API ]
|
| Lưu file (Disk / Object Storage)
| Ghi metadata vào Database
|
v
[ Job Queue ]
|
v
[ AI Worker (Background Processing) ]
|
| Detect & Classify
| Lưu kết quả
v
[ Database / File Storage ]
|
v
[ Client xem kết quả ]

```

---

### 4.2 Nguyên lý thiết kế

#### Upload & lưu trữ
- Backend **chỉ xử lý upload**
- File ảnh / SVS:
  - Lưu vào disk hoặc object storage
  - Metadata (tên file, trạng thái, thời gian) lưu vào database

#### AI chạy ngầm (Background Processing)
- Sau khi upload xong:
  - Backend **đẩy job vào hàng đợi**
- AI Worker chạy độc lập:
  - Multi-process / multi-thread
  - Không block request của người dùng

#### Theo dõi tiến trình
- Có thể mở rộng:
  - WebSocket
  - Polling API
- Trạng thái xử lý:
  - `UPLOADED`
  - `PROCESSING`
  - `DONE`
  - `FAILED`

---

### 4.3 Ưu điểm kiến trúc
- Không bị treo hệ thống khi xử lý file lớn
- Scale tốt khi nhiều người upload cùng lúc
- AI có thể chạy trên server hoặc GPU riêng
- Phù hợp triển khai trong môi trường bệnh viện / phòng lab

---

## 5. Yêu cầu hệ thống

- Python ≥ 3.9
- TensorFlow / Keras
- OpenSlide
- OpenCV
- NumPy
- Matplotlib

📌 **Google Colab đã có sẵn hầu hết các thư viện cần thiết**

---

## 6. Ghi chú

- Model hiện tại phục vụ mục đích nghiên cứu và học tập
- Có thể huấn luyện lại model bằng notebook training
- Pipeline dễ dàng mở rộng thành:
  - REST API
  - Web Application
  - Hệ thống hỗ trợ chẩn đoán cho bác sĩ

---

## 7. Định hướng mở rộng

- Tách AI thành microservice riêng
- Thêm cơ chế progress realtime
- Tối ưu detect cho file SVS dung lượng lớn
- Chuẩn hoá để dùng trong nghiên cứu hoặc sản phẩm thương mại
```

---

## 8. Cách huấn luyện mô hình và Kết quả mong đợi

Phần huấn luyện trong dự án này sử dụng **transfer learning** để tận dụng sức mạnh của các mô hình học sâu đã được huấn luyện sẵn, giúp tiết kiệm thời gian và tăng độ chính xác cho bài toán phân loại tế bào cổ tử cung.

---

### 8.1 Cách huấn luyện mô hình

#### Sử dụng mô hình có sẵn (DenseNet121)
Dự án sử dụng **DenseNet121**, một mô hình học sâu đã được huấn luyện trước trên tập dữ liệu ImageNet.

Mô hình này giúp:
- Trích xuất đặc trưng hình ảnh tốt
- Phù hợp với các bài toán ảnh y sinh
- Giảm nhu cầu cần lượng dữ liệu huấn luyện lớn

---

#### Huấn luyện theo 2 giai đoạn

Để mô hình học hiệu quả và ổn định, quá trình huấn luyện được chia thành **2 giai đoạn**:

**Giai đoạn 1 – Huấn luyện phần phân loại:**
- Giữ nguyên (đóng băng) toàn bộ DenseNet121
- Chỉ huấn luyện phần phân loại phía sau

Mục tiêu:
- Giúp mô hình học cách phân biệt các loại tế bào
- Tránh làm hỏng các đặc trưng đã học sẵn

---

**Giai đoạn 2 – Tinh chỉnh mô hình (fine-tuning):**
- Chỉ thực hiện khi kết quả giai đoạn 1 tương đối tốt
- Mở khóa một số layer cuối của DenseNet121
- Huấn luyện với learning rate rất nhỏ

Mục tiêu:
- Giúp mô hình thích nghi tốt hơn với ảnh tế bào cổ tử cung
- Cải thiện độ chính xác mà vẫn giữ được độ ổn định

---

#### Tăng cường dữ liệu nhẹ
Do hình dạng tế bào rất nhạy cảm, dự án **chỉ sử dụng tăng cường dữ liệu ở mức nhẹ**, bao gồm:
- Lật ảnh theo chiều ngang
- Xoay ảnh một góc nhỏ

Việc này giúp:
- Mô hình học tổng quát tốt hơn
- Không làm biến dạng cấu trúc tế bào

---

#### Cân bằng dữ liệu giữa các lớp
Một số loại tế bào xuất hiện ít hơn các loại khác.  
Để tránh mô hình thiên lệch, dự án sử dụng **trọng số lớp (class weight)** trong quá trình huấn luyện.

Điều này giúp:
- Mô hình chú ý hơn đến các lớp ít dữ liệu
- Kết quả phân loại cân bằng hơn

---

#### Các cơ chế hỗ trợ huấn luyện
Quá trình huấn luyện có sử dụng các cơ chế sau:
- Lưu lại mô hình tốt nhất
- Dừng sớm khi mô hình không còn cải thiện
- Tự động giảm learning rate khi kết quả bị chững lại

Nhờ đó:
- Tránh overfitting
- Tiết kiệm thời gian huấn luyện
- Giúp mô hình hội tụ tốt hơn

---

### 8.2 Kết quả mong đợi

#### Độ khó của bài toán
Phân loại tế bào cổ tử cung là bài toán khó do:
- Các loại tế bào rất giống nhau
- Ảnh có chất lượng không đồng đều
- Dữ liệu giữa các lớp không cân bằng



---



