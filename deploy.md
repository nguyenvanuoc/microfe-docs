---
layout: page
title: Đóng gói và triển khai
permalink: /deployment/
---
Việc phát triển, đóng gói và triển khai các MicroApp trong 1 hệ thống hướng frontend theo các bước sau

* Phát triển MicroApp
* Đóng gói và triển khai
* Tích hợp vào Switching Layer

Trong đó, quá trình đóng gói MicroApp sẽ phải lưu ý một số vấn đề. Nguyên nhân là trong hệ thống **Micro Frontend**, các **MicroApp được triển khai phân mảnh, tại các thư mục khác nhau, thậm chí là các server khác nhau**. Từ đó nảy sinh 1 số vấn đề

* Vấn đề về đường dẫn tới Asserts, đường dẫn tới Image, css, có thể bị nhầm lẫn
* Vấn đề về Lazyload module, đường dẫn tới các file js mở rộng sẽ bị sai lệch

```bash
#package.json
 "build:single-spa:start": "ng build start --configuration=production",
```

Bên trên là lệnh build MicoApp `start`.

**Tham số --deploy-url phải chính xác với thư mục, hoặc server triển khai MicroApp thực tế**


```bash
# Khi triển khai tất cả các MicroApp trên cùng 1 server
"build:single-spa:settings": "ng build settings --configuration=production --deploy-url /apps/apps/settings/",

#Khi triển khai MicroApp trên hệ thống server phân tán
"build:single-spa:settings": "ng build settings -configuration=production --deploy-url https://example.com:8080/",
```

Theo đó đường dẫn tới `main.js`, `asserts`... phải đúng với đường dẫn tại tham số `--deploy-url`

Build tất cả các microapp trong 1 workspace

```bash
# kết quả build tại thư mục /workspace/dist
cd ./workspace
npm run build:all
```
Nếu triển khai trên cùng 1 hệ server
Copy danh sách apps trong file start vào thư mục gốc, đồng thời các app con vào trong cùng 1 thư mục

