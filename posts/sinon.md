# sinon.js

## spy
```
spy = sinon.spy()
spy = sinon.spy(object, 'method')
```

## stub
```
stub = sinon.sub()
stub = sinon.sub(object,"method")
```

### API

#### return

##### stub.returns(Promise.reject('a failure'))
指定返回

##### stub.returned(value) @return {boolean}
返回值与传入值是否相同

##### stub.throws('message')

##### stub.withArgs(...arg).returns(value);
指定参数执行时返回设定值

#### called
stub like a spy

##### stub.called @return {boolean} 
被调用过

##### stub.callCount @return {number}
执行次数

##### stub.calledWith(*) @return {boolean} 
判断被指定参数执行过

#### calls
##### stub.lastCall.args @return {array}
调用时使用的参数

##### sinon.stub.callsArgWith(index, cbArgs)
指定传入回调函数在执行时使用的参数

##### stub.callsArg(index)
当方法的参数为类数组时，执行参数集中的某个方法参数

##### stub.callsArgWith(index,[arg1,arg2...])
执行时，传入设定参数

##### stub.callsFake(func)
将包裹的方法替换成其它方法

##### stub.yieldTo({method},[arg1,arg2...])	@return {Array}
当方法的参数为对象时，需要执行配置对象内某个方法参数，并传入指定参数。忽略 stub 包裹的方法的实际上下文。

#### other
##### stub.reset()
声明 stub = sinon.stub() 后使用

##### stub.restore()
声明 stub = sinon.stub(object,method) 后使用，复原 object.method 原方法




