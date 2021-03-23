---
layout: post
title: useCallback & useMemo
nav_order: 87
last_modified_date: 2021-03-23 22:18
lastmod: 2021-03-23 22:18
---

# **리액트 hooks: useCallback & useMemo 사용**
## Intro
회사에서 React를 사용하는데, 예전부터 써왔던거라 컴포넌트들이 대부분 Class Component였다. React Hooks가 나온 이후로 여러가지 사용하기 간편한 점이 많아서 최근에 개발하고 개선하는 작업들에서 Class에서 Functional Component로 바꾸는 작업을 하고 있다.

이 작업을 하면서 `useEffect`, `useState` 그리고 redux를 사용하기 때문에 `useSelector`, `useDispatch` 등을 이용해서 Class Component보다 더 직관적이게 컴포넌트를 제어하고 개발할 수 있게 되었다. 그런데 이상하게 그 외의 다양한 기능(`useCallback`, `useMemo`)에 대해서는 아직 낯설어서 잘 사용하지 못했다.

그러던 중에 코드리뷰에서 `useCallback`을 사용하라는 리뷰를 받았다. 그렇게 해야할 상황이 오니 공부하게 되었다🤣

* * *

## useCallback
```typescript
const memoizedCallback = useCallback(
  () => {
    doSomething(a, b);
  },
  [a, b]
)
```
* 메모이제이션된 <span class='text-purple-000'>콜백</span>을 반환한다. 즉, **콜백함수**를 반환하는 것이다. 
* 불필요한 렌더링을 방지한다.
* 콜백의 의존성이 변경되었을 때만 변경된다. (여기서는 a, b)

## useMemo
```typescript
  const memoizedValue = useMemo(() => computeExpensiveValue(a, b), [a, b])
```
* 메모이제이션된 <span class='text-purple-000'>값</span>을 반환한다. 즉, **특정 값**을 반환하는 것이다.
* 모든 렌더링 시의 고비용 계산을 방지하게 해준다.
* 의존성의 변경이 일어났을 때만 변경된다. (여기서는 a, b)

## useMemo vs. useCallback
사실 두개의 차이점은 콜백을 반환하냐, 값을 반환하냐의 차이다.
공식 문서에서는 미묘하게 다르게 설명되어있지만, 내가 이해하는 바는 불필요한 렌더링을 최소화 시키면서 효율적으로 함수나 값을 가져올 수 있게 해주는 기능인 이라는 것이다.
내 머릿속에서 정리한 바로는 이렇다.
1. useMemo: 어떤 값을 화면에 표시하거나 하위 컴포넌트에게 전달해줄 때, 그 값을 (의존성이 변경되기 전까지) 유지하기 위함
2. useCallback: 어떤 함수를 하위 컴포넌트나 어딘가에게 전달해야할 때, 그 콜백함수를 (의존성이 변경되기 전까지) 유지하기 위함

사실, 함수를 어딘가에 전달해주어야 할 일이 뭐가있을지는 추측이 안간다. 아직 useMemo만 사용해봐서 그런지 보통 어떤 함수를 통해 연산된 값을 어딘가에 사용하지, 콜백함수 자체를 사용할 일이 있나...?! 싶다.

* * *

아무튼 그래서 내가 회사에서 만난 문제를 만나보자.

## 내가 적용한 예제
여기서 회사 코드를 해부할 수가 없으니, 간단한 비유를 통해 어떤 상황이었는지 살펴보자.
```tsx
const MyComponent: React.FC<PropsType> = ({ yMin, yMax, xMin, xMax, toggle }: PropsType) => {
  const [xavg, setXavg] = React.setState(getAverage(xMin, xMax))
  const [yavg, setYavg] = React.setState(getAverage(yMin, yMax))

  React.useEffect(() => {
    setXavg(getAverage(xMin, xMax, toogle))
  }, [xMin, xMax])

  React.useEffect(() => {
    setYavg(getAverage(yMin, yMax, toogle))
  }, [yMin, yMax])

  return (
    <div>
      <span>x의 평균은 ${xavg}</span>
      <span>y의 평균은 ${yavg}</span>
      <span>제곱의 평균여부: ${toogle}</span>
    </div>
  )
}
function getAverage(min: number, max: number, toggle: boolean) {
  if (toogle) (Math.pow(min,2)+Math.pow(max,2))/2

  return (min+max)/2
}
```
아주 간단한 예제이다. props를 통해 받은 값을 가지고 평균을 화면에 나타내주는 간단한 컴포넌트가 있다고 가정하자.
그런데 이때 내가 하고싶었던 것은 `getAverage`라는 **공통함수를 만들고 싶었다.** 똑같이 그냥 써주면 되지만, 한번 쓰면, 지금 상황에서는 4군데에서 공통적으로 사용할 수 있기 때문이었다.

여기서는 내가 `xavg`가 xMin, xMax가 바뀌기 전까지는 유지되는 <span class='text-purple-000'>값</span>이므로 `useMemo`를 통해서 간단하게 해결해줄 수 있었다.
```typescript
const xavg = React.useMemo(() => (toogle)? (Math.pow(xMin,2)+Math.pow(xMax,2))/2 : (xMin+xMax)/2, [xMin, xMax, toogle])
const yavg = React.useMemo(() => (toogle)? (Math.pow(yMin,2)+Math.pow(yMax,2))/2 : (yMin+yMax)/2, [yMin, yMax, toogle])
```
이렇게 사용하게 되면, 두 군데(`useState`, `useEffect`)에서 사용되었던 것을 useMemo로 한번만 사용해도 되고 모든 렌더링시에 이 공통 함수가 계속 계산하는 불필요한 연산도 줄일 수 있게 되었다.

간단한 예제라서 두 함수가 같은 연산으로 사용되었는데, 실제 현업에서 해결하려고 했던 문제는 두 함수가 미묘하게 다른 연산을 가지고 있었다. (뭔가 원래의 상황과 딱 맞는 예제가 아니라.. 좀 아쉽다. ~~나래기.. 왜이렇게 설명을 못하냐ㅠ~~)

암튼 이 예제의 요지는 컴포넌트 외부에 계산에 필요한 '공통함수'를 만들어 처리하던 것을 `useMemo`를 이용해서 값을 저장하고 렌더링할 때마다 불필요하게 값을 계산하지 않아도 되게끔 했다는 점이다.

* * *

그런데 마음에 걸리는 것은 공식 문서에
> <span class='text-purple-000'>통상적으로 렌더링 중에는 하지 않는 것을 이 함수 내에서 하지 마세요. 예를 들어, 사이드 이펙트(side effect)는 useEffect에서 하는 일이지 useMemo에서 하는 일이 아닙니다.<span>

이렇게 적혀있는데, 내가 작업한 내용 자체만 보면 `useState`와 `useEffect`에서 하는 일을 `useMemo`로 바꾼 것이기 때문에.. 공식문서에서 하지 말란 일을 한건가 싶기도 하다....🤔

일단, 다시 리뷰요청을 동료분께 드렸으니.. 이제 또 호되게(?) 혼날지 아닐지는 두고봐야겠다(이랬는데, 이렇게 쓰면 안된다고 하시면 어쩌지.... 그럼 바로 이 글은 삭제해야겠다. 하핳)


아직 많이 부족하고 공부할 것은 많다. (틀린게 있거나, 의견이 있으시면 언제든지 댓글 달아주세요.)