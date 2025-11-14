# IP地址检测工具

一个功能全面的IP地址检测网页，集成IP检测、网络连通性测试、WebRTC泄漏检测、DNS检测等多项功能。

## 特性

- ✨ **多源IP检测**: 集成17个IP检测API（包含5个中国检测源，12个国际检测源）
- 🌐 **网络连通性测试**: 检测10个常用网站的访问速度和连通性（包含中国和国际站点）
- 🔒 **WebRTC泄漏检测**: 检测WebRTC可能泄漏的真实IP地址
- 🌍 **DNS泄漏检测**: 基于公共服务和地理位置数据进行DNS服务器检测
- 📱 **响应式设计**: 完美适配手机和PC端浏览器
- ⚡ **实时显示**: 检测结果实时显示，无需等待
- 🎨 **美观界面**: 现代化UI设计，渐变背景
- 📊 **统计信息**: 显示成功/失败检测数量
- 🔍 **系统信息**: 展示浏览器、操作系统、屏幕、插件等详细信息
- 📡 **HTTP请求头**: 显示浏览器发送的HTTP请求头信息（可选）

## 检测源列表

### 国际IP检测源 (12个)
- seeip
- cloudflare
- checkip.amazonaws
- ipecho.net
- ident.me
- icanhazip
- wtfismyip
- ifconfig.me
- ipinfo.io
- httpbin
- tnedi.me
- ipify

### 中国IP检测源 (5个)
- ip.sb (含地理位置信息)
- ip.sb-geoip
- myip.ipip.net
- myip.la
- cz88.net

### 连通性测试站点 (10个)

**中国站点 (2个)**:
- NetEase (网易)
- Bilibili (哔哩哔哩)

**国际站点 (8个)**:
- jsDelivr
- YouTube
- Google
- GitHub
- Docker
- ChatGPT
- Claude.ai
- ip.39203.com

## 检测机制说明

### IP地址检测
使用 `fetch` API 并发请求多个检测源，支持 JSON 和纯文本两种响应格式。自动聚合结果并显示最常见的IP地址。

### 网络连通性测试
使用**元素加载方法**（`<img>` 或 `<script>` 标签）检测网站可达性：
- 每个站点进行3次测试取平均延迟
- 延迟包含：DNS解析 + TLS握手 + 重定向 + 下载时间
- **不是** ICMP ping，而是完整的资源加载时间
- 超时时间：10秒

### WebRTC泄漏检测
通过 WebRTC API 创建 PeerConnection 并收集 ICE candidates，检测可能泄漏的本地IP地址。测试多个STUN服务器（Google、Cloudflare、Twilio等）。

### DNS泄漏检测
基于公共服务和地理位置数据进行最佳努力估算。结果仅供参考，无法100%确认DNS"泄漏"。

## 部署到GitHub Pages

### 方法一: 通过GitHub网页界面

1. 进入您的仓库设置 (Settings)
2. 找到 "Pages" 选项
3. 在 "Source" 下选择分支（通常是 `main` 或 `master`）
4. 选择根目录 `/` (root)
5. 点击 "Save" 保存
6. 等待几分钟，您的网站将在 `https://yourusername.github.io/ipchecker/` 上线

### 方法二: 使用自定义域名 (ip.mydomain.com)

1. 完成上述GitHub Pages设置
2. 在您的DNS服务商处添加CNAME记录:
   ```
   类型: CNAME
   主机: ip
   值: yourusername.github.io
   ```
3. 在仓库根目录创建 `CNAME` 文件，内容为：
   ```
   ip.mydomain.com
   ```
4. 在GitHub Pages设置中填入您的自定义域名
5. 等待DNS生效（可能需要几分钟到几小时）

## 本地测试

直接用浏览器打开 `index.html` 文件即可。

或者使用简单的HTTP服务器：

```bash
# Python 3
python -m http.server 8000

# Node.js
npx serve

# PHP
php -S localhost:8000
```

然后访问 `http://localhost:8000`

## 浏览器兼容性

- ✅ Chrome / Edge (推荐)
- ✅ Firefox
- ✅ Safari
- ✅ 移动端浏览器

## 技术栈

- 纯HTML + CSS + JavaScript
- 无需后端服务器
- 无外部依赖库
- 响应式设计
- 使用 Fetch API 进行网络请求
- 使用 WebRTC API 进行泄漏检测
- 使用元素加载（img/script）进行连通性测试

## 技术细节

### 连通性测试原理
本项目使用元素加载方法替代了传统的 `fetch` + `no-cors` 方案：

**为什么不使用 fetch + no-cors？**
- `no-cors` 模式下返回 opaque response，无法判断请求是否真正成功
- 快速失败（如网络错误、CORS错误）也会在短时间内返回，容易误判为"连接成功"
- 无法获取真实的响应状态码

**元素加载方法的优势：**
- 使用 `<img>` 或 `<script>` 标签加载资源
- 通过 `onload` 和 `onerror` 事件准确判断加载成功/失败
- 测量真实的资源加载时间（包含DNS、TLS、重定向、下载）
- 3次采样取平均值，减少网络抖动影响

### 多重采样机制
每个连通性测试站点会进行3次测试：
1. 第一次测试可能受DNS缓存、TLS会话复用影响
2. 连续3次测试取平均值
3. 更准确反映网络状况，减少偶然因素

### 隐私说明
- 所有检测均在浏览器端完成
- IP检测通过第三方API进行（见检测源列表）
- WebRTC检测通过STUN服务器进行
- DNS检测基于IP地理位置推断
- HTTP Headers检测使用 httpbin.org（可选，失败不影响其他功能）
- 本项目不收集、不存储任何用户数据

## 注意事项

- 某些API可能因为网络原因无法访问（防火墙、企业代理、地区封锁等）
- 部分API可能有访问频率限制
- 建议在HTTPS环境下使用以避免混合内容问题
- 连通性测试的"延迟"不是ICMP ping，而是完整的资源加载时间
- DNS泄漏检测结果仅供参考，无法100%确认
- WebRTC检测需要浏览器支持 WebRTC API

## License

MIT License

## 致谢

感谢所有提供免费IP检测API的服务提供商。
