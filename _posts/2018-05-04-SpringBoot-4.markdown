---
layout: post
title:  "SpringBoot 5일차 (react)"
date:   2018-05-04 02:13:00
author: 이상현
categories: Naver_Hackday_Ready
---

# 1. React가 Tomcat에서 돌아가게 해보자 !
## 알아봐야 할 것들
- npm을 플러그인을 추가하던데, React는 결국에 node로 돌아가는가?
- mvnw springboot:run 명령어는 그냥 인텔리제이의 런과 뭐가 다른가?
- 리액트, 스프링부트 따로 서버두고 해버리면 안되나?
- 인텔리제이 Edit configure에서 maven추가하고 거기서 런하기!

## 여김없이 나오는 에러
> No plugin found for prefix 'springboot' in the current project and in the plugin groups [org.apache.maven.plugins, org.codehaus.mojo] available from the repositories [local (C:\Users\Sanghyun Lee\.m2\repository), central (https://repo.maven.apache.org/maven2)] 에러가 발생함.
<pre>
  인텔리제이의 run기능을 이용하니 react 플러그인이 다운받아지거나 동작하는 콘솔로그가 보이지 않았다.
  그래서 문득 메이븐을 통해 mvnw springboot:run으로 해야겠다는 생각이 들었지만, 위와 같은 오류가 발생하였다.
  성렬이형한테 물어보니 2가지를 해보라고 알려주셨다.
</pre>
1. pom에 다음을 추가
```js
  <repositories>
      <repository>
          <id>spring-releases</id>
          <url>https://repo.spring.io/libs-release</url>
      </repository>
  </repositories>
  <pluginRepositories>
      <pluginRepository>
          <id>spring-releases</id>
          <url>https://repo.spring.io/libs-release</url>
      </pluginRepository>
  </pluginRepositories>
```

2. maven 빌드를 인텔리제이 오른쪽 상단에 있는 edit configuration을 눌러서 추가한 후 그것을 이용하기.
<img src="{{ site.baseurl }}/assets/postImages/20180504/maven.jpg"> <br>

3. 리액트와 관련된 플러그인 npm, babel 등이 설치되고 리액트가 thymeleaf html위에서 도는 것을 확인할 수 있었다.

4. [깃허브]("https://github.com/phpbae/spring-boot-react")의 리드미파일을 참조하여 js파일, package.json, webpack.config.js등을 추가하였다.

# 2. React에서 CRUD를 해보자 !
## 리스트로 주면 어떤 식으로 받아지는지 확인
```js
'use strict';

// tag::vars[]
const React = require('react');
const ReactDOM = require('react-dom');
const client = require('./client');
// end::vars[]

// tag::app[]
class App extends React.Component {

	constructor(props) {
		super(props);
		this.state = {
			isLoading: false,
			boards: [],
		};
	}

	componentDidMount() {
		client({method: 'GET', path: '/selectBoard'}).done(response => {
			this.setState({
				boards: response,
				isLoading: true,
			});
		});
	}

	render() {
		const {isLoading} = this.state;
		if(isLoading === false) {
            return (
                <div>
                    <p>Hi, im react!! Loading ...</p>
                </div>
            )
		}
		else {
			console.log(this.state.boards);
			return (
				<div>
					<p>Boards List</p>
					{/*<EmployeeList employees={this.state.boards}/>*/}
				</div>
			)
        }
	}
}
// end::app[]

// tag::employee-list[]
class EmployeeList extends React.Component{
	render() {
		var employees = this.props.boards.map(employee =>
			<Employee key={employee.bno} employee={employee}/>
		);
		return (
			<table>
				<tbody>
					<tr>
						<th>First Name</th>
						<th>Last Name</th>
						<th>Description</th>
					</tr>
					{employees}
				</tbody>
			</table>
		)
	}
}
// end::employee-list[]

// tag::employee[]
class Employee extends React.Component{
	render() {
		return (
			<tr>
				<td>{this.props.employee.bno}</td>
				<td>{this.props.employee.title}</td>
				<td>{this.props.employee.content}</td>
			</tr>
		)
	}
}

ReactDOM.render(
	<App />,
	document.getElementById('react')
)
```
<pre>
	console.log를 찍어봤더니 아래와 같이 나왔다!
</pre>
<img src="{{ site.baseurl }}/assets/postImages/20180504/select.jpg"> <br>
