# Dự án Phân loại Rủi ro Tín dụng (German Credit Data)

## 1. Giới thiệu
Dự án này nhằm xây dựng một pipeline xử lý dữ liệu và huấn luyện các mô hình phân loại rủi ro tín dụng cho khách hàng dựa trên bộ dữ liệu **German Credit Data**. Mục tiêu là xác định khả năng rủi ro (Risk) của khách hàng từ các thông tin như giới tính, tuổi, tình trạng tài chính và các yếu tố khác. Trong dự án, chúng tôi thử nghiệm và so sánh hiệu năng của năm mô hình:
- Logistic Regression
- Decision Tree
- Random Forest
- XGBoost
- SVM

## 2. Quy trình thực hiện

### 2.1. Xử lý Dữ liệu
- **Đọc dữ liệu**: Sử dụng Pandas để đọc file `german_credit_data.csv` từ Google Drive.
- **Khám phá dữ liệu**: Kiểm tra thông tin (df.info(), df.describe()), hiển thị một vài dòng dữ liệu và xác định các giá trị duy nhất ở các cột như `Saving accounts` và `Checking account`.
- **Làm sạch dữ liệu**: 
  - Loại bỏ cột không cần thiết (ví dụ: `Unnamed: 0`).
  - Kiểm tra, xử lý giá trị thiếu (sử dụng SimpleImputer với chiến lược 'constant' để điền 'unknown').
  - Loại bỏ các bản ghi trùng lặp.
- **Mã hóa biến phân loại**: Sử dụng LabelEncoder cho các cột như `Sex`, `Housing`, `Saving accounts`, `Checking account`, `Purpose`, và biến mục tiêu `Risk`.

### 2.2. Chia dữ liệu và Chuẩn hóa
- **Tách dữ liệu**: Tách thành tập huấn luyện (80%) và tập kiểm tra (20%) với stratify theo nhãn `Risk`.
- **Chuẩn hóa**: Áp dụng StandardScaler cho dữ liệu đầu vào.

### 2.3. Huấn luyện Mô hình
- **Các mô hình được huấn luyện**:
  - *Logistic Regression*: Huấn luyện với tối đa 1000 iteration.
  - *Decision Tree*: Sử dụng DecisionTreeClassifier với tham số mặc định.
  - *Random Forest*: Sử dụng 100 cây, random_state=42.
  - *XGBoost*: Sử dụng XGBClassifier với eval_metric="logloss".
  - *SVM*: Sử dụng SVC với probability=True.
- Mỗi mô hình được huấn luyện trên dữ liệu đã chuẩn hóa (đối với Logistic Regression và SVM) hoặc dữ liệu gốc (với các mô hình còn lại).

### 2.4. Đánh giá Mô hình
- **Đánh giá trên tập kiểm tra**:  
  - In báo cáo đánh giá mô hình bao gồm Precision, Recall, F1-score và ROC AUC.
  - Vẽ ma trận nhầm lẫn cho từng mô hình bằng heatmap.
- **So sánh hiệu suất**:  
  - Sử dụng biểu đồ cột để so sánh các chỉ số (Precision, Recall, F1-score, ROC AUC) giữa các mô hình.
- **Kiểm thử với K-Fold Cross Validation (10-fold)**: Đánh giá độ ổn định của mô hình bằng cách tính accuracy trung bình trên 10 fold.

## 3. Kết quả và Nhận xét
- **Logistic Regression**: Có recall cao nhưng precision thấp; báo nhiều trường hợp rủi ro giả (false positives).
- **Decision Tree**: Hiệu suất không ổn định, AUC thấp và sai số khá lớn.
- **Random Forest**: Hiệu suất cân bằng với precision và recall đều ở mức trên 0.8, AUC trung bình khoảng 0.78; lựa chọn ổn định cho mô hình dễ giải thích.
- **XGBoost**: Có recall cao và AUC tốt (ví dụ: recall ≈ 0.88, AUC ≈ 0.83); F1-score cao, cho thấy khả năng phát hiện khách hàng rủi ro tốt nhưng có thể báo nhầm nhiều.
- **SVM**: Dù có AUC rất cao (ví dụ: 0.92) nhưng recall thấp (ví dụ: 0.73), có xu hướng bỏ sót nhiều khách hàng rủi ro.
  
**Kết luận**:  
- Nếu doanh nghiệp ưu tiên giảm thiểu rủi ro tối đa (tức là không bỏ sót khách hàng rủi ro), XGBoost có thể là lựa chọn tốt.  
- Nếu cần mô hình ổn định và dễ giải thích, Random Forest là lựa chọn an toàn.

## 4. Cách chạy Dự án
1. **Clone Repository**:
   ```bash
   git clone https://github.com/your_username/credit-loans-classification.git
   cd credit-loans-classification

Nếu có sai sót hay nhầm lẫn, vui lòng liên hệ qua mail sau : thanh.van19062004@gmail.com
