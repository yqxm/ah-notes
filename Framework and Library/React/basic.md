# 基础

## JSX

React认为UI逻辑和渲染逻辑应该是一起的。JSX可以达成这个目标，将标记和JS放在一起组成component，实现关注点分离。

### JSX中插值

在大括号中可以放入任意Javascript表达式。

可以使用引号或包裹在大括号中的JS表达式来指定属性（只选择一种使用）

```javascript
const name = "huhu"

const e1 = <h1>Hello, {name}</h1>;
const e2 = <img tabindex="0" />
const e3 = <img src={user.avatarUrl}>
```

## 元素渲染

在根DOM节点内的内容全部由ReactDOM管理。把元素传给ReactDOM，即`ReactDOM.render()`函数，这样就可以将元素渲染到根节点中。

React元素是不可更改的，它代表某一时刻元素的状态，要更新只有重新创建元素，然后再传入到ReactDOM中。

React会将元素和它之前的状态进行比较，然后只更新需要更新的部分。

## 组件与props

```javascript
// 函数式组件
function Welcome(props) {
    return <h1>Hello, {props.name}</h1>
}

// class组件
class Welcome extends React.Componet {
    render() {
        return <h1>Hello, {this.props.name}</h1>
    }
}

```

组件也可以作为元素被渲染。

组件在其输出中可以引用其他组件。

组件设计应该只关注它自身，不关心使用它的上下文。

props是只读的，不能被更改。

## state和生命周期

state就是状态

class类型的组件就像对象。对象有状态，有生命周期。

`componentDidMount()`方法执行组件第一次被渲染的时候会做的事。`componentWillUnmount()`方法执行不需要这个组件的时候会做的事。

### state的正确使用

- 不直接修改state
- state的更新是异步的
- state的更新会被合并

props和state的更新是异步的，所以不要依赖它们的值来更新下一个状态。`setState((state, props) => {})`来避免直接修改state无法更新的问题。第一个参数为上一个状态，第二个参数为此次更新要用的props

### 数据是向下流动的

组件只能向它之下的组件传递数据。

“如果你把一个以组件构成的树想象成一个 props 的数据瀑布的话，那么每一个组件的 state 就像是在任意一点上给瀑布增加额外的水源，但是它只能向下流动。”