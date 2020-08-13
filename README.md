# CS114_CourseProject
### BÀI TOÁN NHẬN DIỆN MỘT NGƯỜI CÓ ĐỘI MŨ BẢO HIỂM HAY KHÔNG

## Mô tả bài toán

Yêu cầu từ thực tế:  Theo số liệu thống kê của Ủy ban An toàn giao thông Quốc gia thì mỗi năm ở nước ta có hơn 8000 người chết vì tai nạn giao thông .
Trong đó , số người đi môtô xe máy gây tai nạn giao thông chiếm 60%, số người đi xe máy là nạn nhân tai nạn giao thông là 85% . Một thống kê khác cho 
thấy  đội mũ bảo hiểm giúp giảm 40% nguy cơ thương vong khi bị tai nạn giao thông. Do đó cần xây dựng một ứng dụng giám sát người tham gia giao thông 
có đội mũ hay không để có các biện pháp xử phạt và ngăn chặn việc tham gia giao thông không đội mũ bảo hiểm , giúp giảm được một phần thiệt hại do tai 
nạn giao thông gây ra .

+ Input: Hình ảnh một người đang tham gia giao thông bằng xe gắn máy .
+ Output: Cho biết người đó đang đội mũ hay không .

## Dữ liệu 
Data sẽ được thu thập theo các bước
* Quay video ở định dạng 4k 30 fps để đảm bảo hình  ảnh trích xuất ra có chất lượng chấp nhận được .
* Trích xuất hình ảnh từ video bằng app Frame Grabber , ứng dụng này cho phép trích xuất tối đa 24 frame/s nhưng bọn em chỉ lấy 
trung bình 3-5frame/s để đảm bảo có sự khác nhau về mặt góc độ  giữa những bức hình  .
* Crop bớt phần rìa để tập trung vào chủ thể là người đang tham gia giao thông bằng xe gắn máy .
* Hình ảnh sẽ được resize về kích thước 224x224 trước khi đưa vào trích xuất đặc trưng 
* ảnh sẽ được chia train/test theo tỉ lệ 8/2 

## Trích xuất các đặc trưng

Ở đây mình sẽ dùng 2 cách:
* Cách 1: Ý tưởng ở đây là sự khác nhau giữa hình dạng của 1 người có đội nón và 1 người không đội nón vì vậy đầu tiên em sử dụng thuật toán tìm cạnh trong OpenCV – CannyEdge sau khi ảnh đã được crop tập trung vào chủ thể con người để rút trích ra đặc trưng của 2 loại ảnh dựa vào hình dáng của chủ thể ( đặc biệt là phần đầu). Tiếp theo em sẽ chia ảnh ra thành các block có kích thước 50x50 trên nền ảnh 200x200 , với 1 block như thế em lấy 2 giá trị đại diện cho ô đó là giá trị np.mean (giá trị trung bình) và np.std (độ lệch chuẩn) 
* Cách 2: Sử dụng pre-trained model (VGG16) để rút trích . Đầu vào resize về kích thước 224x224 và qua các  bước xử lí để phù hợp với đầu vào của mạng VGG16 . 
## Xây dựng model 
* Model em chọn là LogisticRegression và RandomForestClassifier của sklearn vì đây là 2 loại model em đã được học và từng sử dụng trên lớp , hơn nữa 2 model này được sử dụng phổ biến trong các bài toán binary classification. Trong đồ án này , để có thể đảm bảo model có độ performance tốt thì có sử dụng thêm kỹ thuật gridsearchCV tìm ra param tốt nhất cho model .
* Đánh giá model bằng accuracy score , confusion matrix
## Hướng phát triển 
* Sử dụng thêm một số cách feature extraction khác để phát hiện hình dáng hay vật thể trong ảnh .
* Phương pháp crop ảnh theo tọa độ để giảm chi tiết thừa đôi khi sẽ làm mất một số phần quan trọng trong bức ảnh . Vì vậy, hướng phát triển em đề ra là tìm 1 thư viện hoặc phần mềm hỗ trợ bỏ đi backround không cần thiết mà chỉ quan tâm đến chủ thể trong bức ảnh ( phần đầu người) .
* Hướng phát triển khác em muốn đề cập nữa là thiết kế và sử dụng hoàn toàn 1 mạng tích chập neural network dùng chung cho việc từ tiền xử lí ảnh ,rút trích cho đến việc phân loại ảnh. 
