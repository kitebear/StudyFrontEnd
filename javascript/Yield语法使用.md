```JS
const combineAsyncError = tasks => {  
    const doGlide = {}  
    const handler = res => {  
        doGlide.out = res  
        // 预先定义好生成器  
        doGlide.node = (function*(){  
        const { out, data } = doGlide  
        const len = tasks.length  
        // yield把循环带回了JavaScript编程的世界  
        while(doGlide.times < len)  
            yield noErrorAwait(tasks[doGlide.times++])  
        // 全部请求成功(生成器执行完毕)时，返回数据  
        out(data)  
        })()  
    doGlide.node.next()  
    }  
    return new Promise(res => handler(res))  
}
```
