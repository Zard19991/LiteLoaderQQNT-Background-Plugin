# LiteLoaderQQNT - background-plugin

LiteLoaderQQNT插件，用于自动轮换QQNT的背景图片或者视频，并自带一些CSS透明度优化（参考自[LiteLoaderQQNT-Test-Theme](https://github.com/mo-jinran/test-theme)）。
使用前需要安装[LiteLoaderQQNT](https://github.com/mo-jinran/LiteLoaderQQNT)，并在QQNT新版上使用。

建议加入更新日志频道，接收最新更新和使用提示[https://t.me/llqqnt_xh](https://t.me/llqqnt_xh)。

## 使用方法

*建议从`LiteLoaderQQNT`应用商店中直接下载安装，方便快捷。*（新版本1.0`LiteLoaderQQNT`没有插件商店了，请遵循[手动安装方法](https://liteloaderqqnt.github.io/guide/plugins.html)）

**版本不兼容提示**：从0.1.18起，插件已适配1.0版本以上`LiteLoaderQQNT`框架，同时不再兼容旧版框架，请遵循[安装方法](https://liteloaderqqnt.github.io/guide/install.html)更新框架。

**版本不兼容提示**：从0.1.24起，插件的背景视频功能需要1.0.3及以上`LiteLoaderQQNT`框架才支持，请遵循[安装方法](https://liteloaderqqnt.github.io/guide/install.html)更新框架。

**版本提示**：自0.1.25起，配置文件和默认的图片文件夹从插件根目录移动至`LiteLoaderQQNT数据目录`中的background文件夹中，更新此版本后请手动移动config.json（和图片文件夹，如果你往里面存了图片）到对应路径。仅从低版本升级上来的用户需要操作，首次安装本版本不需要。

**版本提示**：自0.1.29起，网络图片会自动保存到**插件数据目录（`LiteLoaderQQNT数据目录\background`）**下面的tmp文件夹，这样在开启多窗体共用背景的情况下可以避免多个窗体重复请求同样的图片。并且如果你随机到了你喜欢的图片，可以前往文件夹里手动复制，后期版本会提供保存到指定目录按钮。



也可以clone（记得执行`npm i`）或下载zip文件解压，保留文件夹结构（文件夹名称为`插件名`，内容为github上的内容），将文件夹移动至`LiteLoaderQQNT数据目录/plugins/`下面，重启QQNT即可。

- 启动QQ后会自动写入默认配置文件到**插件数据目录（`LiteLoaderQQNT数据目录\background`）下面的`config.json`**，然后你对配置文件做的任何修改都会被插件实时应用（详见后文）。若无必要，不建议手动修改`config.json`；
- 你还可以通过QQ设置里的背景插件设置界面对插件进行设置（推荐这种方法，更方便，也能实时应用）。

**如果出现错乱、设置修改后无法应用或无法加载背景的情况，请确认`config.json`是否被正确配置；若不能确定，可先前往设置界面恢复默认设置；如果连设置界面也进不去，那就手动删除`config.json`后重启NTQQ再试（恢复默认后无背景，请去设置背景后再看看有没有问题），若还有问题请发ISSUE。**

可搭配[MSpring主题](https://github.com/MUKAPP/LiteLoaderQQNT-MSpring-Theme)实现更佳效果哦~

默认加载背景的路径是**插件数据目录（`LiteLoaderQQNT数据目录\background`）**下面的imgs文件夹，在QQ的设置里可以切换背景的目录，保存后立刻生效，目前会读取目录里（包含子文件夹）的一些常见格式的图片或视频文件，如下：

图片：`const allowedImgExt= ["JPG", "BMP", "PNG", "APNG", "WEBP", "JPEG", "AVIF", "GIF"];  `

视频：`const allowedVideoExt = ["MP4", "WEBM", "OGG"];`

背景默认是居中适应，所以如果比例不对可能会不好看，尽量选择横着的图片吧~

**目前已支持从网络API获取图片和视频（API支持直接返回图片的，也支持返回JSON的），详见下面的配置说明。**

现在深色浅色模式会根据`@media`媒体选择器自适应啦~

目前还很简陋，代码也比较粗糙，但能用！

## 配置说明

**注意：所有涉及到路径的字符串中的斜杠都是正斜杠（/）。**

*所有直接对配置文件的直接修改都会导致立即重置计时器并更新一次背景（因为无法确定你修改了哪一部分，所以只能全部重载），而在配置界面的修改只会立即应用修改的那一部分。*

imgDir（对应配置界面`本地背景文件夹路径`）：从哪个文件夹自动读取图片/视频文件，仅会读取一级的图片/视频，并不会递归读取子文件夹的哦。如果文件夹里同时有图片和视频，则都可能随机到。（特别注意：视频编码需要兼容，CHROME支持的大部分都支持，格式只支持OGG/WEBM/MP4）。默认：`插件数据目录下面的imgs文件夹`（`LiteLoaderQQNT数据目录\background`）。

imgSaveDir（对应配置界面`保存背景图的路径`）：如果你点击了主界面的保存背景图按钮，背景图会保存到哪个文件夹。注意：仅针对图片来源为网络图片时有效。默认：`插件数据目录下面的imgs文件夹`（`LiteLoaderQQNT数据目录\background`）。

imgApi（对应配置界面`网络背景API链接`）：从哪个api获取网络图片或视频，可选，可以没有这一项，但背景来源为网络图片/网络视频时该项必填。从网络获取的图片/视频必须没有防盗链，可通过GET直接访问（也就是链接放到浏览器里直接就能打开看到），暂不支持JSON格式返回的API（后续将支持）。（特别注意：视频编码需要兼容，CHROME支持的大部分都支持，格式只支持OGG/WEBM/MP4）。默认：`""`。

imgApiJsonPath（对应配置界面`网络背景API JSON路径`）：假设 API 返回格式为 JSON，则需要配置此项来获取原图直链。请查看[帮助文档](./API-JSON路径帮助.md)来辅助配置。默认：`""`。

apiType（配置界面会自动设置该项）：api返回的文件类型，是图片或者视频。可选值：img→图片；video→视频。如果选择错误将导致背景无法加载。默认：`img`。

imgFile（对应配置界面`本地背景路径`）：背景设置为哪个单独文件（可以是图片或者视频），可选，可以没有这一项，但背景来源为文件时该项必填。默认：`""`。

imgSource（对应配置界面`修改背景来源`）：图片来源类型，是文件夹轮播，还是单个文件，还是从网络api获取图片或者获取视频。你还可以取消背景图，仅使用本插件提供的界面透明功能。注意：如果选择从网络API获取，则必须正确选择是网络图片还是网络视频，否则背景会空白。可选值：none→无背景图；folder→文件夹；file→单个文件；network→网络图片；network_video→网络视频。默认：`folder`。

refreshTime（对应配置界面`背景更新间隔`）：多久随机更新一次背景，单位秒。默认：`600`（10分钟）。

isCommonBg（对应配置界面`是否对所有窗口共用背景`）：所有窗口的背景是否一致，这在你选择背景图来源为文件夹或者网络API时有效，其他情况时此项无效。默认：`true`。

isAutoRefresh（对应配置界面`是否自动轮播背景`）：是否自动轮播背景。若关闭，则每次NTQQ启动仅随机一次图片/视频，后续除非手动点击按钮更新，否则不再更新。默认：`true`。

globalTransparentOffset（对应配置界面`调整主背景图覆盖层透明度`）：用来调整背景图在聊天框呈现的时候，上面一层覆盖层的透明度（这一层的作用是避免背景图太亮导致看不清字），默认是中间：0。往左边调是负数，也就是减少透明度（更加透明），往右边是正数，增加透明度（更加不透明）。调整在滑块就位后实时保存与生效。默认：`0`。

enableBackgroundForMediaViewer（对应配置界面`是否对媒体预览器生效背景`）：媒体预览器是否应用背景和透明度（比如QQ内置的图片预览器）。设置为`false`则是QQ原来的样子，否则会带有背景图和透明度。修改配置后不会对当前已打开的预览器生效，需要重新点开一次预览器才会生效。默认：`true`。

enableFrostedGlassStyle（对应配置界面`是否对部分组件启用毛玻璃模糊效果`）：是否对部分组件启用毛玻璃模糊效果，若不喜欢可以关闭。你可以通过开关来对比效果，因为配置修改是实时生效的。默认：`true`。

overrideImgFile（暂无配置界面对应）：无论上面背景来源设置如何，强制使用本参数提供的背景文件路径作为背景图（可以是图片或者视频）（特别注意：视频编码需要兼容，CHROME支持的大部分都支持，格式只支持OGG/WEBM/MP4）。这个参数是用来配合未来手动选择文件夹内某个文件作为背景使用的（目前该功能还没有实装）。默认：`""`。

apiOptions对象：用来配置来源是网络视频的请求。

​	useCache（对应配置界面`是否启用视频缓存`）：若来源是网络视频，是否使用缓存。如果你设置的视频地址是API（也就是每次请求视频不一样），请务必设置为`false`，否则，缓存可能导致视频不会更新*（原理：每次请求会带上一个t=时间戳的参数，这样就能避免缓存）*；若你设置的API地址为单个视频，每次请求均一样，可以设置为`true`。目前网络图片均会自动保存到本地，所以本选项对网络图片无效。默认：`false`。

## 协议及免责

MIT | 禁止用于任何非法用途，插件开发属学习与研究目的，仅自用，未提供给任何第三方使用。任何不当使用导致的任何侵权问题责任自负。
