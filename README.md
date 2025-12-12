# 📅 开发路线图 (Roadmap) - 瓦的小抄

此文档用于追踪《瓦的小抄》小程序开发进度。

**核心目标**：构建一个基于"爬虫+小程序"的《无畏契约》点位速查工具。

## 🏗️ Phase 0: 基础设施搭建 (Infrastructure)

**工期估算**：1-2 天  
**目标**：跑通 Hello World，环境就绪。

### 后端 (Golang)
- [ ] 初始化 Go Modules (go mod init)
- [ ] 搭建 Gin 框架基础结构 (Router, Controller, Service)
- [ ] 配置 Viper 读取 config.yaml
- [ ] 编写 Makefile (run, build, clean)

### 数据库 & 存储
- [ ] 本地安装 MySQL 8.0+
- [ ] 创建数据库 valorant_helper
- [ ] 执行建表 SQL: 创建 agents 表和 lineups 表 (参考 PRD 3.2)
- [ ] 开通对象存储 (阿里云OSS/七牛云/腾讯云COS)
- [ ] 获取 OSS AccessKey/SecretKey 并配置到 config.yaml

### 小程序前端
- [ ] 注册微信小程序账号，获取 AppID
- [ ] 初始化微信开发者工具项目
- [ ] 引入 Vant Weapp 组件库 (npm init, npm i @vant/weapp)
- [ ] 配置小程序 app.json (Tabbar, 页面路由)

## 🕷️ Phase 1: MVP 核心功能 (Core Features)

**工期估算**：2 周  
**目标**：上线核心地图 (Ascent, Bind, Haven) 的点位查询功能。

### 1. 爬虫与数据 (Crawler & Data)
- [ ] Colly 基础脚本：抓取目标网站 (Valoplant/Blitz) 的列表页 HTML
- [ ] 详情页解析：编写 GoQuery 选择器提取图片 URL 和 标题
- [ ] 图片转存服务：实现 DownloadImage(url) -> UploadToOSS(data) -> Return NewURL
- [ ] 坐标清洗：编写正则提取 CSS left: %; top: % 坐标数据
- [ ] 入库逻辑：实现 GORM 模型绑定与批量 Insert
- [ ] 数据验证：手动跑通 1 个地图 (如 Ascent) 的 Sova 数据，检查坐标是否准确

### 2. 后端 API (Backend API)
- [ ] GET /api/v1/maps: 获取地图列表
- [ ] GET /api/v1/agents: 获取英雄列表
- [ ] GET /api/v1/lineups: 获取点位列表 (支持 query 参数: map_id, agent_id, side, skill)
- [ ] GET /api/v1/lineups/:id: 获取点位详情

### 3. 小程序前端 (Frontend)
- [ ] 首页 (Home)：实现地图卡片网格布局
- [ ] 地图页 (Map View) - 核心难点
  - [ ] 实现底图容器，支持双指缩放 (利用 scroll-view 或 movable-area)
  - [ ] 实现筛选栏 (英雄头像/技能图标/攻守方)
  - [ ] 实现 absolute 定位的点位渲染 (根据 API 返回的 x,y 百分比)
- [ ] 详情页 (Detail)
  - [ ] 展示 站位图/瞄准图/演示视频
  - [ ] 图片预览组件 (wx.previewImage)

### 4. 测试与部署 (Deploy)
- [ ] 购买/配置云服务器 (推荐 2核4G, 安装 Nginx + Go Binary)
- [ ] 购买域名并配置 SSL 证书 (小程序强制 HTTPS)
- [ ] 提交微信小程序审核

## 🚀 Phase 2: 数据扩充与优化 (Expansion)

**工期估算**：2 周

- [ ] 全量数据爬取：覆盖所有地图和所有英雄
- [ ] 搜索功能：后端实现模糊查询 (LIKE %keyword%)
- [ ] 前端性能优化：
  - [ ] 图片 CDN 缓存策略
  - [ ] 列表页增加骨架屏 (Skeleton)
- [ ] 用户反馈入口：添加"报错/反馈"按钮

## 🔮 Phase 3: 社区化 (Community - Future)

- [ ] 用户登录系统 (微信一键登录)
- [ ] "我的收藏"功能
- [ ] UGC 上传入口 (用户上传自己的点位图)
