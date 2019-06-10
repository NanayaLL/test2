# 1.React开发环境搭建

<http://es6.ruanyifeng.com/#docs/class>  ES6语法

## 1.1 安装create-react-app脚手架工具

~~~java
npm install  //别人的项目启动之前需要npm install以下 安装依赖

npm install -g create-react-app  //安装create-react-app脚手架工具

//进入桌面 创建react-app项目 即react的项目
cd desktop
create-react-app todolist   //创建名叫todolist的react项目
yarn start  //启动项目
npm run start  //启动项目 
~~~

## 1.2 目录结构

~~~
yarn.lock 项目依赖的版本号
README 项目的说明 可以全部删除
package.json 脚手架和项目的一些介绍  是node的包文件 可以让项目变成一个包
gitignore git上传时不想上传的文件可以留在这里
node_modules 外部的一些包文件 第三方模块 不要动
public  favicon.ico 网站的图标 可以替换成自己的
src目录 所有项目源代码  所有代码入口在index.js文件
manifest.json 如果用了PWA 那么将网址显示在手机桌面上时 显示什么图标 就定义在这个文件中
~~~

~~~
serviceWorker 这个js可以让用户访问完这个网页之后 即使断网了 也可以打开 因为serviceWorker帮助你把之前的网页存储在浏览器之内

PWA 用web来写手机端的app
~~~

# 2.React中的组件

## 2.1 组件定义

~~~java
//能直接引入react是因为cli脚手架工具把react设置好了
import React,{  Component } from 'react'
//等价于
improt React from 'react'
const Component = React.Compent
~~~

~~~javascript
//定义一个类继承Component 那这个类就是react的组件了
//类中的render方法返回的东西就是导入这个组件后会显示的东西
class App extends Component {
	render(){
		return (
			<div>
				hello world
			</div>
		);
	}
}
~~~

~~~javascript
//ReactDOM 是一个第三方的组件
//它里面有个render方法 可以把一个组件挂载到一个dom姐弟拿上
import React from 'react';
import ReactDOM from 'react-dom';
import App from './App';

ReactDOM.render(<App />, document.getElementById('root'));
//引入react是因为里面有个JSX语法 当你把APP当作标签<APP/>渲染的时候 实际上就是使用了这个语法 所以必须import React from 'react';
//并且render中return的标签 也是JSX的语法 因此组件也必须引入react
~~~

# 3. JSX语法

## 3.1 JSX最最最基础的语法

~~~javascript
//JSX语法中 如果我们要使用自己创建的组件 直接通过标签形式来使用我们定义的组件名就可以了
//JSX语法中 要使用自己组件 组件名开头必须是大写
例如
import app from './App';
ReactDOM.render(<app />, document.getElementById('root'));
//这种写法是错误的 因为自定义的组件名开头必须大写
~~~

## 3.2使用react实现一个todolist功能

~~~javascript
//一个组件返回的内容 外部必须有一个包裹元素 即最外层不能有两个同级元素 因此把所有包裹在一个div中即可
//但是有时候不想让最外层包裹的元素显示出来 那么就可以使用Fragment的占位符 然后用Fragment标签替代最外层的div标签即可

//导入Fragment
import React, { Component ,Fragment } from 'react';

class TodoList extends Component {
    render () {
        return (
            //使用Fragment标签
            <Fragment>
                <div><input /><button>提交</button></div>
                <ul>
                    <li>学英语</li>
                    <li>Learning React</li>
                </ul>
            </Fragment>
        );
    }
}
//将组件的类导出 让别的js能够使用自定义组件
export default TodoList;
~~~

## 3.3 React中的响应式设计思想和事件绑定

~~~javascript
//React思想是不操作DOM而是操作数据 React会自动响应你的数据变化 之后就再也不用关注DOM的操作 关注数据的操作即可
~~~

### 3.3.1 如何定义一个数据

~~~javascript
//class是一个组件 或者说是一个类 一个类中必然会有一个constructor(){}的构造函数当我们创建一个组件的实例 或者说当我们使用自定义的组件的时候  这个构造函数是比其他任何函数都优先执行函数
//构造方法里有个props的参数 先这么写 后面会有解释  并且调用父类的构造函数 把props参数传递进去
//之后就可以再里面定义数据 react定义数据需要把数据定义在状态里面(state)
//定义完数据 需要和标签进行绑定  下面的绑定必须加大括号 因为再JSX语法中使用js的变量 需要包上大括号
class TodoList extends Component {
    constructor (props) {
        super(props);
        this.state = {
            inputValue: 'Hello!!!',
            list: []
        }
    }

