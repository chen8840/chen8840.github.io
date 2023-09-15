### Promise里捕捉错误的最佳实践

Promise里的同步部分不需要try catch

```javascript
new Promise((resolve, reject) => {
    throw new Error('error');
    setTimeout(() => {

    }, 100);
}).catch(e => {
    console.log('log', e);
});
```
异步部分需要try catch
```javascript
new Promise((resolve, reject) => {

    setTimeout(() => {
        try{
            throw new Error('error');
        } catch(e) {
            reject(e);
        }
    }, 100);
}).catch(e => {
    console.log('log', e);
});;
```
