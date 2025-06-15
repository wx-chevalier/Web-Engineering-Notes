# tsup

tsup 是一个基于 esbuild 的 TypeScript 打包工具，它提供了简单易用的命令行界面，能够快速构建 TypeScript 项目。

## 特点

- 🚀 极快的构建速度（基于 esbuild）
- 📦 零配置，开箱即用
- 🔥 支持 TypeScript
- 🎯 支持多种输出格式（ESM、CommonJS、UMD）
- 🔍 支持源码映射（Source Maps）
- 🛠 支持自定义配置

## 安装

```bash
# npm
npm install tsup --save-dev

# yarn
yarn add tsup --dev

# pnpm
pnpm add tsup -D
```

## 基本使用

### 命令行使用

```bash
# 构建单个文件
tsup src/index.ts

# 构建多个文件
tsup src/index.ts src/cli.ts

# 监听模式
tsup src/index.ts --watch

# 指定输出格式
tsup src/index.ts --format esm,cjs
```

### 配置文件

创建 `tsup.config.ts` 文件：

```typescript
import { defineConfig } from "tsup";

export default defineConfig({
  entry: ["src/index.ts"],
  format: ["cjs", "esm"],
  dts: true,
  splitting: false,
  sourcemap: true,
  clean: true,
});
```

## 常用配置选项

- `entry`: 入口文件
- `format`: 输出格式（'cjs' | 'esm' | 'iife'）
- `dts`: 是否生成类型声明文件
- `splitting`: 是否启用代码分割
- `sourcemap`: 是否生成 sourcemap
- `clean`: 构建前是否清理输出目录
- `minify`: 是否压缩代码
- `target`: 目标环境（'node' | 'browser'）
- `outDir`: 输出目录
- `env`: 环境变量

## 高级用法

### 自定义构建过程

```typescript
import { defineConfig } from "tsup";

export default defineConfig({
  entry: ["src/index.ts"],
  async onSuccess() {
    // 构建成功后的回调
    console.log("Build completed!");
  },
  async onFailure() {
    // 构建失败后的回调
    console.error("Build failed!");
  },
});
```

### 使用插件

```typescript
import { defineConfig } from "tsup";

export default defineConfig({
  entry: ["src/index.ts"],
  esbuildPlugins: [
    // 自定义 esbuild 插件
  ],
});
```

## 最佳实践

1. 使用 `dts: true` 自动生成类型声明文件
2. 在开发时使用 `--watch` 模式
3. 使用 `clean: true` 确保每次构建都是干净的
4. 根据项目需求选择合适的输出格式
5. 使用 `sourcemap: true` 方便调试

## 常见问题

### Q: 如何处理外部依赖？

A: 使用 `external` 选项指定不需要打包的依赖：

```typescript
export default defineConfig({
  external: ["react", "react-dom"],
});
```

### Q: 如何配置环境变量？

A: 使用 `env` 选项：

```typescript
export default defineConfig({
  env: {
    NODE_ENV: process.env.NODE_ENV,
  },
});
```

## 相关资源

- [官方文档](https://tsup.egoist.dev/)
- [GitHub 仓库](https://github.com/egoist/tsup)
- [esbuild 文档](https://esbuild.github.io/)
