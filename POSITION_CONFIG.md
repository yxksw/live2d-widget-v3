# Live2D Widget 位置配置说明

## 使用方法

### 在博客中配置位置

在 `BaseLayout.astro` 中找到以下代码：

```javascript
// 配置看板娘位置：'left' 或 'right'
const live2dPosition = 'left' // 修改这里：'left' = 左下角，'right' = 右下角
```

修改 `live2dPosition` 的值：
- `'left'` - 显示在**左下角**
- `'right'` - 显示在**右下角**（默认）

### 示例

#### 显示在左下角
```javascript
const live2dPosition = 'left'
```

#### 显示在右下角
```javascript
const live2dPosition = 'right'
```

## CSS 类说明

### waifu-left
看板娘显示在左侧
```css
#waifu.waifu-left {
    left: 0;
    right: auto;
}
```

### waifu-right
看板娘显示在右侧
```css
#waifu.waifu-right {
    right: 10px;
    left: auto;
}
```

## 工作原理

1. **加载顺序**：
   - 加载 waifu.css（基础样式）
   - 加载 waifu-dark.css（深色模式样式）
   - 初始化 Live2D Widget
   - 应用位置类（waifu-left 或 waifu-right）

2. **样式优先级**：
   - waifu.css 定义默认位置（右侧）
   - waifu-left/waifu-right 类覆盖默认位置
   - waifu-dark.css 中的样式使用 `!important` 确保生效

3. **深色模式适配**：
   - 位置类在深色模式下同样有效
   - 深色模式样式会自动应用到对应位置

## 动态切换位置（可选）

如果你想让用户动态切换位置，可以添加以下代码：

```javascript
// 切换位置的函数
function toggleLive2DPosition() {
    const waifu = document.getElementById('waifu')
    if (waifu) {
        if (waifu.classList.contains('waifu-left')) {
            waifu.classList.remove('waifu-left')
            waifu.classList.add('waifu-right')
        } else {
            waifu.classList.remove('waifu-right')
            waifu.classList.add('waifu-left')
        }
    }
}

// 例如：在控制台调用 toggleLive2DPosition()
```

## 注意事项

1. **修改后需要刷新页面**才能看到位置变化
2. **位置配置在初始化时应用**，运行时修改需要重新加载
3. **拖拽功能不受影响**，用户仍然可以拖动看板娘
4. **移动端会自动隐藏**看板娘

## 文件结构

```
live2d-widget-v3/
├── waifu.css              # 基础样式（包含位置类）
├── waifu-dark.css         # 深色模式样式（包含位置类）
├── waifu-tips.js          # 提示逻辑
├── waifu-tips.json        # 提示文本
├── Resources/             # 模型资源
└── README.md              # 使用说明
```

## 兼容性

- ✅ 支持所有现代浏览器
- ✅ 支持深色模式
- ✅ 支持拖拽功能
- ✅ 支持移动端（自动隐藏）
- ✅ 不影响其他功能

## 示例配置

### 完整配置示例

```javascript
const cdnPath = 'https://cdn.jsdelivr.net/gh/yxksw/live2d-widget-v3@main/'

// 配置看板娘位置
const live2dPosition = 'left' // 左下角

const config = {
    path: {
        cssPath: cdnPath + '/waifu.css',
        darkCssPath: cdnPath + '/waifu-dark.css',
        // ... 其他配置
    },
    // ... 其他配置
}

// 加载后应用位置
setTimeout(() => {
    const waifu = document.getElementById('waifu')
    if (waifu) {
        waifu.classList.add(live2dPosition === 'left' ? 'waifu-left' : 'waifu-right')
    }
}, 100)
```

## 故障排除

### 问题：看板娘位置没有变化
**解决方案**：
1. 检查 `live2dPosition` 变量是否正确设置
2. 清除浏览器缓存
3. 检查浏览器控制台是否有错误

### 问题：深色模式下位置不正确
**解决方案**：
1. 确保 waifu-dark.css 已正确加载
2. 检查 waifu-dark.css 中的位置类是否正确
3. 刷新页面重新应用样式

## 更多帮助

如有问题，请查看：
- [原项目文档](https://github.com/dogxii/live2d-widget-v3)
- [深色模式说明](./DARK_MODE_README.md)
