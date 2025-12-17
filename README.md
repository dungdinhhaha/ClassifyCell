Phát hiện & Phân loại Tế bào Cổ tử cung
1. Giới thiệu

Dự án này cung cấp pipeline phát hiện và phân loại tế bào cổ tử cung từ ảnh quang học hoặc file SVS, sử dụng TensorFlow/Keras.
Hệ thống có thể chạy trực tiếp trên Google Colab.

Toàn bộ mã nguồn, model và dữ liệu mẫu có thể tải nhanh về Colab bằng lệnh git clone.

2. Cấu trúc thư mục
PhatHienTeBao/

SIMPLE_CELL_DETECTION.ipynb: Notebook dùng để phát hiện và phân loại tế bào

TRAIN_CLASSIFIER_FROM_FOLDERS.ipynb: Notebook dùng để huấn luyện model từ các thư mục ảnh

best_cytology_model.keras: File model đã được huấn luyện sẵn

sample.svs: File SVS mẫu để thử nghiệm

images/: Thư mục chứa các ảnh tế bào đã được cắt và phân loại

1/: Class 1

2/: Class 2

…

11/: Class 11 (mỗi thư mục tương ứng một loại tế bào)

README.md: File hướng dẫn sử dụng dự án


3. Hướng dẫn sử dụng nhanh trên Google Colab
Bước 1: Tải dự án về Colab bằng git clone
!git clone https://github.com/dungdinhhaha/ClassifyCell /content/PhatHienTeBao
%cd /content/PhatHienTeBao

Bước 2: Upload file SVS của bạn

Upload trực tiếp lên Colab

Hoặc copy từ Google Drive

Bước 3: Mount Google Drive để lưu kết quả (không bắt buộc)
from google.colab import drive
drive.mount('/content/drive')

Bước 4: Mở và chạy notebook phát hiện tế bào

Mở file:
SIMPLE_CELL_DETECTION.ipynb

Chạy từng cell theo thứ tự từ trên xuống

Bước 5: Xem kết quả

Ảnh tế bào đã detect

Kết quả phân loại theo class

File CSV / JSON (nếu bật lưu kết quả)

4. Yêu cầu hệ thống

Python ≥ 3.9

TensorFlow / Keras

OpenSlide

OpenCV

NumPy, Matplotlib

(Google Colab đã có sẵn hầu hết các thư viện cần thiết)


