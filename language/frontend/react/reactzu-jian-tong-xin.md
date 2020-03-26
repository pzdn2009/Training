# React-组件通信

本质：均是通过props进行传递，格式：`属性=表达式`


# 1. 父子传递

 * `p={}`进行传递
 * `this.props.p`获取

# 2. 子父传递

 * `p={callback}`
 * 子父：onChange-->this.props.callBack(state)
 * 函数是一等公民，当做一个对象传递个子组件