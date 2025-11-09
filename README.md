# ShibaCard Plugin

Plugin nạp thẻ điện thoại và bank transfer cho Minecraft server - Tích hợp với ShibaCard.net

[![Java](https://img.shields.io/badge/Java-1.8+-orange.svg)](https://www.oracle.com/java/)
[![Minecraft](https://img.shields.io/badge/Minecraft-1.20.1+-green.svg)](https://www.minecraft.net/)
[![License](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)

## 📋 Mục lục

- [Tính năng](#-tính-năng)
- [Yêu cầu](#-yêu-cầu)
- [Cài đặt](#-cài-đặt)
- [Cấu hình](#-cấu-hình)
- [Commands](#-commands)
- [Permissions](#-permissions)
- [Placeholders](#-placeholders)
- [API](#-api)
- [Hỗ trợ](#-hỗ-trợ)

## ✨ Tính năng

### 🎫 Nạp thẻ điện thoại
- Hỗ trợ nhiều loại thẻ: Viettel, Vinaphone, Mobifone, Vietnamobile, Gate, Garena, Vcoin, Zing
- GUI Menu cho Java players (không cần DeluxeMenus)
- Custom Form cho Bedrock players (Floodgate)
- Chat menu và Anvil GUI cho nhập thông tin
- Fast command để nạp nhanh: `/napthe <loại> <mệnh giá> <seri> <pin>`
- Tự động thực thi commands khi nạp thành công
- Hỗ trợ MySQL và Flatfile database

### 💳 PayOS Bank Transfer
- Tích hợp PayOS API để nạp tiền qua chuyển khoản ngân hàng
- Tạo QR code map trong game để quét thanh toán
- Hỗ trợ offline payments - xử lý thanh toán khi player offline
- Tự động kiểm tra trạng thái thanh toán
- Tỷ lệ quy đổi xu tùy chỉnh
- Thực thi commands khi nạp thành công

### 📊 Thống kê và Top
- Top nạp thẻ với GUI menu
- Thống kê theo ngày, tuần, tháng, năm, all-time
- PlaceholderAPI integration
- Milestone rewards

### 🎮 GUI Menus
- Menu chọn loại thẻ (GUI)
- Menu chọn mệnh giá (GUI)
- Menu top nạp thẻ (GUI)
- Tất cả đều native, không cần plugin bên ngoài

## 📦 Yêu cầu

- **Minecraft Server**: Spigot/Paper 1.20.1+
- **Java**: 1.8 hoặc cao hơn
- **API Key**: Lấy từ [ShibaCard.net](https://shibacard.net)
- **Optional**:
  - PlaceholderAPI (cho placeholders)
  - Floodgate (cho Bedrock players)
  - MySQL (cho database)

## 🚀 Cài đặt

### Bước 1: Tải plugin
1. Tải file `ShibaCard-1.0.0.jar` từ [Releases](https://github.com/your-repo/releases)
2. Đặt file vào thư mục `plugins/` của server

### Bước 2: Khởi động server
1. Khởi động server lần đầu để tạo các file config
2. Dừng server

### Bước 3: Cấu hình API
1. Mở file `plugins/ShibaCard/config.yml`
2. Điền `ShibaCard-API.key` và `ShibaCard-API.secret` từ tài khoản ShibaCard.net
3. Khởi động lại server

### Bước 4: Cấu hình tính năng (Optional)
- **PayOS Bank Transfer**: Cấu hình trong `plugins/ShibaCard/bank.yml`
- **Bedrock Form**: Cấu hình trong `plugins/ShibaCard/bedrock.yml`
- **MySQL**: Cấu hình trong `plugins/ShibaCard/config.yml`

## ⚙️ Cấu hình

### config.yml
File cấu hình chính của plugin.

```yaml
# API từ ShibaCard.net
ShibaCard-API:
  key: 'your-api-key'
  secret: 'your-api-secret'

# MySQL (optional)
mysql:
  enable: false
  host: localhost
  port: 3306
  user: root
  password: root
  database: shibacard

# Các loại thẻ được phép
card:
  enable:
    - Viettel
    - Vinaphone
    - Mobifone
    # ...

# Commands khi nạp thành công
card:
  command:
    10000:
      - 'console:points give {player} 10'
      - 'op:broadcast {player} vừa ủng hộ 10k!'
```

### bank.yml
Cấu hình PayOS Bank Transfer.

```yaml
payos:
  enabled: true
  client-id: 'your-client-id'
  api-key: 'your-api-key'
  checksum-key: 'your-checksum-key'
  min-amount: 1000
  max-amount: 50000000
  exchange-rate: 1.0
  qr-map:
    expire-time: 300
  bank-success-commands:
    - 'console:points give {player} {xu_rounded}'
```

### bedrock.yml
Cấu hình form cho Bedrock players (Floodgate).

```yaml
title: "Nạp thẻ"
header:
  - "&aNạp thẻ ủng hộ máy chủ"
# ...
```

## 📝 Commands

### Người chơi

| Command | Aliases | Mô tả |
|---------|---------|-------|
| `/napthe` | `/donate`, `/ungho` | Mở GUI menu nạp thẻ |
| `/napthe <loại> <mệnh giá> <seri> <pin>` | - | Nạp thẻ nhanh (nếu `fastcmd: true`) |
| `/bank <amount>` | `/banking`, `/payos` | Tạo QR code để nạp tiền qua bank |
| `/bank cancel` | - | Hủy giao dịch bank đang hoạt động |
| `/bank info` | - | Xem thông tin giao dịch bank |
| `/topnapthe` | `/topdonate` | Mở GUI menu top nạp thẻ |

### Quản trị viên

| Command | Aliases | Permission | Mô tả |
|---------|---------|------------|-------|
| `/shibacard reload` | `/sc reload`, `/shiba reload` | `shibacard.admin` | Reload tất cả config |
| `/shibacard give <player> <amount>` | `/sc give`, `/shiba give` | `shibacard.admin` | Nạp tiền cho player |
| `/shibacard top [type]` | `/sc top`, `/shiba top` | `shibacard.admin` | Xem top nạp thẻ (daily/weekly/monthly/year/alltime) |
| `/bank rate [rate]` | - | `shibacard.admin` | Xem/đổi tỷ lệ quy đổi xu |

## 🔐 Permissions

| Permission | Mô tả | Mặc định |
|------------|-------|----------|
| `shibacard.admin` | Quyền admin, sử dụng `/shibacard` | `op` |
| `shibacard.use` | Sử dụng `/napthe` | `true` |
| `shibacard.bank` | Sử dụng `/bank` | `true` |

## 📊 Placeholders

Plugin hỗ trợ PlaceholderAPI (cần cài đặt PlaceholderAPI plugin) với các placeholders sau:

### Tổng nạp thẻ
- `%sc_total%` - Tổng nạp thẻ all-time (format: `100,000₫`)
- `%sc_total_daily%` - Tổng nạp thẻ hôm nay
- `%sc_total_weekly%` - Tổng nạp thẻ tuần này
- `%sc_total_monthly%` - Tổng nạp thẻ tháng này
- `%sc_total_year%` - Tổng nạp thẻ năm nay

### Nạp thẻ của player
- `%sc_player%` - Số tiền player hiện tại đã nạp (format: `50,000đ`)

### Top nạp thẻ (All-time)
- `%sc_top_1%` - Top 1 (format: `#1 PlayerName - 100,000₫`)
- `%sc_top_2%` - Top 2
- `%sc_top_3%` - Top 3
- `%sc_top_4%` - Top 4
- `%sc_top_5%` - Top 5
- `%sc_top_6%` - Top 6
- `%sc_top_7%` - Top 7
- `%sc_top_8%` - Top 8
- `%sc_top_9%` - Top 9
- `%sc_top_10%` - Top 10

### Top theo thời gian
- `%sc_top_alltime_1%` đến `%sc_top_alltime_10%` - Top all-time
- `%sc_top_daily_1%` đến `%sc_top_daily_10%` - Top hôm nay
- `%sc_top_weekly_1%` đến `%sc_top_weekly_10%` - Top tuần này
- `%sc_top_monthly_1%` đến `%sc_top_monthly_10%` - Top tháng này
- `%sc_top_year_1%` đến `%sc_top_year_10%` - Top năm nay

**Lưu ý**: 
- Thay `_1` bằng số từ 1-10 để lấy top tương ứng
- Format có thể tùy chỉnh trong `lang.yml`
- PlaceholderAPI là optional dependency

### Ví dụ sử dụng
```
/say Tổng nạp thẻ: %sc_total%
/say Bạn đã nạp: %sc_player%
/say Top 1: %sc_top_1%
/say Top 1 hôm nay: %sc_top_daily_1%
```

### Sử dụng trong DeluxeMenus
```
display_name: "%sc_top_1%"
lore:
  - "Top 1: %sc_top_1%"
  - "Top 2: %sc_top_2%"
```

### Sử dụng trong Chat
```
[Server] Top nạp thẻ: %sc_top_1%
[Server] Bạn đã nạp: %sc_player%
```

## 🎮 Hướng dẫn sử dụng

### Nạp thẻ (Java Players)

1. Gõ `/napthe` để mở GUI menu
2. Chọn loại thẻ (Viettel, Vinaphone, ...)
3. Chọn mệnh giá (10k, 20k, 50k, ...)
4. Nhập số seri trong Anvil GUI
5. Nhập mã thẻ trong Anvil GUI
6. Chờ xử lý và nhận phần thưởng

### Nạp thẻ (Bedrock Players)

1. Gõ `/napthe` để mở Custom Form
2. Chọn loại thẻ từ dropdown
3. Chọn mệnh giá từ dropdown
4. Nhập số seri
5. Nhập mã thẻ
6. Tick xác nhận và submit
7. Chờ xử lý và nhận phần thưởng

### Nạp tiền qua Bank (PayOS)

1. Đảm bảo tay chính trống
2. Gõ `/bank <số tiền>` (ví dụ: `/bank 100000`)
3. Nhận QR map trong inventory
4. Quét QR code bằng app ngân hàng
5. Chuyển khoản đúng số tiền
6. Chờ xác nhận và nhận phần thưởng

**Lưu ý**: 
- QR map sẽ tự động hết hạn sau thời gian cấu hình
- Có thể hủy giao dịch bằng `/bank cancel`
- Hỗ trợ offline - nếu bạn offline khi thanh toán, sẽ nhận phần thưởng khi join lại

## 🛠️ Setup PayOS

1. Đăng ký tài khoản tại [PayOS](https://pay.payos.vn/)
2. Tạo ứng dụng và lấy:
   - Client ID
   - API Key
   - Checksum Key
3. Điền vào `plugins/ShibaCard/bank.yml`:
   ```yaml
   payos:
     enabled: true
     client-id: 'your-client-id'
     api-key: 'your-api-key'
     checksum-key: 'your-checksum-key'
   ```
4. Chạy `/shibacard reload` hoặc restart server

## 🛠️ Setup Bedrock Form (Floodgate)

1. Cài đặt [Floodgate](https://github.com/GeyserMC/Floodgate) plugin
2. Cấu hình Floodgate theo hướng dẫn
3. Plugin sẽ tự động detect và enable form cho Bedrock players
4. Tùy chỉnh form trong `plugins/ShibaCard/bedrock.yml`

## 📁 Cấu trúc file

```
plugins/ShibaCard/
├── config.yml          # Config chính
├── bank.yml            # Config PayOS bank transfer
├── bedrock.yml         # Config Bedrock form
├── lang.yml            # Messages
├── log_success.txt     # Log nạp thẻ (nếu dùng Flatfile)
└── offline_payments.yml # Pending offline payments
```

## 🐛 Troubleshooting

### Plugin không load
- Kiểm tra Java version (cần 1.8+)
- Kiểm tra Minecraft version (cần 1.20.1+)
- Xem console log để biết lỗi cụ thể

### Không nạp được thẻ
- Kiểm tra API key và secret trong `config.yml`
- Kiểm tra loại thẻ có trong `card.enable` không
- Bật `debug: true` để xem log chi tiết
- Kiểm tra kết nối internet và API ShibaCard.net

### PayOS không hoạt động
- Kiểm tra `bank.yml` có đúng thông tin không
- Kiểm tra `payos.enabled: true`
- Chạy `/shibacard reload` sau khi sửa config
- Kiểm tra tay chính có trống không khi tạo QR
- Kiểm tra thông tin PayOS API có đúng không
- Xem console log để biết lỗi cụ thể

### Bedrock form không hiện
- Kiểm tra Floodgate đã cài đặt chưa
- Kiểm tra `bedrock.yml` có tồn tại không
- Xem console log khi enable plugin
- Kiểm tra player có phải Bedrock player không

### GUI Menu không mở
- Kiểm tra player có đang mở inventory khác không
- Thử restart server
- Xem console log để biết lỗi

### Placeholder không hoạt động
- Kiểm tra PlaceholderAPI đã cài đặt chưa
- Chạy `/papi reload` sau khi cài plugin
- Kiểm tra placeholder có đúng format không
- Xem console log để biết lỗi

## 📄 License

MIT License - Xem file [LICENSE](LICENSE) để biết thêm chi tiết.

## 🔗 Links

- **Website**: [https://shibacard.net](https://shibacard.net)
- **PayOS**: [https://pay.payos.vn/](https://pay.payos.vn/)
- **Floodgate**: [https://github.com/GeyserMC/Floodgate](https://github.com/GeyserMC/Floodgate)

## 📦 Building

### Yêu cầu
- Java 1.8+
- Maven 3.6+

### Build plugin
```bash
cd ShibaCard_Plugin
mvn clean package
```

File JAR sẽ được tạo tại: `target/ShibaCard-1.0.0.jar`

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## 📝 Changelog

### Version 1.0.0
- ✨ Tính năng nạp thẻ điện thoại
- ✨ GUI Menu cho Java players
- ✨ Custom Form cho Bedrock players (Floodgate)
- ✨ PayOS Bank Transfer với QR code map
- ✨ Hỗ trợ offline payments
- ✨ Top nạp thẻ với GUI menu
- ✨ PlaceholderAPI integration
- ✨ MySQL và Flatfile database support
- ✨ Milestone rewards

## 👨‍💻 Tác giả

Plugin được phát triển bởi Shiba Network

## 🙏 Cảm ơn

Cảm ơn tất cả người dùng đã sử dụng và đóng góp cho plugin!

---

**Lưu ý**: Plugin này yêu cầu API key từ ShibaCard.net để hoạt động. Vui lòng đăng ký tài khoản tại [shibacard.net](https://shibacard.net) để lấy API key.
