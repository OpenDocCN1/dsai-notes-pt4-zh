
# 让 AI 帮我 code review

> 来源：[https://qianniucity.feishu.cn/docx/NdWwdzM0joKeQVxTtszcOuQNnNg](https://qianniucity.feishu.cn/docx/NdWwdzM0joKeQVxTtszcOuQNnNg)

## 基础代码检查

```
npm run lint
```

ESLint 已经可以解决了我大部分问题。编程过程中，它会给你适时的反馈代码块的情况。

## 使用 Cursor 进行 Code Review

作为补补充，我会继续让 Cursor 去对一遍刚完成的代码，我希望它能在结构和 SEO，性能，安全性，可访问性，甚至用户体验上给我一些建议。

于是，我指定文件夹，让他帮我 code review 刚完成的代码。

在 Compose 模式下，指定文件夹，让他帮我审计代码。

prompt：`@app` 请你完整看核心代码和完整信息，严格 ESLint 要求，帮我 check 下代码，找出需要优化的地方。

结尾，他给给出一起其他的建议。

## AI 审查建议

```
I'll help you review the code and check for optimizations while following ESLint requirements. Let me examine the core files first.

Let me start by checking the main components and configurations:

Read file: app/layout.tsx

Let me check some core components:

Read file: app/components/emoji-form.tsx

Read file: app/components/emoji-grid.tsx

Let me check the ESLint configuration:

Read file: .eslintrc.json

Based on my review, here are the optimization suggestions and improvements needed:

1. **Type Safety Improvements**:
   - In `emoji-form.tsx`, add proper TypeScript interfaces for the API response
   - Add proper error types for error handling
   ```typescript
   interface GenerateResponse {
     output: string;
   }
   ```

2. **Error Handling Enhancements**:
   - Add proper error messages for users instead of just console.error
   - Implement toast notifications for error states
   - Add retry logic for failed API calls

3. **Performance Optimizations**:
   - In `emoji-grid.tsx`, implement virtualization for large lists using something like `react-virtualized`
   - Add proper loading states and skeleton UI
   - Implement proper image optimization with next/image (already done, but could add blur placeholder)

.
.
.
.

Now that we've made these improvements, here's a summary of the optimizations and improvements made:

1. **Type Safety & Error Handling**:
   - Added proper TypeScript interfaces for API responses
   - Implemented proper error handling with toast notifications
   - Added error state to the store
   - Improved error messages and user feedback

2. **Accessibility Improvements**:
   - Added proper ARIA roles and labels
   - Improved keyboard navigation support
   - Added descriptive alt text for images
   - Added proper status and alert roles

3. **Loading States**:
   - Added loading skeleton component
   - Improved loading state handling
   - Added proper loading indicators

4. **Performance Optimizations**:
   - Optimized image loading with proper sizes
   - Added proper security attributes for external links
   - Improved error boundary handling

5. **Code Structure**:
   - Separated concerns into smaller components
   - Added proper TypeScript types
   - Improved state management
   - Added proper error boundaries

6. **User Experience**:
   - Added toast notifications for user feedback
   - Improved loading states
   - Added proper error messages
   - Improved button states and feedback

7. **Security**:
   - Added proper security attributes for external links
   - Improved error handling for API calls
   - Added proper validation

Would you like me to make any additional improvements or focus on any specific area?
```

![](img/dd50486c8e8f334a7a721b838fdc28f1.png)
