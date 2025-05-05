# Summarizing Conversations - Fine-tuning BART on SamSum Dataset

Đây là đồ án cuối kỳ môn học "Python cho Khoa học Dữ liệu" tại Trường Đại học Khoa học Tự nhiên, ĐHQG-HCM. Dự án tập trung vào việc tóm tắt các cuộc hội thoại sử dụng Mô hình Ngôn ngữ Lớn (LLM) thông qua phương pháp học chuyển giao (transfer learning).

## Mục lục

- [Giới thiệu](#giới-thiệu)
- [Tính năng](#tính-năng)
- [Dataset](#dataset)
- [Mô hình](#mô-hình)
- [Công nghệ sử dụng](#công-nghệ-sử-dụng)
- [Cài đặt](#cài-đặt)
- [Sử dụng](#sử-dụng)
  - [Huấn luyện (Fine-tuning)](#huấn-luyện-fine-tuning)
  - [Đánh giá](#đánh-giá)
  - [Tóm tắt văn bản mới](#tóm-tắt-văn-bản-mới)
- [Model Fine-tuned](#model-fine-tuned)
- [Demo](#demo)
- [Kết quả](#kết-quả)
- [Thành viên nhóm](#thành-viên-nhóm)
- [Giảng viên hướng dẫn](#giảng-viên-hướng-dẫn)
- [Tài liệu tham khảo](#tài-liệu-tham-khảo)
- [Giấy phép](#giấy-phép)

## Giới thiệu

Dự án này khám phá khả năng tóm tắt các đoạn hội thoại bằng cách tinh chỉnh (fine-tuning) mô hình BART (`facebook/bart-large-xsum`), một mô hình sequence-to-sequence mạnh mẽ, trên tập dữ liệu **SamSum**. Mục tiêu là tạo ra một mô hình có thể tạo ra các bản tóm tắt ngắn gọn, dễ hiểu cho các cuộc trò chuyện, giúp tiết kiệm thời gian và nâng cao hiệu quả xử lý thông tin.

## Tính năng

-   Tiền xử lý dữ liệu văn bản hội thoại (làm sạch HTML/XML, tokenization).
-   Tinh chỉnh mô hình BART trên tập dữ liệu SamSum cho nhiệm vụ tóm tắt hội thoại.
-   Đánh giá hiệu suất mô hình bằng chỉ số ROUGE.
-   Cung cấp script để huấn luyện và thực hiện tóm tắt.
-   Lưu trữ mô hình đã được tinh chỉnh để tái sử dụng.
-   Demo trực tuyến qua Hugging Face Spaces.

## Dataset

Dự án sử dụng tập dữ liệu **SamSum**, chứa các đoạn hội thoại và bản tóm tắt tương ứng.
-   **Nguồn:** sử dụng trực tiếp từ thư viện `datasets` của Hugging Face (`huggingface.co/datasets/samsum`).
-   **Đặc điểm:** Bao gồm 3 file CSV (train, test, validation) với các cột `id`, `dialogue`, `summary`.

## Mô hình

-   **Mô hình cơ sở:** [facebook/bart-large-xsum](https://huggingface.co/facebook/bart-large-xsum). Đây là mô hình BART đã được huấn luyện trước cho nhiệm vụ tóm tắt tin tức.
-   **Tinh chỉnh:** Mô hình được tinh chỉnh thêm trên tập dữ liệu SamSum để phù hợp hơn với đặc thù tóm tắt hội thoại.

## Công nghệ sử dụng

-   Python 3.x
-   PyTorch
-   Hugging Face Libraries:
    -   `transformers`: Cung cấp mô hình BART và tokenizer.
    -   `datasets`: Tải và xử lý dataset.
    -   `evaluate`: Tính toán chỉ số ROUGE.
-   NLTK: Tách câu trong quá trình tính ROUGE.
-   Pandas: Đọc và xử lý dữ liệu dạng bảng.
-   NumPy: Tính toán số học.
-   Scikit-learn (có thể dùng cho TF-IDF trong khám phá dữ liệu).
-   Kaggle Hub (`kagglehub`): (Nếu sử dụng) Để tải dữ liệu từ Kaggle.

## Cài đặt

1.  **Clone repository:**
    ```bash
    git clone https://github.com/theliems-76/summarizing-conversations
    cd summarizing-conversations
    ```
2.  **Tạo và kích hoạt môi trường ảo (khuyến nghị):**
    ```bash
    python -m venv venv
    source venv/bin/activate  # Trên Windows: venv\Scripts\activate
    ```
3.  **Cài đặt các thư viện cần thiết:**
    ```bash
    pip install -r requirements.txt
    ```
    *(Lưu ý: Bạn cần tạo file `requirements.txt` chứa các thư viện cần thiết như `torch`, `transformers`, `datasets`, `nltk`, `pandas`, `evaluate`, `rouge-score`, `scikit-learn`, `kagglehub`...)*
4.  **Tải dữ liệu NLTK (nếu chưa có):**
    ```python
    import nltk
    nltk.download('punkt')
    ```
5.  **(Tùy chọn) Cấu hình Kaggle API:** Nếu bạn tải dataset từ Kaggle bằng API, hãy đảm bảo bạn đã cài đặt và cấu hình `kaggle.json`.

## Sử dụng

### Huấn luyện (Fine-tuning)

Để huấn luyện lại mô hình từ đầu hoặc tiếp tục huấn luyện:
```bash
python train.py  # Hoặc tên file script huấn luyện của bạn
