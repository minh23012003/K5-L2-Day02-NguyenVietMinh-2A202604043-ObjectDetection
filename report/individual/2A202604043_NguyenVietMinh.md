# Báo cáo cá nhân — Ngày 2 Object Detection

Đổi tên file theo `MSSV_HoTen.md`, ví dụ `20241234_NguyenVanAn.md`. Điền kết quả thật, xóa dòng hướng dẫn khi nộp. Xem [quy định nộp](../../SUBMISSION.md).

## 1. Thông tin học viên

- Họ tên:Nguyễn Viết Minh
- MSSV:2A202604043
- Email:
- Lớp:
- Link repo bài làm:https://github.com/minh23012003/K5-L2-Day02-NguyenVietMinh-2A202604043-ObjectDetection
- Link Colab đã chạy:https://colab.research.google.com/drive/1B8C3m71TSc-0usrDbvxpp4olnHMMih_m

## 2. Schema và mini guideline

Dẫn tới [GUIDELINE_MINI.md](../../GUIDELINE_MINI.md). Chi tiết nằm trong GUIDELINE_MINI.md. Schema gồm 4 lớp: car, truck, bus, van (cấu hình huấn luyện nc=4). Ba ca mơ hồ đã quyết định, cách áp dụng schema và phần cần giảng viên làm rõ: chưa có dữ kiện trong hội thoại, cần điền từ GUIDELINE_MINI.md và nhãn đã vẽ..

## 3. Annotation và self-QC

- Số file nhãn train: 50 (log YOLO quét 50 ảnh, 0 corrupt, 0 background; ngoài ra có 10 ảnh eval).
- Kết quả checker và lỗi đã xử lý: chưa có dữ kiện, cần điền từ lần chạy checker của bạn.
- Danh sách ít nhất 10 ảnh đã rà bằng mắt: chưa có dữ kiện, cần liệt kê tên ảnh.
- Lỗi bỏ sót/box/class phát hiện khi rà và cách sửa:chưa có dữ kiện, cần điền.
- Commit khóa nhãn ban đầu:chưa xác định. Commit mới nhất của repo khi chạy Colab là 3e63caa ("Add files via upload"); chỉ ghi nó là commit khóa nhãn nếu đó đúng là commit bạn đã khóa nhãn.

## 4. Gold QC và rework

| Chỉ số | Trước sửa | Sau sửa nếu có |
|---|---:|---:|
| TP | 545 |  |
| FP | 844 | |
| FN | 108 | |
| Precision | 0.3924 | |
| Recall | 0.8346 | |
| F1 | 0.5338 | |
| mean_iou_of_matches | 0.8526 | |
| passed | | |

Dẫn tới `outputs/annotation_evaluation.json` trong repo. Nếu rework, ghi ảnh, lỗi đã kiểm, quyết định sửa/giữ và lý do. Không suy nguyên nhân chỉ từ FP/FN. Không có rework thì ghi rõ.

## 5. Kết quả YOLO

- Runtime, GPU/CPU, phiên bản Ultralytics và Torch:Colab, GPU Tesla T4; Ultralytics 8.4.208, Torch 2.14.1+cu130
- Các tham số thay đổi so với notebook nếu có và lý do:model yolo26n.pt, 50 epoch, imgsz 640, batch=-1 (AutoBatch chọn 41), seed 20260909, optimizer auto (chọn AdamW, lr 0.00125), 50 ảnh train và 10 ảnh val
- Đường dẫn best checkpoint trong phiên chạy:/content/Day2-Lab/outputs/yolo26_runs/vehicle_annotation/weights/best.pt

| Chỉ số trên 10 ảnh eval | Giá trị |
|---|---:|
| Precision | 0.612 |
| Recall | 0.387 |
| mAP50 | 0.422 |
| mAP50–95 | 0.283 |

Đặt cả 2 folder `assets/eval_predictions/` và `assets/yolo26_runs/` (giải nén từ `day2_colab_outputs.zip`) cạnh báo cáo và dẫn link tương đối. Không chỉ ghi số; chỉ ra ít nhất một ảnh và vật thể cụ thể.

Trả lời bốn câu hỏi cuối notebook:

