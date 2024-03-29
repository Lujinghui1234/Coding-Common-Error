# 项目开发易错点
## 1. 类型混淆
```js
const hasLastRound = 'true'||'false';
if(hasLastRound){
    ...do something;
};
```
问题：hasLastRound为'false'也会执行，因为是string类型。

正确写法：
```js
const lastRound = hasLastRound==='true' ? hasLastRound : undefined;
if(lastRound){
    ...do something;
};
```
## 2. 解构赋值中的剩余参数...rest
```js
const {a,b,...rest} = formContent;
const newFormContent = {
    a,
    ...rest
};
console.log(rest);//rest不包含b了，没用到b就不要解构
```
## 3. react中的setState是异步行为
```js
const [data,setData] = useState('');
const res = await callApi1();
const resName = res.payload.data.name;
setData(resName);
await callApi2(data);//data还是''
```
   问题：setState一直是异步行为，所以data还是旧数据。
   
   正确写法：
```js
await callApi2(resName);//考虑useState是否有必要？
```
## 4.自测的时候没有覆盖到所有场景，比如某些数据没有填写直接提交，导致空数据场景报错，如：在undefined或null中解构某属性
## 5. 同一个api，页面call：cancel，但postman call：200
因为postman和浏览器的环境不同，postman会设置了某些东西，用户使用环境也是浏览器，肯定要以浏览器为准。
## 6. 线上dev环境和local页面不一致，检查dev版本是否最新，切换无痕浏览器清除缓存更新版本
## 7. 使用react-router的location.state存储数据，刷新页面数据丢失
这个方案不是持久化存储数据方式，可替换为localStorage、redux-persist（redux的插件，刷新数据不丢失）
## 8. 不要滥用可选链，看为什么是空数据，是api原因还是前端代码写错了？有些明知道不会是空数据的，就不要加可选链了。
## 9.常见ts报错：

![image.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/912ef097268a48f780741251f314f871~tplv-k3u1fbpfcp-watermark.image?)
答案：https://blog.csdn.net/qq_40864647/article/details/125764130

最简单的方法是断言：

![image.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/b13858456251461d879e5ee68fd8337a~tplv-k3u1fbpfcp-watermark.image?)
## 10. 为什么不会发生变化的常量，要写在Function外面？
因为写在Function里面，rerender的时候diff算法会去计算这个变量的值是否和上次一样，而写在外面不会，这样写可以省去计算消耗的时间。补充：写在Function外面的变量和import的资源在reRender时都不会去执行。
```js
const data = '123';
const Com = ():ReactElement=>{
    <div>hello world !</div>
}
```
## 11. setState数组不更新
错误写法：
```js
const [list,setList] = useState([]);
const data = await callApi();
setList(data);
const newList = list.map(item=>{//此处的list并不是最新的。
    a:item.a;
});
```
正确写法：
```
const [list,setList] = useState([]);
const data = await callApi();
const newList = [...list,...data];
setList(newList);
console.log(newList);//拿到最新的，如果打印list是拿不到的，但html中使用list会是最新的。
```
参考React文档：
https://zh-hans.react.dev/reference/react/useState#ive-updated-the-state-but-logging-gives-me-the-old-value
参考别人总结的资料：
![e03fed3e04d226de56b2c28664b79d83](https://github.com/Lujinghui1234/Coding-Common-Error/assets/109168485/a9a510f8-0feb-4b45-9676-dfd68a555154)

## 12. 自定义hook执行多次
## 13. git commit 报错：pidtree\lib\pidtree.js    new Error: no matching pid found
这个报错是因为本地进程太多了，解决方法是清理本地内存空间：清空回收站、关闭不使用的进程，关闭编辑器，直接用git bash操作。
## 14. 

