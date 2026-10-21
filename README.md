[README.md](https://github.com/user-attachments/files/33786086/README.md)
# OBS 页面壁纸 · OBS Page Wallpaper

![Platform](https://img.shields.io/badge/platform-Windows%2010%20%7C%2011-0078D4)
![OBS](https://img.shields.io/badge/OBS-32.x-2f2f37)
![License](https://img.shields.io/badge/license-MIT-3DA639)

**给 OBS Studio 主界面换一张自己的壁纸。**  
场景、来源、混音器和控制面板会变成半透明玻璃，壁纸从面板后透出来；直播画面、场景和推流内容完全不受影响。

Custom wallpaper for the OBS Studio UI — translucent glass panels, a small configurator, zero impact on your stream.

> **这不是场景贴图 / 不是直播 overlay。**  
> 它只改 OBS 这个软件本身的界面，不会进入直播或录像画面。

**下载：** [设置器 exe](dist/OBS页面壁纸设置器.exe) · [完整 ZIP](dist/OBS-Page-Wallpaper-v1.0.zip)

![设置器界面](docs/app.png)

## 功能

- 支持任意 JPG / PNG / BMP / GIF 图片，也可以直接把图片拖进窗口
- 三种显示方式：填充裁切 / 完整适应 / 拉伸铺满
- 「面板不透明度」可调：想让壁纸更明显就调低，想更易读就调高
- 「画面压暗」可调：亮色壁纸也不会让文字看不清
- 实时模拟预览
- 一键恢复默认外观
- 只写入 OBS 主题目录，**不会删除或修改你的场景、来源、录像、其它主题**

## 使用（推荐）

1. 双击 `dist\OBS页面壁纸设置器.exe`（绿色免安装，不需要管理员权限）。
2. 选择图片，或直接把图片拖进窗口；调一下不透明度和压暗。
3. 点 **「应用到 OBS」**。
4. **完全退出 OBS**（包括托盘里的 OBS），再重新打开。
5. 如果壁纸没有自动出现：打开 `设置 → 外观`，在主题里选择 **「页面壁纸」**。

第一次应用后，程序还会在 OBS 的 `user.ini` 里写入 `Theme=local.pagewallpaper`，所以正常情况下重启就会自动使用该主题。

## 恢复默认

点程序里的 **「恢复默认外观」**。它只会：

- 删除本工具添加的 `PageWallpaper.ovt` 和 `PageWallpaper\wallpaper.png`
- 把 OBS 主题恢复成安装前的那个主题

**不会删除**你的场景、来源、录像、插件或其它主题。如果 OBS 正在运行，程序会提示先退出，避免 OBS 退出时把设置覆盖回去。

## 文件写到哪里

```
%APPDATA%\obs-studio\themes\PageWallpaper.ovt
%APPDATA%\obs-studio\themes\PageWallpaper\wallpaper.png
%APPDATA%\obs-studio\user.ini        （只改 [Appearance] 的 Theme 一行）
```

程序自己的设置和上次的图片路径保存在：

```
%APPDATA%\OBS-Page-Wallpaper\state.ini
```

## 手动安装 / 卸载

如果不想用设置器：

1. 把 `PageWallpaper.ovt` 和 `PageWallpaper\wallpaper.png` 放到 `%APPDATA%\obs-studio\themes\`。
2. 在 OBS 的 `设置 → 外观 → 主题` 里选择「页面壁纸」，重启 OBS。
3. 卸载：在 OBS 里换回其它主题，然后删除上面两个路径即可。

## 为什么是主题，不是 DLL 插件？

OBS 没有向 Lua / Python 脚本开放“修改主界面壁纸”的 API，直接换 UI 背景需要改 Qt 前端。最稳妥、可跨 OBS 小版本使用的方式，是 OBS 官方支持的 **主题扩展（.ovt）**：

- `PageWallpaper.ovt` 继承 OBS 自带的 Yami 主题
- 用 `border-image` 把壁纸铺满主窗口
- 用 QSS 把 `OBSDock` 面板做成半透明玻璃
- 设置器负责选图、裁切、压暗、写入主题和切换主题

所以它是“主题型插件 + 图形设置器”，不是 `.dll` 原生插件，但安装和使用方式就是普通 OBS 插件：选图 → 应用 → 重启 OBS。

## 兼容性

- 在 **OBS Studio 32.2.2 (64-bit)** 上开发、验证
- 使用 OBS 31+ 的新主题格式（`.ovt` + `@OBSThemeMeta`）
- Windows 10 / 11，需要 .NET Framework 4.8（系统自带）
- 只对当前 Windows 用户生效

## 常见问题

**Q：重启后壁纸没变？**  
A：先确认 OBS 是完全退出后重开的；然后到 `设置 → 外观` 看主题是否为「页面壁纸」。如果之前手动改过主题，重新点一次「应用到 OBS」。

**Q：面板太透明，字看不清？**  
A：把「面板不透明度」调到 75–85；或者把「画面压暗」调高。亮色壁纸建议两个都调高。

**Q：会不会影响直播画面？**  
A：不会。主题只改 OBS 前端界面，不进入场景渲染，也不影响录像/推流。

**Q：OBS 更新后还在吗？**  
A：主题文件在用户目录，OBS 小版本更新通常保留；如果更新后主题被重置，重新应用一次即可。

## 目录结构

```
obs-page-wallpaper/
├─ dist/
│  └─ OBS页面壁纸设置器.exe       # 最终程序
├─ src/
│  └─ WallpaperConfigurator.cs    # C# WinForms 源码
├─ assets/
│  ├─ app.ico
│  └─ sample-aurora.png           # 内置示例壁纸
├─ docs/
│  ├─ app.png                     # 设置器截图
│  └─ preview.png                 # 效果预览
├─ build.ps1                      # 重新编译
└─ README.md
```

## 重新编译

需要 Windows 自带的 .NET Framework 4.x 编译器（`csc.exe`）：

```powershell
powershell -ExecutionPolicy Bypass -File .\build.ps1
```

---

如果你只是想要“场景里的网页贴图”，那是另一种东西（Browser Source / 场景叠加）；这个项目专门解决 **OBS 主界面壁纸**。
