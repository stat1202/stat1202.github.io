---
categories: [프론트엔드, Tanstack-query]
tags: [리액트 쿼리, react-query, 탄스택 쿼리, tanstack-query]
---

# Tanstack-query
`react-query`, `tanstack-query` 프론트엔드 공부를 하다보면 한번쯤은 들어봤을 라이브러리이다.

간단하게 설명하자면 서버에서 데이터를 받을 때 서버 상태를 관리하기 위해 만들어진 라이브러리이다.

`tanstack-query`는 다음과 같은 기능을 제공한다.

- 캐싱
- 동일한 데이터 중복 요청 단일 요청으로 통합
- 백그라운드에서 오래된 데이터 업데이트
- 데이터 얼마나 되었는지 확인 가능
- 데이터 업데이트 빠르게 반영
- 페이지네이션 및 데이터 지연로드에서 성능 최적화
- 서버 상태 메모리 및 가비지 수집 관리
- 구조 공유를사용해 쿼리 결과 메모화


## 사용하기

### GET

서버에서 데이터를 가져오기 위해서는 `useQuery`를 사용하면된다.

```js
import { useQuery } from '@tanstack/react-query';
```

`useQuery`에는 두 가지 요소를 전달해야한다. 하나는 `queryKey`이고, 다른 하나는 `queryFn`이다.

`queryKey`는 데이터를 고유하게 식별하기 위한 key이고,
`queryFn`은 호출하고자 하는 함수를 넣어주면 된다.

```js
function Todos() {
  const { isPending, isError, data, error } = useQuery({
    queryKey: ['todos'],
    queryFn: fetchTodoList,
  })

  if (isPending) {
    return <span>Loading...</span>
  }

  if (isError) {
    return <span>Error: {error.message}</span>
  }

  // We can assume by this point that `isSuccess === true`
  return (
    <ul>
      {data.map((todo) => (
        <li key={todo.id}>{todo.title}</li>
      ))}
    </ul>
  )
}

```
`tanstack-query`의 공식문서에서 나와있는 예시이다.

이렇게 인자를 넘겨준다면 `isPending`, `isError`, `data`, `error`를 반환해준다.


### POST, PATCH, PUT, DELETE

데이터를 GET하는 것 외에는 useMutation을 사용하면 된다.

먼저, `useMutation`을 `import` 한다.

```js
import { useMutation } from '@tanstack/react-query';
```

```js
useMutation({
  mutationFn: addTodo,
  onMutate: (variables) => {
    // A mutation is about to happen!

    // Optionally return a context containing data to use when for example rolling back
    return { id: 1 }
  },
  onError: (error, variables, context) => {
    // An error happened!
    console.log(`rolling back optimistic update with id ${context.id}`)
  },
  onSuccess: (data, variables, context) => {
    // Boom baby!
  },
  onSettled: (data, error, variables, context) => {
    // Error or success... doesn't matter!
  },
})
```
다음과 같이 사용할 수 있다.
`onMutate`는  mutation 함수가 실행되기 전,
`onSettled`는 요청의 성공, 실패 상관없이 마지막에 실행된다.

우선 간단하게 `tanstack-query`에 대해 알아보았다. 다음에는 이를 실제로 어떻게 적용할 수 있을지 써보려고 한다.