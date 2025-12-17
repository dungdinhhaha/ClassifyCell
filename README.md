

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

[https://colab.research.google.com/github/dungdinhhaha/ClassifyCell/blob/master/SIMPLE_CELL_DETECTION.ipynb](https://colab.research.google.com/github/dungdinhhaha/ClassifyCell/blob/master/SIMPLE_CELL_DETECTION.ipynb)

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