    render () {
        return (
            <Fragment>
                <div><input value={this.state.inputValue }/><button>提交</button></div>
                <ul>
                    <li>学英语</li>
                    <li>Learning React</li>
                </ul>
            </Fragment>
        );
    }
}
//这时候无论你怎么再input中输入 都不会发生改变 因为它的值是由inputValue来决定的
~~~

### 3.3.2 事件绑定

~~~javascript
//由于上面的问题 因此就需要给input进行事件绑定
//react的事件绑定on后面的必须是大写比如onChange
    render () {
        return (
            <Fragment>                
                <div>
                    {/* 由于this的穿透问题 因此必须用bind方法给handle方法的this重新进行指向 bind的this指向的就是TodoList的实例化对象 */}
                    <input value={this.state.inputValue } onChange={this.handleInputChange.bind(this)}/>
                    <button>提交</button>
                </div>
                <ul>
                    <li>学英语</li>
                    <li>Learning React</li>
                </ul>
            </Fragment>
        );
    }

    handleInputChange(e) {
        //this的指向有问题 因此下面的方法会报错 state未定义
        console.log(this);
        console.log(e.target.value);
        //如果上面使用了bind（this）这里的this就能正常使用
        //这样也是不可以使用的 想要改变react中数据的值 react提供了一个setState的方法
        // this.state.inputValue = e.target.value;
        this.setState({
            inputValue: e.target.value
        })
    }
~~~

总结

state负责存储组件里面的数据

JSX里面想用js里面的表达式 需要用花括号{}包裹

函数绑定的时候需要用bind(this)对函数的作用域进行变更

如果想改变数据的内容 不能直接改变 需要通过setState方法 向里面传入对象来进行变更

### 3.33 新增删除功能

~~~javascript
//es5里面有一个map方法 map方法可以对数据进行遍历 之后返回一个新的数组
//展开运算符list: [...this.state.list,xxx] 可以把list数据展开 通过方括号包裹形成一个新的数组
//react中循环渲染一个元素的时候 需要给每一个元素添加一个key值 并且key值的内容要不同 这里使用了循环的下标 但是真正使用的时候切忌使用这个作为key值
class TodoList extends Component {
    constructor (props) {
        super(props);
        this.state = {
            inputValue: '',
            list: []
        }
    }

    render () {
        return (
            <Fragment>
                <div>
                    <input value={this.state.inputValue} onChange={this.handleInputChange.bind(this)} />
                    <button onClick={this.handleClick.bind(this)}>提交</button>
                </div>
                <ul>
                        //这里遍历list 返还一个新的数组 每个li有一个唯一的key值 真正项目中不要使用index
                    {
                        this.state.list.map((item,index) => {
                            return (<li key={index}>{item}</li>)
                        })
                    }
                </ul>
            </Fragment>
        );
    }

    handleInputChange(e) {
        this.setState({
            inputValue: e.target.value
        })
    }

    handleClick() {
        this.setState({
            //这里使用展开运算符
            list: [...this.state.list,this.state.inputValue],
            inputValue: ''
        })
    }
}
~~~

~~~javascript
//可以将参数放在bind的第二个参数位置中 方法里就可以加上一个形参来接收这个参数
//删除数组 
const list = [...this.state.list]
list.splice(index,1)	//删除下表的元素 往后删除1个
this.setState({
	list: list
})
//不可以
this.setState({
	list: this.state.list.splice(index,1)
})
//因为react里面有个immutable的概念 state不允许我们做任何的改变
~~~

~~~javascript
//删除
    render () {
        return (
            <Fragment>
                <div>
                    <input value={this.state.inputValue} onChange={this.handleInputChange.bind(this)} />
                    <button onClick={this.handleClick.bind(this)}>提交</button>
                </div>
                <ul>
                    {
                        this.state.list.map((item,index) => {
                            return (<li key={index} onClick={this.handleDelete.bind(this,index)}>{item}</li>)
                        })
                    }
                </ul>
            </Fragment>
        );
    }

    handleInputChange(e) {
        this.setState({
            inputValue: e.target.value
        })
    }

    handleClick() {
        this.setState({
            list: [...this.state.list,this.state.inputValue],
            inputValue: ''
        })
    }

    handleDelete(index) {
        const list = [...this.state.list];
        list.splice(index,1);

        this.setState({
            list: list
        })
    }
}
~~~

### 3.3.4JSX细节补充

~~~javascript
//注释
{/*xxxx*/}
{
	//xxxx
}
//引入css
import './todolist.css';
//不建议使用class="xxx" 因为与类class同名
//建议使用className来代替class

