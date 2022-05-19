## JS 面试题

### 1. 防抖

```js
function debounce(fn, time) {
  let timer;
  return function(..args) {
    if (timer) clearTimeout(timer);

    timer = setTimeout(() => {
      fn.call(this, ...args)
    }, time)
  }
}
```

### 2. 节流

```js
function thorttle(fn, time) {
  let timer;
  return function (...args) {
    if (!timer) {
      timer = setTimeout(() => {
        timer = null;
        fn.call(this, ...args);
      }, time);
    }
  };
}
```

### 3. call/apply/bind

```js
Function.prototype.myCall = function (_this = globalThis, ...args) {
  const key = Symbol('key');
  _this[key] = this;
  const res = _this[key](...args);
  delete _this[key];
  return res;
};

Function.prototype.myApply = function (_this = globalThis, arg) {
  const key = Symbol('key');
  _this[key] = this;
  const res = _this[key](...arg);
  delete _this[key];
  return res;
};

Function.prototype.myBind = function (_this = globalThis, ...arg) {
  return (...val) => {
    return this.call(_this, ...arg, ...val);
  };
};
```

### 4. new 操作符

```js
function myNew(...args) {
  const obj = {};
  const constructor = [].shift.call(args);
  obj.__proto__ = constructor.prototype;
  const res = constructor.call(obj, ...args);

  return typeof res === 'object' ? res || obj : obj;
}
```

### 5. 柯里化

```js
const curry = (fn, arr = []) => {
  return (...args) => {
    if ([...arr, ...args].length == fn.length) {
      return fn(...[...arr, ...args]);
    } else {
      return curry(fn, [...arr, ...args]);
    }
  };
};

// 可以用立即执行函数简写为
const curry = (fn, arr = []) => {
  return (...args) => {
    return ((val) => (val.length === fn.length ? fn(...val) : curry(fn, val)))([
      ...arr,
      ...args,
    ]);
  };
};
```

### 6. 偏函数

```js
const partial = (fn, ...args) => {
  return (...val) => {
    return fn(...args, ...val);
  };
};
```

### 7. typeof

```js
function typeOf(obj) {
  return Object.prototype.toString.call(obj).slice(8, -1).toLowerCase();
}
```

### 8. instanceof

```js
function instanceOf(left, right) {
  let proto = left.__proto__;

  while (true) {
    if (proto === right.prototype) return true;
    if (proto === null) return null;
    proto = proto.__proto__;
  }
}
```

### 9. 深拷贝

```js
function deepClone(obj) {
  if (typeof obj !== 'object') return 'not obj';

  const newObj = obj instanceof Array ? [] : {};

  for (let key in obj) {
    if (obj.hasOwnProperty(key)) {
      newObj[key] =
        typeof obj[key] === 'object' ? deepClone(obj[key]) : obj[key];
    }
  }
}
```

### 10. Promise/A+

```js
const Pending = 'pending';
const Fulfilled = 'fulfilled';
const Rejected = 'rejected';

const resolvePromise = (p2, x, resolve, reject) => {
  if (p2 === x) {
    throw Error('promise 不可以循环');
  }
  // thenable
  if ((typeof x === 'object' || typeof x === 'function') && x !== null) {
    if (typeof x.then === 'function') {
      x.then((r) => {
        resolve(r);
      }, reject);
    } else {
      resolve(x);
    }
  } else {
    resolve(x);
  }
};

class P {
  constructor(executor) {
    this.state = Pending;
    this.val = undefined;
    this.resolveQueue = [];
    this.rejectQueue = [];

    const reject = (val) => {
      setTimeout(() => {
        this.state = Rejected;
        this.val = val;
        this.rejectQueue.map((fn) => fn());
      });
    };
    const resolve = (val) => {

      // 当 resolve 参数是 Promise 的时候
      if (typeof val === 'object' || typeof val === 'function') {
        resolvePromise(this, val, resolve, reject);
        return;
      }
      setTimeout(() => {
        this.state = Fulfilled;
        this.val = val;
        this.resolveQueue.map((fn) => fn());
      });
    };

    try {
      executor(resolve, reject);
    } catch (err) {
      reject(err);
    }
  }

  static all(promiseArr) {
    let num = 0;
    let result = [];

    return new P((resolve, reject) => {
      const resultFn = (data, i) => {
        num++;
        result[i];
        if (num === promiseArr.length) {
          resolve(result);
        }
      };

      promiseArr.forEach((p, i) => {
        if (p && typeof p.then === 'function') {
          p.then((data) => {
            resultFn(data, i);
          }, reject);
        } else {
          resultFn(p, i);
        }
      });
    });
  }

  static resolve(data) {
    return new P((resolve) => {resolve(data)})
  }

  static reject(err) {
    return new P((null, reject) => {reject(err)})
  }

  static race(promiseArr) {
    return new P((resolve, reject) => {
      for (let p of promiseArr) {
        P.resolve(p).then(val => {
          resolve(val)
        }, reject)
      }
    })
  }

  then(onFulfilled, onRejected) {
    // 值穿透
    typeof onFulfilled === 'function' ? onFulfilled : (val) => val;
    typeof onRejected === 'function'
      ? onRejected
      : (err) => {
          throw err;
        };

    const promise2 = new P((resolve, reject) => {
      if (this.state === Pending) {
        this.resolveQueue.push(() => {
          const x = onFulfilled(this.val);
          resolve(x);
        });
        this.rejectQueue.push(() => {
          const x = onRejected(this.val);
          reject(x);
        });
      }

      if (this.state === Fulfilled) {
        resolve(this.val);
      }

      if (this.state === Rejected) {
        reject(this.val);
      }
    });

    return promise2;
  }

  catch(rejectFn) {
    return this.then(null, rejectFn);
  }

  finally(callback) {
    return this.then(
      data => P.resolve(callback()).then(() => data),
      err => P.resolve(callback()).then(() => data)
    )
  }
}
```

### 11. 原型链继承

```js
function Car() {
  this.colors = ['red', 'green'];
}
Car.prototype.say = function () {
  console.log(this.name);
};
function MyCar() {}
MyCar.prototype = new Car();
```

缺点：

多个实例对引用类型的操作会被篡改。

### 12. 构造函数继承

```js
function Car() {
  this.colors = ['red', 'green'];
}
Car.prototype.say = function () {
  console.log(this.name);
};
function MyCar() {
  Car.call(this);
}
```

缺点：

1. 只能继承父类的实例属性和方法，不能继承原型属性/方法
1. 无法实现复用，每个子类都有父类实例函数的副本，影响性能

### 13. 组合继承

```js
function Car() {
  this.colors = ['red', 'green'];
}
Car.prototype.say = function () {
  console.log(this.name);
};
function MyCar() {
  Car.call(this);
}
MyCar.prototype = new Car();
```

缺点：

1. 会多次调用 new Car()，而且 MyCar 的实例对象上会存在两份相同的属性/方法

### 14. 寄生组合继承

```js
function myExtends(child, parent) {
  const p = Object.create(parent.prototype);
  p.constructor = child;
  child.prototype = p;
}

function Car() {
  this.colors = ['red', 'green'];
}
Car.prototype.say = function () {
  console.log(this.name);
};
function MyCar() {
  Car.call(this);
}

myExtends(MyCar, Car);
```
