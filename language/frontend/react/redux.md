# Redux

基本原则：
* 唯一数据源
 * 一个store
* 保持只读状态
 * 不能修改store的状态
 * 出发action
* 数据改变只能通过纯函数来完成
 * 纯函数：reducer(state,action)
 * state表示当前的状态
 * action表示接收到的action
 * 返回一个新的对象

四大组件：
* state
* store
* reducer
* action

