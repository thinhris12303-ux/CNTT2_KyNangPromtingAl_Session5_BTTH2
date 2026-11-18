# BÀI TẬP: XÂY DỰNG QUY TRÌNH PROMPT CHAINING GỒM 3 BƯỚC

## 1. Tổng quan quy trình Prompt Chaining

**Prompt Chaining** là kỹ thuật chia một nhiệm vụ phức tạp thành nhiều
công đoạn nhỏ. Kết quả đầu ra của prompt trước được sử dụng làm dữ liệu
đầu vào bắt buộc cho prompt tiếp theo.

Trong bài toán Marketing, chúng ta thiết kế chuỗi gồm 3 prompt:

``` text
PROMPT 1: NGHIÊN CỨU KHÁCH HÀNG
    |
    | Output 1: Customer Research
    v
PROMPT 2: XÂY DỰNG DÀN Ý PR
    |
    | Output 2: PR Outline
    v
PROMPT 3: VIẾT BÀI PR VÀ SEO
    |
    v
OUTPUT 3: BÀI PR HOÀN CHỈNH
```

Mục tiêu: Không để AI tự suy diễn lại dữ liệu ở mỗi bước, mà phải kế
thừa và duy trì thông tin xuyên suốt toàn bộ quy trình.

------------------------------------------------------------------------

## 2. Thiết kế Prompt 1 -- Nghiên cứu khách hàng mục tiêu

**Mục tiêu:** Phân tích chân dung khách hàng (Customer Persona), xác
định nhu cầu và những vấn đề mà AI Chatbot có thể giải quyết.

Prompt 1 phải tạo ra dữ liệu có cấu trúc để Prompt 2 sử dụng, không chỉ
trả lời bằng một đoạn văn thông thường.

### PROMPT 1 -- CUSTOMER RESEARCH

``` text
# PROMPT 1: NGHIÊN CỨU KHÁCH HÀNG MỤC TIÊU

Vai trò: Bạn là chuyên gia nghiên cứu thị trường và chiến lược Marketing B2B.

Bối cảnh:
Phòng Marketing đang chuẩn bị sản xuất bài PR chuyên sâu giới thiệu giải pháp AI Chatbot mới dành cho doanh nghiệp.

Nhiệm vụ:

1. Xác định 3–4 nhóm khách hàng mục tiêu có khả năng sử dụng AI Chatbot.
2. Xây dựng chân dung khách hàng (Customer Persona), bao gồm:
   - Đặc điểm doanh nghiệp.
   - Quy mô doanh nghiệp.
   - Người ra quyết định mua hàng.
   - Nhu cầu sử dụng.
3. Phân tích ít nhất 5 Pain Points:
   - Khó khăn trong chăm sóc khách hàng.
   - Chi phí nhân sự vận hành cao.
   - Thời gian phản hồi khách hàng chậm.
   - Khó quản lý lượng lớn yêu cầu tư vấn.
   - Khó mở rộng dịch vụ ngoài giờ hành chính.
4. Phân tích hậu quả của từng Pain Point.
5. Đề xuất cách AI Chatbot giải quyết từng vấn đề.
6. Xác định lợi ích nổi bật và thông điệp Marketing phù hợp.

Quy tắc:
- Không tự bịa tính năng hoặc số liệu của sản phẩm.
- Nếu thiếu thông tin, đánh dấu "CHƯA CÓ DỮ LIỆU".
- Phân biệt rõ dữ liệu thực tế và giả định.
- Tập trung vào khách hàng doanh nghiệp (B2B).

ĐỊNH DẠNG OUTPUT BẮT BUỘC:

Xuất kết quả thành cấu trúc JSON:

{
  "customer_personas": [],
  "pain_points": [
    {
      "problem": "",
      "impact": "",
      "chatbot_solution": "",
      "customer_benefit": ""
    }
  ],
  "key_benefits": [],
  "marketing_message": "",
  "customer_insights": []
}

Yêu cầu cuối cùng:
Tạo dữ liệu nghiên cứu có chiều sâu, rõ ràng, có thể sử dụng trực tiếp làm đầu vào cho Prompt 2. Không viết bài PR ở bước này.
```

**Đầu ra Prompt 1:** Một bộ dữ liệu nghiên cứu khách hàng gồm Customer
Persona, Pain Points, giải pháp và lợi ích.

------------------------------------------------------------------------

## 3. Thiết kế Prompt 2 -- Xây dựng dàn ý bài PR

**Mục tiêu:** Sử dụng toàn bộ dữ liệu nghiên cứu từ Prompt 1 để xây dựng
dàn ý chi tiết cho bài PR dài 1.000--1.500 từ.

