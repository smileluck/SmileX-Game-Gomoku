# 全息手势五子棋 - 开发文档

## 1. 项目架构

### 1.1 整体架构

```
┌─────────────────────────────────────────────────────────┐
│                   客户端浏览器                           │
├─────────────────────────────────────────────────────────┤
│  ┌─────────────────┐  ┌─────────────────┐  ┌───────────┐ │
│  │   Three.js      │  │ MediaPipe Hands │  │  游戏逻辑  │ │
│  │   3D场景渲染    │  │  手势追踪       │  │  (Gomoku)  │ │
│  └─────────────────┘  └─────────────────┘  └───────────┘ │
│          │                   │                   │       │
│          └───────────┬───────┘───────────┬───────┘       │
│                      │                   │               │
│              ┌───────────────────────────┐               │
│              │        游戏状态管理        │               │
│              └───────────────────────────┘               │
└─────────────────────────────────────────────────────────┘
```

### 1.2 核心模块

| 模块 | 主要职责 | 文件位置 | 技术栈 |
|------|----------|----------|--------|
| 3D场景渲染 | 创建和管理3D场景、相机、灯光、模型等 | `index.html` (HolographicGomoku类的initThreeJS方法) | Three.js |
| 手势追踪 | 实时追踪手部动作并转换为游戏指令 | `index.html` (HolographicGomoku类的initMediaPipe方法) | MediaPipe Hands |
| 游戏逻辑 | 处理落子、胜负判断、AI决策等 | `index.html` (HolographicGomoku类的核心方法) | JavaScript |
| UI控制 | 管理游戏信息显示、按钮事件等 | `index.html` (HTML结构 + CSS + JavaScript) | HTML/CSS/JavaScript |
| 操作说明 | 提供游戏规则和操作指南 | `index.html` (game-controls元素) | HTML/CSS/JavaScript |

## 2. 核心类与方法

### 2.1 HolographicGomoku 类

这是游戏的核心类，负责管理整个游戏的生命周期和所有组件。

```javascript
class HolographicGomoku {
    constructor() { /* 初始化游戏状态 */ }
    init() { /* 初始化游戏 */ }
    initThreeJS() { /* 初始化3D场景 */ }
    initMediaPipe() { /* 初始化手势追踪 */ }
    initBoard() { /* 初始化棋盘 */ }
    initCursor() { /* 初始化光标 */ }
    placePiece(x, z, player) { /* 落子逻辑 */ }
    checkWin(x, z, player) { /* 检查胜负 */ }
    aiMove() { /* AI落子 */ }
    minimax() { /* 极大极小值算法 */ }
    // ... 其他方法
}
```

#### 关键方法说明

| 方法名 | 描述 | 参数 | 返回值 |
|--------|------|------|--------|
| `init()` | 初始化游戏的入口方法，调用其他初始化方法 | 无 | 无 |
| `initThreeJS()` | 创建3D场景、相机、灯光和渲染器 | 无 | 无 |
| `initMediaPipe()` | 初始化MediaPipe手势追踪 | 无 | 无 |
| `initBoard()` | 创建3D棋盘网格 | 无 | 无 |
| `initCursor()` | 创建3D光标 | 无 | 无 |
| `placePiece(x, z, player)` | 处理落子逻辑，更新棋盘状态 | x: 棋盘X坐标<br>z: 棋盘Z坐标<br>player: 玩家类型(1:玩家, 2:AI) | 无 |
| `checkWin(x, z, player)` | 检查指定位置落子后是否获胜 | x: 棋盘X坐标<br>z: 棋盘Z坐标<br>player: 玩家类型 | boolean (是否获胜) |
| `aiMove()` | AI落子决策，调用minimax算法 | 无 | 无 |
| `minimax(depth, alpha, beta, isMaximizing)` | 极大极小值算法，带Alpha-Beta剪枝 | depth: 搜索深度<br>alpha: Alpha值<br>beta: Beta值<br>isMaximizing: 是否最大化 | {x, z, score} (最佳落子位置和得分) |

### 2.2 辅助函数

| 函数名 | 描述 | 参数 | 返回值 |
|--------|------|------|--------|
| `initControlsToggle()` | 初始化操作说明的折叠/展开功能 | 无 | 无 |

