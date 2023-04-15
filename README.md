* computed 속성: 템플릿의 데이터 표현을 더 직관적으로 간결하게 도와주는 속성
  * 인자를 받지 못함
```
<div>{{ message.split('').reverse().join('') }}</div>
 //대신 
 computed: {
  reverseMessage() {
    return this.message.split('').reverse().join('');
  }
}
<div>{{ reverseMessage }}</div>


```
* watch 속성: 특정 데이터의 변화를 감지하여 자동으로 특정 로직을 수행
```
new Vue({
  data() {
    return {
      message: 'Hello'
    }
  },
  watch: {
    message: function(value, oldValue) {
      console.log(value);
    }
  }
})
//message가 바뀔때마다 watch문 실행
```
* store란: vuex라는 라이브러리를 사용 -> 리덕스 같은거인듯 ?
* vuex
* state: 여러 컴포넌트간의 공유되는 데이터
```
this.$store.state.counter //를 사용해서 가져옴
```
* mutation: action은 mutation 접근할 수 있도록 인자 제공, 매개변수를 commit라는api를 이용해서 mutation에 data를 넘긴다. -> 인자로 받은 결과로 state속성값을 변경한다 -> Mutations을 통해서만 state를 변경해야 함
  * 주로 state를 변경시키는 역할을 한다. 
```
setMenuList(state, data) {
      state.menuList = data
    },
  
  //다른파일에서 실행
  this.$store.commit('setMenuList', 데이터)
```
* action: 주로 mutation을 실행시키는 역할을한다. api호출은 actions에서 선언, action을 호출할때는 dispatch 호출
```
setMenuId(context, data) {
      context.commit('setMenuId', data)
    },
```
* Getters: 컴포넌트 데이터 불러올 대 사용
```
getters: {
  doubleCounter: function (state) {
    return state.counter * 2;
  }
},

//다른파일
doubleCounter() {
  return this.$store.getters.doubleCounter;
},
```
* mapActions: mapActions
```
this.$store.dispatch('addUsers', userObj) 대신, 
...mapActions(['addUsers']) 를 넣어주고
this.addUsers(userObj) 로 사용가능, 그럼 해당 vue 인스턴스에 선언된것처럼 this를 사용해서 사용가능
```
* created: 인스턴스가 작성된 후 동기적으로 호출된다. 부모 자식 관계의 컴퍼넌트가 렌더링될때 mounted보다 먼저 호촐되며 부모, 자식순으로 실행한다. 주로 데이터 초기화 선언을 created에서 많이한다. 가상돔을 건드릴 수 없음($element속성 사용x)
* mounted: created 다음으로 호출된다. 부모, 자식 관계의 컴퍼넌트가 렌더링 될떄 created 다음으로 호출되며 자식, 부모순으로 실행, 돔 조작관련을 mounted 영역
 * main.js: vue 실행하면 제일처음 실행 되는곳
```
new Vue({
  router,
  store,
  render: h => h(App)
}).$mount('#app')  //id가 app인 곳에 마운트 시키겠다는 뜻
```


# Vue.js 2부터 다시시작
* vue.js란: MVVM 패턴의 ViewModel 레이어에 해당하는 화면단 라이브러리
  * 데이터 바인딩과 화면 단위를 컴포넌트 형태로 제공, 롼련 apu를 지원하는데 궁극적 목적이있다.
  * 컴포넌트간 통신의 기보 골격은 React처럼 단방향 흐름 부모 ->자식
  * 리액트나 엥귤러보다 상대적으로 가볍고 빠르다
  *  문법이 단순하고 간결해서 초기학습비용이 낮고 누구나 쉽게 접근가능

* 라이프사이클초기화메서드(updated, destoryed 등)

* props전달 후 부모 emit이벤트  실행법
```
  <!-- 상위 컴포넌트 -->
<div id="app">
  <!-- 하위 컴포넌트에 상위 컴포넌트가 갖고 있는 message를 전달함 -->
  <child-component v-bind:propsdata="message"></child-component>
</div>
```

```
// 하위 컴포넌트
Vue.component("child-component", {
  // 상위 컴포넌트의 data 속성인 message를 propsdata라는 속성으로 넘겨받음
  props: ["propsdata"],
  template: '<p>{{ propsdata }}</p>'
});

// 상위 컴포넌트
var app = new Vue({
  el: "#app",
  data: {
    message: "Hello Vue! from Parent Component"
  }
});
```
* 주의점: props 변수명을 카멜기법으로 정의하면 html태그에서 사용할때는 케밥기법(-)dmfh tkdydgodigksek
* 컴포넌트간의 직접적인 통신은 불가능하도록 되어있는게 vue의 기본구조다
* 상 하위 간의 관계가 아닌 컴포넌트간 통신을 위해서는 event bus를 활용
```
// 화면 개발을 위한 인스턴스와 다른 별도의 인스턴스를 생성하여 활용
var eventBus = new Vue();

new Vue({
  // ...
});
```

```
이벤트를 발생시킬 컴포넌트에서 emit 호출
eventBus.$emit("refresh", 10);
```
```// 이벤트 버스 이벤트는 일반적으로 라이프 사이클 함수에서 수신
new Vue({
  created: function() {
    eventBus.$on("refresh", function(data) {
      console.log(data); // 10
    });
  }
});

```
* vue 라우터는 기본적으로 url/#/라우터의 이름 으로되어있다 #을 제외하고싶으면 mode속성을 추가
```
new VueRouter({
  mode: "history"
});
```
* vue의 한계점
  * 모든 컴포넌트에 고유의 이름을 붙여야함
  * js파일에서 template 안의 html의 문법강조가 되지안흠
  * js파일 상에서 css스타일링 작업이 거의 불가
  * ES5를 이용해서 계속 앱을 작성할ㅇ경우 babel의 빌드가 지원되지 않는다

* 싱글파일 컴포넌트로 개발하려면 webpacp과 같은 번들링 도구가 필요하다.
* vue Loader: 싱글파일 컴포넌트를 브라우저에서 실행할 수 있게 자바프크립트 파일로 변환해주는 웹팩로더. 뷰로더를 사용하면 다음과 같은 장점이 있음
  * ES6지원
  * \<style>\</style> 과 \<template>\</template> 에 대한 각각의 웹팩로더지원 ex) sass, jade
  * 각 .vue 컴포넌트의 스코프로 좁힌 css스타일링 지원
  * 웹팩의 모듈 번들링에 대한 지원과 의존성 관리가 제공
  * 개발시 hot module replacement 지원 -> 새로고침하지 않아도 변경사항을 즉시 확인할 수 있는기능
  * 