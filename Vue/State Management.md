# 상태 관리 State Management

## 목차

- [상태 관리 State Management](#상태-관리-state-management)

- [State management library (Pinia)](#state-management-library-pinia--vue-공식-상태-관리-라이브러리)

- [Pinia 실습](#pinia-실습)

- [Local Storage](#local-storage--브라우저-내에-key-value-쌍을-저장하는-웹-스토리지-객체)

- [참고](#참고)

---

- Vue 컴포넌트는 이미 반응형 상태를 관리하고 있음 (상태 === 데이터)

- 컴포넌트 구조의 단순화
  
  - '단방향 데이터 흐름'의 간단한 표현 ( 상태 -> 뷰 -> 기능 -> 상태 반복)
  
  - 상태 (State) : 앱 구동에 필요한 기본 데이터
  
  - 뷰 (View) : 상태를 선언적으로 매핑하여 시각화
  
  - 기능 (Actions) : 뷰에서 사용자 입력에 대해 반응적으로 상태를 변경할 수 있게 정의된 동작
  
  > ```js
  > <template>
  >   <!-- 뷰(view) -->
  >   <div>{{ count }}</div>
  > </template>
  > 
  > <script setup>
  > import { ref } from 'vue'
  > 
  > // 상태(State)
  > const count = ref(0)
  > 
  > // 기능(Actions)const increment = function () {
  >     count.value++
  > }
  > </script>
  > ```

- 상태 관리의 단순성이 무너지는 시점
  
  - 여러 컴포넌트가 상태를 공유할 때
    
    - 여러 뷰가 동일한 상태에 종속되는 경우
      
      - 공유 상태를 공통 조상 컴포넌트로 '끌어올린' 다음 props로 전달하는 것
      
      - 하지만 계층 구조가 깊어질 경우 비효율적, 관리가 어려워 짐
    
    - 서로 다른 뷰의 기능이 동일한 상태를 변경시켜야 하는 경우
      
      - 발신(emit)된 이벤트를 통해 상태의 여러 복사본을 변경 및 동기화하는 것
      
      - 마찬가지로 관리의 패턴이 깨지기 쉽고 유지 관리할 수 없는 코드가 됨
  
  - 해결책
    
    - 각 컴포넌트의 공유 상태를 추출하여, 전역에서 참조할 수 있는 저장소에서 관리
    
    - 컴포넌트 트리는 하나의 큰 `View`가 되고 모든 컴포넌트는 트리 계층 구조에 관계없이 상태에 접근하거나 기능을 사용할 수 없음
    
    - Vue의 공식 상태 관리 라이브러리 === **Pinia** (기능, 상태 관리)

---

## State management library (Pinia)

: Vue 공식 상태 관리 라이브러리

- Pinia 설치
  
  - Vite 프로젝트 빌드 시 Pinia 라이브러리 추가
    
    - `npm create vue@latest` 이후 `Add Pinia for state management?` 에서 `Yes` 선택
  
  - Vue 프로젝트 구조 변화 : `stores` 폴더 신규 생성

- Pinia 구성 요소
  
  ```js
  // sotres/counter.js
  
  import { ref, computed } from 'vue'
  import { defineStore } from 'pinia'
  
  export const useCounterStore = defineStore('counter', () => {
      const count = ref(0)
  
      const doubleCount = computed(() => count.value * 2_
  
      const increment = function () {
          count.value++
      }
  
      return { count, doubleCount, increment }
  })
  ```
  
  - **`store` : 중앙 저장소**
    
    - 모든 컴포넌트가 공유하는 상태, 기능 등이 작성됨
    
    - `defineStore()`의 반환 값의 이름은 `use`와 `store`를 사용하는 것을 권장
    
    - `defineStore()`의 첫번째 인자는 어플리케이션 전체에 걸쳐 사용하는 store의 고유 ID
    
    > ```js
    > export const useCounterStore = defineStore('counter', () => {
    >     const count = ref(0)
    > 
    >     const doubleCount = computed(() => count.value * 2_
    > 
    >     const increment = function () {
    >         count.value++
    >     }
    > 
    >     return { count, doubleCount, increment }
    > })
    > ```
  
  - **`state` : 반응형 상태(데이터)**
    
    - `ref() === state`
    
    > ` const count = ref(0)`
  
  - **`getters` : 계산된 값**
    
    - `computed() === getters`
    
    > `const doubleCount - computed(() => count.value * 2)`
  
  - **`actions` : 메서드**
    
    - `function() === actions`
    
    > ```js
    > const increment = function () {
    >         count.value++
    > }
    > ```
  
  - Setup Stores의 반환 값
    
    - pinia의 상태들을 사용하려면 반드시 반환해야 함
    
    - **store에서는 공유하지 않는 private한 상태 속성을 가지지 않음**
    
    > `return { count, doubleCount, increment }`
  
  - **`plugin`**
    
    - 애플리케이션의 상태 관리에 필요한 추가 기능을 제공하거나 확장하는 도구나 모듈
    
    - 애플리케이션의 상태 관리를 더욱 간편하고 유연하게 만드렁주며 패키지 매니저로 설치 이후 별도 설정을 통해 추가됨
  
  - 정리
    
    - Pinia는 `store`라는 저장소를 가짐
    
    - `store`는 `state`,`getters`, `actions`으로 이루어지며 각각 `ref()`, `computed()`, `function()`과 동일함

- Pinia 구성 요소 활용
  
  - `State`
    
    - 각 컴포넌트 깊이에 관계 없이 `store` 인스턴스로 `state`로 접근하여 직접 읽고 쓸 수 있음
    
    - 만약 `store`에 `state`를 정의하지 않았다면 컴포넌트에서 새로 추가할 수 없음
    
    > ```js
    > <!-- App.vue -->
    > 
    > import { useCounterStore } from '@/stores/counter'
    > 
    > const store = useCounterStore()
    > 
    > // state 참조 및 변경
    > console.log(store.count)
    > const newNumber = store.count + 1
    > ```
    > 
    > ```js
    > <!-- App.vue -->
    > 
    > <template>
    >   <div>
    >     <p>state : {{ store.count }}</p>
    >   </div>
    > </template>
    > ```
  
  - `Getters`
    
    - `store`의 모든 `getters` 또한 `state`처럼 직접 접근할 수 있음
    
    > ```js
    > <!-- App.vue -->
    > 
    > // getters 참조
    > console.log(store.doubleCount)
    > ```
    > 
    > ```js
    > <!-- App.vue -->
    > 
    > <template>
    >   <div>
    >     <p>getters : {{ store.doubleCount }}</p>
    >   </div>
    > </template>
    > ```
  
  - `Actions`
    
    - `store`의 모든 `actions` 또한 직접 접근 및 호출할 수 있음
    
    - `getters`와 달리 `state` 조작, 비동기, API 호출이나 다른 로직을 진행할 수 있음
    
    > ```js
    > <!-- App.vue -->
    > 
    > // actions 호출
    > store.increment
    > ```
    > 
    > ```js
    > <!-- App.vue -->
    > 
    > <template>
    >   <div>
    >     <button @click="store.increment()">+++</button>
    >   </div>
    > </template>
    > ```
  
  - Vue devtools로 Pinia 구성 요소 확인하기
    
    - 개발자 도구 -> Vue -> Pinia 에서 확인 가능

- Pinia 실습 - Todo 프로젝트 구현
  
  - 컴포넌트 구성
    
    - App
      
      - TodoForm
      
      - TodoList
        
        - TodoListItem
  
  - 사전 준비
    
    > 1. 초기 생성된 컴포넌트 모두 삭제 (`App.vue` 제외)
    > 
    > 2. `src/assets` 내부 파일 모두 삭제
    > 
    > 3. `main.js` 해당 코드 삭제
    >    
    >    ```js
    >    // main.js
    >    
    >    import './assets/main.css'
    >    ```
    > 
    > 4. `TodoListItem` 컴포넌트 작성
    >    
    >    ```js
    >    <!-- TodoListItem.vue -->
    >    
    >    <template>
    >      <div>
    >        TodoListItem
    >      </div>
    >    </template
    >    ```
    > 
    > 5. `TodoList` 컴포넌트 작성 및 `TodoListItem` 컴포넌트 등록
    >    
    >    ```js
    >    <!-- TodoList.vue -->
    >    
    >    <template>
    >      <div>
    >        <TodoListItem />
    >      </div>
    >    </template>
    >    
    >    <script setup>
    >    import TodoListItem from '@/components/TodoListItem.vue'
    >    </script>
    >    ```
    > 
    > 6. `TodoForm` 컴포넌트 작성
    >    
    >    ```js
    >    <!-- TodoForm.vue -->
    >    
    >    <template>
    >      <div>
    >        TodoForm
    >      </div>
    >    </template>
    >    ```
    > 
    > 7. `App` 컴포넌트에 `TodoList`, `TodoForm` 컴포넌트 등록
    >    
    >    ```js
    >    <!-- App.vue -->
    >    
    >    <template>
    >      <div>
    >        <h1>Todo Project</h1>
    >        <TodoList />
    >        <TodoForm />
    >      </div>
    >    </template>
    >    
    >    <script setup>
    >    import TodoForm from '@/components/TodoForm.vue'
    >    import TodoList from '@/components/TodoList.vue'
    >    </script>
    >    ```
    > 
    > 8. 컴포넌트 구성 확인
    >    
    >    - 개발자 도구 -> Vue -> Components
  
  - Read Todo
    
    > 1. `store`에 임시 `todos ` 목록 `state` 정의
    >    
    >    ```js
    >    // stores/counter.js
    >    
    >    import { ref, computed } from 'vue'
    >    import { defineStore } from 'pinia'
    >    
    >    export const useCounterStore = defineStore('counter', () => {
    >        let id = 0
    >        const todos = ref([
    >            { id: id++, text: '할 일 1', isDone: false},
    >            { id: id++, text: '할 일 2', isDone: false}
    >           ])
    >      return { todos }
    >    })
    >    ```
    > 
    > 2. `store`의 `todos state`를 참조
    >    
    >    - 하위 컴포넌트인 `TodoListItem`을 반복하면서 개별 `todo`를 `props`로 전달
    >    
    >    ```js
    >    <!-- TodoList.vue -->
    >    
    >    import { useCounterStore } from '@/stores/counter'
    >    import TodoListItem from '@/components/TodoListItem.vue'
    >    
    >    const store = useCounterSore()
    >    ```
    >    
    >    ```js
    >    <!-- TodoList.vue -->
    >    
    >    <template>
    >      <div>
    >        <TodoListItem
    >          v-for="todo in store.todos"
    >          :key="todo.id"
    >          :todo="todo"
    >        />
    >      </div>
    >    </template>
    >    ```
    > 
    > 3. `props` 정의 후 데이터 출력 확인
    >    
    >    ```js
    >    <!-- TodoListItem.vue -->
    >    
    >    <tempalte>
    >      <div>{{ todo.text }}</div>
    >    </template>
    >    
    >    <script setup>
    >    defineProps({
    >      todo: Object
    >    })
    >    </script>
    >    ```
  
  - Create Todo
    
    > 1. `todos` 목록에 `todo`를 생성 및 추가하는 `addTodo` 액션 정의
    >    
    >    ```js
    >    // stores/counter.js
    >    
    >    const addTodo = function (todoText) {
    >        todos.value.push({
    >            id: id++,
    >            test: todoText,
    >            isDone: false
    >        })
    >    }
    >    
    >    return { todos, addTodo }
    >    ```
    > 
    > 2. `TodoForm`에서 실시간으로 입력되는 사용자 데이터를 양방향 바인딩하여 반응형 변수로 할당
    >    
    >    ```js
    >    <!-- TodoForm.vue -->
    >    
    >    import { ref } from 'vue'
    >    
    >    const todoText = ref('')
    >    ```
    >    
    >    ```js
    >    <!-- TodoForm.vue -->
    >    
    >    <template>
    >      <div>
    >        <form>
    >          <input type="text" v-model="todoText">
    >          <input type="submit">
    >        </form>
    >      </div>
    >    </template>
    >    ```
    > 
    > 3. `submit` 이벤트가 발생했을 때 사용자 입력 텍스트를 인자로 전달하여  `store` 에 정의한 `addTodo` 액션 메서드를 호출
    >    
    >    ```js
    >    <!-- TodoForm.vue -->
    >    
    >    import { useCounterSotre } from '@/stores/counter'
    >    
    >    const store = useCounterStore()
    >    
    >    const createTodo = function (todoText) {
    >        store.addTodo(todoText)
    >    }
    >    ```
    >    
    >    ```j
    >    <!-- TodoForm.vue -->
    >    
    >    <template>
    >      <div>
    >        <form @submit.prevent="createTodo(todoTexst)">
    >          <input type="text" v-model="todoText">
    >          <input type="submit">
    >        </form>
    >      </div>
    >    </template>
    >    ```
    > 
    > 4. `form` 요소를 선택하여 `todo` 입력 후 `input` 데이터를 초기화할 수 있도록 처리
    >    
    >    ```js
    >    <!-- TodoForm.vue -->
    >    
    >    <form @submit.prevent="createTodo(todoTexst)" ref="formElem">
    >      <input type="text" v-model="todoText">
    >      <input type="submit">
    >    </form>
    >    ```
    >    
    >    ```js
    >    <!-- TodoForm.vue -->
    >    
    >    const formElem = ref(null)
    >    
    >    const createTodo = function (todoText) {
    >        store.addTodo(todoText)
    >        formElem.value.reset()
    >    }
    >    ```
  
  - Delete Todo
    
    > 1. `todos` 목록에서 특정 `todo`를 삭제하는 `deleteTodo` 액션 정의
    >    
    >    ```js
    >    // stores/counter.js
    >    
    >    const deleteTodo = function () {
    >        console.log('delete')
    >    }
    >    
    >    return { todos, addTodo, deleteTodo }
    >    ```
    > 
    > 2. 각 `todo`에 삭제 버튼을 작성
    >    
    >    - 버튼을 클릭하면 선택된 `todo`의 `id`를 인자로 전달해 `deleteTodo` 메서드 호출
    >    
    >    ```js
    >    <!-- TodoListItem.vue -->
    >    
    >    import { useCounterStore } from '@/stores/counter'
    >    
    >    const store = useCounterStore()
    >    ```
    >    
    >    ```js
    >    <!-- TodoListItem.vue -->
    >    
    >    <div>
    >      <span>{{ todo.text }}</span>
    >      <button @click="store.deleteTodo(todo.id)">Delete</button>
    >    </div>
    >    ```
    > 
    > 3. 전달받은 `todo`의 `id` 값을 활용해 선택된 `todo`의 인텍스를 구함
    >    
    >    - 특정 인덱스 `todo`를 삭제 후 `todos` 배열을 재설정
    >    
    >    ```js
    >    // stores/counter.js
    >    
    >    const deleteTodo = function(todoId) {
    >        const index = todos.value.findIndex((todo) => todo.id === todoId)
    >        todos.value.splice(index, 1)
    >    }
    >    
    >    return { todos, addTodo, deleteTodo }
    >    ```
  
  - Update Todo
    
    > - 각 `todo` 상태의 `isDone` 속성을 변경하여 `todo`의 완료 유무 처리하기
    > 
    > - 완료된 `todo` 에는 취소선 스타일 적용하기 
    > 1. `todos` 목록에서 특정 `todo`의 `isDone` 속성을 변경하는 `updateTodo` 액션 정의
    >    
    >    ```js
    >    // stores/counter.js
    >    
    >    const updateTodo = function () {
    >        console.log('update')
    >    }
    >    
    >    return { todos, addTodo, deleteTodo, updateTodo }
    >    ```
    > 
    > 2. `todo` 내용을 클릭하면 선택된 `todo`의 `id`를 인자로 전달해 `updateTodo` 메서드를 호출
    >    
    >    ```js
    >    <!-- TodoListItem.vue -->
    >    
    >    <div>
    >      <span @click="store.updateTodo(todo.id)">
    >        {{ todo.text }}
    >      </span>
    >      <button @click="store.deleteTodo(todo.id)">Delete</button>
    >    </div>
    >    ```
    > 
    > 3. 전달받은 `todo`의 `id` 값을 활용해 선택된 `todo`와 동일한 `todo`를 목록에서 검색
    >    
    >    - 일치하는 `todo` 데이터의 `isDone` 속성 값을 반대로 재할당 후 새로운 `todo` 목록 반환
    >    
    >    ```js
    >    // stores/counter.js
    >    
    >    const updateTodo = function (todoId) {
    >        todos.value = todos.value.map((todo) => {
    >          if (todo.id === todoId) {
    >            todo.isDone = !todo.isDone
    >          }
    >          return todo
    >        })
    >    }
    >    ```
    > 
    > 4. `todo` 객체의 `isDone` 속성 값에 따라 스타일 바인딩 적용하기
    >    
    >    ```js
    >    <!-- TodoListItem.vue -->
    >    
    >    <style scoped>
    >    .is-done {
    >        text-decoration: line-through;
    >    }
    >    </style>
    >    ```
    >    
    >    ```js
    >    <!-- TodoListItem.vue -->
    >    
    >    <span @click="store.updateTodo(todo.id)" :class="{ 'is-done': todo.isDone }">
    >      {{ todo.text }}
    >    </span>
    >    ```
  
  - Counting Todo (완료된 Todo 개수 계산)
    
    > 1. `todos` 배열의 길이 값을 밙환하는 함수 `doneTodosCount` 작성 (`getters`)
    >    
    >    ```js
    >    // stores/counter/js
    >    
    >    const dontTodosCount = computed(() => {
    >        const doneTodos = todos.value.filter((todo) => todo.isDone)
    >        return doneTodos.length
    >    })
    >    
    >    return { todos, addTodo, deleteTodo, updateTodo, doneTodosCount }
    >    ```
    > 
    > 2. App 컴포넌트에서 doneTodosCount getter를 참조
    >    
    >    ```js
    >    <!-- App.vue -->
    >    
    >    import { useCounterStore} from '@/stores/counter'
    >    
    >    const store = useCounterStore()
    >    ```
    >    
    >    ```js
    >    <!-- App.vue -->
    >    
    >    <div>
    >     <h1>Todo Project</h1>
    >     <h2>완료도니 Todo : {{ store.doneTodosCount }}</h2>
    >     <TodoList />
    >     <TodoForm />
    >    </div>
    >    ```

- #### Local Storage
  
  : 브라우저 내에 key-value 쌍을 저장하는 웹 스토리지 객체
  
  - 특징
    
    - 페이지를 새로고침하고 브라우저를 다시 실행해도 데이터가 유지
    
    - 쿠키와 다르게 네트워크 요청 시 서버로 전송되지 않음
    
    - 여러 탭이나 창 간에 데이터를 공유할 수 있음
  
  - 사용 목적
    
    - 웹 애플리케이션에서 사용자 설정, 상태 정보, 캐시 데이터 들을 클라이언트 측에서 보관하여 웹사이트의 성능을 향상시키고 사용자 경험을 개선하기 위함

- ###### pinia-plugin-persistedstate
  
  - Pinia의 플러그인(plugin) 중 하나
  
  - 웹 애플리케이션의 상태(`state`)를 브라우저의 `local storate`나 `session storage`에 영구적으로 젖아하고 복원하는 기능을 제공
  
  - [참고 자료](https://prazdevs.github.io/pinia-plugin-persistedstate/)
  
  - 설정
    
    > 1. 설치 및 등록
    >    
    >    `$ npm i pinia-plugin-persistedstate`
    >    
    >    ```js
    >    // main.js
    >    
    >    import piniaPluginPersistedstate from 'pinia-plugin-persistedstate'
    >    
    >    const app = create(App)
    >    const pinia = createPinia()
    >    
    >    pinia.use(piniaPluginPersistedstate)
    >    
    >    //app.use(createPinia())
    >    app.use(pinia)
    >    
    >    app.mount('#app')
    >    ```
    > 
    > 2. `defineStore()`의 3번째 인자로 관련 객체 추가
    >    
    >    ```js
    >    // stores/counter.js
    >    
    >    export const useCounterSotre = defineStore('counter', () => {
    >        ...,
    >        return { todos, addTodo, deleteTodo, updateTodo, doneTodosCount }
    >    }, { persist: true })
    >    ```
    > 
    > 3. 개발자 도구 -> Application -> Local Storage 에서 결과 확인
    >    
    >    - 브라우저의 `Local Storage`에 저장되는 `todos state` 확인

---

## 참고

#### Pinia 활용 시점

- 이제 모든 데이터를 `store`에서 관리해야 할까?
  
  - Pinia를 사용한다고 해서 모든 데이터를 `state`에 넣어야하는 것은 아님
  
  - `pass props`, `emit event`를 함께 사용하여 애플리케이션을 구성해야 함
  
  - 상황에 따라 적절하게 사용하는 것이 필요

- Pinia, 언제 사용해야 할까?
  
  - Pinia는 공유된 상태를 관리하는데 유용하지만, 구조적인 개념에 대한 이해와 시작하는 비용이 큼
  
  - 애플리케이션이 단순하다면 Pinia가 없는 것이 더 효율적일 수 있음
  
  - 그러나 중대형 규모의 SPA를 구축하는 경우 Pinia는 자연스럽게 선택할 수 있는 단계가 오게 됨
  
  - 결과적으로 적절한 상황에서 활용했을 때 Pinia 효용을 극대화할 수 있음
