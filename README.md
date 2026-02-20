# 接口小助手

一个极简的网页工具，用来手动发起 HTTP 请求做联调与排查。页面入口：[index.html](file:///Users/purpleplane/code/purify/index.html)。

## 功能
- 支持 GET 与 POST。
- POST 强校验 JSON：在发送前先 `JSON.parse`，失败会立刻报错，成功后再 `JSON.stringify`。
- 自动设置 `Content-Type: application/json`（POST）。
- 显示响应状态码与耗时。
- 尝试美化 JSON 响应：若能解析为 JSON，则以两空格缩进显示；非 JSON 原样展示。
- 历史记录：保存最近 5 次请求（方法、URL、Body），点击可回填。
- GET 时隐藏 Body 文本框，POST 时显示。

## 使用方法
- 打开 [index.html](file:///Users/purpleplane/code/purify/index.html)（直接用浏览器或开启本地静态服务器）。
- 选择方法（GET/POST），填写完整 URL（如 `https://httpbin.org/get`）。
- 若为 POST，在“POST Body（JSON）”文本框中填写合法 JSON（如 `{"name":"alice"}`）。
- 点击“发送”，上方显示“状态: <code> | 耗时: <ms>”，下方显示响应文本。
- 历史记录位于“历史（点击回填）”区域，点按钮即可恢复上一条请求。
- 清空历史：在浏览器控制台执行 `localStorage.removeItem("history")`。

## 常见问题
- 跨域（CORS）：直接在浏览器请求其他域名时，目标服务必须允许跨域访问（返回 `Access-Control-Allow-Origin` 等）。若服务端未开启跨域，会报错，这是服务端策略导致，并非本工具问题。
- 非 JSON 响应：页面会原样显示；若是可解析的 JSON，页面会自动缩进美化。
- 请求头与认证：当前仅设置了 `Content-Type`；如需自定义请求头（例如 `Authorization`），可按下文“扩展开发”自行添加。
- 仅支持 GET/POST：若需 PUT/DELETE/PATCH，请扩展。

## 扩展开发
- 支持更多方法：在方法下拉框中增加 `PUT/DELETE/PATCH`，并在发送逻辑中处理。
- 自定义请求头：增加一个“请求头”文本框，填写如 `{"Authorization":"Bearer xxx"}`，在发送前合并到 `options.headers`。
- 更友好错误信息：区分 JSON 语法错误与网络/CORS 错误，分别显示提示。
- 响应高亮：引入轻量的代码高亮库或自己实现关键字着色，以便阅读。

## 目录与代码位置
- 页面与逻辑均在单文件：[index.html](file:///Users/purpleplane/code/purify/index.html)。
  - 事件绑定与发送逻辑： [index.html:L114-L175](file:///Users/purpleplane/code/purify/index.html#L114-L175)
  - 历史记录渲染： [index.html:L84-L106](file:///Users/purpleplane/code/purify/index.html#L84-L106)
  - 方法切换隐藏/显示 Body： [index.html:L108-L113](file:///Users/purpleplane/code/purify/index.html#L108-L113)

## 本地预览
- 可直接双击打开 `index.html`。
- 若需要通过 http 服务访问（便于调试跨域或某些接口），任选其一：
  - Node：`npx serve .`
  - Python：`python3 -m http.server 5500`
  - 打开浏览器访问 `http://127.0.0.1:5500/index.html`

## 免责声明
- 此工具仅用于开发联调与教学演示，请勿在生产场景中直接使用。
- 切勿在请求中包含敏感信息或密钥；本工具不会持久化除历史记录以外的内容，历史保存在浏览器 `localStorage` 中。

