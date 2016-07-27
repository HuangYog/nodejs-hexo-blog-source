---
title: React-学习笔记
date: 2016-07-04 16:03:40
tags:
 - 我的Blog
 - React
 - React-Native
categories:
- 日志
- 笔记
---
本教程参照阮一峰老师的{% link React教程 http://www.ruanyifeng.com/blog/2015/03/react.html true 阮一峰博客 %}学习所写的笔记
## React HTML 源码
```
<!DOCTYPE html>
<html lang='en'>
  <head>
    <script src="../build/react.js"></script>
    <script src="../build/react-dom.js"></script>
    <script src="../build/browser.min.js"></script>
  </head>
  <body>
    <div id="example"></div>
    <script type="text/babel">
      // 这是注释
    </script>
  </body>
</html>
```
使用React的时候需要加载三个基本的js，其加载顺序为`react.js、react-dom.js、Browser.min.js`.
- `react.js` 是React 的核心库;
- `react-dom.js` 是React 提供与DOM相关的功能;
- `Browser.min.js` 是将JSX语法转为javascript语法.
在上面代码中的script标签中有一个 `type` 属性为 `text/babel`,因为React规定使用的是自己的JSX语法，和javascript是不兼容的，在代码中使用到JSX语法的地方都要加上`type="text/balel"`.
```
$ jsx src --out-dir build
```
这是将src子目录的js文件进行语法转换，并将转换好的文件放到build子目录.

## ReactDOM.render()
```
ReactDOM.render(
<h1>Hello, World!</h1>,
document.getElementById('example')
);
```
`ReactDOM.render()`是`react`最基本的方法,主要用来将模板转为`HTML`,并插入到指定的`DOM`节点.再上面代码中将一个`h1`标题插入`example`节点。

## JSX 语法
`JSX语法`允许`HTML语言`直接写在`javascript语言`中，不需要加任何引号,他允许`HTML与javascript混写`.
```
var names = ['Al', 'Bl', 'Cl', 'El'];
ReactDOM.render(<div>
    	{
        	names.map(function(name){
            	return <div>Hello {name} !</div>
            })
        }
    </div>,
    document.getElementById('example')
);
```
上面代码的基本规则为: 遇到`HTML`标签(以` < `打头 的)就用HTML规则解析，遇到代码块(以 ` { ` 打头的)就用javascript规则解析。
JSX 允许直接在模板中查人javascript变量，如果变量是一个数组，会展开这个数组的所有成员,如下：
```
var arr = [
<h1>Hello World!</h1>,
<h2>React is awesome</h2>,
];
ReactDOM.render(
<div>{arr}</div>,
document.getElementById('example')
);
```
## 组件
`React`允许将代买分装成`组件(component)`然后像插入普通的HTML标签一样在网页中插入这个组件，`React.createClass()`方法就用于生成一个组件类.
```
var HelloMessage = React.createClass({
	render: function(){
    	return <h1>Hello {this.props.name}</h1>
    }
});
ReactDOM.render(
<HelloMessage name='Johe' />,
document.getElementById('example')
);
```
上面代码中变量`HelloMessage`就是一个组件，模板插入`<HelloMessage />`时会自动生成 `HelloMessage`的一个实例(在下稳重出现的‘组件’都是指组件类的实例),所有的组件都要有自己的`render(渲染)`方法，用于输出组件。在创建组件类的时候要注意`第一个字母必须大写`,否则会报错，如上面的HelloMessage 不能写成helloMessage，还有就是组件只能包含一个顶层标签，不然也会报错，如下代码:
```
var HelloMessage = React.createClass(
{
	render: function(){
    // 这里会报错，因为HelloMessage 组件包含了两个顶层标签 h1和p
    	return <h1>hello {this.props.name}</h1><p>World</p> ;
    }
}
);
```
组件的用法与原生的HTML标签一致，可以任意加入属性`<HelloMessage name="Johe">`,就是`HelloMessage`组件加入一个`name`属性，值为 `Johe`,组件的属性可以在组件类的`this.props`对象上获取，如`name`属性可以通过`this.props.name` 获取。在添加组件属性的时候需要注意`class`属性需要写成`className`、`for`属性需要写成`htmlFor`，因为`class`、`for`是javascript`的保留字。
## this.props.children
`this.props`对象的属性与组件的属性一一对应,但有个例外是`this.props.children`,它表示组件的所有子节点，如下例子:
```
var NotesList= React.createClass(
{
	render: function(){
    return(
    	<ol>{
        React.Children.map(this.props.children, function(child){
        return <li>{child}</li>
        })
        }
        </ol>
    );
    }
});
ReactDOM.render(
<NotesList>
	<span>hello</span>
    <span>world</span>
</NotesList>,
document.body
);
```
`NotesList`组件有两个`span`子节点，可以通过this.props.children获取到。
`注意` `this.props.children`的值有三种可能:
- 如果当前组件没有子节点，那它就是`undefined`；
- 如果有一个子节点，那获取的数据类型为`object`;
- 如果有多个子节点,那获取的数据类型就是`array`,所以处理`this.props.children`的时候需要注意。
React 提供一个工具方法 `React.Children` 来处理`this.props.children`,我们可以用`React.Children.map`来遍历子节点，而不用担心`this.props.children`的数据类型是什么了，需要了解很多的`React.Children`的方法请移步到{% link 官方文档 https://facebook.github.io/react/docs/top-level-api.html#react.children %}

## PropTypes
在上面的介绍中说过`组件的属性`可以接受任意值，字符串、对象、函数等等,但有的时候我们需要一种机制来验证别人使用的组件提供的参数是否符合要求。
组件类的`PropTypes`属性就是用来验证组件实例的属性是否符合要求，如下:
```
var MyTitle = React.createClass({
propTypes:{
	title: React.PropTypes.string.isRequired
},
render: function(){
	return <h1>{this.props.title}</h1>
}
});
```
在上面的`MyTitle`组件有一个title属性，`PropTypes` 告诉`React`这个`title`属性是必须的，而且它的值必须是字符串；
```
var data = 123;
ReactDOM.render(
<MyTitle title = {data} />,
document.body
);
```
如上代码中`title`的属性就通不过验证，控制台会报错。
`PropTypes`的更多设置请查看{%link 官方文档 http://facebook.github.io/react/docs/reusable-components.html %}
此外，getDefaultProps方法可以用来设置组件属性的默认值,如下:
```
var MyTitle = React.createClass({
getDefaultProps: function(){
	return {
    title: 'Hello World'
    };
},
render: function(){
	return <h1>{this.props.title}</h1>
}
});
ReactDOM.render(
<MyTitle />,
document.body
);
```
## 获取真是的DOM节点
我们在上面说的组件它并不是真实的DOM节点,而是存在于内存之中的一种数据结构，`Virtual DOM(虚拟 DOM)`，只有当组件插入文档以后才会变成真实的DOM,根据React的设计所有的DOM变动都现在虚拟的DOM上发生然后在将实际发生变动部分反应到真实的DOM上，这种设计算法叫做 {% link DOM diff  http://calendar.perfplanet.com/2013/diff %},这种算法可以极大的提高网页的性能表现，还有的时候需要从组件获取真实的DOM节点，这时候就要用到`ref`属性，如下:
```
var MyComponent = React.createClass({
handleClick: function(){
	this.refs.myTextInput.fous();
},
render: function(){
	return(
    	<div>
        	<input type = 'text' ref = 'myTextInput'/>
            <input type = 'button' value = 'Fous the text input' onClick = {this.handleCilck}/>
        </div>
    );
}
});
ReactDOM.render(
	<MyComponent />,
    documnet.getElementById('example')
);
```
在上面的代码中,组件`MyComponent`的子节点有一个文本输入框,用于获取文本的输入，这种情况就必须获取真实的DOM节点，虚拟的DOM节点是获取不了用户输入的，为了拿到用户输入的数据，文本框中必须有一个ref属性.然后`this.refs.[refName]`就返回这个真实的DOM节点。
注意,由于`this.refs.[refName]`属性获取的是真实的DOM，所以必须等到虚拟DOM插入文档后才能使用这个属性，否则报错，上面的代码中通过组件指定`Click`事件的回调函数确保了只有等到真实DOM发生`Click`事件之后才会读取`this.refs.[refName]`属性。
`React`组件支持很多事件，除了`Click`还有`KeyDown`,`Copy`,`Scroll`等等，详细请查看{% link 官方文档 http://facebook.github.io/react/docs/events.html#supported-events %}

## this.state
组件在很多情况是需要和用户互动，React设计的一大创新就是将组件看成一个状态机，开始用一个初始状态，然后用户互动导致状态改变，从而触发重新渲染UI，如下:
```
var LikeButton = React.createClass({
getInitialState: function(){
	return {liked: false};
},
handleClick: function(event){
	this.setState({liked: !this.state.liked});
},
render: function(){
	var text = this.state.liked ? "like": "haven't liked";
    return (
    <p onClick = {this.handleClick}>
    	Your {text} this. Click to toggle.
    </p>
    );
}
});
ReactDOM.render(
<LikeButton />,
documnet.getElementById('example')
);
```
LikeButton 是一个组件，它的`getInitialState`方法用于定义初始状态，也就是一个对象，这个对象可以通this.state属性读取，当用户点击组件，导致状态变化，`this.setState`方法就修改状态值，每次修改以后自动调用`this.render`方法进行渲染组件。
由于 `this.props`和`this.state`都用与描述组件的特性，可能会产生混淆，简单的区分方法是 `this.props`表示那些一旦定义就不会在改变的特性,而`this.state`是会随着用户互动而产生变化的特性。

## 表单
用户在表单填入的内容属于用户跟组件互动的，所以不能用`this.props`读取,如下:
```
var Input = React.createClass({
getInitialState: function(){
return {value: 'Hello!'};
},
handleChange: function(event){
	this.setState({value: event.target.value});
},
render: function(){
var value = this.state.value;
return(
<div>
	<input type="text" value={value} onChange={this.handleChange}/>
    <p>{value}</p>
</div>
)}
});
```
在上面的代码中输入框<input/>的值不能用`this.props.value`读取，而要定义一个`onChange` 事件的回调函数，通过`event.target.value`读取用户输入的值。`textarea`标签、`select`标签、`radio`标签都属于这种情况，查看更多 {% link 官方文档 http://facebook.github.io/react/docs/forms.html %}

## 组件的{% link 生命周期 https://facebook.github.io/react/docs/working-with-the-browser.html#component-lifecycle %}
组件的生命周期有三个状态:
- Mounting:插入真实 DOM(组件被插入到DOM中)
- Updating: 正在被重新渲染(组件被重新渲染，查明DOM是否应该刷新)
- Unmounting:已移出真实DOM( 组件从DOM中移除)

React 为每个状态提供两种处理函数,`will`函数在进入状态之前调用,`did`函数在进入状态之后调用,三中状态共计五种处理函数：
- componentWillMount()--（在挂载发生之前立即被调用）
- componentDidMount()--（在挂载结束之后马上被调用。需要DOM节点的初始化操作应该放在这里）
- componentWillUpdate(object nextProps, object nextState)--（在更新发生之前被调用。你可以在这里调用this.setState()）
- componentDidUpdate(object prevProps, object prevState)--（在更新发生之后调用）
- componentWillUnmount()--（在组件移除和销毁之前被调用。清理工作应该放在这里）
此外React还提供两种特殊状态的处理函数。
- componentWillReceiveProps(object nextProps)--（当一个挂载的组件接收到新的props的时候被调用。该方法应该用于比较this.props和nextProps，然后使用this.setState()来改变state）
- shouldComponentUpdate(object nextProps,object nextState):--（boolean当组件做出是否要更新DOM的决定的时候被调用。实现该函数，优化this.props和nextProps，以及this.state和nextState的比较，如果不需要React更新DOM，则返回false）
- findDOMNode() --(DOMElement可以在任何挂载的组件上面调用，用于获取一个指向它的渲染DOM节点的引用)
- forceUpdate() --(当你知道一些很深的组件state已经改变了的时候，可以在该组件上面调用，而不是使用this.setState() )
这些方法的详细说明都可以参考{% link 官方文档 http://facebook.github.io/react/docs/component-specs.html#lifecycle-methods %}，如下例子:
```
var Hello = React.Class({
getInitalState: function(){
return{opacity:1.0};
},
componentDidMount: function(){
this.tmer = setInterval(function(){
	var opacity = this.state.opacity;
    opacity -= 0.5;
    if(opacity < 0.1){
    opacity = 1.0;
    }
    this.setState({
    opacity: opacity
    });
}.bind(this),100);
},
render:function(){
	return (
    <div style={{opacity: this.state.opacity}}>
    Hello {this.props.name}
    </div>
    );
}
});
ReactDOM.render(
<Hello name = 'World'/>,
document.body
);
```
上面代码在`Hello`组件加载以后，通过`componentDidMount`方法设置一个定时器，每隔100毫秒就重新设置组件的透明度，从而引发重新渲染，另外组件的`style`属性的设置方法不能写成`style='opacity:{this.state.opacity}'`而要写成
```
style={{opacity:this.state.opacity}}
```
这是因为React的组件样式是一个对象，所以第一层大括号表javascript语法，第二层大括号表示样式对象,更多介绍查看{% link 官方文档 https://facebook.github.io/react/tips/inline-styles.html %}

## Ajax
组件的数据来源通常是通过Ajax请求服务器获取，可以使用`componentDidMount`方法设置Ajax请求，等到请求成功再用`this.setState`方法重新渲染UI,如下:
```
var UserGist = React.createCalss({
getInitialState: function(){
	return {
    	username: '',
        lastGistUrl: ''
    };
},
componentDidMount: function(){
$.get(this.props.source, function(result){
	var lastGist = result[0];
    if (this.isMounted()){
    	this.setState({
        	username: lastGist: ower.login,
            lastGistUrl:lastGist.html_url
        });
    }
}.bind(this));
},
render: function(){
return(
<div>
	{this.state.username}'s last gist is <a href={this.state.lastGistUrl}>here</a>
</div>
);
}
});

ReactDOM.render(
<UserGist source='https://api.github.com/users/octocat/gists'/>,
document.body
);
```
上面的代码使用JQuery 完成Ajax请求，这是为了便于说明React本身没有任何依赖，完全可以不用JQuery,而使用其他库。我们甚至可以将一个Promise对象传入组件，如下:
```
ReactDOM.render(
<Repolist promise={$.getJSON('https://api.github.com/search/repositories?q=javascript&sort=stars')}/>,
documnet.body
);
```
这段代码是从github上抓取数据然后将Promise对象作为属性，传给`RepoList`组件，如果Promise对象正在抓取数据(pending状态),组件显示'正在加载';如果Promise对象报错(rejected状态),组件显示报错信息；如果promise对象抓取数据成功(fulfilled状态)，组件显示获取的数据,如下:
```
var RepoList = React.createClass({
getInitialState: function(){
	return {loading: true, error: null, data: null};
},
componentDidMount: function(){
	this.props.promise.then(
    	value => this.setState({loading: false, data: value}),
        error => this.setState({loading: false, error: error});
    );
},
render: function(){
if (this.state.loading){
	return <span>Loading...</span>
} else if(this.state.error != null){
	return <span>Error: {this.state.error.message}</span>
} else {
	var repos = this.state.data.items;
    var repoList = repos.map(function(repo){
    	return(
        <li><a href = {repo.html_url}>{repo.name}</a>({repo.stargazers_count} stars)<br/>
        {repo.description}
        </li>
        );
    });
    return ({
    <main>
    	<h1>Most Popular javascript ProJects in GitHub</h1>
        <ol>{repoList}</ol>
    </main>
    });
}
}
});
```
Over~














