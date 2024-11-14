# Vue with DRF

<목록>

- [기본적인 요청과 응답 프로젝트 사전 준비](#기본적인-요청과-응답-프로젝트-사전-준비)

- 메인 페이지 구현
  
  - [게시글 목록 출력](#게시글-목록-출력)
  
  - [DRF와의 요청과 응답](#drf와의-요청과-응답)

- [CORS Policy](#cors-policy)
  
  - [CORS Headers 설정](#cors-headers-설정)

- Article CR 구현
  
  - [전체 게시글 목록 저장 및 출력](#전체-게시글-목록-저장-및-출력)
  
  - [단일 게시글 데이터 출력](#단일-게시글-데이터-출력)
  
  - [게시글 작성](#게시글-작성)

---

### 기본적인 요청과 응답 프로젝트 사전 준비

- DRF 프로젝트
  
  1. 가상 환경 생성 및 활성화
     
     `$ python -m venv venv`
     
     `$ source venv/Scripts/activate`
  
  2. 패키지 설치
     
     `$ pip install -r requirements.txt`
  
  3. Migration 진행
     
     `$python manage.py makemigrations`
     
     `$ python manage.py migrate`
  
  4. Fixtures 데이터 로드
     
     `$ python manage.py loaddata articles.json`

- Vue 프로젝트
  
  1. 패키지 설치
     
     `$ npm install`
  
  2. 서버 실행
     
     `$ npm run dev`

---

### 게시글 목록 출력

1. `ArticleView`의 `route` 관련 코드 주석 해제
   
   ```javascript
   // router/index.js
   import ArticleView from '@/views/ArticleView.vue'
   const router = createRouter({
     history: createWebHistory(import.meta.env.BASE_URL),
     routes: [
       {
         path: '/',
         name: 'ArticleView',
         component: ArticleView
       },
     ]
   })
   ```

2. `App` 컴포넌트에 `ArticleView` 컴포넌트로 이동하는 `RouterLink` 작성
   
   ```html
   <!-- App.vue -->
   <template>
     <header>
       <nav>
         <RouterLink :to="{ name: 'ArticleView' }">Articles</RouterLink>
       </nav>
     </header>
     <RouterView />
   </template>
   
   <script setup>
   import { RouterView, RouterLink } from 'vue-router'
   </script>
   ```

3. `ArticleView` 컴포넌트에 `ArticleList` 컴포넌트 등록
   
   ```html
   <!-- views/ArticleView.vue -->
   <template>
     <div>
       <h1>Article Page</h1>
     </div>
   </template>
   
   <script setup>
   import ArticleList from '@/components/ARticleList.vue'
   </script>
   ```

4. `store`에 임시 데이터 `articles` 배열 작성하기
   
   ```javascript
   // store/counter.js
   export const useCounterStore = defineStore('counter', () => {
     const articles = ref([
       { id: 1, title: 'Article 1', content: 'Content of article 1' },
       { id: 2, title: 'Article 2', content: 'Content of article 2' }
     ])
     return { articles }
   }, { persist: true })
   ```

5. `ArticleList` 컴포넌트에서 게시글 목록 출력
   
   - `store`의 `articles` 데이터 참조
   
   - `v-for`를 활용하여 하위 컴포넌트에서 사용할 `article` 단일 객체 정보를 `props`로 전달
   
   ```html
   <!-- components/ArticleList.vue -->
   <template>
     <div>
       <h3>Article List</h3>
       <ArticleListItem
         v-for="article in store.articles"
         :key="article.id"
         :article="article"
       />
     </div>
   </template>
   
   <script setup>
   import { useCounterStore } from '@/stores/counter'
   import ArticleListItem from '@/components/ArticleListItem.vue'
   const store = userCounterStore()
   </script>
   ```

6. `ArticleListItem` 컴포넌트는 내려 받은 `porps`를 정의 후 출력
   
   ```html
   <!-- components/ArticleListItem.vue -->
   <template>
     <div>
       <h5>{{ article.id }}</h5>
       <p>{{ article.title }}</p>
       <p>{{ article.content }}</p>
       <hr>
     </div>
   </template>
   
   <script setup>
   defineProps({
     article: Object
   })
   </script>
   ```

7. 메인 페이지에서 게시글 목록 출력 확인

---

### DRF와의 요청과 응답

1. DRF 서버로의 AJAX 요청을 위한 `axios` 설치 및 관련 코드 작성
   
   `$ npm install axios`
   
   ```javascript
   // store/counter.js
   import { ref, computed } from 'vue'
   import { defineStore } from 'pinia'
   import axios from 'axios'
   
   export const useCounterStore = defineStore('counter', () => {
     const articles = ref([])
     const API_URL = 'http://127.0.0.1:8000'
   }, {persist: true })
   ```

2. DRF 서버로 요청을 보내고 응답 데이터를 처리하는 `getArticles` 함수 작성
   
   ```javascript
   // store/counter.js
   export const useCounterStore = defineStore('counter', () => {
     const getArticles = function () {
       axios({
         method: 'get',
         url: `${API_URL}/api/v1/articles/`
       })
       .catch(err => console.log(err))
     }
     return { articles. API_URL, getArticles }
   }, {persist: true })
   ```

3. `ArticleView` 컴포넌트가 마운트 될 때 `getArticles` 함수가 실행되도록 함
   
   - 해당 컴포넌트가 렌더링 될 때 항상 최신 게시글 목록을 불러오기 위함
   
   ```html
   <!-- views/ArticleView.vue -->
   <script setup>
   import { onMounted } from 'vue'
   import { useCounterStore } from '@/stores/counter'
   import ArticleList from '@/components/ArticleList.vue'
   
   const store = useCounterStore()
   onMounted(() => {
     store.getArticles()
   })
   </script>
   ```

4. Vue와 DRF 서버를 모두 실행한 후 응답 데이터 확인
   
   - 에러 발생
   
   - 그런데 DRF 서버 측에서는 문제 없이 응답했음
   
   - 서버는 응답했으나 브라우저 측에서 거절한 것
   
   - <mark>**브라우저가 거절한 이유**</mark>
     
     - `localhost:5173`에서 `127.0.0.1:8000/api/v1/articles/`의 `XMLHttpRequest`에 대한 접근이 `CORS policy`에 의해 차단됨 

---

### CORS Policy

- **SOP (Same-origin policy)**
  
  : 동일 출처 정책
  
  - 어떤 출처(Origin)에서 불러온 문서나 스크립트가 다른 출처에서 가져온 리소스와 상호 작용하는 것을 제한하는 보안 방식
  
  - "다른 곳에서 가져온 자료는 일단 막는다."
  
  - 웹 애플리케이션의 도메인이 다른 도메인의 리소스에 접근하는 것을 제어하여 사용자의 개인 정보와 데이터의 보안을 보호하고, 잠재적인 보안 위협을 방지
  
  - 잠재적으로 해로울 수 있는 문서를 분리함으로써 공격받을 수 있는 경로를 줄임

- **Origin (출처)**
  
  : `URL` 의 `Protocol`, `Host`, `Port`를 모두 포함하여 "출처"라고 부름
  
  - 아래 **세 영역이 일치하는 경우에만 동일 출처(Same-origin)** 로 인정
    
    ![image](https://github.com/user-attachments/assets/d74b03d8-9fcb-4518-93c4-2bdce469105f)

- **CORS (Cross-Origin Resource Sharing)**
  
  : 교차 출처 리소스 공유
  
  - 특정 출처에서 실행 중인 웹 애플리케이션이 다른 출처의 자원에 접근할 수 있는 권한을 부여하도록 브라우저에 알려주는 체제
  
  - 만약 다른 출처의 리소스를 가져오기 위해서는 이를 제공하는 서버가 브라우저에게 다른 출처지만 접근해도 된다는 사실을 알려야 함
  
  > CORS 적용 방법
  > 
  > 1. 도메인 B 왈 : 브라우저님, 저희 응답 HTTP Header에 "Access-Control-Allow-Origin: 도메인A"가 포함되었습니다. 이제 도메인A에서의 요청은 우리 자원에 접근할 수 있음을 알려드립니다.
  > 
  > 2. 도메인 A 왈 : 이제 저희 웹 애플리케이션은 도메인B의 데이터를 안전하게 사용할 수 있겠군요.

- **CORS Policy (Cross-Origin Resource Sharing Policy)**
  
  : 교차 출처 리소스 공유 정책
  
  - 다른 출처에서 온 리소스를 공유하는 것에 대한 정책
  
  - 서버에서 설정되며, 브라우저가 해당 정책을 확인하여 요청이 허용되는지 여부를 결정
  
  - 다른 출처의 리소스를 불러오려면 그 다른 출처에서 올바른 **CORS header를 포함한 응답을 반환**해야 함
  
  > CORS policy의 등장
  > 
  > - 기본적으로 웹 브라우저는 같은 출처에서만 요청하는 것을 허용하며, 다른 출처로의 요청은 보안상의 이유로 차단됨
  > 
  > - 하지만 현대 웹 애플리케이션은 다양한 출처로부터 리소스를 요청하는 경우가 많기 때문에 CORS 정책이 필요하게 되었음
  > 
  > - **CORS**는 웹 서버가 리소스에 대한 서로 다른 출처 간 접근을 허용하도록 선택할 수 있는 기능을 제공

---

### CORS Headers 설정

- django-cors-headers 사용하기
  
  1. 설치 (`requirements.txt`로 인해 사전에 설치되어 있음)
     
     `$ pip install django-cors-headers`
  
  2. 관련 코드 주석 해체
     
     ```python
     # settings.py
     INSTALLED_APPS = [
         ...
         'corsheaders',
         ...
     ]
     
     MIDDLEWARE = [
         ...
         'corsheaders.middleware.CorsMiddleware',
         'django.middleware.common.CommonMiddleware',
         ...
     ]
     ```
  
  3. CORS를 허용한 Vue 프로젝트의 Domain 등록
     
     ```python
     # settings.py
     CORS_ALLOWED_ORIGINS = [
         'http://127.0.0.1:5173',
         'http://localhost:5173',
     ]
     ```

- CORS 처리 결과
  
  1. 메인 페이지에서 DRF 응답 데이터 재확인
  
  2. 응답 객체에서 `Access-Control-Allow-Origin` Header 확인
     
     - 개발자도구 - NEtwork = Fetch/XHR

---

### 전체 게시글 목록 저장 및 출력

1. 응답 답은 데이터에서 각 게시글의 데이터 구성 확인 (`id`, `title`, `content`)

2. `store`에 게시글 목록 데이터 저장
   
   ```javascript
   // store/counter.js
   export const useCounterStore = defineStore('counter', () => {
     ...
     const getArticles = funtion () {
       axios({
         method: 'get',
         url: `${API_URL}/api/v1/articles/`
       })
         .then(res => {
           articles.value = res.data
         })
         .catch(err => console.log(err))
     }
     return { articles. getArticles }
   }, { persist: true })
   ```

3. `store`에 저장된 게시글 목록 출력 확인
   
   - `pinia-plugin-persistedstate`에 의해 브라우저 `Local Storage`에 저장됨

---

### 단일 게시글 데이터 출력

1. `DetailVue` 관련 `route` 주석 해제
   
   ```javascript
   // router/index.js
   import DetailView from '@/view/DetailView.vue'
   const router = createRouter({
     history: createWebHistory(import.meta.env.BASE_URL),
     routes: [
       {
         path: '/',
         name: 'ArticleView',
         component: ArticleView
       },
       {
         path: 'Articles/:id',
         name: 'DetailView',
         component: DetailView
       }...
   ]
   })
   ```

2. `ArticleListItem`에 `DetailVie` 컴포넌트로 가기 위한 `RouterLink` 작성
   
   ```html
   <!-- components/ArticleListItem.vue -->
   <template>
     <div>
       <h5>{{ article.id }}</h5>
       <p>{{ article.title }}</p>
       <p>{{ article.content }}</p>
       <RouterLink :to="{ name: 'DetailView', params: { id: article.id } }">
         [DETAIL]
       </RouterLink>
       <hr>
     </div>
   </template>
   
   <script setup>
   import { RouterLink } from 'vue-router'
   ...
   </script>
   ```

3. `DetailView`가 마운트될 때 특정 게시글을 조회하는 AJAX 요청 진행 (아래에서 재수정)
   
   ```javascript
   // views/DetailView.vue
   import axios from 'axios'
   import { onMounted } from 'vue'
   import { userRoute } from 'vue-router'
   import { userCounterStore } from '@/stores/counter'
   
   const store = useCounterSotre()
   const route = useRoute()
   
   onMounted(() => {
     axios({
       method: 'get',
       url: `${store.API_URL}/api/v1/articles/${route.params.id}/`,
     })
       .then((res) => {
         console.log(res.data)
       })
       .catch(err => console.log(err))
   })
   ```

4. 응답 데이터 확인

5. 응답 데이터 저장 후 출력
   
   ```html
   <!-- views/DetailView.vue -->
   <template>
     <div>
       <h1>Detail</h1>
       <div v-if="article">
         <p>글 번호 : {{ article.id }}</p>
         <p>제목 : {{ article.title }}</p>
         <p>내용 : {{ article.content }}</p>
         <p>작성시간 : {{ article.created_at }}</p>
         <p>수정시간 : {{ articleupdated_at }}</p>
       </div>
     </div>
   </template>
   ```
   
   ```java
   // views/DetailView.vue
   import axios from 'axios'
   import { onMounted, ref } from 'vue'
   import { userRoute } from 'vue-router'
   import { userCounterStore } from '@/stores/counter'
   
   const store = useCounterSotre()
   const route = useRoute()
   const article = ref(null)
   
   onMounted(() => {
     axios({
       method: 'get',
       url: `${store.API_URL}/api/v1/articles/${route.params.id}/`,
     })
       .then((res) => {
         article.value = res.data
       })
       .catch(err => console.log(err))
   })
   ```

6. 결과 확인

---

### 게시글 작성

1. `CrateView` 관련 `route` 주석 해제
   
   ```javascript
   // router/index.js
   import CreateView from '@/views/CreateView.vue'
   const router = createRouter({
     history: createWebHistory(import.meta.env.BASE_URL),
     routes: [
       ...
       {
         path: '/create',
         name: 'CreateView',
         component: CreateView
       }
   ]
   })
   ```

2. `ArticleView`에 `CreateView` 컴포넌트로 가기 위한 `RouterLink` 작성
   
   ```html
   <!-- views/ArticleView.vue -->
   <template>
     <div>
       <h1>Article Page</h1>
       <RouterLink :to="{ name: 'CreateView' }">
         [CREATE]
       </RouterLink>
       <hr>
       <ArticleList />
     </div>
   </template>
   
   <script setup>
   import { onMounted } from 'vue'
   import { userCounterStore } from '@/stores.counter'
   import { RouterLink } from 'vue-router'
   import ArticleList from '@components/ArticleList.vue'
   </script>
   ```

3. `v-modle`을 사용해 사용자 입력 데이터를 양방향 바인딩 (아래에서 재수정)
   
   `v-model`의 `trim` 수식어를 사용해 사용자 입력 데이터의 공백을 제거
   
   ```html
   <!-- views/CreateView.vue -->
   <template>
     <div>
       <h1>게시글 작성</h1>
       <form>
         <label for="title">제목 : </label>
         <input type="text" id="title" v-model.trim="title"><br>
         <label for="content">내용 : </label>
         <textarea id="content" v-model.trim="content"></textarea><br>
         <input type="submit">
       </form>
     </div>
   </template>
   
   <script setup>
   import { ref } from 'vue'
   const title = ref(null)
   const content = ref(null)
   </script>
   ```

4. 양방향 바인딩 데이터 입력 확인

5. 게시글 생성 요청을 담당하는 `createArticle` 함수 작성
   
   게시글 생성이 성공한다면 `ArticleView` 컴포넌트로 이동
   
   ```javascript
   // views/CreateView.vue
   import axios from 'axios'
   import { useCounterStore } from '@/stores/counter'
   import { useRouter } from 'vue-router'
   
   const store = useCounterStore()
   const router = useRouter()
   const createArticle = funtion () {
     axios({
       method: 'post'
       url: `${store.API_URL}/api/v1/articles/`,
       data: {
         title: title.value,
         content: content.value
       },
     }).then(() => {
         router.push({ name: 'ArticleView' })
     }).catech(err => console.log(err))
   }
   ```

6. `submit` 이벤트가 발생하면 `createArticle` 함수를 호출
   
   `v-on`의 `prvent` 수식어를 사용해 submit 이벤트의 기본 동작 취소
   
   ```html
   <!-- views/CreateView.vue -->
   <template>
     <div>
       <h1>게시글 작성</h1>
       <form @submit.prevent="createArticle">
         <label for="title">제목 : </label>
         <input type="text" id="title" v-model.trim"title"><br>
         <label for="content">내용 : </label>
         <textarea id="content" v-model.trim="content"></textarea><br>
         <input type="submit">
       </form>
     </div>
   </template>
   ```

7. 게시글 생성 결과 확인

8. 서버 측 DB 확인

---
