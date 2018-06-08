# sinon.js

## spy
侦查，不改变包裹的函数。
```
spy = sinon.spy()
spy = sinon.spy(fn)
spy = sinon.spy(object, 'method')
```

### API

##### spy.called @return \{boolean}
被调用过

##### spy.callCount @return \{number}
被调用次数

##### spy.calledWith(*) @return \{boolean} 
是否被指定参数执行过

##### spy.returned(*) @return \{boolean} 
是否返回指定值

##### spy.returnValues @return \{array} 
返回值

##### spy.args @return \{array} 
执行时的参数

## stub
存根，可以改变原函数的行为、输入、返回。
```
stub = sinon.sub()
stub = sinon.sub(object,"method")
```

### API

#### return

##### stub.returns(Promise.reject('a failure'))
指定返回

##### stub.returned(value) @return \{boolean}
返回值与传入值是否相同

##### stub.throws('message')

##### stub.withArgs(...arg).returns(value);
指定参数执行时返回设定值

#### called
stub like a spy

##### stub.called @return \{boolean} 
被调用过

##### stub.callCount @return \{number}
被调用次数

##### stub.calledWith(*) @return \{boolean} 
判断被指定参数执行过

##### stub.lastCall.args @return \{array}
调用时使用的参数

#### calls

##### sinon.stub.callsArgWith(index, cbArgs)
指定传入回调函数在执行时使用的参数

##### stub.callsArg(index)
当方法的参数为类数组时，执行参数集中的某个方法参数

##### stub.callsArgWith(index,[arg1,arg2...])
执行时，传入设定参数

##### stub.callsFake(func)
将包裹的方法替换成其它方法

##### stub.yieldsTo(method,[arg1,arg2...])
当方法的参数为对象时，需要执行配置对象内某个方法参数，并传入指定参数。忽略 stub 包裹的方法的实际上下文。
```
sinon.stub(object,method)
object.method.yieldsTo(options.property,[arg...])
object.method(options)
```

##### stub.yieldTo(callback,[arg1,arg2...])
类似前者。
```
var stub = sinon.stub()
var options = \{}
stub(options)
stub.yieldTo(options.property,[arg...])
```

#### other
##### stub.reset()
声明 stub = sinon.stub() 后使用

##### stub.restore()
声明 stub = sinon.stub(object,method) 后使用，复原 object.method 原方法

## assert
既然提供内置方法，那么需要使用对应的断言库进行校验。

#### sinon.assert.called(cfn)
被执行过

#### sinon.assert.calledWith(spy[,arg1,arg2...])
被指定参数执行过

#### sinon.assert.calledOnce(cfn)


