```javascript
// instance/index.js

// instance/init.js
initMixin(Vue)
  uid=0
  Vue.prototype._init = function(){
    vm._uid = uid ++
    mergeOptions()
    initLifecycle(vm){
      // 对实例全局变量附初始 $parent $root $children $refs
    }
    initEvents(vm){
      // 绑定 $on $off
    }
    callHook(vm, 'beforeCreate'){
      // 触发生命周期钩子
    }

    initState(vm){
      initProps(vm)
      initData(vm){
        // 数据响应式，定义依赖收集
        // dep 收集 watcher 到 sub， dep.notify 内遍历 sub 内的 watcher，并执行 watcher.update(){ this.cb } => cb(){ vm._update(vm.render()) }

        observe(data){
          return new Observer(data){
            this.dep = new Dep()

            Object.keys(data).forEach(){
              defineReactive(obj,key,val){
                const dep = new Dep()
                Object.defineProperty(obj,key,{
                  get(){
                    if(Dep.target){
                      dep.depend()
                    }
                  },
                  set(){
                    dep.notify()
                  }
                })
              }
            }
          }
        }(options.data)
      }
      initComputed(vm)
      initMethods(vm)
      initWatch(vm)
    }

    callHook(vm, 'created'){
      // 触发生命周期钩子
    }

    initRender(vm){
      // 附初始值：$vnode _vnode $elots
      vm.$mount(vm.$options.el){
        // 挂载 DOM
        this._mount()
      }
    }
  }


stateMixin(Vue){
  // 附初值：$set $delete $watch
}

eventsMixin(Vue){
  // 定义 prototype... $on $off $once $emit
}

lifecycleMixin(Vue){
  // define Vue.prototype ... $forceUpdate _mount _update $destroy

  Vue.prototype.$forceUpdate(){
    vm._watcher.update()
  }

  Vue.prototype._mount(){
    vm._watcher = new Watcher(vm, () => {
      vm._update(vm._render(), hydrating)
    }, noop){
      // Watcher 的构造函数内会 watcher 实例赋值给 Dep.target

      dp.pushTarget() // Dep.target = this

      cb() // 执行 vm._update()

      dp.popTarget() // Dep.target = null
    }

    callHook(vm, 'mounted')
  }

  Vue.prototype._update(VNode){
    callHook(vm, 'beforeUpdate')

    vm.$el = __patch__(prevVNode, newVNode)

    vm.$parent.$el = vm.$el

    callHook(vm, 'updated')
  }
}

renderMixin(Vue){
  // define Vue.prototype ... $nextTick _render
  _render(): VNode
}
```

```javascript
// webpack-runtime.js
Vue.prototype.__patch__ = config._isServer ? noop : patch

// web/runtime/patch.js
export const patch = createPatchFunction(nodeOps, modules){
  // nodeOps [Object] 内提供操作 node 的工具方法，包括 removeChild appendChild createTextNode
  // modules [Array] eg. [attr,event,props] attr = {update(oldVnode, newVnode){ update...newVnode.elm },create}

  return function patch (oldVnode, newVnode, hydrating, removeOnly) {

    if(!oldVnode){
      createElement(newVnode){
        // 根据 vnode 构建 node
        return vnode.elm
      }
    }else{
      if(isSameVnode){
        patchVnode(oldVnode, vnode, insertedVnodeQueue, removeOnly){
          // 根据比对新旧 vnode 的差异，更新 node
        }
      }

    }

    return return vnode.elm // 真实 DOM
  }
}


```

```javascript
// web-compiler.js
export function compile() {}
```
