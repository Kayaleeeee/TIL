# NextJS에 Mobx 세팅하기

[목표]

- Next JS프로젝트에 Mobx환경을 설치할 수 있다.
- MobX를 이용해 global 상태를 관리할 수 있다.

<br /><br /> 


## 왜 Mobx를 사용 하는가?
 - Mobx는 리액트 프로젝트에서 Global State Management를 위해 사용하는 라이브러리다. <br>
 - 이를 통해 프론트 단에서 페이지간 이동에 관계없는 글로벌 변수 / 함수를 사용할 수 있다. <br>
 - 나는 E-commerce 플랫폼을 만들면서 쇼핑카트 구현과 & 로그인 시 Nav bar에 유저 정보를 띄워주기 위해 사용했다.



<br /><br />

### 1. Mobx 설치하기

[mobx-react]

```javascript
 npm install mobx-react --save

 //or

 yarn add mobx mobx-react
```

[babel] : 데코레이터 사용을 위한 babel설치 

```javascript
npm install @babel/plugin-proposal-class-properties @babel/plugin-proposal-decorators


//or

yarn add @babel/plugin-proposal-class-properties @babel/plugin-proposal-decorators
```


<br />

### 2. .babelrc 생성

-  Next js 프로젝트에서 바벨 사용을 위해, 프로젝트 최상단에 .babelrc 생성


````js

// .babalrc

{
    "presets": ["next/babel"],
    "plugins": [
        [
            "@babel/plugin-proposal-decorators",
            {
                "legacy": true
            }
        ],
        [
            "@babel/plugin-proposal-class-properties",
            {
                "loose": true
            }
        ]
    ]
}

````



<br />

### 3. _app.js에 mobx Provider 세팅해주기

- 모든 컴포넌트들의 최상위 페이지인 app.js에서 
`<Provider></Provider>` 태그로 컴포넌트들을 감싸줌
- 이 태그로 하위 컴포넌트들 모두에 mobx store inject 가능
- 여기서는 Store class의 useStore함수를 통해 초기 스토어 값을 하위 컴포넌트에게 넘겨줌
- **UseStore** 동작 순서: 
    _app.js useStore() 실행 => InitializeStore()을 통해 InitalState 전달 => 서버 / 클라이언트 환경에 따라 InitalState를 받은 (Hydrate된) new Store 생성


<br />

````js

///_app.js

import { Provider } from "mobx-react";
import { useStore } from "../stores/Store";

function MyApp({ Component, pageProps }) {
    const store = useStore(pageProps);
    return (
        <div className="page__wrap">
                <Provider store={store}>
                    <Component {...pageProps} />
                </Provider>
            </div>

        </div>
    );
}

````

<br />




````js
// /stores/Store.js

import { action, observable, makeObservable } from "mobx";
import { enableStaticRendering, MobXProviderContext } from "mobx-react";
import { useMemo, useContext } from "react";

import CartStore from "./CartStore";
import UserStore from "./UserStore";

// eslint-disable-next-line react-hooks/rules-of-hooks
enableStaticRendering(typeof window === "undefined");

let store = null;

const isServer = typeof window === "undefined";

class Store {
    cartStore = new CartStore();
    userStore = new UserStore();

    constructor(props) {
        makeObservable(this);
        this.hydrate(props);
    }

    @action
    hydrate = data => {
        if (data) {
            this.userStore = new UserStore(data);
        }
    };
}

function initializeStore(initialData) {
    // if _store is null or undifined return new Store
    //otherwise use _store

    // If your page has Next.js data fetching methods that use a Mobx store, it will
    // get hydrated here, check `pages/ssg.js` and `pages/ssr.js` for more details


    // For SSG and SSR always create a new store
    if (isServer) {
        return {
            cartStore: new CartStore(),
            userStore: new UserStore(initialData),
        };
    }

    // Create the store once in the client
    if (store === null) {
        store = {
            cartStore: new CartStore(),
            userStore: new UserStore(initialData),
        };
    }

    return store;
}

export function useStore(initialState) {
    const store = useMemo(() => initializeStore(initialState), [initialState]);
    return store;
}
````




<br />
<br />


[참고 문서]
- [https://github.com/vercel/next.js/tree/canary/examples/with-mobx](https://github.com/vercel/next.js/tree/canary/examples/with-mobx)

- Mobx Next Js 
[https://www.themikelewis.com/post/nextjs-with-mobx](https://www.themikelewis.com/post/nextjs-with-mobx)

- Mobx Github [https://github.com/mobxjs/mobx-react#the-set-of-provided-stores-has-changed-error](https://github.com/mobxjs/mobx-react#the-set-of-provided-stores-has-changed-error).

- Vercel Mobx [https://github.com/vercel/next.js/blob/canary/examples/with-mobx/store.js](https://github.com/vercel/next.js/blob/canary/examples/with-mobx/store.js)