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
  * vuex란: 뷰의 상태관리를 위한 패턴이자 라이브러리
  * 상태관리가 필요한이유: 컴포넌트기반 프레임워크는 작은단위로 쪼개진 여러개의 컴포넌트로 화면을 구성 -> 컴포넌트간 통신이나 데이터 전달을 할때 좀더 유기적으로 관리할 필요성이 있다.
  * 데이터 통신을 한곳에 중앙 집중식으로 관리하는것이 상태관리
  * state: 컴포넌트간에 공유할 data
  * view: 데이터가 필요될 template
  * actions: 사용자의 입력에 다라 반응할 methods
  ```
  new Vue({
  // state
  data() {
    return {
      counter: 0
    };
  },
  // view
  template: `
    <div>{{ counter }}</div>
  `,
  // actions
  methods: {
    increment() {
      this.counter++;
    }
  }
  });


*  화면단위를 쪼갤수록 한컴포넌트의 데이터를 다른컴포넌트의 화면에서 표시할일이많아짐 => 최상위 컴포넌트의 맨아래까지 전달하기 위해 모든 컴포넌트에 PROPS, EVENT EMIT를 선언해줘야함 => vuex 상태관리
```   
Vue.use(Vuex);

export const store = new Vuex.Store({
  state: {
    counter: 0
  }
});

port Vue from "vue";
import App from "./App.vue";
// store.js를 불러오는 코드
import { store } from "./store";

