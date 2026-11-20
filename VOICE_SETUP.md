# Nightplay 双人语音设置

- WebRTC 负责浏览器间音频；现有 `/ws` WebSocket 负责转发 `ready / offer / answer / ice / hangup` 信令。
- 房间上限仍是两人；只有用户点击“开启麦克风”并授予浏览器权限后才采集音频。
- 静音会禁用本地音轨；结束语音或离开房间会停止音轨并关闭 PeerConnection。
- 当前配置使用 Google 公共 STUN。部分 NAT、运营商网络、企业防火墙环境需要 TURN。

## TURN 建议
部署 coturn 或托管 TURN 后，可在前端 `makePeer()` 的 `iceServers` 中增加 TURN 服务器配置。生产环境不要把长期 TURN 密码写进公开前端；应由服务端签发短期凭据并提供给已加入房间的客户端。当前包未内置 TURN 凭据服务。

## 验收
两台设备加入同一房间，双方分别点开启麦克风，测试双向声音、静音、取消静音、结束语音、离开房间后麦克风指示灯熄灭。
