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