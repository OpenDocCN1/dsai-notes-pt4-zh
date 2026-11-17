
# Project Structure

> 来源：[https://qianniucity.feishu.cn/docx/LymddZSIroA5QWxO4bicRnfSntf](https://qianniucity.feishu.cn/docx/LymddZSIroA5QWxO4bicRnfSntf)

```
├── node_modules
├── prisma
│   ├── db.sqlite          # SQLite database file
│   └── schema.prisma      # Prisma schema definition
├── public                 # project static file,like img,logo,etc
├── app                # Application logic
│   ├── (auth)         # NextAuth layout and page
│   ├── (dashboard)    # Dashboard layout and page
|   ├── (docs)         # docs layout and page
│   ├── (marketing)    # marketing layout and page
│   ├── (editor)       # editor layout and page
│   ├── api            # api route (service api)
├── assets             # Assets not exposed to the public,like fonts
├── components         # React components used on pages
├── config             # BuouAI configuration folder
│   ├── dashboard      # Configuration for dashboard mainNav,sidebarNav
│   ├── docs           # Configuration for docs mainNav,sidebarNav
│   ├── marketing      # Configuration for marketing mainNav,sidebarNav
│   ├── site           # Configuration for BuouAI (site metadata, description, author, etc.)
│   ├── subscriptions  # Configuration for subscriptions
├── lib                # Libraries (session, paypal, cloudflare r2, stripe, subscriptions, etc.)
├── pages              # Next.js pages
├── styles             # Styling
├── types              # Typings definition
├── env.mjs                # Environment variables schema
├── middleware.ts          # next-intl middleware (handles redirects and rewrites based on user locale)
├── .env.example           # Example environment file
├── .eslintrc.cjs          # ESLint configuration
├── .gitignore
├── next-sitemap.config.js # sitemap
├── next.config.mjs        # Next.js, next-intl, and Nextra configuration
├── package.json
├── pnpm-lock.yaml         # pnpm depedency
├── postcss.config.cjs
├── prettier.config.cjs
├── README.md
├── tailwind.config.ts     # Tailwind configuration
└── tsconfig.json          # TypeScript configuration
```
