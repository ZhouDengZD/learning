参考：https://github.com/ReactTraining/history/blob/master/docs

#### 一、三种方法创建对象：
1.createBrowserHistory 适用于支持HTML5历史记录API的现代Web浏览器
2.createHashHistory 用于需要将位置存储在当前URL的哈希中的情况，以避免在页面重新加载时将其发送到服务器
3.createMemoryHistory 用作参考实现，也可以在非DOM环境中使用，例如React Native或测试

注：三种方法创建对象时的默认options
```
createBrowserHistory({
  basename: '', // The base URL of the app，此选项将给定的字符串添加到所有URL的开头。
  forceRefresh: false, // Set true to force full page refreshes，默认情况下，createBrowserHistory使用HTML5 pushState和replaceState来防止在浏览时从服务器重新加载整个页面。 相反，如果想在URL更改时重新加载，设为true。
  keyLength: 6, // The length of location.key
  getUserConfirmation: (message, callback) => callback(window.confirm(message))   // A function to use to confirm navigation with the user
});

createHashHistory({
  basename: '', 
  hashType: 'slash', // slash：斜杠。默认情况下，createHashHistory在基于哈希的URL中使用前导斜杠。 可以使用hashType选项来使用其他哈希格式。
  getUserConfirmation: (message, callback) => callback(window.confirm(message))   // A function to use to confirm navigation with the user
});

createMemoryHistory({
  initialEntries: ['/'], // The initial URLs in the history stack
  initialIndex: 0, // The starting index in the history stack
  keyLength: 6, 
  getUserConfirmation: null // A function to use to confirm navigation with the user. Required if you return string prompts from transition hooks
});
```

#### 二、基本用法:
```
import { createBrowserHistory } from 'history';

const history = createBrowserHistory();

const location = history.location; // Get the current location.获取当前location

// Listen for changes to the current location. 监听当前location的变化
const unlisten = history.listen((location, action) => { // location is an object like window.location
  console.log(action, location.pathname, location.state);
});

history.push('/home', { some: 'state' }); // Use push, replace, and go to navigate around.

unlisten(); // To stop listening, call the function returned from listen(). 通过调用listen()返回的方法停止监听
```

##### 2.1 listen方法：
可以通过history.listen监听当前location变化
```
history.listen((location, action) => {
  console.log(
    `The current URL is ${location.pathname}${location.search}${location.hash}`
  );
  console.log(`The last navigation action was ${action}`);
});
```

l注：
（1）location和window.location相似，包括：
- location.pathname
- location.search
- location.hash
也许还有
- location.state - 此 location 的某些 不在URL中 其他状态 (supported in createBrowserHistory and createMemoryHistory)
- location.key - 代表此 location 的唯一字符串 (supported in createBrowserHistory and createMemoryHistory)

（2）action是 PUSH, REPLACE, POP之一，具体取决于用户如何访问当前url。

#### 三、history的属性 ：
1. history.length - 历史堆栈（history stack）中entries的数量
2. history.location - 当前 location
3. history.action - 当前导航行为（navigation action）
另外, createMemoryHistory 还提供 history.index 和 history.entries 让我们可以检查历史记录对战

#### 四、history的方法 ：
1.history.push(path, [state])
2.history.replace(path, [state])
3.history.go(n)
4.history.goBack()
5.history.goForward()
6.history.canGo(n) (only in createMemoryHistory)

使用push、replace时，既可以将URL路径和状态都指定为单独的参数，也可以将所有内容都包含在单个类似于位置的对象中作为第一个参数。例如：
```
history.push('/home?the=query', { some: 'state' });

history.push({
  pathname: '/home',
  search: '?the=query',
  state: { some: 'state' }
});
```

以下两个作用相同:
```
history.go(-1);
history.goBack();
```

