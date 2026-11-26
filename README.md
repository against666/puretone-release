# PureTone 更新配置

App 启动时读取 `config.json`，比对版本号决定是否提示更新。

## 读取地址（App 内按顺序尝试）

1. `https://raw.githubusercontent.com/against666/puretone-release/main/config.json` —— 内容最新
2. `https://cdn.jsdelivr.net/gh/against666/puretone-release@main/config.json` —— 国内可达，但缓存约 12 小时

## 发布新版本怎么做

1. 改 App 的 `versionCode`（必须比上一版大）和 `versionName`
2. 打包 APK，传到本仓库的 Releases
3. 修改 `config.json`：
   ```json
   {
     "versionCode": 3,
     "versionName": "0.3.0",
     "downloadUrl": "https://github.com/against666/puretone-release/releases/download/v0.3.0/puretone-0.3.0.apk",
     "note": "这次更新了什么"
   }
   ```
4. 推送后清一次 CDN 缓存：
   ```
   curl "https://purge.jsdelivr.net/gh/against666/puretone-release@main/config.json"
   ```

## 字段说明

| 字段 | 说明 |
|---|---|
| `versionCode` | 整数，必须大于 App 内的 versionCode 才会提示 |
| `versionName` | 显示给用户看的版本名 |
| `downloadUrl` | 点「去更新」打开这个地址，通常是 APK 直链或 Releases 页面 |
| `note` | 更新说明，可留空；正文过长会自动换行 |

## 注意

- 仓库必须是 **public**，否则 App 匿名读不到
- 用户点「稍后」后会记住该 versionCode，同一版本不再打扰；想重新测试可以卸载重装
