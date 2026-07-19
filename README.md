<div align="center">

# [网腾无限AI - 科学胎教与胎动音律专家]

**[一个支持八音盒印章打卡与五种特色胎教流派的科学胎教与胎动音律工具，具备深色玻璃拟态自适应交互与微信端 H5 体验]**

[Vue 3] · [TypeScript] · [Vite] · [Vanilla CSS] · [开源协议 MIT]

[![GitHub stars](https://img.shields.io/github/stars/WT-Agent/ai-taijiao?style=social)](https://github.com/WT-Agent/ai-taijiao)
[![GitHub license](https://img.shields.io/github/license/WT-Agent/ai-taijiao)](https://github.com/WT-Agent/ai-taijiao/blob/main/LICENSE)

[在线演示](#在线演示) · [快速启动](#快速启动) · [参与贡献](#参与贡献) · [支持一下](#支持一下)

</div>

## 关于我们

团队成员均来自 C9 等顶尖学府，在字节、腾讯、阿里的工程师组成，全职创业研发开源 AI 应用产品，让所有人感受 AI 的魅力。

本项目旨在为音乐胎教、语言对话、抚触互动、孕妇冥想及爸爸参与群体提供高品质的科学胎教与胎动音律服务。用户只需输入孕周数与环境细节，AI 即可根据多维科学度看板自动输出胎宝发育月龄与感官特征、科学胎教音律与曲目建议、爸爸对话脚本与亲子故事及孕妇身心放松与胎动呼应。页面内置了支持梦幻八音盒晶莹音效的“八音盒”印章，供用户在胎教体验时增加仪式感。

**我们不搞概念，不卖课，只写能跑起来的代码。**

欢迎 Star、Fork、提 Issue，一起让这个项目变得更好用。

核心特性：
- **极简自适应交互**：提供毛玻璃质感的深色玻璃拟态自适应 Web 界面，高度适配移动端 H5 微信浏览器与 PC 体验。
- **胎音律动八音盒打卡印章 (Fetal Music Box Stamp)**：基于前端 Web Audio API 动态合成梦幻八音盒晶莹音效，点击印章即可累积胎教打卡次数并伴随渐隐上升动画。
- **五大科学胎教流派**：
  - **莫扎特音乐与音律熏陶流**：基于莫扎效应与低频乐音，注重听觉神经滋养。
  - **爸爸对话与亲子语言流**：强调父亲中频声音穿透力，提供深情亲子对话、童谣。
  - **抚触律动与游戏互动流**：结合腹部温和抚触、律动推压、光照互动。
  - **自然声响与静心冥想流**：融入溪流鸟鸣、白噪音、孕妇瑜伽呼吸。
  - **艺术审美与早启智育流**：引入高雅诗词吟诵、经典名画色彩描述。
- **AI 胎教质量看板**：自动提取 AI 回复中的共识数据，以简洁的单轨进度条在前端直观展示听觉安全度、亲子联结度、胎动呼应度、孕妇放松度及发育契合度。
- **演示案例与分享卡片**：内置 30 条不同主题的精选科学胎教精彩演示样例，并支持一键卡片化截图分享。
- **一键零成本部署**：纯前端静态网页结构，支持零成本部署于 Vercel、GitHub Pages 或 CDN/OSS 静态托管服务。
- **安全开发代理**：本地开发支持使用个人 API 密钥发起代理请求，密钥由 Vite 服务器中转，无需担心前端泄露。
- **裂变解锁与留存**：内置微信朋友圈扫码分享拦截与额度重置机制，提升流量转化与留存。

## 快速启动

### 1. 克隆项目
```bash
git clone https://github.com/WT-Agent/ai-taijiao.git
cd ai-taijiao
```

### 2. 安装依赖
项目强制使用 pnpm 作为包管理器：
```bash
pnpm install
```

### 3. 配置本地开发环境变量
复制并修改环境变量配置文件：
```bash
cp .env.example .env
```
根据微应用的功能类型，在 `.env` 中配置您的开发者密钥：
- `DEEPSEEK_API_KEY`: 您的 DeepSeek 开发者 API 密钥（用于文本生成任务）
- `DASHSCOPE_API_KEY`: 您的通义千问/通义万相开发者 API 密钥（用于多模态与生图任务）

### 4. 启动本地开发服务
```bash
pnpm dev
```
启动成功后在浏览器访问控制台输出的地址即可。

### 5. 生产构建打包
```bash
pnpm build
```
打包后生成的 `dist` 目录即为纯静态网页资源，可直接上传部署。

## 脚手架集成说明

本模板由私有总控仓库 `ai.wuxian.xyz` 中的 `@wuxian/cli` 脚手架统一管理，支持以下批量运维操作：

### 初始化或更新单个子项目

```bash
node bin/cli.js ai-taijiao
```

脚手架将自动：
1. 读取子仓库的 `README.md` 首行作为 Prompt 主题。
2. 注入 Vue 3 静态页面结构及标准配置文件。
3. 保留原有的 `.git` 配置与 `README.md`，不覆盖个性化内容。

### 批量同步所有子项目

```bash
node bin/cli.js all
```

将模板 the latest 变更（如 SSO 逻辑、额度控制）一键同步至全部 31 个子项目。

### Agent 配置维护接口

```bash
# 读取子项目配置
node bin/cli.js get ai-taijiao

# 写入/更新配置（支持热更新 prompt、model、title、temperature 等）
node bin/cli.js set ai-taijiao prompt "你是一位资深胎教音律大师、围产期心理学家兼亲子语言启蒙导师..."
node bin/cli.js set ai-taijiao model deepseek-chat
```

## 联系方式

- GitHub Issues: [提交反馈](https://github.com/WT-Agent/ai-taijiao/issues)
- 邮箱: us@wuxian.xyz

## 打赏支持

如果本项目对您有帮助，欢迎请作者喝杯咖啡。您的支持是持续维护与更新的动力。

<div align="center">

**微信支付** | **支付宝**
:---:|:---:
<img src="./asset/tenpay.png" width="200" alt="微信支付"> | <img src="./asset/alipay.png" width="200" alt="支付宝">

</div>

## 版权与许可

本项目基于 MIT License 开源协议。

Copyright (c) 2026. All rights reserved.
