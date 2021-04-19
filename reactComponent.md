[不推荐用React.FC的原因](https://github.com/facebook/create-react-app/pull/8177)

- 不支持defaultProps
- 提供子项的隐式定义（不期望子项，但是用此组件时传入了子项，typescript不会报错）![image-20210419174714491](/Users/xiaoxin/Library/Application Support/typora-user-images/image-20210419174714491.png)
- 不能写范型

