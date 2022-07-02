# 从一个简单的中间件 Demo 来了解 Redux applyMiddleware

多的不说，直接上代码，一步一步拆解 applyMiddleware 源码。

## 从 applyMiddleware 参数开始

```js
export default function applyMiddleware(...middlewares) {
  return (createStore) => {
    // xxx 做了其他的东西
  };
}
```

applyMiddleware 接收的参数是多个中间件，我们先写一个简单的函数代表中间件然后把它传入进去。他返回了一个参数是 createStore 的函数。

```js
import { applyMiddleware, legacy_createStore } from 'redux';
const myMiddleware = () => {};
const store = legacy_createStore(reducer, applyMiddleware(myMiddleware));
```

接着 redux 会在 createStore 函数里执行传递进来的 applyMiddleware 的返回的函数。

```js
export function createStore(reducer, preloadedState, enhancer) {
  if (typeof preloadedState === 'function' && typeof enhancer === 'undefined') {
    enhancer = preloadedState;
    preloadedState = undefined;
  }

  if (typeof enhancer !== 'undefined') {
    // 这里的 enhancer 就是 applyMiddleware 返回的函数，然后把 createStore 作为参数传递给他，然后再次执行
    return enhancer(createStore)(reducer, preloadedState);
  }
}
```

执行完 enhancer 之后会把 reducer 和 preloadedState 作为参数传递进去再次执行，也就是执行下面的 fn3。然后 redux 会在 fn3 里创建一个 store ，一个 dispatch 函数, 默认是抛出一个错误，创建一个 middlewareAPI 的对象，里里面有 store.getState 和前面声明的 dispatch 方法。接着会遍历 接收的中间件参数 middlewares，执行每一个中间件函数并且把 middlewareAPI 传递过去，所以我们写的中间件会接受 middlewareAPI 这个参数。

```js
// fn1
function applyMiddleware(...middlewares) {
  // fn2
  return (createStore) => {
    // fn3
    return (...args) => {
      const store = createStore(...args);
      let dispatch = () => {
        throw new Error(
          'Dispatching while constructing your middleware is not allowed. ' +
            'Other middleware would not be applied to this dispatch.'
        );
      };

      const middlewareAPI = {
        getState: store.getState,
        dispatch: (...args) => dispatch(...args),
      };
      // 遍历 middlewares 并且执行里面每一个中间件函数，吧 middlewareAPI 传进去
      const chain = middlewares.map((middleware) => middleware(middlewareAPI));
    };
  };
}

const myMiddleware = (middlewareAPI) => {};
```

然后 redux 会把 chain 传递给 compose 在他的里面对 chain 的值做函数组合，然后再次执行并且把 store.dispatch 作为参数传递进去。所以我们的中间件也需要返回一个函数，返回的这个函数的参数是 store.dispatch。接着会把我们中间件函数的返回值赋值给前面声明的 dispatch 函数，返回出去的时候会覆盖掉 store 里的 dispatch。然后因为我们正常的 dispatch 是一个函数，他接收一个 action 参数，所以我们的中间件需要再返回一个函数，同样也是接收 action 作为参数。

```js
// applyMiddleware
const chain = middlewares.map((middleware) => middleware(middlewareAPI));
dispatch = compose(...chain)(store.dispatch);

return {
  ...store,
  dispatch,
};


export default function compose(...funcs) {
  if (funcs.length === 0) {
    return (arg) => arg;
  }

  if (funcs.length === 1) {
    return funcs[0];
  }

// 对于有多种中间件的情况下，如果不做函数组合的话，正常是  middleware1(middleware2(middleware3(...args))) 这种使用情况。。通过一下这种方式做一个函数组合
  return funcs.reduce(
    (a, b) =>
      (...args) =>
        a(b(...args))
  );
}

const myMiddleware = (middlewareAPI) => {
  // 因为 middlewareAPI 里也有 dispatch 这个属性，所以这里接受的参数用 next 代替
  return (next) => {
    return (action) => {
      // xxx 做一些事情
      // 执行 dispatch
      next(action)
    };
  };
};
```
以上就是 redux applyMiddleware 的大致逻辑了。