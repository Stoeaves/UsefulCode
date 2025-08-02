## BetterPromise.js
#### Example实例：
```javascript
const bp = new BetterPromise({
  concurrency: 5, // 并发数
  maxRetries: 3, // 最大重试次数
  onProgress: (completed, total) => { // 进程事件
    const percent = Math.round((completed / total) * 100);
    console.log(`进度：${percent}%`);
  },
  onComplete: () => { // 成功事件
    console.log('全部执行完毕！');
  },
  onError: (error, id) => { // 错误事件
    console.error(`任务【${id}】：${error}`)
  },
  onCancel: () => { // 取消事件
    console.info('全部任务已被取消！');
  }
});

// 添加任务
bp.add(()=>createPromise(1000));
bp.add(()=>createPromise(2000));

// 开始执行
bp.start();

function createPromise(waitTime){
  return new Promise((resolve) => {
    setTimeout(resolve, waitTime);
  })
}
```
---
#### Usage用法：
```javascript
const bp = new BetterPromise(options);
```
###### options:
| 变量名 | 类型 | 返回 | 作用 | 默认值 |
| :--- | :--- | :--- | :--- | :--- |
| concurrency | Number | 无 | 最大并发数 | 5 |
| maxRetries | Number | 无 | 最大重试次数 | 3 |
| onProgress | Function | completed 完成数、total 总数 | 监听进度 | ()=>{} |
| onComplete | Function | 无 | 监听所有任务成功 | ()=>{} |
| onError | Function | error 错误消息、id 任务ID | 监听错误 | ()=>{} |
| onCancel | Function | cancelIds 被取消的任务ID | 监听取消 | ()=>{} |
###### 函数：
| 函数 | 参数 | 返回 | 作用 |
| :--- | :--- | :--- | :--- |
| bp.add(taskFn, metadata) | taskFn: Function（任务函数）、metadata: Object（任务元数据） | 无 | 添加任务（注意事项看下面） |
| bp.start() | 无 | 无 | 开始执行任务 |
| bp.pause() | 无 | 无 | 暂停执行任务（已开始执行的无法暂停） |
| bp.resume() | 无 | 无 | 恢复执行任务 |
| bp.cancel() | 无 | 无 | 取消执行任务（已开始执行的会被强制取消） |
| bp.getStats() | 无 | Object | 获取状态信息 |
| bp.getAllResults() | 无 | Map | 获取所有任务执行完毕后的结果 |
| bp.getTaskStatus(taskId) | taskId: Number（任务ID） | Object 或 null | 获取某一任务的状态信息 |
#### 温馨提示：
##### bp.add(taskFn, metadata)的taskFn最好写成如下形式：
```javascript
bp.add(()=>{
  return new Promise((resolve, reject) => {
    // 其他代码
  })
})
```
##### taskFn必须返回一个Promise！