//如果想让input中输入的参数不转义输出再html中
//使用dangerouslySetInnerHTML={{__html: xxx}}
<ul>
    {
        this.state.list.map((item,index) => {
            return (<li 
                        key={index} 
                        onClick={this.handleDelete.bind(this,index)}
                        dangerouslySetInnerHTML={{__html: item}}
                    >
                    </li>)
        })
    }
</ul>
//label标签用于给input加大点击范围
//但是label标签上加上for属性 会出现警告 因为和for循环同名 因此要换成htmlFor="id"
 <label htmlFor="inputArea">输入内容</label>
                    <input id="inputArea" value={this.state.inputValue} onChange={this.handleInputChange.bind(this)} className='input' />
~~~

### 3.3.5组件传值

~~~javascript
//父组件传值给子组件 通过属性值
//父组件import子组件 并且使用子组件
<child content='aaa'></child>
//子组件中接收父组件的值 通过props属性 this.props.属性名
<input value={this.props.content }>
~~~

~~~javascript
//子组件
render() {
        return (
            <li>{this.props.content}</li>
        );
    }
    
//父组件
//map返回的东西都必须包含在一个标签内
<TodoItem content={item}></TodoItem>
~~~

~~~javascript
//给方法this绑定也可以通过再构造函数进行绑定

//子组件要调用父组件传递过来的方法 再父组件里面传递时 必须加上bind(this)绑定this到父组件的类上 因为子组件调用的时候会使用this.props.handleClick({this.props.index})这句话相当于this.handleClick()而this这里指的就是子组件 子组件没有这个handleCLick方法 所以只要再父组件中传递的时候bind(this) 那么这时候this指的就是父组件
//子组件
class TodoItem extends Component {
    constructor(props){
        super(props);
        //给方法this绑定也可以通过再构造函数进行绑定
        this.handleDelete=this.handleDelete.bind(this);
    }

    render() {
        return (
            <li onClick={this.handleDelete}>{this.props.content}</li>
        );
    }

    handleDelete() {
        this.props.deleteItem(this.props.index);
    }

}


//父组件
<TodoItem content={item} deleteItem={this.handleDelete.bind(this)} index={index}>
~~~

~~~javascript
const { content } = this.props
//等价于
this.props.content
//用第一个方法写 后面直接用content变量名即可
//方法的bind建议全部放在构造函数里进行初始化
~~~

### 3.3.6 代码性能优化

~~~javascript
//可以把map循环返回的提取成一个方法 然后再里面直接调用方法{this.getTodoItem()}由于不是方法绑定 所以不需要bind(this)
//this.setState({})这种写法已经不推荐 建议使用this.setState(()=>{return { xxx:xxx }}）这种返回对象的方 法 但这个是一个异步的setState这样写需要把e.target.value在外面用const value = e.target.value进行一个保存 然后在里面附上这个value
//
	this.setState((prevState)=>({
	list: [...prevState.list]
}))
//setState可以传递一个prevState的值 这个值保存了上一个state的状态 就相当于this.state 这样写的好处在于可以防止篡改state的值
//ES6里面{list:list}对象的名字和变量名相同 可以简写成{list}
//循环的key值	
~~~

### 3.3.7 React的思考

~~~javascript
//React是一个单向数据流
//即React父组件传递一个值给子组件 子组件只能使用这个值 //而不能操作更改这个值 因为传递的值是只读的 如果尝试去操作 那么就会报错

//React相当于一个视图层的框架 主要用于页面数据的渲染
//传值问题需要依靠别的框架
~~~

# 4 React高级

## 4.1安装React开发调试工具

## 4.2 PropTypes和DefaultProps

~~~javascript
//用于进行子组件接收父组件传值时的强校验 要使用必须引用PropTypes
import PropTypes from 'prop-types';
//对属性进行强校验 写在最外层
Todoitem(组件名).propTypes = {
	content: PropTypes.string, //content这个属性必须是string类型
	deleteItem: PropTypes.func,	//deleteItem必须时方法
	index: PropTypes.number 	//index必须是数字
}

//如果子组件需要父组件传一个test的属性 但是父组件没传
//代码不会报错 但可以通过配置 要求它必须传
 TodoItem.propTypes = {
    content: PropTypes.string,
    deleteIten: PropTypes.func,
    index: PropTypes.number,
    //isRequired
    test: PropTypes.string.isRequired
}

//如果我们必须要父组件传test 但父组件确实没法传 那么可以定义一个默认值
TodoItem.defaultProps = {
    test: 'hello world'
}

