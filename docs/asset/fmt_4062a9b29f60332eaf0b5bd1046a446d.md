
# Next-Auth的quick-start

> 来源：[https://now.feishu.cn/docx/Svm0dqU9gobS5NxEKtNctOzin2g](https://now.feishu.cn/docx/Svm0dqU9gobS5NxEKtNctOzin2g)

## 安装 NextAuth

```bash
npm install next-auth
```

```bash
yarn add next-auth
```

```bash
pnpm add next-auth
```

### Add API route 添加 API 路由

`/app/api/auth/[...nextauth]/route.ts`

```typescript
import NextAuth from "next-auth";

const handler = NextAuth({
  ...
})

export { handler as GET, handler as POST }
```