## 3. 游戏状态管理

### 3.1 游戏状态对象

```javascript
this.gameState = {
    board: Array(15).fill().map(() => Array(15).fill(0)), // 0:空, 1:玩家, 2:AI
    currentPlayer: 1, // 当前玩家
    gameOver: false,  // 游戏是否结束
    winner: 0         // 获胜者(0:无, 1:玩家, 2:AI)
};
```

### 3.2 状态转换

```
游戏开始 → 玩家落子 → 检查胜负 → AI落子 → 检查胜负 → ... → 游戏结束
```

## 4. 开发流程

### 4.1 环境搭建

1. **克隆项目**
   ```bash
   git clone https://github.com/your-username/holographic-gomoku.git
   cd holographic-gomoku
   ```

2. **启动本地服务器**
   ```bash
   python -m http.server 8000
   ```

3. **访问开发地址**
   ```
   http://localhost:8000
   ```

### 4.2 代码修改与调试

1. **修改代码**：直接编辑 `index.html` 文件
2. **实时预览**：保存文件后刷新浏览器
3. **调试工具**：使用浏览器开发者工具（F12）进行调试
   - **Console**：查看日志和错误信息
   - **Elements**：检查DOM结构和样式
   - **Sources**：断点调试JavaScript代码
   - **Performance**：分析性能问题

### 4.3 代码风格

- **缩进**：使用4个空格进行缩进
- **命名**：
  - 类名：使用大驼峰命名法（如 `HolographicGomoku`）
  - 方法名和变量名：使用小驼峰命名法（如 `initThreeJS`）
  - 常量：使用全大写加下划线（如 `BOARD_SIZE`）
- **注释**：
  - 类和方法添加JSDoc风格注释
  - 复杂逻辑添加单行注释
- **代码组织**：
  - 按功能模块组织代码
  - 相关方法放在一起
  - 遵循"高内聚、低耦合"原则

## 5. 核心功能实现细节

### 5.1 3D场景渲染

#### 5.1.1 场景初始化

```javascript
// 创建场景
this.scene = new THREE.Scene();

// 设置深空径向渐变背景
this.scene.background = new THREE.Color(0x000000);

// 创建相机
this.camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
this.camera.position.set(0, 200, 400);
this.camera.lookAt(0, 0, 0);

// 创建渲染器
this.renderer = new THREE.WebGLRenderer({ canvas: this.threeCanvas, antialias: true, alpha: true });
```

#### 5.1.2 辉光效果

使用Three.js的后处理模块实现辉光效果：

```javascript
// 初始化辉光后处理
this.initBloomEffect();

// 在animate方法中渲染
if (this.composer) {
    this.composer.render();
} else {
    this.renderer.render(this.scene, this.camera);
}
```

### 5.2 手势追踪

#### 5.2.1 MediaPipe Hands 初始化

```javascript
this.hands = new window.Hands({
    locateFile: (file) => {
        return `https://cdn.jsdelivr.net/npm/@mediapipe/hands@0.4.1646424915/${file}`;
    }
});

// 配置Hands模型
this.hands.setOptions({
    maxNumHands: 1,
    modelComplexity: 1,
    minDetectionConfidence: 0.7,
    minTrackingConfidence: 0.7
});

// 设置结果回调
this.hands.onResults((results) => {
    this.onHandResults(results);
});
```

#### 5.2.2 手势到游戏指令的转换

```javascript
// 获取食指指尖和拇指指尖坐标
const indexTip = landmarks[8];
const thumbTip = landmarks[4];

// 计算拇指与食指的距离
const distance = Math.hypot(
    thumbTip.x - indexTip.x,
    thumbTip.y - indexTip.y
);

