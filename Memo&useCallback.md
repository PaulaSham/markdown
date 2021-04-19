>  对于 React FC, 我们尽可能遵循一个原则: **能不重新渲染的, 就不要重新渲染**

## memo

memo 是memory的意思，在React里，Memo表示记住

##### [before you memo](https://overreacted.io/zh-hans/before-you-memo/) 

>  需要思考的问题是:从不变的部分分割出变的部分再考虑用memo

原则：让 **变化** 不导致高消耗组件重新渲染

- 技巧一：把变化的部分封装成子组件，变化的组件与高消耗组件同级
- 技巧二：把变化的部分封装成父组件，高消耗组件作为子组件，当变化产生时会记住children属性

##### [memo 的使用场景](https://dmitripavlutin.com/use-react-memo-wisely/)

前置知识：update dom之前React 会对比前后两次渲染结果，不一致才会update dom

memo作用：包着React.memo的组件，React会记住渲染结果，在下一次渲染开始前，如果props没有变，React 会用之前记住的渲染结果，跳过渲染组件，不会进行虚拟dom差异检测。

> by reusing the memoized content, React skips rendering the component and doesn’t perform a virtual DOM difference check

适用场景：经常带着相同的props渲染的函数组件

![](https://dmitripavlutin.com/static/c07d2ce4ede6301197b9605a75ae9b4e/5fd6b/when-to-use-react-memo-infographic.jpg)



避免使用memo的场景：

- 每次带着不同的props渲染，即组件的props经常变化（React在每次渲染都多做对比前后差异的工作）
- 如果不确定用memo是否会带来性能上的提升，不要用
- 不要用React.Memo包裹class组件，可以考虑pureComponent

####Memo和useCallback 

callback作为memo组件 **参数** 时，该callback在传入的时候需要用useCallback包裹。因为callback在每次渲染中都不相同，导致memo失效，因此需要保持一个function实例。



### useCallback

> 观点来自：https://dmitripavlutin.com/dont-overuse-react-usecallback/

#####使用useCallback的原因

callback函数在每次渲染中都重新创建，用useCallback 可以保持一个 **function insatnce**

> maintain one function instance between renderings 

#####在渲染中需要记住**function实例**的情况

- 1. 被包在 **React.memo()**并接受一个 **函数** 作为属性的函数组件，例如：

  ```js
  import React from 'react';
  import useSearch from './fetch-items';
  
  function MyBigList({ term, onItemClick }) {
    const items = useSearch(term);
  
    const map = item => <div onClick={onItemClick}>{item}</div>;
  
    return <div>{items.map(map)}</div>;
  }
  
  export default React.memo(MyBigList);
  
  ```

  在传入时，onItemClick需要用useCallback包裹

- 2. 方法是某个**hooks**(e.g.useEffect)的依赖

  > the hook returns (aka memoizes) the function instance between renderings

