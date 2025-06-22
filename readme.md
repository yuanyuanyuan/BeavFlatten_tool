<div align="center">
  <img src="assets/logo.png" alt="BeavFlatten Logo" width="150"/>
  <h1 align="center">BeavFlatten (狸压侠)</h1>
  <p align="center">
    <i>"Like a beaver building a dam, we flatten your archives."</i>
    <br />
    <a href="https://yuanyuanyuan.github.io/BeavFlatten_tool/"><strong>Explore the Tool »</strong></a>
    <br />
    <br />
    <a href="README.md">English</a> | <a href="README.zh-CN.md">简体中文</a> | <a href="README.zh-TW.md">繁體中文</a>
  </p>
</div>

---

## About The Project

**BeavFlatten** is a powerful, client-side web tool designed to instantly flatten complex directory structures within ZIP archives. Have you ever downloaded a project from GitHub and found yourself digging through endless nested folders? BeavFlatten solves this by extracting all files, moving them to a single top-level directory, and intelligently renaming them based on their original path.

It's fast, secure, and incredibly easy to use.

### Built With

*   **HTML5, CSS3, JavaScript (ES6+)**
*   **Tailwind CSS**: For a modern, responsive UI.
*   **JSZip.js**: For client-side ZIP processing.
*   **FileSaver.js**: For enabling file downloads in the browser.

## Key Features

✨ **Purely Client-Side**: No file uploads. Your data stays 100% private and secure in your browser.
🗂️ **Directory Flattening**: Intelligently converts `path/to/your/file.txt` into `path--to--your--file.txt`.
🧠 **Smart Exclusion**: Automatically ignores common junk like `.git` folders and `.DS_Store` files.
🔧 **Configurable Exceptions**: You have full control to specify files or folders that should *not* be excluded.
🌍 **Multi-language Support**: Full UI support for English, Simplified Chinese (简体中文), and Traditional Chinese (繁體中文).
🚀 **Two Versions, One Goal**: Explore two different UX approaches with the **DeepSeek** and **GeminiPro** versions.

## Getting Started

There's no installation required! Simply visit our official website:

**[https://yuanyuanyuan.github.io/BeavFlatten_tool/](https://yuanyuanyuan.github.io/BeavFlatten_tool/)**

### How to Use

1.  **Visit the site**: Open the link above.
2.  **Choose a version**: Select either the DeepSeek or GeminiPro version.
3.  **Upload your ZIP**: Drag and drop your `.zip` file onto the upload area, or click to select it.
4.  **Configure (Optional)**: Adjust the exception list if needed.
5.  **Process**: Click the "Process" button.
6.  **Download**: Download your newly flattened `.zip` file!

## Demo

<div align="center">
  <video src="assets/demo-03.mp4.mp4" width="700" controls autoplay muted loop playsinline>
    Your browser does not support the video tag.
  </video>
</div>

## Roadmap

- [x] Core Flattening Logic
- [x] Multi-language Support
- [x] Modern, Responsive UI
- [x] SEO and AI Search Optimization
- [ ] Automated Deployment with GitHub Actions
- [ ] Advanced configuration options
- [ ] Support for other archive formats (e.g., `.tar.gz`)

See the [open issues](https://github.com/yuanyuanyuan/BeavFlatten_tool/issues) for a full list of proposed features (and known issues).

## Contributing

Contributions are what make the open source community such an amazing place to learn, inspire, and create. Any contributions you make are **greatly appreciated**.

If you have a suggestion that would make this better, please fork the repo and create a pull request. You can also simply open an issue with the tag "enhancement".

1.  Fork the Project
2.  Create your Feature Branch (`git checkout -b feature/AmazingFeature`)
3.  Commit your Changes (`git commit -m 'Add some AmazingFeature'`)
4.  Push to the Branch (`git push origin feature/AmazingFeature`)
5.  Open a Pull Request

## License

Distributed under the MIT License. See `LICENSE.txt` for more information.

## Contact

Stark Yuan - [yuanyuanyuan (at) example.com](mailto:yuanyuanyuan@example.com)

Project Link: [https://github.com/yuanyuanyuan/BeavFlatten_tool](https://github.com/yuanyuanyuan/BeavFlatten_tool)

## Acknowledgments

*   [JSZip](https://stuk.github.io/jszip/)
*   [FileSaver.js](https://github.com/eligrey/FileSaver.js/)
*   [Tailwind CSS](https://tailwindcss.com/)
*   [Heroicons](https://heroicons.com/)

---
<div align="center">
  <p>Crafted with ❤️ by <a href="https://github.com/yuanyuanyuan">Stark Yuan</a></p>
</div>
