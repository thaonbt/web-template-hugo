# web-template-hugo

Template repo dùng để dựng site Hugo mới nhanh chóng, quản lý theme qua **Hugo Modules** (không dùng git submodule).

## Yêu cầu môi trường

| Công cụ               | Version tối thiểu                           | Ghi chú                                                                                                                                                |
| ------------------------- | ----------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [Hugo](https://gohugo.io/) | Extended                                      | Bắt buộc bản Extended để hỗ trợ SCSS/Sass mà đa số theme hiện đại cần. Kiểm tra bằng`hugo version`, output phải có`+extended` |
| [Go](https://go.dev/)      | >= 1.12 (khuyến nghị dùng bản mới nhất) | Cần cho Hugo Modules resolve theme qua Go module system                                                                                                |
| Git                     | Bất kỳ                                      | Chỉ dùng clone/push bình thường,**không cần** `--recurse-submodules`                                                                   |

## Setup lần đầu (sau khi clone hoặc "Use this template")

```bash
git clone git@github-thaonbt:thaonbt/web-template-hugo.git
cd web-template-hugo
```

Không cần thêm bước nào khác để lấy theme — Hugo Modules tự resolve dependency khai báo trong `go.mod` khi build.

## Chạy local

```bash
hugo server -D
```

Mở `http://localhost:1313`. Flag `-D` để hiển thị cả draft content khi đang phát triển.

## Cấu trúc thư mục

```
.
├── archetypes/     # template mặc định cho content mới (hugo new)
├── content/        # bài viết / page thật
├── layouts/        # override layout của theme (nếu cần custom)
├── static/         # asset tĩnh (ảnh, font...)
├── themes/         # KHÔNG dùng — theme quản lý qua Hugo Modules, không nằm ở đây
├── go.mod          # khai báo theme + version Go
├── go.sum          # checksum dependency, không sửa tay
└── hugo.toml       # config chính, bao gồm khai báo [module.imports]
```

## Quản lý theme qua Hugo Modules

Theme đang khai báo trong `hugo.toml`:

```toml
[module]
  [[module.imports]]
    path = "github.com/luizdepra/hugo-coder"
```

### Update theme lên version mới nhất

```bash
hugo mod get -u github.com/luizdepra/hugo-coder
hugo mod tidy
```

### Đổi sang theme khác

```bash
hugo mod get github.com/<theme-author>/<theme-name>
```

Sửa lại `path` trong `[[module.imports]]` của `hugo.toml`, sau đó:

```bash
hugo mod tidy
```

### Xoá cache module (khi gặp lỗi resolve dependency lạ)

```bash
hugo mod clean
```

## Tạo content mới

```bash
hugo new content posts/ten-bai-viet.md
```

## Build production

```bash
hugo --minify
```

Output nằm ở thư mục `public/` (đã có trong `.gitignore`, không commit).

## Vì sao dùng Hugo Modules thay vì git submodule

|                                     | git submodule                    | Hugo Modules (đang dùng) |
| ------------------------------------- | ---------------------------------- | ---------------------------- |
| Cần cài thêm gì                 | Không                           | Go                         |
| Clone cần flag đặc biệt         | Có (`--recurse-submodules`) | Không                     |
| Cách Hugo khuyến nghị hiện tại | Không                           | Có                        |

Đánh đổi: người dùng lại template này cần cài Go trước khi build — bù lại quy trình clone và update theme đơn giản hơn, đúng hướng phát triển chính thức của Hugo.

## Ghi chú

Đây là template repo (`Settings > Template repository` đã bật) — dùng nút **"Use this template"** trên GitHub để tạo site thật từ base này thay vì fork hoặc clone trực tiếp.

