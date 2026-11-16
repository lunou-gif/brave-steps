# Brave Steps — Exposure Tracker (Beta 0.1)

A quick, private way to log anxiety ratings (0–10) during exposure practice and see progress over time.
All data stays on the user's device (browser localStorage). No accounts, no server, no analytics.

## Files
- `index.html` — the whole app
- `manifest.webmanifest`, `icons/` — lets people "Add to Home Screen" like an app
- `sw.js` — offline support

## 用 GitHub Pages 免费上线（约 5 分钟）
1. 在 GitHub 新建一个 **public** repo，比如 `brave-steps`。
2. 点 **Add file → Upload files**，把这个文件夹里的所有文件（含 `icons/` 文件夹）拖进去，Commit。
3. 进 **Settings → Pages**，Source 选 **Deploy from a branch**，Branch 选 `main`、文件夹选 `/ (root)`，Save。
4. 等 1–2 分钟，网址会是 `https://<你的GitHub用户名>.github.io/brave-steps/`。用手机打开测试，然后发给测试者。

## 上线前要改的一处
在 `index.html` 里找到：

    const FEEDBACK_URL = "";

把你的反馈表链接（比如 Google Form）粘进引号里。填了之后，设置页会出现 “Send beta feedback” 按钮；不填就不显示。

## 设置 Pro 候补名单（收集付费意愿）
App 里的 Pro 页面会随机给每个用户看两个价格之一：A 组 $4.99/月 或 $39/年，B 组 $7.99/月 或 $59/年。同一个用户每次看到的价格都一样。用户勾选想要的功能，点 “Join the Pro waitlist” 后，会打开你的 Google Form，并自动填好价格组和勾选的功能。

1. 新建一个 Google Form，问题建议：
   - **Price group**（短答，自动填）
   - **Price shown**（短答，自动填）
   - **Features wanted**（段落，自动填）
   - Email（选填，用于上线通知）
   - Who are you?（单选：Parent of a child / Adult doing exposures myself / Therapist / Other）
   - Would you pay this price? （单选：Yes / Maybe / No, too much / No, I'd only use free）
   - What's the most you'd pay per month?（短答）
   - Anything else?（段落）
2. 表单右上角 ⋮ → **Get pre-filled link**。在前三题分别填 `VARIANT`、`PRICE`、`FEATURES`，点 Get link 并复制。
3. 把链接里的 `VARIANT`、`PRICE`、`FEATURES` 分别换成 `{variant}`、`{price}`、`{features}`，粘到 `index.html` 的 `PRO_FORM_URL = ""` 引号里。
4. 在 Google Form 的 Responses 里点 “Link to Sheets”，就可以按 Price group 分组算转化率。

## 看有多少人在用（GoatCounter 匿名统计）
1. 在 goatcounter.com 注册（非商业用途免费），给站点取个代码，比如 `bravesteps`。
2. 在 `index.html` 里设置：`const COUNT_URL = "https://bravesteps.goatcounter.com/count";`
3. 重新上传到 GitHub。介绍页、设置页的隐私说明会自动改成“我们会统计匿名访问”，设置页也会出现两个开关。

**会收到哪些事件**（只有事件名，不含任何记录、名字、分数或备注）：
| 事件 | 含义 | 谁会发送 |
|---|---|---|
| `open` | 打开了 app | 所有人（可以在设置里关闭） |
| `log-done` | 完成了一次记录 | 所有人（可以关闭） |
| `pro-view-A` / `pro-view-B` | 看了 Pro 页（价格组 A 或 B） | 所有人（可以关闭） |
| `pro-join-A` / `pro-join-B` | 点了加入候补 | 所有人（可以关闭） |
| `usage-active` | 每周一次的活跃信号 | 只有主动打开“每周使用摘要”的人 |
| `usage-week-logs-0/1-2/3-5/6plus` | 过去一周记录了几次（区间） | 同上 |
| `usage-total-logs-0/1-5/6-20/21plus` | 总共记录了几次（区间） | 同上 |
| `usage-steps-default/custom` | 有没有自定义步骤 | 同上 |

**怎么读数据：**
- GoatCounter 的 Visitors（独立访客）≈ 使用人数。
- `log-done` ÷ `open` = 打开后真的记录的比例。
- Pro 转化率：`pro-join-A` ÷ `pro-view-A`，再和 B 组对比。
- `usage-*` 只代表愿意分享的那部分用户，看分布和趋势就好，别当成总量。

注意：GoatCounter 会看到访问者的 IP 和浏览器信息（它自己说不保存可识别个人的数据）。对心理健康类 app，按华盛顿 My Health My Data Act，这可能也算健康相关数据。正式收费前建议先找律师确认；beta 阶段至少保持透明，app 里已经写明了。

## 发新版本时
改完文件后，把 `sw.js` 第一行的 `CACHE` 版本号加一（例如 `v0.1.1`），否则装到主屏幕的用户可能还在用旧缓存。

## 给测试者的说明（可直接复制）
- iPhone：用 Safari 打开 → 分享 → 添加到主屏幕。Android：Chrome 菜单 → 添加到主屏幕。
- 数据只存在你自己的手机里。偶尔在 ⚙︎ 设置里点 “Save a backup file” 备份一下。
- 这是记录工具，不是医疗建议。