//要么是string 要么是number arrayOf就表示一个或者的语法 
TodoItem.propTypes = {
    //这么写
    content: PropTypes.arrayOf(PropTypes.number,PropTypes.string),//这表示他是一个数组 数组中的内容是一个number或一个string 可以用oneOfType([PropTypes.number,PropTypes.string])来表示一个number或一个string
    deleteIten: PropTypes.func,
    index: PropTypes.number,
    test: PropTypes.string.isRequired
}
~~~

## 4.3 props state 和 render函数的关系

~~~javascript
//只要state或者props发生改变 render函数就会重新执行
//可以通过再render函数里面写上向控制台打印 一句话证
render() {
        console.log('render')
        return (
            <Fragment>
                <div>
                    <label htmlFor="inputArea">输入内容</label>
                    <input id="inputArea" value={this.state.inputValue} onChange={this.handleInputChange} className='input' />
                    <button onClick={this.handleClick}>提交</button>
                </div>
                <ul>
                    {this.getTodoItem()}
                </ul>
            </Fragment>
        );
    }
    

//当父组件的render函数重新执行的时候 他的子组件的render函数也会被重新执行
~~~

## 4.4 什么是虚拟DOM

~~~java
//react中的重新渲染速度是很快的 因为引入了虚拟DOM的概念

//模拟React框架
//1.state数据
//2.JSX模板
//3.数据 + 模板 结合，生成真实的DOM，来显示
//4.state 发生改变
//5.数据 + 模板 结合，生成真实的DOM，替换原始的DOM

缺陷：
第一次生成了一个完整的DOM片段
第二次生成了一个完整的DOM片段
第二次的DOM替换第一次的DOM，非常耗性能

//1.state数据
//2.JSX 模板
//3.数据 + 模板 结合，生成真实的DOM，来显示
//4.state 发生改变
//5.数据 + 模板 结合，生成真实的DOM，并不直接替换原始的DOM
//6.新的DOM（DoucumentFragment）和原始的DOM 做对比，找差异
//7.找出input框发生了变化
//8.只用新的DOM中的input元素，替换掉老的DOM中的input元素

缺陷：
性能提升并不明显
新DOM和原始DOM对比也会损耗性能

//react的原理
//1.state 数据
//2.JSX模板
//4..数据+模板 结合，生成虚拟DOM（虚拟DOM就是一个JS对象，用它来描述真实DOM）（损耗了性能 但是js生成一个js对象性能消耗极低）
['div',{id:'abc'},['span',{},'hello world']]
//3用虚拟DOM的结构生成真实的DOM，来显示
<div id='abc'><span>hello world</span></div> 

//5.state 发生变化
//6.生成新的虚拟DOM
['div',{id:'abc'},['span',{},'bye bye']]
//7.比较原始虚拟DOM和新的虚拟DOM的区别，找到区别是span中内容（极大的提升性能）
//8.直接操作DOM，改变span中的内容

优点：
//1.性能提升了
//2.它使得跨端应用得以实现。React Native
~~~

## 4.5深入理解虚拟DOM

 ~~~
JSX -> React.createElement()->虚拟DOM（JS 对象）->真实DOM 
 ~~~

## 4.6虚拟DOM中的Diff算法

~~~javascript
diff=difference
//同级比对和key值比对都是diff算法中的一部分
//React虚拟DOM是同层比对 如果第一层不同 那么下面就不比对了 直接删除下面的节点重新生成虚拟DOM
//React的setState是异步的 这样如果很短时间内连续调用三次setState 那么React会把这三次合并成一次 来进行虚拟DOM的diff对比 这样就大大提升了性能
//key值问题 由于key值可以再虚拟DOM发生变化时用diff算法进行比对 key值和dom是一一对应的 但如果用循环的index当作key值 那么当减少一个dom时 key值就会发生改变 新的虚拟DOMkey值和原先的虚拟DOMkey值就会不同 这样diff算法就失效了
~~~

![1559789182981](C:\Users\44321\AppData\Roaming\Typora\typora-user-images\1559789182981.png)

 ## 4.7 ref

~~~javascript
//ref也可以用来获取JSX节点中的元素 和e.target.value的作用一致
//首先再节点上定义ref
<input id="inputArea" value={this.state.inputValue} onChange={this.handleInputChange} className='input'
ref={(input) => {this.input = input}}
/>
//之后再handleInputChange方法中使用input
    handleInputChange(e) {
        // const value = e.target.value;
        const value = this.input.value;
        this.setState(() => ({
            inputValue: value
        }));
    }
~~~

