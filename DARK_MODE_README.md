# Live2D Widget - Imm 博客深色模式适配

## 文件说明

### waifu-dark.css
专门为 Imm 博客深色模式适配的 CSS 文件，用于覆盖 waifu.css 中的样式。

## 使用方法

### 1. 在 Imm 博客中引用

在 BaseLayout.astro 或其他布局文件中添加：

```javascript
const cdnPath = 'https://cdn.jsdelivr.net/gh/dogxii/live2d-widget-v3@main/'

// 加载资源
Promise.all([
    loadExternalResource(cdnPath + 'waifu.css', 'css'),
    loadExternalResource(cdnPath + 'waifu-dark.css', 'css'),
    // ... 其他资源
]).then(() => {
    initWidget({
        // ... 配置
    })
})
```

### 2. 深色模式检测

确保你的博客有主题检测机制，设置 `data-theme` 属性：

```javascript
// 浅色模式
document.documentElement.setAttribute('data-theme', 'light')

// 深色模式
document.documentElement.setAttribute('data-theme', 'dark')
```

### 3. 主题切换监听

```javascript
// 监听主题变化
window.matchMedia('(prefers-color-scheme: dark)').addEventListener('change', (e) => {
    document.documentElement.setAttribute('data-theme', e.matches ? 'dark' : 'light')
    // 触发 Live2D 重绘
    const waifu = document.getElementById('waifu')
    if (waifu) {
        waifu.style.display = 'none'
        setTimeout(() => {
            waifu.style.display = 'block'
        }, 10)
    }
})
```

## 深色模式样式说明

### 对话框样式
- 背景：深黑色半透明 `rgba(20, 20, 20, 0.98)`
- 边框：白色半透明 `rgba(255, 255, 255, 0.1)`
- 文字：纯白色 `#ffffff`
- 阴影：深黑色 `rgba(0, 0, 0, 0.8)`

### 工具栏样式
- 默认颜色：浅灰色 `#cccccc`
- 悬停颜色：绿色 `#07C160`

## 注意事项

1. **加载顺序**：必须先加载 `waifu.css`，再加载 `waifu-dark.css`
2. **选择器优先级**：使用了 `!important` 确保覆盖原样式
3. **主题属性**：确保 HTML 元素上有 `data-theme` 属性
4. **CDN 缓存**：修改后可能需要清除 CDN 缓存

## 技术细节

### CSS 选择器
```css
/* 使用 html[data-theme="dark"] 选择器 */
html[data-theme="dark"] #waifu .waifu-tips {
    /* 深色模式样式 */
}
```

### 覆盖的元素
- 对话框（waifu-tips）
- 对话框内所有文字元素（*, a, span, div, p, font）
- 工具栏按钮（waifu-tool）
- 菜单（waifu-menu）
- 拍照界面（waifu-photo）
- 一言界面（waifu-hitokoto）
- 信息面板（waifu-info）

## 兼容性

- ✅ 支持系统深色模式（prefers-color-scheme）
- ✅ 支持手动主题切换（data-theme 属性）
- ✅ 支持 MutationObserver 监控动态变化
- ✅ 移动端自动隐藏

## 示例项目

- Imm 博客：https://github.com/zsxcoder/imm-blog
- Live2D Widget: https://github.com/dogxii/live2d-widget-v3

## 许可证

遵循原项目许可证
