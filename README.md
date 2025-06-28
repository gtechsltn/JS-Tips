# JS-Tips

## Chặn click chuột phải (Right Click) bằng JavaScript
```
<script>
    document.addEventListener('contextmenu', event => event.preventDefault());
</script>
```

## Disable phím tắt F12, Ctrl+S, Ctrl+U, etc.
```
<script>
    document.onkeydown = function(e) {
        if (e.key === "F12" || (e.ctrlKey && (e.key === "s" || e.key === "u"))) {
            e.preventDefault();
            return false;
        }
    };
</script>
```

## Yêu cầu xác thực (Authentication) cho các trang
```
[Authorize]
public ActionResult SecretPage()
{
    return View();
}
```

## Tránh gửi dữ liệu nhạy cảm sang client
+ Không render dữ liệu bảo mật trong HTML (ngay cả ẩn bằng display: none)
+ Chỉ trả dữ liệu nhạy cảm qua API có xác thực, hoặc render server-side trong nội bộ hệ thống.

## Đặt headers để chống cache và indexing

```
Response.Cache.SetCacheability(HttpCacheability.NoCache);
Response.Cache.SetNoStore();
```

## Chống tải ảnh
+ Hiển thị ảnh dưới dạng canvas, không phải <img>
+ Dùng watermark
+ Đổi ảnh sang background-image hoặc base64
+ Hoặc load ảnh bằng API có token (chống download trực tiếp)

## Sử dụng ViewRendering tại server thay vì expose JSON
+ Ví dụ: dùng PartialView từ server thay vì đẩy toàn bộ data ra client qua JavaScript hoặc JSON.

## Stream tài liệu từ server, không cung cấp đường dẫn trực tiếp
```
public ActionResult ViewFile(string id)
{
    var path = Server.MapPath($"~/App_Data/SecureFiles/{id}.pdf");
    var stream = new FileStream(path, FileMode.Open);
    return new FileStreamResult(stream, "application/pdf");
}
```

## Hiển thị tài liệu bằng <iframe> + Content-Disposition: inline
```
Response.AppendHeader("Content-Disposition", "inline; filename=document.pdf");
return File(fileBytes, "application/pdf");
```

## Chèn watermark với tên người dùng / IP / thời gian
```
var watermark = $"Confidential - {User.Identity.Name} - {DateTime.UtcNow}";
```

## Vô hiệu hóa chuột phải, phím tắt tải xuống
```
<script>
    document.addEventListener('contextmenu', e => e.preventDefault());
    document.addEventListener('keydown', e => {
        if ((e.ctrlKey && e.key === 's') || e.key === 'F12') {
            e.preventDefault();
        }
    });
</script>
```

## Hiển thị PDF bằng Viewer JS không hỗ trợ tải
+ Dùng PDF.js (của Mozilla): https://mozilla.github.io/pdf.js/
+ Không hiển thị nút tải file (có thể tùy chỉnh toolbar)

```
<iframe src="/pdfjs/web/viewer.html?file=/api/pdf/stream/123" style="width:100%; height:600px;"></iframe>
```

## Với video/audio:
+ Stream từ controller (không để đường dẫn .mp4 lộ)
+ Dùng HTML5 video player + chặn click chuột phải
+ Không hiển thị nút "Download"

## Phát hiện hành vi đáng ngờ
+ Log IP, User-Agent, thời điểm
+ Nếu người dùng tải quá nhiều, có thể block
+ Có thể sử dụng JS fingerprinting hoặc window.onbeforeunload để cảnh báo
