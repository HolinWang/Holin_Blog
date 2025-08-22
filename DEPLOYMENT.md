# VitePress 部署到 Vercel 指南

## 问题解决

如果你遇到 404 错误，请确保：

1. **项目根目录包含 `vercel.json` 配置文件**
2. **VitePress 配置中设置了正确的 `base` 路径**
3. **构建命令和输出目录配置正确**

## 部署步骤

### 1. 本地测试
```bash
# 安装依赖
pnpm install

# 本地开发
pnpm docs:dev

# 构建测试
pnpm docs:build
```

### 2. Vercel 部署
1. 将代码推送到 GitHub
2. 在 Vercel 中导入项目
3. Vercel 会自动检测 `vercel.json` 配置
4. 部署完成后访问你的域名

## 配置文件说明

### vercel.json
```json
{
  "buildCommand": "pnpm docs:build",
  "outputDirectory": "docs/.vitepress/dist",
  "framework": null,
  "rewrites": [
    {
      "source": "/(.*)",
      "destination": "/index.html"
    }
  ]
}
```

### docs/.vitepress/config.mts
确保设置了正确的 `base` 路径：
```typescript
export default defineConfig({
  base: '/',
  // ... 其他配置
})
```

## 常见问题

1. **404 错误**: 检查 `vercel.json` 是否存在且配置正确
2. **构建失败**: 确保所有依赖都已安装
3. **路由问题**: 检查 `base` 路径配置

## 优化建议

1. 添加缓存策略
2. 配置自定义域名
3. 设置环境变量（如需要） 