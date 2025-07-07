# Stickman Fighter Game - 项目交接文档

## 项目概述

火柴人格斗游戏是一个基于HTML5 Canvas的双平台格斗游戏，支持桌面双人对战和移动端单人对战AI。

**在线地址**: https://oxcod.github.io/stickman-fighter-game/

## 项目结构

```
stickman-fighter-game/
├── index.html                              # 主入口页面（自动设备检测）
├── index-mobile.html                       # 移动端版本
├── favicon.svg                            # 游戏图标
├── 114_full_fight-song_0161_preview.mp3   # 背景音乐
├── README.md                              # 项目说明
└── DEVELOPMENT.md                         # 开发交接文档（本文件）
```

## 版本历史

### v2.1 (当前版本)
- ✅ 移动端完整BGM支持
- ✅ 自定义favicon图标
- ✅ 移动端控制按钮位置优化
- ✅ 统一桌面和移动端音频体验

### v2.0
- ✅ 修复UI重叠问题（血条与控制按钮）
- ✅ 修复现代浏览器AudioContext问题
- ✅ 简化项目结构

### v1.x
- ✅ 基础游戏功能
- ✅ 双语支持（中英文）
- ✅ 桌面双人对战 + 移动端AI对战

## 技术架构

### 核心技术栈
- **前端**: 纯HTML5 + CSS3 + JavaScript (ES6+)
- **渲染**: Canvas 2D API
- **音频**: Web Audio API
- **设备检测**: Navigator API + Touch Events

### 主要模块

#### 1. 设备检测与路由 (`index.html`)
```javascript
const isMobile = /Android|webOS|iPhone|iPad|iPod|BlackBerry|IEMobile|Opera Mini/i.test(navigator.userAgent);
if (isMobile) window.location.href = 'index-mobile.html';
```

#### 2. 游戏引擎架构
```
Game Class
├── Player Class (角色控制)
├── SoundManager (音频管理)
├── EffectManager (特效管理)
├── ParticleSystem (粒子系统)
└── Input Handler (输入处理)
```

#### 3. 音频系统
- **音效**: 通过Web Audio API生成合成音效
- **BGM**: 加载MP3文件循环播放
- **兼容性**: 处理现代浏览器AudioContext暂停问题

## 关键功能实现

### 双语切换系统
```javascript
// HTML元素使用data属性存储多语言文本
<element data-en="English Text" data-zh="中文文本">

// JavaScript动态切换
function switchLanguage() {
    document.querySelectorAll('[data-en], [data-zh]').forEach(elem => {
        elem.textContent = elem.getAttribute(`data-${currentLang}`);
    });
}
```

### 格斗系统
- **基础攻击**: 拳击(15伤害)、踢腿(20伤害)、特殊技(35伤害)
- **连击系统**: 2秒连击窗口
- **无敌帧**: 被击中后0.5秒无敌时间
- **技能冷却**: 特殊技3秒冷却

### AI系统 (移动端)
```javascript
// AI状态机: approach -> attack -> retreat
switch(this.aiState) {
    case 'approach': // 接近玩家
    case 'attack':   // 执行攻击
    case 'retreat':  // 撤退重新定位
}
```

## 开发环境配置

### 本地开发
```bash
# 克隆项目
git clone https://github.com/oxcod/stickman-fighter-game.git

# 启动本地服务器（推荐）
python -m http.server 8000
# 或使用其他HTTP服务器

# 访问
http://localhost:8000
```

### 部署
项目使用GitHub Pages自动部署，推送到main分支即可自动更新在线版本。

## 已知问题与解决方案

### 1. 移动端浏览器兼容性
**问题**: 不同移动浏览器底部工具栏高度不一致
**解决**: 控制按钮距离底部40px避免遮挡

### 2. 音频自动播放限制
**问题**: 现代浏览器阻止自动播放音频
**解决**: 添加AudioContext状态检测和用户交互后恢复

### 3. 屏幕方向适配
**问题**: 横屏时控制按钮过大
**解决**: 使用CSS媒体查询调整横屏时的控件大小

## 维护指南

### 文件修改优先级
1. **index.html** - 桌面版主要逻辑
2. **index-mobile.html** - 移动版功能
3. **README.md** - 用户文档
4. **favicon.svg** - 品牌图标

### 常见修改场景

#### 调整游戏平衡性
```javascript
// 在Player类的checkHit方法中修改伤害值
case 'punch': damage = 15;   // 拳击伤害
case 'kick': damage = 20;    // 踢腿伤害
case 'special': damage = 35; // 特殊技伤害
```

#### 添加新音效
```javascript
// 在SoundManager的playSound方法中添加新的音效类型
case 'newSound':
    osc.frequency.setValueAtTime(frequency, currentTime);
    // 配置音效参数
```

#### UI布局调整
```css
/* 主要CSS选择器 */
#ui                    /* 血条区域 */
#mobileControls        /* 移动端控制区域 */
.controlBtn           /* 控制按钮样式 */
#controls             /* 操作说明区域 */
```

## 扩展建议

### 短期优化
- [ ] 添加更多角色皮肤选择
- [ ] 增加更多特殊技能类型
- [ ] 添加简单的成就系统

### 长期功能
- [ ] 在线多人对战支持
- [ ] 关卡模式
- [ ] 角色升级系统
- [ ] 更丰富的音效和背景音乐

## 联系信息

**原开发者**: Claude Code (AI Assistant)
**项目仓库**: https://github.com/oxcod/stickman-fighter-game
**最后更新**: 2025-07-07

---

## 快速问题排查

### 音频不播放
1. 检查浏览器是否阻止自动播放
2. 确认MP3文件路径正确
3. 查看控制台AudioContext错误信息

### 移动端控制失效
1. 检查touch events是否正确绑定
2. 确认CSS touch-action设置
3. 验证设备检测逻辑

### 性能问题
1. 检查Canvas渲染帧率
2. 优化粒子系统数量
3. 减少不必要的DOM操作

---

*本文档最后更新于项目v2.1版本，如有技术问题可通过GitHub Issues反馈。*