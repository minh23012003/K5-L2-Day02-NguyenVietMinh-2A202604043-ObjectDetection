# Mini guideline — bài làm cá nhân

> Điền nội dung này trước khi bắt đầu gán nhãn. Giữ lại các quyết định sau khi self-QC.

## Định nghĩa lớp

    | Lớp | Định nghĩa của bạn | Ví dụ/điểm phân biệt |
    | --- | --- | --- |
    | `car` |Xe con chở người: sedan, hatchback, SUV, taxi |Thân thấp, một khoang hành khách; lớp mặc định khi xe nhỏ và không có dấu hiệu của van/truck/bus |
    | `truck` |Xe tải có cabin riêng và thùng hàng, hoặc xe tải thân dài |Cabin tách khỏi thùng; không có hàng cửa sổ hành khách |
    | `bus` |Xe buýt/xe khách thân dài, kể cả xe hai tầng |Nhiều cửa sổ thành hàng dọc thân, chiều dài rõ rệt hơn mọi lớp khác (như xe buýt ở drive_052, 053) |
    | `van` |Xe thùng hoặc minivan, thân cao liền khối, cỡ giữa car và truck |Cao hơn car, không có cabin tách rời như truck, ngắn hơn bus |

    ## Quy tắc chung

- Box bám sát vật thể: chữ nhật nhỏ nhất ôm phần nhìn thấy của thân xe, gồm bánh, không gồm bóng đổ hay mặt đường.
- Vật thể bị che khuất: vẽ box theo phần nhìn thấy, không đoán phần bị che.
- Vật thể bị cắt bởi rìa ảnh: vẽ box tới sát mép ảnh, giữ nhãn lớp như khi xe nguyên vẹn.
- Vật thể rất nhỏ/mơ hồ: chỉ gán nhãn khi nhìn ra là xe và xác định được lớp; nếu không xác định được lớp thì ghi lại ca đó và áp dụng cùng một cách cho mọi ảnh.

    ## Ghi lại quyết định cho ít nhất ba ca mơ hồ

    | Ảnh | Tình huống | Quyết định cuối | Lý do theo schema |
    | --- | --- | --- | --- |
    | `drive_012` |xe ở mép ảnh chỉ hiện thị 15% xe  | không gán nhãn | |
    | `drive_041` |xe trắng phía bên trái mép màn hình không phân biệt được van/car |quyết định gán car |xe không phải xe thùng/ thân cao liền khối, thân ngắn phù hợp hơn với car |
    | `drive_047` | Chiếc xe bạc ở mép trái bị cắt mất phần đầu, đi gần chiếc SUV trắng và một xe tải thùng kín màu trắng bên phải| Gán van cho xe bạc (cạnh trái chạm mép ảnh), car cho chiếc SUV trắng và truck cho xe tải thùng kín | Xe bạc vẫn lộ rõ thân hộp cao liền khối và cửa lùa đặc trưng của van; SUV có đầu ca-pô thuộc car, còn xe có cabin rời thùng hàng vuông phía sau thuộc truck.|
