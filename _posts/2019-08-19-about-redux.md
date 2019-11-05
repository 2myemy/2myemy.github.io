---
title: "Redux를 이해해봅시다"
date: 2019-06-19 08:26:28 -0400
categories: React
---


>https://qiita.com/mpyw/items/a816c6380219b1d5a3bf?utm_campaign=popular_items&utm_medium=feed&utm_source=popular_items 를 참고하였습니다.
>>https://medium.com/@ca3rot/%EC%95%84%EB%A7%88-%EC%9D%B4%EA%B2%8C-%EC%A0%9C%EC%9D%BC-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0-%EC%89%AC%EC%9A%B8%EA%B1%B8%EC%9A%94-react-redux-%ED%94%8C%EB%A1%9C%EC%9A%B0%EC%9D%98-%EC%9D%B4%ED%95%B4-1585e911a0a6 를 인용하였습니다.

<h2>Redux란 무엇인가?</h2><br>
vue를 이용한 대형 프로젝트를 진행했고, vuex를 자유자재로 다룰 줄 알게되었기 때문에,

Redux도 아주 쉽게 이해할 수 있을 줄 알았다...

하지만 vue가 너무 쉬웠던 것.....

React의 길은 험난했다 


<h2>Redux의 기본 플로우</h2><br>
![reduxflow](https://user-images.githubusercontent.com/49894861/63241570-25b15000-c28f-11e9-8120-5754a1e922a5.png)

Redux의 기본 플로우는 vuex와 아주 유사하다.

action, store, reducer 이 세가지 개념을 먼저 생각해보자.



<h3>Action</h3>
store에 바로 접근할 수 없고, action을 거쳐야만 store에 접근할 수 있다.

따라서 action은 store에 보내는 데이터 묶음, 이벤트 생성자 같은 역할을 한다.


{% highlight javascript %}
{
    type: "액션의 종류를 한번에 식별할 수 있는 문자열 혹은 심볼",
    payload: "액션의 실행에 필요한 임의의 데이터",
}
{% endhighlight %}
action은 보통 위와 같은 형식을 갖고있다.

<h3>Reducer</h3><br>
![reducer](https://user-images.githubusercontent.com/49894861/63242331-010aa780-c292-11e9-8ab6-ee139b45443d.png)

>액션은 무언가 일어난다는 사실을 기술하지만, 그 결과 애플리케이션의 상태가 어떻게 바뀌는지는 특정하지 않습니다. 이것은 리듀서가 할 일이죠.

{% highlight javascript %}
import { AUTH_USER, AUTH_ERROR } from '../actions/types';

const INITIAL_STATE = {
  authenticated: '',
  errorMessage: '',
};

export default function authReducer(state = INITIAL_STATE, action) {
  switch (action.type) {
    case AUTH_USER:
      return {
        ...state,
        authenticated: action.payload,
      };
    case AUTH_ERROR:
      return {
        ...state,
        errorMessage: action.payload,
      };
    default:
      return state;
  }
}
{% endhighlight %}
reducer의 예제는 위와 같다.
{% highlight javascript %}
import { combineReducers } from 'redux';
​
const sessionReducer = (state = {loggedIn: false, user: null}, payload) => {
    /* blahblah */
};
const timelineReducer = (state = {type: "home", statuses: []}, payload) => {
    /* blahblah */
};
const notificationReducer = (state = [], payload) => {
    /* blahblah */
};
​
export default combineReducers({
    session: sessionReducer,
    timeline: timelineReducer,
    notification: notificationReducer,
})
{% endhighlight %}
대규모 개발에 Reducer를 미세하게 분할하는 경우 Redux에서 제공하는 ```combineReducers```함수를 이용하여 위와 같이 쓸 수 있다.



<h2>Connect</h2><br>

redux엔 새로운 것이 등장했다.

connect라는 함수를 쓰면, 리덕스 패턴 사용할 수 있는 컴포넌트를 생성할 수 있다.

즉, redux에 연결하고 싶은 컴포넌트를 connect함수의 인수로 전달하면 된다.

>1.Store가 가진 state를 어떻게 props에 엮을 지 정한다(이 동작을 정의하는 함수는 mapStateToProps라고 불립니다)<br>
2.Reducer에 action을 알리는 함수 dispatch를 어떻게 props에 엮을 지 정한다(이 동작을 정의하는 함수는 mapDispatchToProps라고 불립니다)<br>
3.위에 두가지가 적용된 props를 받을 Component를 정한다<br>
4.Store와 Reducer를 연결시킬 수 있도록 만들어진 Component가 반환값이 된다


{% highlight javascript %}
import {
  startCall,
  rejectCall,
  setToken
} from '../actions';

const mapStateToProps = state => ({
  applicantName: `${state.applicant.name}`,
  loggedIn: state.auth.authenticated,
  callWindow: state.callWindow,
  loader: state.loader,
  modal: state.modal,
});

const actionCreators = {
  rejectCall,
  startCall,
  setToken
};

export default connect(
  mapStateToProps,
  actionCreators,
)(withRouter(App));
{% endhighlight %}

내가 하는 프로젝트에서 connect를 예로 들자면, 위과 같다.

위의 코드는 Component가 아닌 Container라는 곳에 정리해둔다.


<h2>Container</h2>
>수많은 Component가 리스트 형식으로 모여있는데 가운데 각 요소의 Component를 각각 연결하면 수습이 안 되므로, 대표적인 자식요소를 안고 있는 하나의 부모Component가 connect 되는 경우

{% highlight javascript %}
<UsersList>
    <User />
    <User />
    <User />
    <User />
</UsersList>
{% endhighlight %}

>이 대표로서 connect될 부모 Component를 Container라고 부릅니다. Container는 가독성을 높이기 위해, Component와는 디렉토리를 따로 나누는 경우가 많습니다.






뭔가 처음보는 개념도, 어려운 개념도 많아서 베끼기에 급급한 포스팅이 된 것 같다.

조금더 연습해보고, 익힌 다음에 포스팅을 수정해야겠다. :(
