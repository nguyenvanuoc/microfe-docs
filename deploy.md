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
Cấu hình trong file `webpack.prod.config.js`

```typescript
const { ModuleFederationPlugin } = require("webpack").container;

/** @type {require('webpack').Configuration} */
module.exports = {
    output: {
        publicPath: "https://microfontend-dev.vercel.app/", // we setup the `publicHost` in `angular.json` file
        uniqueName: "start",
    },
    optimization: {
        runtimeChunk: false,
    },
    experiments: {
        // Allow output javascript files as module source type.
        outputModule: true,
    },
    module: {
        rules: [
            {
                test: /\.(less)$/,
                use: [
                    {
                        loader: "less-loader", // compiles Less to CSS
                        options: {
                            lessOptions: {
                                javascriptEnabled: true,
                            },
                        },
                    },
                ],
            },
        ],
    },
    plugins: [
        new ModuleFederationPlugin({
            library: {
                // because Angular v14 will output ESModule
                type: "module",
            },
            remotes: {
                mailbox:
                    "https://microfontend-dev.vercel.app/mailbox/remoteEntry.js",
                calendar:
                    "https://microfontend-dev.vercel.app/calendar/remoteEntry.js",
                home: "https://microfontend-dev.vercel.app/home/remoteEntry.js",
                example:
                    "https://microfontend-dev.vercel.app/example/remoteEntry.js",
                login: "https://microfontend-dev.vercel.app/login/remoteEntry.js",
                account:
                    "https://microfontend-dev.vercel.app/account/remoteEntry.js",
            },
            /**
             * shared can be an object of type SharedConfig
             * you can change this to select something can be shared
             */
            shared: {
                "@angular/core": {
                    eager: true,
                    singleton: true,
                    requiredVersion: false,
                },
                "@angular/common": {
                    eager: true,
                    singleton: true,
                    requiredVersion: false,
                },
                "@angular/common/http": {
                    eager: true,
                    singleton: true,
                    requiredVersion: false,
                },
                "@angular/router": {
                    eager: true,
                    singleton: true,
                    requiredVersion: false,
                },
                "@angular/forms": {
                    eager: true,
                    singleton: true,
                    requiredVersion: false,
                },
                "ng-zorro-antd/tabs": {
                    eager: true,
                    singleton: true,
                    requiredVersion: false,
                },
                "ng-zorro-antd/modal": {
                    eager: true,
                    singleton: true,
                    requiredVersion: false,
                },
                "ng-zorro-antd/message": {
                    eager: true,
                    singleton: true,
                    requiredVersion: false,
                },
                "ng-zorro-antd/notification": {
                    eager: true,
                    singleton: true,
                    requiredVersion: false,
                },
                "ng-zorro-antd/icon": {
                    eager: true,
                    singleton: true,
                    requiredVersion: false,
                },
                "ng-zorro-antd/i18n": {
                    eager: true,
                    singleton: true,
                    requiredVersion: false,
                },
                "ng-zorro-antd/table": {
                    eager: true,
                    singleton: true,
                    requiredVersion: false,
                },
                "@ant-design/icons-angular": {
                    eager: true,
                    singleton: true,
                    requiredVersion: false,
                },
                "ng-zorro-antd/form": {
                    eager: true,
                    singleton: true,
                    requiredVersion: false,
                },
                "ng-zorro-antd/dropdown": {
                    eager: true,
                    singleton: true,
                    requiredVersion: false,
                },
                "ng-zorro-antd/collapse": {
                    eager: true,
                    singleton: true,
                    requiredVersion: false,
                },
                "ng-zorro-antd/tooltip": {
                    eager: true,
                    singleton: true,
                    requiredVersion: false,
                },
                "ng-zorro-antd/spin": {
                    eager: true,
                    singleton: true,
                    requiredVersion: false,
                },
                "ng-zorro-antd/core": {
                    eager: true,
                    singleton: true,
                    requiredVersion: false,
                },
                "@angular/cdk/overlay": {
                    eager: true,
                    singleton: true,
                    requiredVersion: false,
                },
            },
        }),
    ],
};

```

Với đường dẫn `https://microfontend-dev.vercel.app/` là ứng tên miền chứa app

Nếu triển khai trên cùng 1 hệ server
Copy danh sách apps trong file start vào thư mục gốc, đồng thời các app con vào trong cùng 1 thư mục


#### Cấu hình server nginx 

Sau khi cài đặt, bạn cần cấu hình Nginx để phục vụ các tệp tĩnh từ thư mục xây dựng của các ứng dụng Angular. Dưới đây là một cấu hình ví dụ:


```bash
server {
    listen 80;
    server_name your_domain.com;

    root /path/to/your/angular/start;

    location /microapp1/ {
        alias /path/to/your/angular/apps/microapp1/;
        try_files $uri $uri/ /microapp1/index.html;
    }

    location /microapp2/ {
        alias /path/to/your/angular/apps/microapp2/;
        try_files $uri $uri/ /microapp2/index.html;
    }

    location /microapp3/ {
        alias /path/to/your/angular/apps/microapp3/;
        try_files $uri $uri/ /microapp3/index.html;
    }

    location / {
        try_files $uri $uri/ /index.html;
    }
}
```