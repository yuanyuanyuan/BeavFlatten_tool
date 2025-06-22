<div align="center">
  <img src="assets/logo.png" alt="BeavFlatten Logo" width="150"/>
  <h1 align="center">BeavFlatten (狸压侠)</h1>
  <p align="center">
    <i>"像河狸筑坝一样，我们'压'平你的压缩包。"</i>
    <br />
    <a href="https://yuanyuanyuan.github.io/BeavFlatten_tool/"><strong>体验工具 »</strong></a>
    <br />
    <br />
    <a href="README.md">English</a> | <a href="README.zh-CN.md">简体中文</a> | <a href="README.zh-TW.md">繁體中文</a>
  </p>
</div>

---

## 关于项目

**BeavFlatten (狸压侠)** 是一个强大的、完全在浏览器端运行的网页工具，旨在即时"压"平 ZIP 压缩包中复杂的目录结构。您是否曾从 GitHub 下载项目后，陷入无休止的嵌套文件夹中？BeavFlatten 通过提取所有文件、将它们移动到单个顶级目录，并根据其原始路径智能重命名，完美解决了这个问题。

它快速、安全，并且极其易用。

### 技术栈

*   **HTML5, CSS3, JavaScript (ES6+)**
*   **Tailwind CSS**: 用于构建现代化的响应式界面。
*   **JSZip.js**: 用于在客户端处理 ZIP 文件。
*   **FileSaver.js**: 用于在浏览器中触发文件下载。

## 主要特性

✨ **纯客户端运行**: 无需上传文件。您的数据 100% 安全私密，只留在您的浏览器中。
🗂️ **目录扁平化**: 智能地将 `path/to/your/file.txt` 转换为 `path--to--your--file.txt`。
🧠 **智能排除**: 自动忽略 `.git` 文件夹、`.DS_Store` 文件等常见冗余内容。
🔧 **可配置例外**: 您可以完全控制，指定不应被排除的文件或文件夹。
🌍 **多语言支持**: 完整的用户界面，支持英语、简体中文和繁體中文。
🚀 **双版本，单目标**: 通过 **DeepSeek** 和 **GeminiPro** 两个版本，探索不同的用户体验。

## 开始使用

无需安装！只需访问我们的官方网站：

**[https://yuanyuanyuan.github.io/BeavFlatten_tool/](https://yuanyuanyuan.github.io/BeavFlatten_tool/)**

### 如何操作

1.  **访问网站**: 打开上面的链接。
2.  **选择版本**: 选择 DeepSeek 或 GeminiPro 版本。
3.  **上传您的 ZIP**: 将 `.zip` 文件拖放到上传区域，或点击选择。
4.  **配置 (可选)**: 如果需要，调整例外列表。
5.  **处理**: 点击"处理"按钮。
6.  **下载**: 下载您全新的、已扁平化的 `.zip` 文件！

## 演示

<div align="center">

[![BeavFlatten 演示](assets/demo-01.png)](https://yuanyuanyuan.github.io/BeavFlatten_tool/)

*点击图片，立即体验！*

</div>

## 路线图

- [x] 核心扁平化逻辑
- [x] 多语言支持
- [x] 现代化的响应式界面
- [x] SEO 与 AI 搜索优化
- [ ] 使用 GitHub Actions 实现自动化部署
- [ ] 更高级的配置选项
- [ ] 支持其他压缩格式 (如 `.tar.gz`)

查看 [open issues](https://github.com/yuanyuanyuan/BeavFlatten_tool/issues) 获取完整的功能建议（和已知问题）列表。

## 贡献

贡献是开源社区如此迷人的原因——在这里，我们学习、启发和创造。我们**非常感谢**您的任何贡献。

如果您有任何能让这个项目变得更好的建议，请 fork 本仓库并创建一个 pull request。您也可以简单地提交一个带有 "enhancement" 标签的 issue。

1.  Fork 本项目
2.  创建您的功能分支 (`git checkout -b feature/AmazingFeature`)
3.  提交您的更改 (`git commit -m 'Add some AmazingFeature'`)
4.  推送到分支 (`git push origin feature/AmazingFeature`)
5.  开启一个 Pull Request

## 许可证

根据 MIT 许可证分发。更多信息请参见 `LICENSE.txt` 文件。

## 联系方式

Stark Yuan - [yuanyuanyuan (at) example.com](mailto:yuanyuanyuan@example.com)

项目链接: [https://github.com/yuanyuanyuan/BeavFlatten_tool](https://github.com/yuanyuanyuan/BeavFlatten_tool)

## 致谢

*   [JSZip](https://stuk.github.io/jszip/)
*   [FileSaver.js](https://github.com/eligrey/FileSaver.js/)
*   [Tailwind CSS](https://tailwindcss.com/)
*   [Heroicons](https://heroicons.com/)

---
<div align="center">
  <p>由 <a href="https://github.com/yuanyuanyuan">Stark Yuan</a> 用 ❤️ 精心打造</p>
</div> 