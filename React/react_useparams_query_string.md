
### useParams() 란??

---

리액트에서 라우터 사용 시에 useParams 훅을 사용해서 url 의 `동적인` 파라미터 정보를 가져올 수 있다. 여기서 말하는 동적인 파라미터란? `url` 구조에서 `...url/path/1` 에서 이 1이 파라미터가 되는 것이다.

```
{
  path: "/post/:postId",
  element: <PostDetail />,
},
```
```
function UserProfile() {
  const { postId } = useParams(); // 현재 URL에서 postId 값을 가져옴
}
```

리액트의 useParams 훅으로 게시글 ID에 따라 다른 URL로 이동하는 예제로 이해해보자.

우선 라이브러리를 설치해주고,

라우터를 설정해준다.

```javascript
// App.js
import React from 'react';
import { BrowserRouter as Router, Route, Routes } from 'react-router-dom';
import PostList from './PostList';
import PostDetail from './PostDetail';

function App() {
  return (
    <Router>
      <Routes>
        <Route path="/" element={<PostList />} />
        <Route path="/post/:id" element={<PostDetail />} />
      </Routes>
    </Router>
  );
}

export default App;
```
---
```javascript
// PostList.js
import React from 'react';
import { Link } from 'react-router-dom';

function PostList() {
  const posts = [
    { id: 1, title: '게시글 1' },
    { id: 2, title: '게시글 2' },
    { id: 3, title: '게시글 3' },
  ];

  return (
    <div>
      <h1>게시글 목록</h1>
      <ul>
        {posts.map((post) => (
          <li key={post.id}>
            <Link to={`/post/${post.id}`}>{post.title}</Link>
          </li>
        ))}
      </ul>
    </div>
  );
}

export default PostList;
```

---
```javascript
// PostDetail.js
import React from 'react';
import { useParams } from 'react-router-dom';

function PostDetail() {
  const { id } = useParams();

  return (
    <div>
      <h1>게시글 상세 페이지</h1>
    </div>
  );
}

export default PostDetail;
```

### 쿼리 스트링

쿼리스트링은? URL 의 한 부분이며, 요청하고자 하는 URL에 부가적인 정보를 포함하고 싶을 때 사용한다.


기존 URL 요청 예시

>FE : /list - 리스트 요청
     /detail - 상세 페이지 요청
BE : /product - 단일 상품 응답
     /products - 상품 들 응

만약 상품의 종류가 많아질 경우 /list 페이지에서 모든 상품을 보여주는 것은 비효율적이다.

예를 들어 1억개의 상품 정보를 모두 불러오는 것도, 비효율적이지만 유저는 판매량이나 최신순 같은 기준을 두고 정렬된 데이터를 보고싶어하는 경우가 일반적이기 때문이다.

이런 상황에서 FE는 `상품 리스트 보여줘 + 최신순 10개` 와 같이 구체적인 요청을 할 수 있어야 한다.

#### 쿼리스트링의 형태

![](https://velog.velcdn.com/images/gawgjiug/post/99884c4c-74bc-4225-b044-97913bafc707/image.png)

쿼리스트링은 이름 그대로 문자열 타입이며 key = value 로 표현된다. 또한 URL의 일부로 쿼리스트링의 시작점은 `?` 으로 표시된다.

```
<Link to="/list?sort=popular" />
 
navigate("/list?sort=popular")
```

쿼리스트링을 포함해서 Routing 하는 방법은 간단히 Link 나 navigate를 이용하면 된다.

#### 페이지네이션

페이지네이션?

페이지네이션은 전체 데이터를 페이지 별로 분리해서 보여주는 UI다 페이지네이션을 구현하기 위해선 `offset` `limit` 이라는 두 가지 기준이 필요한데, offset 은 몇 번째 아이템 부터 보여줄 것인가. 를 말하는 것이고

limit 은 한 번에 몇 개를 보여줄 것인가를 말하는 것이다. 

예를들어 페이지당 10개의 아이템을 보여주는 UI를 구현하고 싶다면 

1페이지 - 0번째 이후 10개 아이템 보여줘 `offset = 0번째 이후` `limit = 10` 이 되는 것

`?offset=0&limit=10` 

물론 프로젝트 마다 offset 이 start 라던지 limit 이 size 라던지 용어는 변경될 수 있다.

#### 구현

1. URL 정보를 `useSearchParams()` 훅을 이용해서 가져온다
2. `offset` 과 `limit` 변수에 값을 저장해준다.
3. fetch에 백엔드 API 호출하는 offset 과 limit  값으로 위 두 변수 값을 넣어준다.
4. 불러온 값을 posts state에 저장한다.
5. posts 데이터를 이용해서 map 리렌더링


이 부분은 백엔드와의 협업도 필요한데, 예를 들면 API를 설계할 때 프론트에서 offset 과 limit 값을 설정해서 get 요청을 날릴 것이니, 백엔드에서도 그것을 감안해서 범위에 맞는 데이터만 반환하도록 쿼리를 작성해야한다.

```sql
SELECT * FROM posts LIMIT :limit OFFSET :offset;
```

프론트엔드에서는 페이지네이션을 구현하기 위해 현재 페이지 상태를 관리해야한다. 이때 offset 과 limit은 useState를 통해 관리할 수 있다.

```javascript
const [posts, setPosts] = useState([]);
const [total, setTotal] = useState(0);
const [currentPage, setCurrentPage] = useState(1);
const limit = 10;

useEffect(() => {
    const offset = (currentPage - 1) * limit;
    fetch(`/api/posts?offset=${offset}&limit=${limit}`)
        .then(response => response.json())
        .then(data => {
            setPosts(data.data);
            setTotal(data.total);
        });
}, [currentPage]);
```