Điểm quan trọng: Prompt 2 không được tự ý thay đổi chân dung khách hàng
hoặc tạo thêm Pain Points không có trong dữ liệu nghiên cứu.

### PROMPT 2 -- PR OUTLINE

``` text
# PROMPT 2: XÂY DỰNG DÀN Ý BÀI PR

Vai trò: Bạn là chuyên gia Content Marketing và SEO.

Đầu vào bắt buộc:
Sử dụng toàn bộ JSON OUTPUT từ PROMPT 1.

[CHÈN OUTPUT PROMPT 1 VÀO ĐÂY]

Nhiệm vụ:

Dựa trên dữ liệu nghiên cứu khách hàng, hãy xây dựng dàn ý chi tiết cho bài PR giới thiệu giải pháp AI Chatbot mới dành cho doanh nghiệp.

Yêu cầu cấu trúc:

1. Headline:
   - Thu hút người đọc.
   - Nêu bật giá trị của AI Chatbot.
   - Có chứa từ khóa SEO chính.

2. Lead:
   - Mở đầu bằng vấn đề thực tế của doanh nghiệp.
   - Khơi gợi sự quan tâm.
   - Dẫn dắt đến giải pháp AI Chatbot.

3. Nội dung chính:
   - H2: Thực trạng và khó khăn của doanh nghiệp.
   - H2: Những hạn chế của phương pháp chăm sóc khách hàng truyền thống.
   - H2: AI Chatbot – Giải pháp chuyển đổi hoạt động chăm sóc khách hàng.
   - H3: Tự động hóa tư vấn và phản hồi.
   - H3: Hỗ trợ khách hàng liên tục.
   - H3: Tối ưu nguồn lực vận hành.
   - H2: Những lợi ích doanh nghiệp nhận được.
   - H2: Vì sao doanh nghiệp nên ứng dụng AI Chatbot ngay hôm nay?

4. CTA:
   - Kêu gọi doanh nghiệp tìm hiểu, đăng ký tư vấn hoặc trải nghiệm sản phẩm.

Yêu cầu SEO:
- Xác định 1 từ khóa chính.
- Đề xuất 5–8 từ khóa phụ.
- Xác định Search Intent.
- Phân bổ từ khóa tự nhiên vào Headline, Lead, H2 và nội dung.
- Tránh nhồi nhét từ khóa.

Quy tắc nhất quán:
- Mọi luận điểm phải liên kết với Pain Points từ Prompt 1.
- Không tự bịa tính năng, số liệu hoặc cam kết hiệu quả.
- Duy trì giọng văn chuyên nghiệp, đáng tin cậy.
- Dự kiến tổng độ dài bài viết từ 1.000–1.500 từ.

ĐỊNH DẠNG OUTPUT:

{
  "headline": "",
  "lead_direction": "",
  "target_word_count": "1000-1500",
  "primary_keyword": "",
  "secondary_keywords": [],
  "search_intent": "",
  "tone_of_voice": "Chuyên nghiệp, tin cậy",
  "outline": [
    {
      "heading": "",
      "level": "H2",
      "purpose": "",
      "pain_points_addressed": [],
      "key_arguments": [],
      "target_words": 0
    }
  ],
  "cta": "",
  "consistency_rules": []
}

Yêu cầu cuối cùng:
Chỉ xây dựng dàn ý, chưa viết bài PR hoàn chỉnh. Output phải đủ chi tiết để Prompt 3 triển khai mà không cần suy đoán lại nội dung.
```

**Đầu ra Prompt 2:** Một dàn ý hoàn chỉnh, có thứ tự H2/H3, mục tiêu
từng phần, từ khóa SEO và số lượng từ dự kiến.

------------------------------------------------------------------------

## 4. Thiết kế Prompt 3 -- Viết bài PR và tối ưu SEO

**Mục tiêu:** Biến dàn ý thành bài PR hoàn chỉnh dài 1.000--1.500 từ.

Đây là bước quan trọng nhất vì AI phải đồng thời đảm bảo:

-   Nội dung có chiều sâu.
-   Không bỏ sót dàn ý.
-   Không lặp ý.
-   Giữ nguyên giọng văn.
-   Tối ưu SEO.
-   Có khả năng tiếp tục viết khi bị giới hạn token.

### PROMPT 3 -- CONTENT GENERATION

