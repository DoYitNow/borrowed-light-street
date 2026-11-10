# 借光街

一款用手提灯与日常物件拼出真实投影的皮影小游戏。沿小街前进，操作机关和晾衣绳，在最后的房间里让钟、大衣、椅子与收拢雨伞形成“归人”。

## 游玩

- 浏览器版：本仓库的 GitHub Pages 页面。
- Windows 版：在仓库右侧 **Releases** 下载完整 ZIP，全部解压后启动 `BorrowedLightStreet.exe`。
- `A / D` 移动，`Shift` 跑，`Space` 跳；`E` 拿放灯，`F` 近距离操作或取挂物件，`L` 切换区域灯光，`R` 重新开始。
- 灯放稳后投影会稳定。第三幕可把灯放在左帷幕旁的抽屉台上观察人物和独立伞影。

Windows ZIP 附中文玩法和版本记录；同一 Release 的 `SHA256SUMS.txt` 与 `release-manifest.json` 可核对下载与完整运行依赖。

## 发布目录约定

本仓库仅存可公开的发行产物与部署说明。不要把 Unity 工程、源素材、私人布置覆盖、验证日志或 Windows ZIP 提交到 Git。

```text
.github/workflows/pages.yml
README.md
release.json
```

Windows ZIP 与 WebGL 产物均作为 **GitHub Release 附件**保存，避免把超过 Git 单文件限制的数据提交到仓库。

将本地验证通过的 WebGL 导出压成固定名称 `BorrowedLightStreet-WebGL.zip` 并上传到对应 Release。ZIP 根目录应直接包含 `index.html`、`Build/` 和导出所需的其他文件；也接受单个外层目录。`release.json` 保存已验证的 Release tag 与 ZIP 的 SHA256。工作流下载该附件，核对 SHA256，再安全解压为临时 `web/` 目录，仅上传这个站点到 Pages。`web/` 不入 Git。

维护者可在本地原工程执行 `python Tools/Package-WebGLPages.py --source <已验证的WebGL导出目录> --output <全新的站点目录> --report Validation/WebGL/pages-package.json`，将大的压缩数据拆成不超过 32 MiB 的文件。工具逐片核对 SHA256 并离线核对重组结果，保留原 loader、WASM 和压缩数据内容；游戏页面下载校验后再启动。将新的站点目录压成上述 ZIP，再通过真实浏览器验证后发布。网页仅显示游戏画面，自动加载；首次交互请求浏览器全屏。

维护者在本地 Unity 原工程执行：`Tools/Set-GitHubPagesRelease.ps1 -Tag <版本标签> -WebGLZip <已验证ZIP路径>`。这些维护工具不包含在公开发行仓库中。该命令只写独立发布目录的 `release.json`，不登录或操作远程仓库。也可在 Actions 的 **Run workflow** 输入 `tag`、`sha256`；留空时使用仓库配置。

仓库默认分支使用 `main`。在 GitHub 仓库 **Settings → Pages → Build and deployment → Source** 选择 **GitHub Actions**。上传 Release 附件并提交正确的 `release.json` 后会自动部署，也可手动运行工作流。仓库本身作为独立发布根目录，不要在 Unity 工程根目录执行发布提交。未设置已验证 tag/hash 时，工作流会拒绝空发布。

维护者在本地原工程执行 `Tools/Build-InteractionRevision.ps1 -Validate` 后，使用 `Tools/Package-GitHubRelease.ps1 -DeliveryPath Validation/InteractionRevision/delivery.json` 从交付证书指定的发行目录生成 Windows 包，输出为 `Publish/WindowsRelease/<版本>/`。将其中 ZIP、SHA256 与清单作为 Release 附件上传，避免 Git 的大文件限制。

Pages 工作流采用官方 [upload-pages-artifact](https://github.com/actions/upload-pages-artifact) 与 [deploy-pages](https://github.com/actions/deploy-pages)，只需要仓库默认的 `GITHUB_TOKEN`；不在仓库写入个人访问令牌。
