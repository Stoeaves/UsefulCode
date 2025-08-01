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
