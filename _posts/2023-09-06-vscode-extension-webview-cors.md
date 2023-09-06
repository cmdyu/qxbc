---
layout: post
title: 解决vscode扩展插件开发webview中的请求跨域问题
categories: vscode扩展插件开发
---
## 在webview中是无法发送跨域请求的，可以通过消息机制，在插件中发请求，然后将请求结果传递给webview

我的代码是基于[vscode-webview-ui-toolkit-samples-vue](https://github.com/microsoft/vscode-webview-ui-toolkit-samples/tree/main/frameworks/hello-world-vue)来写的

## webview vue组件中的代码示例

```javascript
async function initData() {
  // 向插件发送消息
  vscode.postMessage({
    command: 'getData',
    text: 'demo'
  })
}

// 监听插件消息
window.addEventListener('message', event => {
  const message = event.data; // The JSON data our extension sent

  switch (message.command) {
    case 'resultData':
      // 接收到数据
      const data = message.data
      // 然后就可以在webview中使用数据了
      break;
  }
})

onBeforeMount(() => {
  initData()
})
```

## 插件panel中的代码示例

```javascript
/**
   * Sets up an event listener to listen for messages passed from the webview context and
   * executes code based on the message that is recieved.
   *
   * @param webview A reference to the extension webview
   * @param context A reference to the extension context
   */
  private _setWebviewMessageListener(webview: Webview) {
    // 监听webview消息
    webview.onDidReceiveMessage(
      (message: any) => {
        const command = message.command;
        const text = message.text;

        switch (command) {
          case "getData":
            axios.get(`http://x.x.x.x:10200/${text}/list`).then(res => {
              // 将接口返回的数据发送给webview
              // 注意用axios发请求的化，发送数据中的data需要取res.data，而不能直接用res
              // 否则会报"TypeError: Converting circular structure to JSON"错误
              webview.postMessage({command: 'resultData', data: res.data});
            });
            return;
          // Add more switch case statements here as more webview message commands
          // are created within the webview context (i.e. inside media/main.js)
        }
      },
      undefined,
      this._disposables
    );
  }
```
