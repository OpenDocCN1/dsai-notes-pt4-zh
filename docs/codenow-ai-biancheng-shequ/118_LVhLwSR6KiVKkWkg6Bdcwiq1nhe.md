# Next-Auth的quick-start

> 来源：[https://now.feishu.cn/docx/Svm0dqU9gobS5NxEKtNctOzin2g](https://now.feishu.cn/docx/Svm0dqU9gobS5NxEKtNctOzin2g)

## 安装NextAuth

```
npm install next-auth
```

```
yarn add next-auth
```

```
pnpm add next-auth
```

### Add API route 添加 API 路由

```
/app/api/auth/[...nextauth]/route.ts

import NextAuth from &quot;next-auth&quot;

const handler = NextAuth({
  ...
})

export { handler as GET, handler as POST }
```