``` text
# PROMPT 3: TRIỂN KHAI BÀI PR HOÀN CHỈNH

Vai trò: Bạn là chuyên gia Content Marketing, PR và SEO với kinh nghiệm viết nội dung B2B.

ĐẦU VÀO BẮT BUỘC:

1. OUTPUT PROMPT 1: Dữ liệu nghiên cứu khách hàng.
[CHÈN OUTPUT PROMPT 1]

2. OUTPUT PROMPT 2: Dàn ý bài PR.
[CHÈN OUTPUT PROMPT 2]

NHIỆM VỤ:

Dựa trên hai đầu vào trên, triển khai bài PR chuyên sâu giới thiệu giải pháp AI Chatbot mới cho doanh nghiệp.

YÊU CẦU NỘI DUNG:

1. Độ dài: 1.000–1.500 từ.
2. Viết đầy đủ:
   - Headline hấp dẫn.
   - Lead giải quyết Pain Points.
   - Các phần H2, H3 đúng theo dàn ý.
   - Nội dung phân tích chuyên sâu.
   - CTA rõ ràng ở cuối bài.
3. Mỗi Pain Point phải được liên kết với giải pháp và lợi ích tương ứng.
4. Có sự chuyển tiếp tự nhiên giữa các phần.
5. Không lặp lại cùng một luận điểm ở nhiều phần.
6. Không tự tạo thông tin sản phẩm chưa được cung cấp.

QUY TẮC GIỌNG VĂN:

- Chuyên nghiệp.
- Tin cậy.
- Thuyết phục nhưng không phóng đại.
- Ngôn ngữ phù hợp với người ra quyết định trong doanh nghiệp.
- Giữ nguyên Tone of Voice trong toàn bộ bài viết.

QUY TẮC SEO:

- Sử dụng từ khóa chính trong Headline và Lead.
- Phân bổ từ khóa phụ vào các H2, H3 và nội dung.
- Viết tiêu đề rõ ràng, dễ đọc.
- Tối ưu khả năng đọc trên nền tảng số.
- Không nhồi nhét từ khóa.

QUY TẮC XỬ LÝ KHI BỊ NGẮT QUÃNG:

Nếu bài viết vượt giới hạn token:

1. Dừng tại ranh giới một đoạn văn hoàn chỉnh.
2. Không kết thúc bài viết nếu chưa đến CTA.
3. Ghi lại:
   - Các phần đã hoàn thành.
   - Tiêu đề tiếp theo cần viết.
   - Số từ đã hoàn thành.
   - Từ khóa đã sử dụng.
   - Câu cuối cùng đã viết.
4. Chờ lệnh tiếp tục từ người dùng.

ĐỊNH DẠNG CHECKPOINT:

{
  "completed_sections": [],
  "next_section": "",
  "last_completed_sentence": "",
  "word_count_so_far": 0,
  "keywords_used": [],
  "tone_of_voice": "Chuyên nghiệp, tin cậy",
  "remaining_sections": []
}

YÊU CẦU ĐẦU RA:

Xuất bài PR theo đúng thứ tự dàn ý. Sau mỗi phần, cập nhật trạng thái hoàn thành để đảm bảo không bỏ sót nội dung.

Chỉ kết thúc khi đã hoàn thành toàn bộ bài viết và CTA.
```

------------------------------------------------------------------------

## 5. Xử lý đứt gãy ngữ cảnh khi AI đạt giới hạn Token

Đây là kỹ thuật quan trọng để duy trì tính liên tục khi viết bài dài.

### 5.1. Nguyên nhân

Khi bài viết dài, AI có thể gặp giới hạn độ dài đầu ra (Max Tokens), dẫn
đến:

-   Bài viết bị dừng giữa chừng.
-   Chưa hoàn thành các tiêu đề H2/H3.
-   Nội dung phần sau bị lặp lại.
-   Giọng văn hoặc cách diễn đạt thay đổi.

### 5.2. Giải pháp: Context Checkpoint

Trước khi dừng, yêu cầu AI lưu trạng thái hiện tại bằng cấu trúc
Checkpoint.

**Ví dụ Checkpoint -- trạng thái minh họa:**

  Trạng thái       Nội dung
  ---------------- ----------------------------------
  Tiến độ          50%
  Đã hoàn thành    3/6 phần
  Phần tiếp theo   H2: Lợi ích doanh nghiệp
  Giọng văn        Chuyên nghiệp, tin cậy
  Hành động        Viết tiếp, không lặp nội dung cũ

Các phần đã hoàn thành:

-   [x] H2: Thực trạng doanh nghiệp.
-   [x] H2: Hạn chế phương pháp truyền thống.
-   [x] H2: Giải pháp AI Chatbot.
-   [ ] H2: Lợi ích doanh nghiệp.
-   [ ] H2: Lý do nên ứng dụng.
-   [ ] CTA.