new Vue({
  el: "#app",
  // 뷰 인스턴스의 store 속성에 연결
  store: store,
  render: h => h(App)
});
```
* 접근법
```
 Parent counter : {{ $store.state.counter }} <br />
  methods: {
    addCounter() {
      this.$store.state.counter++;
    },
```
* 이처럼 뷰엑스를 사용하면 여러 컴포넌트간에 공유할 데이터를 효율적으로 관리가능
* getter: computed 속성을 사용
```
<!-- App.vue -->
<div id="app">
  Parent counter : {{ parentCounter }}
  <!-- ... -->
</div>

<!-- Child.vue -->
<div>
  Child counter : {{ childCounter }}
  <!-- ... -->
</div>

// App.vue
computed: {
  parentCounter() {
    return this.$store.state.counter;
  }
},

// Child.vue
computed: {
  childCounter() {
    return this.$store.state.counter;
  }
},
```
* gettsrs를 vuex에 추가해도됨
```
// store.js
export const store = new Vuex.Store({
  // ...
  getters: {
    getCounter: function (state) {
      return state.counter;
    }
  }
});

// App.vue

computed: {
  parentCounter() {
    this.$store.getters.getCounter;
  }
},


```
* getters장점: 단순히 state 값을 반환하는 것뿐만아니라 getters에 선언된 속성에서 filter(), reverse()등의 추가적인 계산 로직이 들어갈때 발휘
* mutation: vuex의 데이터, 즉 state값을 변경하는 로직들을 의미 method에 등록한다
* Actions과의 차이점: 인자르 받아 vuex에 넘겨줄소 있고 computed가 아닌 method에 등록
* return this.$store.state.counter++; 같이 컴포넌트에서 직접 state에 접근하여 변경하는것은 안티패턴으로 vue의 reactivity 체계와 상태관리 패턴에 맞지않은 구현방식 => 여러개의 컴포넌트에서 같은 state값을 동시에 제어하게되면 state 갑싱 어느컴포넌트에서 호출해서 변경된건지 추적이 어렵다. -> 상태변화를 명시적으로 수행함으로서 테스팅, 디버깅, reactive성질 준수의 혜택이 있다.
```
// store.js
export const store = new Vuex.Store({
  // ...
  mutations: {
    addCounter: function (state, payload) {
      return state.counter++;
    }
  }
});

<!-- App.vue -->
<div id="app">
  Parent counter : {{ parentCounter }} <br>
  <button @click="addCounter">+</button>
  <!-- ... -->
</div>

methods: {
  addCounter() {
    this.$store.state.counter++;
  }
},

// App.vue
methods: {
  addCounter() {
    // this.$store.state.counter++; -> 접근불가
    this.$store.commit('addCounter');
  }
},
```
* mutation에 인자값 넘기는법
```
this.$store.commit('addCounter', 10);
this.$store.commit('addCounter', {
  value: 10,
  arr: ["a", "b", "c"]
});

//받는법
mutations: {
  // payload 가 { value : 10 } 일 경우
  addCounter: function (state, payload) {
    state.counter = payload.value;
  }
}
```
* action: mutaions에는 순차적인 로직들만 선언하고 actions 에는 비 순차적 똔느 비동기 처리 로직들을 선언한다.
* mutation이 있는데 action을 사용하는 이유는 mutation은 역할자체가 state관리에 주안점을 두고있고 상태관리 자체가 한 데이터에 대해 여러개의 컴포넌트가 관여하는것을 효율적으로 관리하기 위함인데 mutation에 비동기 처리 로직들을 ㅊ포함하면 같은 값아대해 여러개의 컴포넌트에서 변경을 요청했을때 변경 순서 파악이 어렵기 때문
* 따라서 비동기처리로직은 Actions, 동기처리로직은 mutations에 나눠 구현한다 -> ex) setTimeout(), http통신 등등 처리결과를 받아올 타이밍이 예측되지 않는로직을 actions에선언
* 
```
// store.js
export const store = new Vuex.Store({
  // ...
  mutations: {
    addCounter: function (state, payload) {
      return state.counter++;
    }
  },
  actions: {
    addCounter: function (context) {
      // commit 의 대상인 addCounter 는 mutations 의 메서드를 의미한다.
      return context.commit('addCounter');
    }
  }
});
```
```
/ store.js
export const store = new Vuex.Store({
  actions: {
    getServerData: function (context) {
      return axios.get("sample.json").then(function() {
        // ...
      });
    },
    delayFewMinutes: function (context) {
      return setTimeout(function () {
        commit('addCounter');
      }, 1000);
    }
  }
});
```
* actions를 호출할때는 dispatch 사용
```
 addCounter() {
    this.$store.commit('addCounter');
  }
```
* actions에 인자값 넘기는법
```
\<button @click="asyncIncrement({ by: 50, duration: 500 })">Increment\</button>

export const store = new Vuex.Store({
  actions: {
    // payload 는 일반적으로 사용하는 인자 명
    asyncIncrement: function (context, payload) {
      return setTimeout(function () {
        context.commit('increment', payload.by);
      }, payload.duration);
    }
  }
})
```

* 네비게이션 가드: 뷰 ㄹ우터로 특정 url에 접근할 때 해당 url의 접근을 막는 방법을 말한다 ex) 사용자의 인증정보가 없으면 특정 페이지에 접근하지 못하게 할때 사용하는 기술
* 종류
  * 애플리케이션 전역에서 동작하는 전역가드
  * 특정 url에서만 동작하는 라우터가드
  * 라우터 컴포넌트 안에 정의하는 컴포넌트 가드

* 전역가드 설정하는법
```
var router = new VueRouter();

router.beforeEach(function (to, from, next) {
  // to : 이동할 url
  // from : 현재 url
  // next : to에서 지정한 url로 이동하기 위해 꼭 호출해야 하는 함수
});
```

* router.beforeEach()를 호출하고 나면 모든 라우팅이 대기 상태, 원래 url이 변경되고 나면 해당 url에 따라 화면이 자연스럽게 이동 but 전역가드 설정이 되어있으면 화면이 전환 되지 않음 -> next() 가호출되면 전환

* 로그인정보 있을때와 없을때 전환 하는코드
```
router.beforeEach(function (to, from, next) {
  // to: 이동할 url에 해당하는 라우팅 객체
  if (to.matched.some(function(routeInfo) {
    return routeInfo.meta.authRequired;
  })) {
    // 이동할 페이지에 인증 정보가 필요하면 경고 창을 띄우고 페이지 전환은 하지 않음
    alert('Login Please!');
  } else {
    console.log("routing success : '" + to.path + "'");
    next(); // 페이지 전환
  };
});
```
* 컴포넌트가드: 라우터로 지정된 특정 컴포넌트에 가드를 설정하는 법
```
const Login = {
  template: '<p>Login Component</p>',
  beforeRouteEnter (to, from, next) {
    // Login 컴포넌트가 화면에 표시되기 전에 수행될 로직
    // Login 컴포넌트는 아직 생성되지 않은 시점
  },
  beforeRouteUpdate (to, from, next) {
    // 화면에 표시된 컴포넌트가 변경될 때 수행될 로직
    // `this`로 Login 컴포넌트를 접근할 수 있음
  },
  beforeRouteLeave (to, from, next) {
    // Login 컴포넌트를 화면에 표시한 url 값이 변경되기 직전의 로직
    // `this`로 Login 컴포넌트를 접근할 수 있음
  }
}
```
* Slot: 컴포넌트의 재사용성을 높여주는 기능 -> 특정컴포넌트에 등록되 하위 컴포넌트의 마크업을 확장하거나 재정의 할수있다.
```
<!-- ButtonTab.vue -->
<template>
  <div class="tab panel">
    <!-- 탭 헤더 -->
    <slot></slot>
    <!-- 탭 본문 -->
    <div class="content">
      Tab Contents
    </div>
  </div>
</template>
```
* slot태그로 빈칸을 남겨둠, 영역을 구현하지 않으면 해당부분은 공백
* 슬롯을 사용하면 컴포넌트의 특정 마크업 영역을 재정의하여 각기다르게 표현가능
* vmodel: Form 요소를 개발할때 사용
```
<input v-model="inputText">

new Vue({
  data: {
    inputText: ''
  }
})

```
* v-model 속성은 v-bind 와 v-on 기능의 조합으로 동작 -> 일일이 v-bind와 v-on 속성을 다 지정해주지 않아도 편하개 개발할수있게 고안된 문법
* v-bind: 뷰 인스턴스의 데이터속성을 해당 html요소에 연결할때 사용
* v-on: 해당 html 요소의 이벤트를 뷰 인스턴스의 로직과 연결할때 사용
* 한국어를 사용할때는 v-model보다 v-bind와 v-on 사용이 나음