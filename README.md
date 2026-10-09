# Loon YouTube 去广告

去除 YouTube App 与网页版（Safari）广告的 Loon 插件，设置界面全中文。只适配 **Loon 3.5.1+**。

## 安装

Loon → 配置 → 插件 → 右上角 **+** → 填入订阅地址：

```
https://raw.githubusercontent.com/imthnio/loon-youtube/main/Plugins/YouTube.lpx
```

或一键导入：[点此导入 Loon](https://www.nsloon.com/openloon/import?plugin=https://raw.githubusercontent.com/imthnio/loon-youtube/main/Plugins/YouTube.lpx)

然后打开 MITM，安装并信任 Loon 证书（设置 → 通用 → 关于本机 → 证书信任设置）。装好后彻底关闭 YouTube App / Safari 标签页再重新打开。

## 功能

| 设置项 | 说明 |
|---|---|
| 后台播放 | 切换 App 或锁屏后普通视频继续播放 |
| 播放请求地区 | 修改 App 请求里的 gl 参数，出口 IP 不变 |
| 字幕翻译 / 字幕目标语言 | 把已有字幕翻译成 zh-CN 或 en-US |
| 隐藏首页 Shorts | 移除首页 Shorts 栏 |
| 屏蔽频道 / 远程屏蔽频道列表 | 按频道 ID、@handle 或名称隐藏视频 |
| 日志工具 / 日志级别 / 日志容量 | 排查问题用，平时保持关闭 |

## 和原版相比的修改

1. **修复 Safari 打开视频前几秒卡顿**：不再对 `*.googlevideo.com`（视频分片）做 MITM，视频流量不再经过 Loon 解密和重新加密；删除了会读取完整视频响应体的“完整媒体采样”和“画质研究采样”。
2. **修复网页版 `player/ad_break` 返回错误格式**：原版对所有客户端都返回空 protobuf，网页播放器只认 JSON，现在网页版返回空 JSON。
3. **App 专用脚本只匹配 App 域名**：`log_event`/`config` 和播放请求脚本内部只处理 App 的 protobuf，原版也会缓冲网页版请求，现在限定在 `youtubei.googleapis.com`。
4. **`log_event` 请求不再读取请求体**：脚本只改请求头，原版却为每个 `log_event` 缓冲请求体。
5. **网页版 QUIC 回落**：新增 `youtube.com` 的 UDP 443 拒绝规则，避免 Safari 走 QUIC 绕过 MITM，导致广告时有时无。
6. 设置界面、脚本名称全部中文。

## 注意

- 不再解密视频域名后，原版针对 App 的 `initplayback` 空白视频实验规则一并移除。App 去广告主要靠清理播放接口返回的广告字段，不受影响；如果 App 出现片头广告，可在 Issue 反馈。
- 没有字幕的视频无法翻译。

## Contributors

- [imthnio](https://github.com/imthnio)

---

基于 [devchristian1337/loon-plugins](https://github.com/devchristian1337/loon-plugins) 的 YouTube 插件修改（MIT）。