1. Lớp nào recall thấp nhất? Ảnh nào minh họa?
truck và bus cùng có recall = 0. Trong 10 ảnh dự đoán, model không xuất ra nhãn truck nào. Nhãn bus có xuất hiện ở 051, 056, 057, 059 nhưng không khớp ground truth nào (P = 0).
- drive_052: chiếc xe buýt hai tầng lớn ở giữa ảnh gần như không có box riêng, ảnh chỉ có 23 car.
- drive_053: xe buýt xanh lớn bị gán van 0.79 bằng một box rất rộng, tức bị nhầm lớp chứ không phải bỏ sót.
- drive_056: xe lớn màu đỏ sẫm ở góc dưới bị gán car 0.60.
2. Nếu mAP50 cao nhưng mAP50–95 thấp, cần kiểm điều gì trên box?
mAP50 chỉ cần IoU ≥ 0.5, còn mAP50–95 đòi box khớp chặt tới IoU 0.95. Dữ kiện ở đây: lớp car có mAP50 = 0.829 nhưng mAP50–95 = 0.570, và van là 0.798 so với 0.518. Model tìm đúng xe nhưng box lệch. Cần kiểm tra:
- Box ôm sát xe chưa, có bị rộng quá (gồm bóng, đường) hoặc chặt quá (cắt mất đầu/đuôi xe).
- Cách vẽ box cho xe bị che khuất hoặc bị cắt ở mép ảnh có nhất quán giữa các ảnh không.
- Xe nhỏ ở xa (drive_058, 060) có bị vẽ ẩu không.
- mean_iou_of_matches từ checker, vì chỉ số này cho biết box của bạn lệch gold bao nhiêu.
3. Lỗi class nào có thể liên quan guideline? Dữ kiện nào hỗ trợ nhận xét?
Ranh giới car/van/bus/truck là nơi dễ liên quan nhất. Dữ kiện:
- drive_053: bus bị gán thành van.
- drive_056: xe lớn bị gán thành car.
- van chỉ đạt recall 0.667 (12 instance trong 6 ảnh).
- truck và bus recall bằng 0.
4. Vì sao không dùng 10 ảnh eval để train? Vai trò validation của chúng trong notebook là gì?
- Nếu train trên chúng thì model có thể nhớ các ảnh đó, metric bị phồng lên, và không còn đo được khả năng tổng quát hóa trên ảnh chưa thấy. Trong notebook, 10 ảnh này là tập val trong data.yaml: sau mỗi epoch Ultralytics đánh giá lên đó (log in P/R/mAP từng epoch), best.pt được chọn theo điểm trên tập này, và bảng chỉ số cuối cũng lấy từ đó. Hạn chế cần ghi thẳng: vì best.pt được chọn trên chính tập này và chỉ có 10 ảnh, kết quả có thể hơi lạc quan và dao động mạnh, như mAP50 tụt xuống 0.116 ở epoch 8 rồi hồi lại 0.422 ở epoch 50. Không có tập test độc lập.

## 6. Khó khăn và cách xử lý

| Bước/thời điểm | Lỗi quan sát | Cách đã thử | Kết quả/còn thiếu |
|---|---|---|---|
| Đẩy bài lên Git| fatal: not a git repository | Khởi tạo repo hoặc clone repo gốc rồi copy bài vào | Repo đã có trên GitHub và Colab clone được  |

## 7. Tự đánh giá theo rubric

Đối chiếu [RUBRIC.md](../../RUBRIC.md): phần nào đã có kết quả, phần nào còn thiếu hoặc chưa kiểm được. Không tự nhận hoàn thành bước chưa chạy.
- Đã có kết quả: huấn luyện và đánh giá YOLO trên 10 ảnh eval, bảng chỉ số, phân tích lỗi theo lớp, trả lời bốn câu hỏi, file day2_colab_outputs.zip.
- Còn thiếu: Gold QC và rework (mục 4), số liệu checker và danh sách ảnh đã rà (mục 3), ba ca mơ hồ của guideline (mục 2), link Colab đã chạy.
- Chưa kiểm được: nhận định về từng ảnh (drive_052, 053, 056) cần đối chiếu lại với ảnh và nhãn gốc.

## 8. Công cụ AI và tài liệu tham khảo

Ghi công cụ đã dùng, dùng ở bước nào và phần đã tự kiểm tra; không dùng thì ghi rõ. Liệt kê nguồn ngoài repo được sử dụng. Không ghi mật khẩu/token hoặc đính kèm gold train.