// 根据距离判断手势状态
if (distance < 0.05) { // 捏合手势
    // 处理落子逻辑
} else {
    // 处理光标移动
}
```

### 5.3 AI决策算法

#### 5.3.1 极大极小值算法（带Alpha-Beta剪枝）

```javascript
minimax(depth, alpha, beta, isMaximizing) {
    // 终止条件
    if (depth === 0 || this.checkWinner() !== 0) {
        return { x: -1, z: -1, score: this.evaluateBoard() };
    }
    
    // 搜索所有可能的落子位置
    // ...
    
    // Alpha-Beta剪枝
    if (isMaximizing) {
        // 最大化玩家（AI）
        // ...
        alpha = Math.max(alpha, score);
        if (beta <= alpha) break;
    } else {
        // 最小化玩家（人类）
        // ...
        beta = Math.min(beta, score);
        if (beta <= alpha) break;
    }
    
    // 返回最佳落子位置和得分
    // ...
}
```

#### 5.3.2 棋盘评估函数

```javascript
evaluateBoard() {
    const playerScore = this.evaluatePlayer(1);
    const aiScore = this.evaluatePlayer(2);
    return aiScore - playerScore;
}

// 评估单个玩家的棋盘价值
evaluatePlayer(player) {
    // 评估每个位置
    // ...
    
    // 中心位置奖励
    const centerDist = Math.hypot(x - 7, z - 7);
    score += (15 - centerDist) * 2;
    
    // 评估四个方向
    score += this.evaluatePosition(x, z, player);
    
    // ...
}
```

## 6. 扩展开发

### 6.1 添加新功能

#### 6.1.1 添加新的视觉效果

1. 在 `placePiece` 方法中添加新的特效调用
2. 实现特效方法，例如：
   ```javascript
   playNewEffect(x, z) {
       // 创建新的特效
       // ...
       
       // 添加到场景
       this.scene.add(effect);
       
       // 动画逻辑
       // ...
   }
   ```

#### 6.1.2 改进AI算法

1. 修改 `minimax` 方法的搜索深度
2. 优化 `evaluateBoard` 方法的评估逻辑
3. 添加新的评估因素，如威胁评估、防守策略等

#### 6.1.3 添加新的操作方式

1. 在 `initEventListeners` 方法中添加新的事件监听器
2. 实现新的输入处理逻辑
3. 更新操作说明文档

### 6.2 性能优化

1. **减少渲染负担**：
   - 优化几何体数量和复杂度
   - 合理使用LOD（细节层次）
   - 避免不必要的渲染调用

2. **优化AI算法**：
   - 调整搜索深度，平衡性能和AI强度
   - 实现启发式搜索，优先搜索更有价值的位置
   - 使用缓存机制，避免重复计算

3. **优化手势追踪**：
   - 调整MediaPipe Hands的配置参数
   - 实现手势追踪结果的平滑处理
   - 减少不必要的计算

## 7. 调试与测试

### 7.1 调试技巧

1. **使用浏览器开发者工具**：
   - 查看Console日志
   - 使用断点调试JavaScript代码
   - 检查3D场景状态（Three.js扩展）
   - 监控性能指标

2. **启用调试模式**：
   - 在代码中添加调试开关
   - 输出详细的日志信息
   - 可视化调试信息（如碰撞检测区域）

3. **测试不同设备**：
   - 在不同浏览器中测试
   - 在不同分辨率的设备上测试
   - 测试不同的摄像头质量

### 7.2 测试场景

| 测试场景 | 预期结果 | 测试方法 |
|----------|----------|----------|
| 玩家落子 | 棋子正确放置在指定位置，游戏状态更新 | 使用手势或鼠标在不同位置落子 |
| AI落子 | AI在合理位置落子，游戏状态更新 | 观察AI的落子行为 |
| 胜负判定 | 正确检测五子连线，游戏结束 | 故意连成五子，观察游戏是否结束 |
| 平局判定 | 棋盘填满后游戏结束，判定为平局 | 填满棋盘，观察游戏是否结束 |
| 重置游戏 | 游戏状态重置，棋盘清空 | 点击重置按钮，观察游戏状态 |
| 手势识别 | 准确识别手指位置和捏合手势 | 使用不同角度和距离测试手势识别 |
| 折叠/展开操作说明 | 操作说明可以正常折叠和展开 | 点击标题栏和按钮测试折叠功能 |

## 8. 部署与发布

### 8.1 本地测试

1. 启动本地服务器
2. 在不同浏览器中测试
3. 检查所有功能是否正常工作
4. 测试不同分辨率和设备

### 8.2 生产部署

1. **优化资源**：
   - 压缩HTML、CSS和JavaScript文件
   - 优化3D模型和纹理（如果有）
   - 考虑使用CDN加速外部资源

2. **配置安全策略**：
   - 配置适当的Content Security Policy (CSP)
   - 确保摄像头访问权限正确配置

3. **部署到服务器**：
   - 上传文件到Web服务器
   - 配置域名和HTTPS
   - 测试生产环境下的性能和功能

## 9. 贡献指南

### 9.1 代码贡献流程

1. **Fork项目**
2. **创建分支**：使用有意义的分支名称，如 `feature/new-effect` 或 `bugfix/cursor-position`
3. **编写代码**：遵循项目的代码风格和架构
4. **测试**：确保代码通过所有测试场景
5. **提交PR**：详细描述修改内容和解决的问题
6. **代码审查**：等待项目维护者的审查
7. **合并**：审查通过后，代码将被合并到主分支

### 9.2 问题报告

1. **使用GitHub Issues**（如果使用GitHub）或其他问题跟踪系统
2. **详细描述**：包括问题现象、复现步骤、浏览器环境等
3. **提供截图或日志**：帮助开发者理解问题
4. **提出解决方案**（可选）：如果你有解决方案的想法，可以提出

## 10. 常见问题与解决方案

### 10.1 摄像头无法访问

**问题**：浏览器提示无法访问摄像头

**解决方案**：
- 确保已授予摄像头访问权限
- 检查摄像头是否被其他应用占用
- 尝试使用HTTPS协议或本地服务器
- 检查浏览器设置，确保摄像头访问权限已启用

### 10.2 手势识别不准确

**问题**：手势识别不稳定或不准确

**解决方案**：
- 确保光线充足
- 保持手部在摄像头视野范围内
- 调整MediaPipe Hands的配置参数
- 清理摄像头镜头

### 10.3 游戏运行卡顿

**问题**：游戏运行不流畅，有卡顿现象

**解决方案**：
- 关闭其他占用资源的应用
- 降低浏览器的缩放比例
- 调整AI算法的搜索深度
- 优化3D场景的复杂度

## 11. 技术栈详细说明

### 11.1 Three.js

- **版本**：0.158.0
- **主要用途**：3D场景渲染、相机控制、灯光、材质、几何体等
- **核心概念**：Scene、Camera、Renderer、Mesh、Material、Geometry
- **文档**：https://threejs.org/docs/

### 11.2 MediaPipe Hands

- **版本**：0.4.1646424915
- **主要用途**：实时手势追踪和识别
- **核心概念**：Hands、Landmarks、Gestures
- **文档**：https://google.github.io/mediapipe/solutions/hands.html

### 11.3 其他技术

- **HTML5 Canvas**：用于绘制手部骨骼
- **CSS3**：用于样式设计和动画效果
- **JavaScript (ES6+)**：核心游戏逻辑

## 12. 未来发展方向

1. **多人对战**：添加网络对战功能，支持玩家间实时对战
2. **更多手势**：支持更多手势，如旋转视角、缩放场景等
3. **自定义主题**：允许玩家自定义游戏主题和视觉效果
4. **AI难度选择**：提供不同难度级别的AI对手
5. **游戏回放**：支持游戏回放功能
6. **移动端优化**：针对移动设备进行优化
7. **VR/AR支持**：添加VR/AR支持，提供沉浸式体验

---

## 附录：代码风格指南

### 命名规范

- **类名**：使用大驼峰命名法，如 `HolographicGomoku`
- **方法名**：使用小驼峰命名法，如 `initThreeJS`
- **变量名**：使用小驼峰命名法，如 `currentPlayer`
- **常量**：使用全大写加下划线，如 `BOARD_SIZE`
- **私有成员**：使用下划线前缀，如 `_privateMethod`

### 代码组织

- 类定义放在文件顶部
- 辅助函数放在文件底部
- 相关方法放在一起
- 保持方法的单一职责
- 避免过长的方法，建议不超过50行

### 注释规范

- 使用JSDoc风格的文档注释
- 为类、方法、函数添加注释
- 为复杂逻辑添加单行注释
- 避免不必要的注释
- 保持注释与代码同步

### 错误处理

- 使用try-catch块处理可能的错误
- 提供有意义的错误信息
- 实现优雅降级，如摄像头不可用时提供替代方案
- 记录错误日志，便于调试

---

**开发愉快！** 🚀
