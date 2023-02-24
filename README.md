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
