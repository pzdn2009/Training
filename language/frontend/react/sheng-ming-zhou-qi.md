# 生命周期

# 1. Mount挂载

* constructor
* getInitialState
* getDefaultProps
* componentWillMount
* render
* componentDidMount

## constructor

* 初始化state
* 绑定成员函数this环境

## render

* 并不做实际的渲染动作
* 只是返回一个JSX
* 最终有React来决定渲染
* 如果返回null，false，做不做任何渲染
* 必须是一个纯函数，不能修改state的值

## componentWillMount

* 在render之前调用

## componentDidMount

* 在render之后调用
* 可以调用第三方库来做扩展

# 2. Update更新

* componentWillReceiveProps
* shouldComponentUpdate(nextProps,nextState)
 * 决定要不要渲染
 * 之后调用componentWillUpdate
* componentWillUpdate
* componentDidUpdate

# 3. Unmount卸载

* componentWillUnmount