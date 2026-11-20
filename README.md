# Nightplay（Netlify 双人房间测试版）

## 本包内容
保留原始项目的数据库 schema、迁移、房间 API、WebSocket 聊天、邀请链接、游戏指令演示和剧情生成演示，并加入双人 WebRTC 语音。

## 重要：如何部署才能测试后端
本项目不是纯静态网站。聊天和语音信令依赖 `netlify/server/index.ts`，数据库依赖 Netlify Database。**不能只把 `public/` 文件夹拖到 Netlify Drop**，那样只会发布静态页面，聊天和语音服务端不会运行。

推荐部署：
1. 解压完整 ZIP。
2. 将全部文件提交到 GitHub 仓库（保留 `netlify/`、`db/`、`public/`、`package.json`、`package-lock.json`、`netlify.toml`）。
3. Netlify 选择 Add new project → Import an existing project，连接该仓库。
4. Build publish directory 使用 `public`；Node.js 使用 24（项目 `.nvmrc` 与 `netlify.toml` 已指定）。
5. 在 Netlify 项目中启用/关联 Netlify Database，并按平台提示应用数据库迁移。
6. 触发部署，查看 Deploy log 确认 Server Function/WebSocket 服务成功构建。
7. 打开 HTTPS 网站，用两台设备进入同一房间测试。

Netlify Drop 的普通静态拖放发布不等于完整后端部署。若你说的“直接上传”是无需 Git、只拖 ZIP 即可完成服务端和数据库部署，目前 Netlify 的静态 Drop 流程无法满足该要求；需要 Netlify 支持的完整构建部署流程。

## 本地开发
需要 Node.js 24+。在项目根目录：
```bash
npm ci
npx netlify-cli@latest dev
```
数据库需要关联 Netlify 项目并完成认证/配置。

## 双人语音测试
1. 两位参与者打开同一个房间邀请链接。
2. 双方分别点击“开启麦克风”并在浏览器中允许麦克风。默认不会自动开麦。
3. 测试双向讲话、静音/取消静音、结束语音、离开房间。
4. 页面离开/房间断开会停止本地麦克风轨道。
5. 当前默认 STUN；若不同网络间连接失败，需配置 TURN 服务。详见 `VOICE_SETUP.md`。

## 当前限制
- 本次修改保留了原数据库结构，不新增迁移。
- WebRTC 音频不经过服务器转发、不录音、不写入聊天记录。
- 语音信令通过已加入房间的 WebSocket 转发，房间上限仍为 2 人。
- 本地环境 Node 版本低于项目要求，且未连接 Netlify Database，因此本次无法进行完整构建和真实双设备端到端验证。请以 Netlify 部署日志和两台设备测试结果为准。
- 年龄确认界面是演示性质，不构成可靠年龄验证；正式公开运营前还需补充身份/年龄验证、隐私政策、举报屏蔽与滥用防护。
