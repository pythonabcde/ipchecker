# IP地址检测工具

一个简洁、美观的IP地址检测网页，通过多个检测源实时查询您的IP地址。

## 特性

- ✨ **多源检测**: 集成22个IP检测API（包含9个中国检测源）
- 📱 **响应式设计**: 完美适配手机和PC端浏览器
- ⚡ **实时显示**: 检测结果实时显示，无需等待
- 🎨 **美观界面**: 现代化UI设计，渐变背景
- 📊 **统计信息**: 显示成功/失败检测数量
- 🌐 **多地区覆盖**: 包含国内外多个检测源

## 检测源列表

### 国际检测源 (13个)
- ipify
- ipapi.co
- ip-api
- seeip
- ipinfo.io
- cloudflare
- icanhazip
- ident.me
- ipecho
- myexternalip
- wtfismyip
- freeipapi
- bigdatacloud
- ip.nf

### 中国检测源 (9个)
- ip.sb
- cip.cc
- pconline
- ipip.net
- chinaz
- ip138
- vore
- tencent
- ipw.cn

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

## 注意事项

- 某些API可能因为网络原因无法访问
- 部分API可能有访问频率限制
- 建议在HTTPS环境下使用以避免混合内容问题

## License

MIT License

## 致谢

感谢所有提供免费IP检测API的服务提供商。