~~~javascript
//setState()是一个异步函数，因此当你写的逻辑放在setState下面的时候 也会被先执行
    handleClick() {
        this.setState((prevState) => ({
            list: [...prevState.list,this.state.inputValue],
            inputValue: ''
        }))
        console.log(this.ul.querySelectorAll('div').length);
    }
  //这情况下每次添加一个li 打印的都是前一个的长度，因此setState提供了第二个参数 这个参数里面是一个箭头函数 箭头函数里面写的逻辑会在执行完setState之后执行
      handleClick() {
        this.setState((prevState) => ({
            list: [...prevState.list,this.state.inputValue],
            inputValue: ''
        }),
        () => {
            console.log(this.ul.querySelectorAll('div').length);
        }
        )  
    }
~~~

## 4.8 React中的生命周期函数

~~~javascript
//生命周期函数是指在某一时刻组件会自动调用执行的函数  生命周期函数是针对组件来说的 每一个组件都有生命周期函数
//React的生命周期 
~~~

![1560131012361](C:\Users\44321\AppData\Roaming\Typora\typora-user-images\1560131012361.png)

~~~javascript
/*1.Initialization 初始化
	会调用constructor函数 对state和事件绑定进行初始化
  2.Mounting 挂载  （组件第一次被渲染到页面的时候叫挂载 因此除了render 别的只会执行一次）
  	componentWillMount()也是一个生命周期函数 会被组件自动调用 在组件即将被挂载到页面的时候自动执行 即页面渲染之前执行
  	render() 将组件渲染到页面上 在state和props改变时会自动执行
  	componentDidMount() 组件被挂载到页面之后自动执行
  3.Updation 组件更新 (props比state多一个流程)
    3.1 props 发生变化 
    	componentWillReceiveProps => shouldComponentUpdate => componentWillUpdate => render => componentDidUpdate
    
    3.2 state 发生变化
    	shouldComponentUpdate => componentWillUpdate => render => componentDidUpdate
    
    shouldComponentUpdate（） 会在props或state（组件）发生变化的时候 会自动执行 他需要返回一个布尔值 返还true 那么组件更新 返还false 那么state或props(组件）不会更新  如果这个返回false 那么后面的生命周期函数都不会执行了（componentWillUpdate => render => componentDidUpdate不会执行）
    componentWillUpdate（） 组件被更新之前会自动执行 但会在shouldComponentUpdate执行之后执行（shouldComponentUpdate必须返回true才会执行）
    render（） props或state改变 所以render会重新执行 渲染页面
    componentDidUpdate（）组件更新完成之后会被执行
    
    componentWillReceiveProps（） 一个组件要从父组件接收参数 只要父组件的render函数被重新执行了 子组件的这个生命周期函数就会被执行（如果这个子组件第一次存在于父组件中 不会执行；如果这个组件之前已经存在于父组件中 才会执行）
  
  4.Unmounting 
  	componentWillUnmount() 当这个组件即将被从页面中剔除的时候，会被执行
    
    
/*
~~~

## 4.9 React生命周期函数的使用场景

~~~javascript
//编写react组件时继承的Component这个类 里面默认实现了所有的生命周期函数 但唯独没有render（） 因此别的生命周期函数可以没有 但是render必须存在
//由于只要父组件的render函数重新执行 子组件的render也一定会执行 这时候就需要在子组件中使用shouldComponentUpdate(); 让这个返回false就不会在执行了 但是这种写法不是最优的写法 可以通过下面代码优化
shouldComponent(nextProps,nextState){
	if(nextProps.content!==this.props.content){
		return true;
	}else{
		return false;
	}
}
~~~



## 4.10 react发送ajax

~~~javascript
//react发送ajax
//ajax不可以写在render函数中 因为如果需要对返还的ajax数据来进行显示 那么每次修改state值都会让render函数再次执行 ajax也会重新发送 这就会形成一个死循环

//因此建议放在componentDidMount 这个生命周期函数只会执行一次 在组件被挂载到页面上之后执行
//如果放在componentWillMount中 一般也没问题 但是如果我们写react Native或者用react做服务器端的同构或者更深的一些技术的时候 可能会有一些冲突 因此ajax请求就放在componetDidMount中
//把ajax放在contructor中也可以 但是把ajax放在componentDidMount中 一定不会有问题
~~~

~~~
//react框架不像jquery封装了ajax 因此需要自己安装一个ajax框架
//1.打开命令行cmd 进入项目的目录
//2.运行 yarn add axios
~~~

~~~
//1. 需要在最上面导入axios
import axios from 'axios';
//2. 在componentDidMount调用axios
//3. 调用 如果成功了就会执行then里面的箭头函数 失败执行catch里面的箭头函数
axios.get('/api/todolist').then(()=>{
	alert('succ')
}).catch(()=>{
	alert('error')
})
~~~



