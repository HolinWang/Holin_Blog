# VitePress 部署到 Vercel 指南

## 问题解决

### 404 错误
如果你遇到 404 错误，请确保：

1. **项目根目录包含 `vercel.json` 配置文件**
2. **VitePress 配置中设置了正确的 `base` 路径**
3. **构建命令和输出目录配置正确**

### 构建错误：`vitepress: command not found`
如果遇到构建错误，请确保：

1. **VitePress 安装在 `devDependencies` 中**（不是 `dependencies`）
2. **package.json 包含正确的项目信息和 engines 配置**
3. **使用 pnpm 作为包管理器**
4. **vercel.json 包含明确的构建命令**

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

### package.json
确保 VitePress 在 `devDependencies` 中，并包含 engines 配置：
```json
{
  "name": "holin-blog",
  "version": "1.0.0",
  "description": "前端知识学习博客",
  "type": "module",
  "engines": {
    "node": ">=18.0.0",
    "pnpm": ">=8.0.0"
  },
  "devDependencies": {
    "vitepress": "^1.6.4"
  },
  "scripts": {
    "docs:dev": "vitepress dev docs",
    "docs:build": "vitepress build docs",
    "docs:preview": "vitepress preview docs"
  }
}
```

### vercel.json
```json
{
  "buildCommand": "pnpm install && pnpm docs:build",
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

### .npmrc
```ini
auto-install-peers=true
shamefully-hoist=true
strict-peer-dependencies=false
```

### .nvmrc
```
18
```

## 常见问题

1. **404 错误**: 检查 `vercel.json` 是否存在且配置正确
2. **构建失败**: 确保所有依赖都已安装
3. **路由问题**: 检查 `base` 路径配置
4. **vitepress: command not found**: 确保 VitePress 在 `devDependencies` 中
5. **Vercel 构建错误**: 检查 `engines` 配置和构建命令

## 优化建议

1. 添加缓存策略
2. 配置自定义域名
3. 设置环境变量（如需要）

## 故障排除

### 如果仍然遇到 `vitepress: command not found` 错误：

1. **检查 package.json**：
   - 确保 `vitepress` 在 `devDependencies` 中
   - 确保包含 `engines` 配置
   - 确保 `type: "module"` 设置正确

2. **检查 vercel.json**：
   - 确保 `buildCommand` 包含 `pnpm install`
   - 确保 `outputDirectory` 路径正确

3. **检查依赖文件**：
   - 确保 `pnpm-lock.yaml` 存在且最新
   - 确保 `.npmrc` 配置正确

4. **重新部署**：
   - 清除 Vercel 缓存
   - 重新触发部署 