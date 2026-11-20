
# CORS 配置问题排查与修复记录

## CORS 配置

```json
[{"maxAgeSeconds": 3600, "method": ["GET", "HEAD", "PUT", "POST", "DELETE"], "origin": ["*"], "responseHeader": ["Content-Type", "Access-Control-Allow-Origin", "Authorization", "X-Requested-With", "Accept", "Origin"]}]
```

CORS 已经设置了。让我检查 Firebase Storage 的初始化配置和代码：

搜索 `storage.ref`，共 1 个结果。

分析 `firebase.ts#L1-35`。

🔍 找到问题了！

第 13 行的 `storageBucket` 配置错误！

- 当前：

  ```
  copypose-7f3b2.firebasestorage.com
  ```

- 正确：

  ```
  copypose-7f3b2.firebasestorage.app
  ```

这就是为什么 CORS 错误持续存在的原因！让我修复：

编辑 `firebase.ts`

```
+1-1
```

## 修复 Balance 为 0 的问题

现在修复 `Balance` 为 0 的问题。需要更新 `redeemPromoCode.ts` 来记录 `creditsAfter`：

分析 `redeemPromoCode.ts#L85-130`。

编辑 `redeemPromoCode.ts`

```
+5-0
```

## Cloud Functions 构建与部署

### 构建 Cloud Functions

现在重新构建 Cloud Functions 并部署：

```bash
~/…/测试开发版251107---ai-pose-shifter (2) $ cd firebase/functions && npm run build
> build
> tsc
```

### 启动 Firebase Emulator

重启开发服务器以应用前端更改：

```bash
~/…/测试开发版251107---ai-pose-shifter (2)   pkill -f "firebase emulators:start" || true
```

```bash
~/…/测试开发版251107---ai-pose-shifter (2) $ firebase emulators:start --only functions
```

<truncated 2 lines>

