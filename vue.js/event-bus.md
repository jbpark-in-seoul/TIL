# Event Bus
`component`간에 데이터를 전달하는 방식으로 사용된다.   
`vuex`는 규모가 큰 경우에 주로 사용되며, `event-bus`는 그보다 규모가 작은 경우에 사용된다.

---

## 이벤트 초기화
`event-bus`를 사용하기 위해서는 하나의 인스턴스를 새로 생성해야 한다.   
새로 만들어진 `event-bus`는 데이터를 주고 받을 두 `component`의 중간 지점에 위치한다.
```javascript
import Vue from 'vue';

const EventBus = new Vue();
export default EventBus;
```

---

## 이벤트 생성
`$emit` 속성을 이용해서 이벤트에 대한 정보를 전달한다.
```javascript
EventBus.$emit('evant-name', payload);
```

---

## 이벤트 등록
`event-bus`에 전달된 이벤트 정보를 돌려 받는다.
```javascript
EventBus.$on('event-name', payload => {
  this.data = payload.data;
});
```

---

## 이벤트 해제
`event-bus`에 등록된 이벤트를 해제한다.
```javascript
// 특정 이벤트 해제
EventBus.$off('event-name');

// 전체 이벤트 해제
EventBus.$off();
```
