### angular里forwardRef的原理

一段会报错的angular代码

```typescript
@Injectable()
class Socket {
  constructor(private buffer: Buffer) { }
}

console.log(Buffer); // undefined

@Injectable()
class Buffer {
  constructor(@Inject(BUFFER_SIZE) private size: Number) { }
}

console.log(Buffer); // [Function: Buffer]
```

翻译一下

```javascript
var providerList = new Map();
function Injectable(cls, deps) {
  providerList.set(cls, {
    deps: deps,
    instance: null
  });
}

function inject(cls) {
  var instanceObj = providerList.get(cls);
  if (instanceObj === undefined) {
    return null;
  } else if (instanceObj.instance === null) {
    var deps = instanceObj.deps.map(dep => inject(dep));
    var instance = cls.apply({}, deps);
    instanceObj.instance = instance;
    return instance;
  } else {
    return instanceObj.instance;
  }
}

var Socket = (function() {
  function Socket(buffer) {
    this.buffer = buffer;
    return this;
  }
  Injectable(Socket, [Buffer]);
  return Socket;
}());

var Buffer = (function () {
  function Buffer() {
    this.sayHello = function() {
      console.log('hello, i am Buffer.');
    }
    return this;
  }
  Injectable(Buffer, []);
  return Buffer;
}());

var socket = inject(Socket);
socket.buffer.sayHello();
```

Socket被注入的时候，他的依赖Buffer还是undefined状态，最后取出来的就是undefined.

看看angular怎么用forwardRef解决这个问题的

```typescript
@Injectable()
class Socket {
  constructor(@Inject(forwardRef(() => Buffer) private buffer) { }
}

@Injectable()
class Buffer {
  constructor() { }
}
```

翻译一下

```javascript
function forwardRef(forwardRefFn) {
  forwardRefFn.__forward_ref__ = forwardRef;
  return forwardRefFn;
}

function resolveForwardRef(type) {
  if (typeof type === 'function' && type.hasOwnProperty('__forward_ref__') && type.__forward_ref__ === forwardRef) {
    return type();
  } else {
    return type;
  }
}

var providerList = new Map();

function Injectable(cls, deps) {
  providerList.set(cls, {
    deps: deps,
    instance: null
  });
}

function inject(cls) {
  cls = resolveForwardRef(cls);
  var instanceObj = providerList.get(cls);
  if (instanceObj === undefined) {
    return null;
  } else if (instanceObj.instance === null) {
    var deps = instanceObj.deps.map(dep => inject(dep));
    var instance = cls.apply({}, deps);
    instanceObj.instance = instance;
    return instance;
  } else {
    return instanceObj.instance;
  }
}

var Socket = (function() {
  function Socket(buffer) {
    this.buffer = buffer;
    return this;
  }
  Injectable(Socket, [forwardRef(() => Buffer)]);
  return Socket;
}());

var Buffer = (function () {
  function Buffer() {
    this.sayHello = function() {
      console.log('hello, i am Buffer.');
    }
    return this;
  }
  Injectable(Buffer, []);
  return Buffer;
}());

var socket = inject(Socket);
socket.buffer.sayHello();
```

原理就是依赖不再是类了，而是一个返回类的函数，这样在inject的时候避免注入undefined。