```
⚠  functions: Application Default Credentials detected. Non-emulated services will access production using these credentials. Be careful!

i  functions: Watching "/Users/maxecho/Downloads/测试开发版251107---ai-pose-shifter (2)/firebase/functions" for Cloud Functions...

⚠  functions: Your requested "node" version "20" doesn't match your global version "24". Using node@24 from host.

i  functions: Loaded environment variables from .env.

Serving at port 8526

✔  functions: Loaded functions definitions from source: createCheckout, deductCredits, creemWebhook, redeemPromoCode.

✔  functions[us-central1-createCheckout]: http function initialized (http://127.0.0.1:5001/copypose-7f3b2/us-central1/createCheckout).

✔  functions[us-central1-deductCredits]: http function initialized (http://127.0.0.1:5001/copypose-7f3b2/us-central1/deductCredits).

✔  functions[us-central1-creemWebhook]: http function initialized (http://127.0.0.1:5001/copypose-7f3b2/us-central1/creemWebhook).

✔  functions[us-central1-redeemPromoCode]: http function initialized (http://127.0.0.1:5001/copypose-7f3b2/us-central1/redeemPromoCode).

┌─────────────────────────────────────────────────────────────┐

│ ✔  All emulators ready! It is now safe to connect your app. │

│ i  View Emulator UI at http://127.0.0.1:4000/               │

└─────────────────────────────────────────────────────────────┘

┌───────────┬────────────────┬─────────────────────────────────┐

│ Emulator  │ Host:Port      │ View in Emulator UI             │

├───────────┼────────────────┼─────────────────────────────────┤

│ Functions │ 127.0.0.1:5001 │ http://127.0.0.1:4000/functions │

└───────────┴────────────────┴─────────────────────────────────┘

Emulator Hub host: 127.0.0.1 port: 4400

Other reserved ports: 4500

Issues? Report them at https://github.com/firebase/firebase-tools/issues and attach the *-debug.log files.

i  functions: Loaded environment variables from .env.

⚠  functions: The Cloud Firestore emulator is not running, so calls to Firestore will affect production.

i  functions: Beginning execution of "us-central1-redeemPromoCode"

i  functions: Finished "us-central1-redeemPromoCode" in 1.368375ms

i  functions: Beginning execution of "us-central1-redeemPromoCode"

> {"verifications":{"app":"MISSING","auth":"VALID"},"logging.googleapis.com/labels":{"firebase-log-type":"callable-request-verification"},"severity":"DEBUG","message":"Callable request verification passed"}

⚠  Google API requested!

+   URL: "https://oauth2.googleapis.com/token"

+   Be careful, this may be a production service.

> Promo code PSTRY-3YCB2R redeemed by user WBmGfrEfiIc44jdX8yfgPsx7d0q1 for 100 credits.

i  functions: Finished "us-central1-redeemPromoCode" in 1965.185792ms

i  functions: Loaded environment variables from .env.

i  functions: Beginning execution of "us-central1-deductCredits"

i  functions: Finished "us-central1-deductCredits" in 1.211083ms

i  functions: Beginning execution of "us-central1-deductCredits"

{"verifications":{"app":"MISSING","auth":"VALID"},"logging.googleapis.com/labels":{"firebase-log-type":"callable-request-verification"},"severity":"DEBUG","message":"Callable request verification passed"}

⚠  Google API requested!

+   URL: "https://oauth2.googleapis.com/token"

+   Be careful, this may be a production service.

> ✅ Credits deducted for WBmGfrEfiIc44jdX8yfgPsx7d0q1: -3 (P:315, S:0, Total:315)

i  functions: Finished "us-central1-deductCredits" in 1625.8163749999999ms

i  functions: Beginning execution of "us-central1-deductCredits"

i  functions: Finished "us-central1-deductCredits" in 2.84525ms

i  functions: Beginning execution of "us-central1-deductCredits"

{"verifications":{"app":"MISSING","auth":"VALID"},"logging.googleapis.com/labels":{"firebase-log-type":"callable-request-verification"},"severity":"DEBUG","message":"Callable request verification passed"}

✅ Credits deducted for DEUwTTRumiaQz6pZFcLtjgUFkYk2: -3 (P:562, S:150, Total:712)

i  functions: Finished "us-central1-deductCredits" in 1016.339792ms

i  functions: Beginning execution of "us-central1-deductCredits"

i  functions: Finished "us-central1-deductCredits" in 6.371292ms

i  functions: Beginning execution of "us-central1-deductCredits"

{"verifications":{"app":"MISSING","auth":"VALID"},"logging.googleapis.com/labels":{"firebase-log-type":"callable-request-verification"},"severity":"DEBUG","message":"Callable request verification passed"}

✅ Credits deducted for DEUwTTRumiaQz6pZFcLtjgUFkYk2: -3 (P:562, S:147, Total:709)

i  functions: Finished "us-central1-deductCredits" in 590.103916ms

i  functions: Beginning execution of "us-central1-redeemPromoCode"

i  functions: Finished "us-central1-redeemPromoCode" in 7.577584ms

i  functions: Beginning execution of "us-central1-redeemPromoCode"

{"verifications":{"app":"MISSING","auth":"VALID"},"logging.googleapis.com/labels":{"firebase-log-type":"callable-request-verification"},"severity":"DEBUG","message":"Callable request verification passed"}

Promo code PSTRY-3JNJKU redeemed by user DEUwTTRumiaQz6pZFcLtjgUFkYk2 for 100 credits.

i  functions: Finished "us-central1-redeemPromoCode" in 1444.506042ms

⚠  functions: Your requested "node" version "20" doesn't match your global version "24". Using node@24 from host.

i  functions: Loaded environment variables from .env.

Serving at port 8838

✔  functions: Loaded functions definitions from source: createCheckout, deductCredits, creemWebhook, redeemPromoCode.

⚠  functions: Your requested "node" version "20" doesn't match your global version "24". Using node@24 from host.

i  functions: Loaded environment variables from .env.

Serving at port 8720

⚠  functions: Your requested "node" version "20" doesn't match your global version "24". Using node@24 from host.

i  functions: Loaded environment variables from .env.

Serving at port 8087

✔  functions: Loaded functions definitions from source: createCheckout, deductCredits, creemWebhook, redeemPromoCode.

⚠  functions: Your requested "node" version "20" doesn't match your global version "24". Using node@24 from host.

i  functions: Loaded environment variables from .env.

Serving at port 8867

✔  functions: Loaded functions definitions from source: createCheckout, deductCredits, creemWebhook, redeemPromoCode, getReferralCode, checkIPLimit, processReferral, recordIPRegistration.

✔  functions[us-central1-getReferralCode]: http function initialized (http://127.0.0.1:5001/copypose-7f3b2/us-central1/getReferralCode).

✔  functions[us-central1-checkIPLimit]: http function initialized (http://127.0.0.1:5001/copypose-7f3b2/us-central1/checkIPLimit).
```
