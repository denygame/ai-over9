# Hiệu năng: CPU và bộ nhớ

[🇬🇧 English](benchmark.md) · [← Về README](../README_VI.md)

AI Over9 chạy cả ngày cạnh các agent nên phải thật nhẹ. Dưới đây là số đo thật, không phải ước đoán — kèm cách đo để
bạn tự kiểm tra trên máy mình.

## Kết quả

Đo trên MacBook Pro (Apple M3 Max, 14 nhân, 36 GB), macOS 26.2, AI Over9 0.9.1, trong lúc một phiên Claude Code đang
làm việc (app đang nhận sự kiện hook của phiên đó). Mỗi 2 giây một mẫu, 3 phút cho mỗi trạng thái (90 mẫu mỗi trạng
thái).

**CPU** — phần trăm của **một** nhân (100% = một nhân chạy hết công suất; máy này có 14 nhân):

| Trạng thái | Trung bình | Trung vị | Phân vị 95 | Cao nhất |
|---|---|---|---|---|
| **Chạy nền** — chỉ thanh menu, cửa sổ đóng | **1,4%** | 0,0% | 6,4% | 15,6% |
| **Mở cửa sổ chính** ở trang Tổng quan | **5,0%** | 0,0% | 32,5% | 43,2% |
| Helper quạt (tiến trình root), cả hai trạng thái | 0,1% | 0,1% | 0,2% | 0,2% |

**Bộ nhớ:**

| Trạng thái | Bộ nhớ thường trú (`top`) | Bộ nhớ vật lý thực dùng (`footprint`) |
|---|---|---|
| Chạy nền | 29–52 MB | 37 MB |
| Mở cửa sổ chính | 61–79 MB | 72 MB |
| Helper quạt | 4 MB | — |

Số liệu chạy nền được đo ngay sau khi app khởi động, lúc đang nạp lịch sử sử dụng; sau đó ổn định ở khoảng 25–30 MB.

**Mức tiêu thụ năng lượng** (cột power của `top`) đi theo CPU: khoảng 1,4 khi chạy nền, 5 khi mở cửa sổ.

### Trước và sau tối ưu ở bản 0.9.1

Thẻ hạn mức trên Tổng quan trước đây tính lại toàn bộ lịch sử sử dụng (kèm phép tính lịch cho từng bản ghi) ở mỗi lần
vẽ lại. Giờ chỉ tính một lần khi có bản ghi mới hoặc sang ngày mới.

| Mở cửa sổ chính | Trước | Sau |
|---|---|---|
| CPU trung bình | 11,0% | **5,0%** (−55%) |
| CPU trung vị | 8,1% | **0,0%** |
| Phân vị 95 | 49,5% | 32,5% |
| Bộ nhớ vật lý thực dùng | 76 MB | 72 MB |

**0.9.3** chỉ đọc các cảm biến nhiệt cho số hợp lệ: khi chạy nền, CPU trung bình giảm từ 1,4% xuống **1,1%** (trung vị
0%, phân vị 95 là 5,1%). Khi mở cửa sổ, mức dùng nằm trong biên dao động giữa các lần đo (5–6%).

## Ý nghĩa

- **Phần lớn thời gian AI Over9 chạy nền**, và khi đó tốn khoảng bằng một đồng hồ trên thanh menu: trung bình chừng 1%
  của một nhân, khoảng 25 MB bộ nhớ. Các đỉnh ngắn đến từ việc xử lý sự kiện của agent, làm mới hạn mức và vòng lấy tin
  Telegram.
- **Cửa sổ chính tốn hơn,** vì các ô trực tiếp, thẻ hạn mức và biểu đồ được vẽ lại khi cửa sổ mở: trung bình khoảng 5%
  một nhân, nghỉ giữa các lần cập nhật (trung vị 0%), có đỉnh ngắn khoảng 30–40%. Đóng cửa sổ khi không xem là trở về
  mức chạy nền — app vẫn làm việc từ thanh menu.
- **Helper quạt gần như không đáng kể**: vài phần mười phần trăm, 4 MB.
- **Hook của agent không bao giờ phải chờ app:** `aio-hook` chuyển sự kiện qua socket cục bộ rồi thoát; nếu app không
  phản hồi trong khoảng 1,5 giây, sự kiện được ghi vào file và app đọc sau.

## Tự đo

Lấy mã tiến trình rồi lấy mẫu mỗi 2 giây trong 3 phút:

```bash
APP=$(pgrep -x -U "$(id -u)" AIOver9)
HELPER=$(pgrep -f dev.huynt.aiover9.fanhelper)        # chỉ khi đã cài helper quạt
top -l 91 -s 2 -pid $APP ${HELPER:+-pid $HELPER} -stats pid,cpu,mem,power,threads -o pid
footprint -p $APP | head -2                            # bộ nhớ vật lý thực dùng
```

Bỏ mẫu đầu tiên của mỗi lần chạy `top` (chưa có khoảng so sánh). Đo một lần khi đóng cửa sổ và một lần khi mở — hai số
rất khác nhau. Activity Monitor (tab *CPU* và *Memory*, lọc "AIOver9") cho cùng số liệu.

**Windows / Linux** (chưa đo): Task Manager › Details (`AIOver9.exe`), hoặc `top -p $(pgrep -f AIOver9)` trên Linux.

## Giới hạn của số liệu

- Một máy, vài phút cho mỗi trạng thái, đo hai lần (trước và sau tối ưu). Máy Mac chậm hơn sẽ có phần trăm cao hơn cho
  cùng khối lượng việc.
- Càng nhiều agent và phiên thì càng nhiều sự kiện; mỗi sự kiện xử lý mất vài mili-giây, nên mức chạy nền tăng chậm
  theo hoạt động.
- App Windows và Linux (mã nguồn riêng) chưa được đo trên máy thật.