### 5.3. Câu lệnh điều khiển tiếp tục chính xác

Khi AI bị ngắt, sử dụng câu lệnh sau:

``` text
Hãy tiếp tục viết bài PR dựa trên nội dung và CHECKPOINT trước đó.

Yêu cầu bắt buộc:

1. Tiếp tục chính xác từ mục "next_section" trong CHECKPOINT.
2. Không viết lại Headline, Lead hoặc các phần đã hoàn thành.
3. Câu đầu tiên phải nối tiếp tự nhiên với nội dung trước đó.
4. Giữ nguyên Tone of Voice: Chuyên nghiệp, tin cậy.
5. Tuân thủ dàn ý OUTPUT PROMPT 2.
6. Tiếp tục tối ưu từ khóa SEO nhưng không nhồi nhét.
7. Không thay đổi thông tin sản phẩm và các Pain Points đã xác định.
8. Sau khi hoàn thành phần tiếp theo, cập nhật CHECKPOINT mới.
9. Chỉ viết CTA khi đã hoàn thành toàn bộ nội dung chính.

Đây là câu cuối cùng của phần trước:
[DÁN last_completed_sentence]

Đây là CHECKPOINT gần nhất:
[DÁN CHECKPOINT]

Hãy bắt đầu từ phần tiếp theo, không lặp lại nội dung cũ.
```

**Lưu ý:** Nếu chuyển sang cuộc trò chuyện mới, cần cung cấp lại dữ liệu
Prompt 1, dàn ý Prompt 2, bản thảo đã viết và Checkpoint gần nhất. Chỉ
có Checkpoint thì chưa đủ để khôi phục toàn bộ ngữ cảnh.

------------------------------------------------------------------------

## 6. Quy tắc truyền dữ liệu giữa 3 Prompt

Để tránh đứt gãy ngữ cảnh, cần thống nhất cấu trúc dữ liệu:

  Giai đoạn   Dữ liệu đầu ra           Dữ liệu đầu vào tiếp theo
  ----------- ------------------------ ---------------------------
  Prompt 1    Customer Research JSON   Prompt 2
  Prompt 2    PR Outline JSON          Prompt 3
  Prompt 3    Bài PR + Checkpoint      Tiếp tục Prompt 3

### 5 nguyên tắc đảm bảo tính nhất quán

1.  Không thay đổi chân dung khách hàng giữa các bước.
2.  Không tự ý thêm Pain Points chưa được xác thực.
3.  Không thay đổi từ khóa SEO chính.
4.  Không thay đổi Tone of Voice.
5.  Mọi phần viết tiếp phải kế thừa nội dung và trạng thái trước đó.

------------------------------------------------------------------------

## 7. Kiểm tra chất lượng đầu ra cuối cùng

Sử dụng bảng kiểm dưới đây để đánh giá bài PR sau khi hoàn thành.

### Checklist nghiệm thu bài PR

-   [ ] Có Headline hấp dẫn và chứa từ khóa chính.
-   [ ] Lead giải quyết được vấn đề khách hàng.
-   [ ] Đầy đủ các tiêu đề H2 và H3 theo dàn ý.
-   [ ] Phân tích đầy đủ các Pain Points.
-   [ ] Giải pháp AI Chatbot gắn với lợi ích thực tế.
-   [ ] Độ dài đạt 1.000--1.500 từ.
-   [ ] Từ khóa SEO được phân bổ tự nhiên.
-   [ ] Giọng văn chuyên nghiệp, tin cậy xuyên suốt.
-   [ ] Không lặp ý giữa các phần.
-   [ ] CTA rõ ràng ở cuối bài.

------------------------------------------------------------------------

## 8. Kết luận

Quy trình Prompt Chaining gồm 3 bước giúp chuyển một nhiệm vụ Marketing
phức tạp thành quy trình có kiểm soát:

-   **Prompt 1 -- Research:** Hiểu khách hàng và xác định nỗi đau.
-   **Prompt 2 -- Outline:** Xây dựng dàn ý dựa trên dữ liệu nghiên cứu.
-   **Prompt 3 -- Writing:** Viết bài PR hoàn chỉnh và tối ưu SEO.

Điểm cốt lõi của kỹ thuật này không chỉ nằm ở việc chia nhỏ prompt, mà
còn nằm ở khả năng **truyền dữ liệu, kế thừa ngữ cảnh và duy trì tính
nhất quán giữa các công đoạn**.

Kết quả là một quy trình sản xuất nội dung có hệ thống, hạn chế lặp ý,
giảm sai lệch thông tin và giúp AI tạo ra bài PR chuyên sâu chất lượng
cao.
