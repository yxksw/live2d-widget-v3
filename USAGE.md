# Live2D Widget 使用指南

一个功能强大的 Live2D 看板娘插件，支持多种模型、表情、动作和主题适配。

## 📚 目录

- [快速开始](#快速开始)
- [配置选项](#配置选项)
- [主题适配](#主题适配)
- [位置设置](#位置设置)
- [CDN 链接](#cdn-链接)
- [完整示例](#完整示例)
- [常见问题](#常见问题)

---

## 🚀 快速开始

### 1. 基础 HTML 结构

在你的博客或网站的 `<body>` 标签中添加以下代码：

```html
<!DOCTYPE html>
<html lang="zh-Hans">
<head>
    <meta charset="UTF-8">
    <title>我的博客</title>
</head>
<body>
    <!-- 你的网站内容 -->
    
    <!-- Live2D 看板娘 -->
    <script>
        // 初始化代码将放在这里
    </script>
</body>
</html>
```

### 2. 引入必要的资源

```html
<script>
    // CDN 基础路径
    const cdnPath = 'https://cdn.jsdelivr.net/gh/yxksw/live2d-widget-v3@main/'
    
    // 加载资源
    function loadExternalResource(url, type) {
        return new Promise((resolve, reject) => {
            let tag
            if (type === 'css') {
                tag = document.createElement('link')
                tag.rel = 'stylesheet'
                tag.href = url
            } else if (type === 'js') {
                tag = document.createElement('script')
                tag.src = url
            }
            tag.onload = () => resolve(url)
            tag.onerror = () => reject(url)
            document.head.appendChild(tag)
        })
    }
    
    // 加载所有资源并初始化
    Promise.all([
        loadExternalResource(cdnPath + 'waifu.css', 'css'),
        loadExternalResource(cdnPath + 'waifu-dark.css', 'css'),
        loadExternalResource(cdnPath + 'Core/live2dcubismcore.js', 'js'),
        loadExternalResource(cdnPath + 'live2d-sdk.js', 'js'),
        loadExternalResource(cdnPath + 'waifu-tips.js', 'js'),
    ]).then(() => {
        initWidget({
            waifuPath: cdnPath + 'waifu-tips.json',
            cdnPath: cdnPath + 'Resources/',
            tools: ['hitokoto', 'asteroids', 'switch-model', 'switch-texture', 'photo', 'info', 'quit'],
            dragEnable: true,
            dragDirection: ['x', 'y'],
            switchType: 'order',
        })
    })
</script>
```

---

## ⚙️ 配置选项

### initWidget 配置参数

```javascript
initWidget({
    // 必要配置
    waifuPath: 'waifu-tips.json 路径',
    cdnPath: '模型资源路径',
    
    // 工具栏配置
    tools: [
        'hitokoto',        // 一言
        'asteroids',       // 小行星游戏
        'express',         // 随机表情
        'switch-model',    // 切换模型
        'switch-texture',  // 切换纹理
        'photo',           // 拍照
        'info',            // 信息
        'quit',            // 隐藏看板娘
    ],
    
    // 拖拽配置
    dragEnable: true,     // 是否允许拖拽
    dragDirection: ['x', 'y'], // 拖拽方向：'x', 'y', 或 ['x', 'y']
    
    // 模型切换配置
    switchType: 'order',  // 'order' = 顺序切换，'random' = 随机切换
})
```

### 工具栏工具说明

| 工具名称 | 功能描述 |
|---------|---------|
| `hitokoto` | 显示一言（随机名句） |
| `asteroids` | 启动小行星游戏 |
| `express` | 随机表情 |
| `switch-model` | 切换下一个模型 |
| `switch-texture` | 切换当前模型的纹理 |
| `photo` | 拍照功能 |
| `info` | 显示项目信息 |
| `quit` | 隐藏看板娘 |

---

## 🎨 主题适配

### 深色模式支持

看板娘已内置深色模式支持，会自动根据系统主题切换样式。

#### 1. 系统主题检测

```javascript
// 检测系统主题
function getSystemTheme() {
    return window.matchMedia('(prefers-color-scheme: dark)').matches ? 'dark' : 'light'
}

// 设置主题
function setTheme(theme) {
    document.documentElement.setAttribute('data-theme', theme)
}

// 初始化主题
setTheme(getSystemTheme())

// 监听主题变化
window.matchMedia('(prefers-color-scheme: dark)').addEventListener('change', (e) => {
    setTheme(e.matches ? 'dark' : 'light')
})
```

#### 2. 手动主题切换

```javascript
// 切换到深色模式
document.documentElement.setAttribute('data-theme', 'dark')

// 切换到浅色模式
document.documentElement.setAttribute('data-theme', 'light')
```

#### 3. 主题样式

- **浅色模式**：对话框为半透明白色背景，蓝色文字
- **深色模式**：对话框为深色背景，白色文字

---

## 📍 位置设置

### 配置看板娘位置

看板娘可以显示在左下角或右下角。

#### 方法 1：使用 CSS 类

```css
/* 左下角 */
#waifu.waifu-left {
    left: 0 !important;
    right: auto !important;
}

/* 右下角 */
#waifu.waifu-right {
    right: 10px !important;
    left: auto !important;
}
```

```javascript
// 初始化后设置位置
setTimeout(() => {
    const waifu = document.getElementById('waifu')
    if (waifu) {
        waifu.classList.add('waifu-left')  // 或 'waifu-right'
    }
}, 100)
```

#### 方法 2：直接修改 waifu.css

修改 `waifu.css` 中的 `#waifu` 样式：

```css
#waifu {
    bottom: 0px;
    left: 0;      /* 左下角 */
    right: auto;  /* 左下角 */
    /* 或者 */
    right: 10px;  /* 右下角 */
    left: auto;   /* 右下角 */
}
```

---

## 🔗 CDN 链接

### 推荐的 CDN 源

```javascript
// jsDelivr (推荐，全球加速)
const cdnPath = 'https://cdn.jsdelivr.net/gh/yxksw/live2d-widget-v3@main/'

// jsdmirror (备用)
const cdnPath = 'https://cdn.jsdmirror.com/gh/yxksw/live2d-widget-v3@main/'
```

### 必要的文件

```
waifu.css              # 基础样式
waifu-dark.css         # 深色模式样式
Core/live2dcubismcore.js  # Live2D 核心库
live2d-sdk.js          # Live2D SDK
waifu-tips.js          # 对话框和工具栏逻辑
waifu-tips.json        # 对话框文本配置
Resources/             # 模型资源目录
```

---

## 📝 完整示例

### 在 Astro 项目中使用

```astro
---
// src/layouts/BaseLayout.astro
---

<!DOCTYPE html>
<html lang="zh-Hans">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title><slot name="title" /></title>
    
    <script>
        // 初始化主题
        (function() {
            function setTheme(theme) {
                document.documentElement.setAttribute('data-theme', theme);
            }
            
            function getSystemTheme() {
                return window.matchMedia('(prefers-color-scheme: dark)').matches ? 'dark' : 'light';
            }
            
            setTheme(getSystemTheme());
            
            window.matchMedia('(prefers-color-scheme: dark)').addEventListener('change', (e) => {
                setTheme(e.matches ? 'dark' : 'light');
            });
        })();
    </script>
</head>
<body>
    <slot />
    
    <!-- Live2D 看板娘 -->
    <script>
        declare function initWidget(config: any): void;
        
        function setTheme(theme: string) {
            document.documentElement.setAttribute('data-theme', theme);
        }
        
        function getCurrentTheme(): string {
            return document.documentElement.getAttribute('data-theme') || 'light';
        }
        
        if (screen.width >= 768) {
            const cdnPath = 'https://cdn.jsdelivr.net/gh/yxksw/live2d-widget-v3@main/'
            const waifuDarkCssUrl = cdnPath + 'waifu-dark.css?t=' + Date.now()
            const live2dPosition = 'left' // 'left' 或 'right'
            
            Promise.all([
                loadExternalResource(cdnPath + 'waifu.css', 'css'),
                loadExternalResource(waifuDarkCssUrl, 'css'),
                loadExternalResource(cdnPath + 'Core/live2dcubismcore.js', 'js'),
                loadExternalResource(cdnPath + 'live2d-sdk.js', 'js'),
                loadExternalResource(cdnPath + 'waifu-tips.js', 'js'),
            ]).then(() => {
                initWidget({
                    waifuPath: cdnPath + 'waifu-tips.json',
                    cdnPath: cdnPath + 'Resources/',
                    tools: ['hitokoto', 'asteroids', 'express', 'switch-model', 'switch-texture', 'photo', 'info', 'quit'],
                    dragEnable: true,
                    dragDirection: ['x', 'y'],
                    switchType: 'order',
                })
                
                setTimeout(() => {
                    const waifu = document.getElementById('waifu')
                    if (waifu) {
                        waifu.classList.remove('waifu-left', 'waifu-right')
                        waifu.classList.add(live2dPosition === 'left' ? 'waifu-left' : 'waifu-right')
                    }
                }, 100)
            })
            
            window.matchMedia('(prefers-color-scheme: dark)').addEventListener('change', () => {
                const waifu = document.getElementById('waifu')
                if (waifu) {
                    waifu.style.opacity = '0'
                    setTimeout(() => {
                        waifu.style.opacity = '1'
                    }, 50)
                }
            })
        }
        
        function loadExternalResource(url: string, type: 'css' | 'js'): Promise<string> {
            return new Promise((resolve, reject) => {
                let tag = document.createElement(type === 'css' ? 'link' : 'script')
                if (type === 'css') {
                    tag.rel = 'stylesheet'
                    tag.href = url
                } else {
                    tag.src = url
                }
                tag.onload = () => resolve(url)
                tag.onerror = () => reject(url)
                document.head.appendChild(tag)
            })
        }
    </script>
</body>
</html>
```

### 在 Hexo 项目中使用

在 `themes/你的主题/layout/_partial/` 目录下创建 `live2d.ejs`：

```ejs
<% if (theme.live2d.enable) { %>
<script>
    (function() {
        const cdnPath = 'https://cdn.jsdelivr.net/gh/yxksw/live2d-widget-v3@main/'
        
        function loadExternalResource(url, type) {
            return new Promise((resolve, reject) => {
                let tag = document.createElement(type === 'css' ? 'link' : 'script')
                if (type === 'css') {
                    tag.rel = 'stylesheet'
                    tag.href = url
                } else {
                    tag.src = url
                }
                tag.onload = () => resolve(url)
                tag.onerror = () => reject(url)
                document.head.appendChild(tag)
            })
        }
        
        if (screen.width >= 768) {
            Promise.all([
                loadExternalResource(cdnPath + 'waifu.css', 'css'),
                loadExternalResource(cdnPath + 'waifu-dark.css', 'css'),
                loadExternalResource(cdnPath + 'Core/live2dcubismcore.js', 'js'),
                loadExternalResource(cdnPath + 'live2d-sdk.js', 'js'),
                loadExternalResource(cdnPath + 'waifu-tips.js', 'js'),
            ]).then(() => {
                initWidget({
                    waifuPath: cdnPath + 'waifu-tips.json',
                    cdnPath: cdnPath + 'Resources/',
                    tools: ['hitokoto', 'switch-model', 'photo', 'info', 'quit'],
                    dragEnable: true,
                    dragDirection: ['x', 'y'],
                })
            })
        }
    })()
</script>
<% } %>
```

在 `themes/你的主题/layout/layout.ejs` 中引入：

```ejs
<body>
    <%- body %>
    <%- partial('_partial/live2d') %>
</body>
```

---

## ❓ 常见问题

### 1. 看板娘不显示

**可能原因：**
- 屏幕宽度小于 768px（移动端不显示）
- CSS/JS 资源加载失败
- `initWidget` 函数未正确调用

**解决方案：**
- 检查浏览器控制台是否有错误
- 确保 CDN 链接可访问
- 确认在桌面端访问（屏幕宽度 >= 768px）

### 2. 对话框样式不正确

**可能原因：**
- CDN 缓存导致样式未更新
- `data-theme` 属性未正确设置

**解决方案：**
```javascript
// 添加时间戳绕过 CDN 缓存
const waifuDarkCssUrl = cdnPath + 'waifu-dark.css?t=' + Date.now()

// 确保设置 data-theme 属性
document.documentElement.setAttribute('data-theme', 'dark') // 或 'light'
```

### 3. 模型无法加载

**可能原因：**
- 模型资源路径错误
- CORS 跨域问题

**解决方案：**
- 确保 `cdnPath` 指向正确的资源目录
- 使用支持 CORS 的 CDN

### 4. 拖拽功能不生效

**解决方案：**
```javascript
initWidget({
    dragEnable: true,
    dragDirection: ['x', 'y'], // 或 ['x'] 或 ['y']
})
```

### 5. 如何自定义对话框文本？

修改 `waifu-tips.json` 文件，然后使用自己的 CDN 或本地文件：

```javascript
initWidget({
    waifuPath: '/path/to/your/waifu-tips.json',
    // ...
})
```

---

## 🎯 最佳实践

### 1. 性能优化

- 只在桌面端加载 Live2D（屏幕宽度 >= 768px）
- 使用 CDN 加速资源加载
- 异步加载资源，不阻塞页面渲染

### 2. 主题适配

- 在页面加载时立即设置 `data-theme` 属性
- 监听系统主题变化事件
- 主题切换时触发看板娘重绘

### 3. 位置配置

- 根据网站布局选择合适的位置
- 避免与网站其他元素重叠
- 考虑响应式设计的需要

---

## 📖 相关资源

- [GitHub 仓库](https://github.com/yxksw/live2d-widget-v3)
- [Live2D Cubism SDK](https://www.live2d.com/en/)
- [jsDelivr CDN](https://www.jsdelivr.com/)

---

## 📄 许可证

本项目基于 [MIT License](LICENSE) 开源协议。

---

**祝你使用愉快！** 🎉
