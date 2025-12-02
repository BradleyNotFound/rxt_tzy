# 融销通实训项目（RongXiaoTong）

本仓库包含一个前后端分离的农业产品交易与服务平台实训项目。

## 技术栈

后端：Spring Boot、MyBatis、Spring Security、JWT 等
前端：Vue3、Vite、Vue Router、Vuex / Pinia、Tailwind CSS 等

本地运行
1. 启动后端（RongXiaoTong）

使用 IDE（IntelliJ IDEA / VS Code）导入 RongXiaoTong 目录为 Maven 项目

确保本地 MySQL 中已创建数据库并修改 src/main/resources/application.yml 中的连接信息

运行启动类：

RongXiaoTongApplication


后端默认端口（按你配置的实际端口填写，比如 9090）。

2. 启动前端（sale）
cd sale
npm install
npm run dev


默认开发地址一般为：

http://localhost:5173/

## 目录结构

```bash
.
├── RongXiaoTong/   # 后端：Spring Boot 项目
└── sale/           # 前端：Vue3 + Vite 项目

