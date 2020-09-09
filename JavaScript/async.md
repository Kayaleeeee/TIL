# Async Function

> - Algorithm Visualizaion Project에서 자바스크립트로 약간 시간 차를 두고 변하는 효과를 주고싶어서 처음 사용해봄 ✨
> - sort 함수 만들 때, 서로 값이 교차하는 시점에서 대기 시간을 늘려서 더 눈에 띄게 해줌
>
> - [MDN 사이트 보기](https://wiki.developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Statements/async_function)
> - [Algorithm Visualizaion Project 보기](https://github.com/Kayaleeeee/algorithm_study)

<br/><br/>

### ⚓️ async function + await

> - await로 비동기로 액션을 취하려면, 함수가 async로 설정되어 있어야 한다.
> - async없이 await함수를 사용하면 에러남
> - 함수 실행 대기시간은 1000 = 1초 / 2000 = 2초

 <br/>

#### async function 작성법

```javascript
async function functionName(params) {
  statements;
}
```

- **await** : await 구문의 이벤트를 실행하는 동안 async 함수 내의 동작은 잠시 대기함
- **Promise** : async 함수에 의해 반환 된 값으로 해결되거나 async함수 내에서 발생하는 캐치되지 않는 예외로 거부되는 값

<br/><br/>

#### 사용 예시 (MDN 사이트 참조)

```javascript
//2초 후 실행되는 함수
function resolveAfter2Seconds() {
  return new Promise((resolve) => {
    setTimeout(() => {
      resolve("resolved");
    }, 2000);
  });
}

// 먼저 실행되는 함수
async function asyncCall() {
  console.log("calling");
  const result = await resolveAfter2Seconds();
  console.log(result);
}

asyncCall();
// 'calling'
// 'resolved'
```

#### 사용예시 (프로젝트 잠조)

> function이 끝나자마자 alert창이 뜨는게 싫어서 300정도 대기시간을 걸어놓음

```javascript
async function onSortNums() {
  await new Promise((resolve) =>
    setTimeout(() => {
      alert("정렬이 완료되었습니다.");
    }, 300)
  );
}
```
