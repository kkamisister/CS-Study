# 📚 CHAPTER 1. 디자인 패턴과 프로그래밍 패러다임

## 📖 SECTION 1.1 디자인 패턴

> **라이브러리**  
> 공통으로 사용될 수 있는 특정한 기능들을 모듈화한 것

> **프레임워크**  
> 공통으로 사용될 수 있는 특정한 기능들을 모듈화한 것. 라이브러리에 비해 규칙이 좀 더 엄격함.

> **디자인 패턴**  
> 프로그램을 설계할 때 발생했던 문제점들을 객체 간의 상호 관계 등을 이용하여 해결할
> 수 있도록 하나의 '규약' 형태로 만들어 놓은 것

#### ⭐ 싱글톤 패턴 (Singleton Pattern)

- **하나의 클래스**에 오직 **하나의 인스턴스**만 가지는 패턴
- 보통 **데이터베이스 연결 모듈**에 많이 사용됨
- 하나의 인스턴스를 만들어 놓고 해당 인스턴스를 다른 모듈들이 공유하며 사용하기 떄문에 **인스턴스를 생성할 때 드는 비용이 줄어드는** 장점
- **의존성이 높아진다**는 단점

```javascript
const obj = {
  a: 27,
}
const obj2 = {
  a: 27,
}
console.log(obj === obj2)
// false
```

→ **obj와 obj2는 다른 인스턴스를 가짐**
new Object라는 클래스에서 나온 단 하나의 인스턴스라서 어느 정도 싱글톤 패턴이라 볼 수 있음

```javascript
class Singleton {
  constructor() {
    if (!Singleton.instance) {
      Singleton.instance = this
    }
    return Singleton.instance
  }
  getInstance() {
    return this
  }
}
const a = new Singleton()
const b = new Singleton()
console.log(a === b) // true
```

→ **a와 b는 하나의 인스턴스를 가진다**
Singleton.instance라는 하나의 인스턴스를 가지는 Singleton 클래스

#### 📌싱글톤 패턴의 단점

- TDD(Test Driven Dvelopment)를 할 때 걸림돌이 됨

  > **TDD** > **테스트 주도 개발.**
  > 코드를 작성하기 전에 테스트 코드부터 먼저 작성하는 개발 방식
  > 왜?? -> **버그 방지 + 설계에 도움**(테스트 먼저 쓰면 기능 단위로 생각하게 됨)

- 싱글톤 패턴은 미리 생성된 하나의 인스턴스를 기반으로 구현하는 패턴
- TDD를 할 때 단위 테스트를 주로 하는데, 단위 테스트는 테스트가 서로 독립적이어야 함
- \+ 테스트를 어떤 순서로든 실행할 수 있어야 함
- 싱글톤 패턴은 각 테스트마다 '독립적인' 인스턴스를 만들기가 어려움

---

#### ⭐ 의존성 주입

- 싱글톤 패턴은 사용하기가 쉽고 굉장히 **실용적**이라는 장점
- 하지만 **모듈 간의 결합**을 강하게 만들 수 있다는 단점
- **의존성 주입**을 통해 모듈 간의 결합을 조금 더 느슨하게 만들어 해결할 수 있다
- 의존성 주입자가 메인 모듈과 하위 모듈의 중간 부분을 가로채, 메인 모듈이 '간접'적으로 의존성을 주입하는 방식
  > **결국 의존성 주입자에 의존하게 되는 것이 아닌가???**
  > 의존성 주입자를 도입하면, 의존성 주입자에게 **'구성을 위임'** 하게 됨
  > 하지만 이는 "강한 결합을 느슨한 결합으로 바꾸는 데 유리한 트레이드오프"
  > **역할 분리 + 테스트 용이성 증가**

#### 📌의존성 주입의 장점

- 모듈을 쉽게 교체할 수 있는 구조가 되어 테스팅하기 쉽다
- **추상화 레이어**를 넣고 이를 기반으로 **구현체**를 넣어줌

  > **추상화 레이어**
  > "어떻게 동작해야 한다"는 규칙만 정의한 것
  > 보통 **인터페이스(interface)** 나 추상 클래스로 표현

  > **구현체**
  > 실제로 그 규칙을 따라 동작하는 구현 클래스나 객체

- 애플리케이션 **의존성 방향이 일관**되고, 애플리케이션을 *쉽게 추론*할 수 있음
- 모듈간의 관계들이 조금 더 명확해진다

#### 📌의존성 주입의 단점

- 클래스 수가 늘어나 복잡성이 증가 될 수 있음
- 런타임 패널티가 생기기도 한다
  > **런타임 패널티**
  > 프로그램이 실행되는 **"런타임(runtime)" 도중에 생기는 성능적인 손해**
  > → 실행 시간이 느려지거나, 메모리 사용이 늘어나는 등의 오버헤드(overhead) 를 의미

#### 📌의존성 주입 원칙

- 상위 모듈은 하위 모듈에서 어떠한 것도 가져오지 않아야 한다
- 또한, 둘 다 **추상화에 의존**해야 하며, 이때 추상화는 **세부 사항에 의존하지 말아야 한다**
  > **의존성 주입 원칙**
  > 상위 모듈이 하위 모듈을 직접 사용하면 강하게 결합됨, 바꾸기 힘듦
  >
  > **→ DIP (의존성 역전 원칙)를 따르면,**
  >
  > - 상위 모듈은 구현체가 아니라 인터페이스(추상화) 에만 의존
  > - 하위 모듈도 그 인터페이스를 구현만 하면 됨
  > - 실제 어떤 구현체가 들어갈지는 외부(DI 컨테이너나 주입자)가 결정함

---

#### ⭐ 팩토리 패턴 (Factory Pattern)

- 객체를 사용하는 코드에서 **객체 생성 부분을 떼어내 추상화한 패턴**
- **상위 클래스**가 중요한 **뼈대**를 결정
- **하위 클래스**에서 **객체 생성에 관한 구체적인 내용**을 결정
  → 느슨한 결합을 가진다 : 상위 클래스와 하위 클래스가 분리됨

```javascript
const num = new Object(42)
const str = new Object('abc')
num.constructor.name // Number
str.constructor.name // String
```

→ **전달받은 값에 따라** 다른 객체를 생성하며 인스턴스의 타입 등을 정하고 있음

```javascript
class Latte {
  constructor() {
    this.name = 'latte'
  }
}
class Espresso {
  constructor() {
    this.name = 'Espresso'
  }
}

class LatteFactory {
  static createCoffee() {
    return new Latte()
  }
}
class EspressoFactory {
  static createCoffee() {
    return new Espresso()
  }
}
const factoryList = { LatteFactory, EspressoFactory }

class CoffeeFactory {
  static createCoffee(type) {
    const factory = factoryList[type]
    return factory.createCoffee()
  }
}
const main = () => {
  // 라떼 커피를 주문한다.
  const coffee = CoffeeFactory.createCoffee('LatteFactory')
  // 커피 이름을 부른다.
  console.log(coffee.name) // latte
}
main()
```

→ 상위 클래스인 CoffeeFactory가 중요한 뼈대를 결정
→ 하위 클래스인 LatteFactory가 구체적인 내용을 결정

- **의존성 주입이라고 볼 수도 있다~!**
  ∵ LatteFactory에서 생성한 인스턴스를 CoffeeFactory에 주입하고 있음

> **팩토리 패턴이 왜 의존성 주입인가?** > **객체를 분리하고 결합도를 낮추는 구조를 만들어준다**는 점에서 의존성 주입이라고 볼 수도 있다~!

```
"CoffeeFactory 클래스를 보면 static 키워드를 통해
createCoffee() 메서드를 정적 메서드로 선언한 겻을 볼 수 있는데, ..."
```

> **정적 메서드?**
> 클래스의 인스턴스를 생성하지 않고도 사용할 수 있는 메서드

---

#### ⭐ 전략 패턴 (Strategy Pattern) (== 정책 패턴. Policy Pattern)

- 객체의 행위를 바꾸고 싶은 경우 **전략이라고 부르는 '캡슐화한 알고리즘'** 을 **컨텍스트 안에서 바꿔주면서** 상호 교체가 가능하게 만듦 (직접 수정하지 않는다)

#### 📌passport

- **passport** : 전략 패턴을 활용한 라이브러리
- Node.js에서 인증 모듈을 구현할 때 쓰는 미들웨어 라이브러리

  > **미들웨어 라이브러리?**
  > 요청(Request)과 응답(Response) 사이에서 중간 역할을 하는 함수

- 여러 가지 '전략'을 기반으로 인증할 수 있게 함 : LocalStrategy 전략, OAuth 전략 등

```javascript
var passport = require('passport'),
  LocalStrategy = require('passport-local').Strategy

passport.use(
  new LocalStrategy(function (username, password, done) {
    User.findOne({ username: username }, function (err, user) {
      if (err) {
        return done(err)
      }
      if (!user) {
        return done(null, false, { message: 'Incorrect username.' })
      }
      if (!user.validPassword(password)) {
        return done(null, false, { message: 'Incorrect password.' })
      }
      return done(null, user)
    })
  }),
)
```

→ passport.use()라는 메서드에 '전략'을 매개변수로 넣어서 로직을 수행함

---

#### ⭐ 옵저버 패턴 (Observer Pattern)

- **주체**가 어떤 객체의 **상태 변화를 관찰**하다가
  → 상태 변화가 있을 때마다 메서드 등을 이용해
  → 옵저버 목록에 있는 **옵저버들에게 변화를 알려줌**
- **주체** : 객체의 상태 변화를 보고 있는 관찰자
- **옵저버** : 이 객체의 상태 변화에 따라 '추가 변화 사항'이 생기는 객체들
- 주로 **이벤트 기반 시스템**에 사용

  > **이벤트 기반 시스템?**
  > 어떤 사건(이벤트)이 발생했을 때 그에 반응하는 방식으로 동작하는 시스템
  > 누가 뭔가를 발생시키고,
  > 거기에 자동으로 반응하는 구조

#### 📌MVC(Model-View-Controller) 패턴

- 주체 역할인 Model에서 변경 사항이 生
  → update() 메서드로
  → 옵저버인 View에게 알려줌
  → 이름 바탕으로 Controller가 작동

#### 📌상속과 구현

- 상속(Extends)

  - 자식 클래스가 부모 클래스의 메서드 등을 상속받아 사용함
  - 이후 자식클래스에서 추가 및 확장을 할 수 있는 것
  - 재사용성, 중복성의 최소화

- 구현(Implements)

  - 부모 인터페이스를 자식 클래스에서 재정의하여 구현
  - 반드시 부모 클래스의 메서드를 재정의하여 구현해야 함

- 상속은 일반 클래스, abstract 클래스를 기반으로 구현하며, 구현은 인터페이스를 기반으로 구현한다
  > **이게 뭔 소린가??**
  > 상속은 일반 클래스나 추상 클래스를 상속받을 때 사용하고, 구현은 인터페이스를 구현할 떄 사용한다

#### 📌자바스크립트에서의 옵저버 패턴

- **프록시 객체**를 통해 구현 가능

#### 📌프록시(Proxy) 객체

- 어떠한 대상의 **기본적인 동작 (속성 접근, 할당, 순회, 열거, 함수 호출 등)의 작업을 가로챌 수 있는 객체**
- 자바스크립트에서 프록시 객체는 두 개의 매개변수 有

1. **target** : 프록시 대상
2. **handler** : target 동작을 가로채고 어떠한 동작을 할 것인지가 설정되어 있는 함수

```javascript
// 자스로 프록시 객체 구현
const handler = {
  get: function (target, name) {
    return name === 'name' ? `${target.a} ${target.b}` : target[name]
  },
}
const p = new Proxy({ a: 'YOOONDOOONG', b: 'IS ZZANG' }, handler)
console.log(p.name) // YOOONDOOONG IS ZZANG
```

→ **name 속성 등 특정 속성에 접근할 때 그 부분을 가로채서 어떠한 로직을 강제함**
→ "name이라는 속성에 접근할 때 a와 b를 합쳐서 문자열을 만들라"라는 handler의 로직

#### 📌프록시 객체를 이용한 옵저버 패턴

```javascript
function createReactiveObject(target, callback) {
  const proxy = new Proxy(target, {
    set(obj, prop, value) {
      if (value !== obj[prop]) {
        const prev = obj[prop]
        obj[prop] = value
        callback(`${prop}가 [${prev}] >> [${value}] 로 변경되었습니다`)
      }
      return true
    },
  })
  return proxy
}
const a = {
  형규: '커플',
}
const b = createReactiveObject(a, console.log)
b.형규 = '커플'
b.형규 = '솔론'
// 형규가 [커플] >> [솔로] 로 변경되었습니다
```

→ get() 함수는 속성과 함수에 대한 접근을 가로챔
→ has() 함수는 in 연산자의 사용을 가로챔
→ set() 함수는 속성에 대한 접근을 가로챔
→ 형규라는 속성이 커플에서 솔로로 되는 것을 감시할 수 있었다~깔깔깔

#### 📌Vue.js 3.0의 옵저버 패턴

- ref나 reactive로 정의하면 해당 값이 변경되었을 때 자동으로 DOM에 있는 값이 변경됨
  → 프록시 객체를 이용한 옵저버 패턴을 이용하여 구현한 것
  > **DOM(Document Object Model)**
  > 문서 객체 모델.
  > 웹 브라우저상의 화면을 이루고 있는 요소들을 지칭

---

#### ⭐ 프록시 패턴(Proxy Pattern)

- 대상 객체에 접근하기 전 그 접근에 대한 흐름을 가로챔
  → 접근을 필터링하거나 수정하는 등의역할을 하는 계층이 존재
  → 객체의 속성, 변환 등을 보완하며 보안, 데이터 검증, 캐싱, 로깅에 사용
  > **프록시 서버에서의 캐싱**
  > 캐시 안에 정보를 담아두고,
  > 캐시 안에 있는 정보를 요구하는 요청에 대해
  > 캐시 안에 있는 데이터를 활용하는 것 (원격 서버에 요청하지 않고)
  > → 불필요하게 외부와 연결하지 않아 트래픽을 줄일 수 있음

#### 📌 프록시 서버(Proxy Server)

- 서버와 클라이언트 사이에서 클라이언트가 자신을 통해 다른 네트워크 서비스에 간접ㅂ적으로 접속할 수 있게 함

#### 📌 프록시 서버로 쓰는 nginx

- 비동기 이벤트 기반의 구조와 다수늬 연결을 효과적으로 처리 가능한 웹 서버
- 주로 Node.js 서버 앞단의 프록시 서버로 활용됨
- 익명 사용자가 직접적으로 서버에 접근하는 것을 차단
- 간접적으로 한 단계를 더 거치게 만들어서 보안 강화
  → nginx를 프록시 서버로 둬서 실제 포트를 숨길 수 있음
  → 정적 자원을 gzip 압축
  → 메인 서버 앞단에서의 로깅 가능

  > **버퍼 오버플로우**
  > 버퍼 : 데이터가 저장되는 메모리 공간
  > 버퍼 오버플로우 : 메모리 공간을 벗어나는 경우
  > 사용되지 않아야 할 영역에 데이터가 덮어씌워져 주소, 값을 바꾸는 공격 발생하기도 함

  > **gzip 압축**
  > DEFLATE 알고리즘을 기반으로 한 압축 기술
  > 데이터 전송량을 줄일 수 있지만, 압축 해제했을 때 서버에서의 CPU 오버헤드도 고려해서 사용유무 판단 필요

#### 📌 프록시 서버로 쓰는 CloudFlare

- 전 세계적으로 분산된 서버가 있고 이를 통해 어떤 시스템의 콘텐츠 전달을 빠르게 할 수 있는 CDN 서비스
  > **CDN(Content Delivery Network)**
  > 각 사용자가 인터넷에 접속하는 곳과 가까운 곳에서 콘텐츠를 캐싱 또는 배포하는 서버 네트워크
- DDOS 공격 방어나 HTTPS 구축에 쓰임
- 의심스러운 트래픽인지를 먼저 판단해 CAPTCHA 등을 기반으로 이를 일정부분 막아줌

#### 📌 DDOS 공격 방어

- 짧은 기간 동안 네트워크에 많은 요청을 보내 네트워크를 마비시킴
  → 웹 사이트의 가용성을 방해하는 사이버 공격 유형

#### 📌 HTTPS 구축

- 서버에서 HTTPS를 구축할 때 인증서를 기반으로 구축 가능
- CloudFlare를 사용하면 별도의 인증서 설치 없이 좀 더 손쉽게 HTTPS 구축 가능

#### 📌 CORS와 프런트엔드의 프록시 서버

- CORS(Cross-Origin-Resource Sharing) : 서버가 웹 브라우저에서 리소스를 로드할 때 다른 오리진을 통해 로드하지 못하게 하는 HTTP 헤더 기반 메커니즘

  > **Origin**
  > 프로토콜과 호스트 이름 포트의 조합

- CORS 에러 : 주로 FE 개발 시 FE 서버를 만들어서 백엔드 서버와 통신할 때 마주침
  → FE 테스팅 포트와 BE 서버 포트 번호가 다른 경우
  → 이를 해결하기 위해 FE에서 프록시 서버를 만들기도 함 : 프록시 서버를 둬서 FE 서버에서 요청되는 Origin을 BE 서버 포트 번호로 바꿈
  → CORS 에러 해결 + 다양한 API 서버와의 통신도 매끄럽게 가능

---

#### ⭐ 이터레이터 패턴 (Iterator Pattern)

- 이터레이터를 사용하여 컬렉션의 요소들에 접근하는 디자인 패턴
  > **컬렉션?**
  > 데이터 여러 개를 담는 자료구조
- 자료형 구조 상관없이 어터레이터라는 하나의 인터페이스로 순회가 가능

```javascript
const mp = new Map()
mp.set('a', 1)
mp.set('b', 2)
mp.set('cccc', 3)
const st = new Set()
st.add(1)
st.add(2)
st.add(3)
const a = []
for (let i = 0; i < 10; i++) a.push(i)

for (let aa of a) console.log(aa)
for (let a of mp) console.log(a)
for (let a of st) console.log(a)
/*
a, b, c
[ 'a', 1 ]
[ 'b', 2 ]
[ 'c', 3 ]
1
2
3
*/
```

- set과 map : 다른 자료 구조임에도, 똑같은 `for a of b`라는 이터레이터 프로토콜을 통회 순회함

  > **이터레이터 프로토콜**
  > 이터러블한 객체들을 순회할 떄 쓰이는 규칙

  > **이터러블한 객체**
  > 반복 가능한 객체로 배열을 일반화한 객체

---

#### ⭐ 노출 모듈 패턴 (Revealing Module Pattern)

- 즉시 실행 함수를 통해 private, public 같은 접근 제어자를 만드는 패턴
- 자바스크립트 : 이런 접근 제어자가 없고 전역 범위에서 스크립트가 실행됨
  → 노출모듈 패턴을 통해 private, public 접근 제어자를 구현하기도 함

```javascript
const pukuba = (() => {
  const a = 1
  const b = () => 2
  const public = {
    c: 2,
    d: () => 3,
  }
  return public
})()
console.log(pukuba)
console.log(pukuba.a)
// { c: 2, d: [Function: d] }
// undefined
```

→ a와 b는 다른 모듈에서 사용할 수 없는 변수나 함수 ∴ private 범위를 가짐
→ c와 d는 다른 모듈에서 사용할 수 있는 변수나 함수 ∴ public 범위를 가짐

> **public**
> 클래스에 정의된 함수에서 접근 가능하며 자식 클래스와 외부 클래스에서 접근 가능한 범위

> **protected**
> 클래스에 정의된 함수에서 접근 간으, 자식 클래스에서 접근 가능하지만 외부 클래스에서 접근 불가능한 범위

> **private**
> 클래스에 정의된 함수에서 접근 가능하지만 자식 클래스와 외부 클래스에서 접근 불가능한 범위

> **즉시 실행 함수**
> 함수를 정의하자마자 바로 호출하는 함수. 초기화 코드, 라이브러리 내 전역 변수의 충돌 방지 등에 사용

> **public/private이 결국 전역/지역 차이 맞는지??**
> ???

> **protected와 private 차이??**
> ???

---

#### ⭐ MVC 패턴

- Model, View, Cotroller로 이루어진 디자인 패턴
- 재사용성과 확장성이 용이하다는 장점
- 애플리케이션이 복잡해질수록 모델과 뷰의 관계가 복잡해진다는 단점

#### 📌 Model

- 애플리케이션의 데이터인 데이터베이스, 상수, 변수 등
- 뷰에서 데이터를 생성하거나 수정하면 컨트롤러를 통해 모델을 생성하거나 갱신

#### 📌 View

- 사용자 인터페이스 요소
- 모델을 기반으로 사용자가 볼 수 있는 화면
- 모델이 가지고 있는 정보를 따로 저장하지 않아야 함
- 단순히 사각형 모양 등 화면에 표시하는 정보만 가지고 있어야 함
- 변경이 일어나면 컨트롤러에 이를 전달해야 함

#### 📌 Controller

- 하나 이상의 모델과 하나 이상의 뷰를 잇는 다리 역할
- 이벤트 등 메인 로직을 담당
- 모델과 뷰의 생명주기 관리
- 모델과 뷰의 변경 통지를 받으면 이를 해석해서 각각에게 이를 알림

#### 📌 MVC 패턴의 예, 스프링

- 애너테이션을 기반으로 사용자의 요청 값들을 쉽게 분석할 수 있음
- 사용자의 어떤 요청이 유효한 요청인지를 쉽게 거를 수 있음
  > **이너테이션??**
  > 코드에 붙이는 주석 같은 메타정보
  > → 컴파일러나 프레임워크가 이 정보를 보고 특정 동작을 자동으로 수행하게 함

---

#### ⭐ MVP 패턴

- MVC 패턴으로부터 파생됨
- Controller가 Presenter로 교체된 패턴
- View와 Presenter는 일대일 관계 → MVC 패턴보다 더 강한 결합을 지닌 디자인 패턴
  > **MVP 패턴??**
  > View는 Presenter에 사용자 이벤트를 전달하고,
  > Presenter는 Model을 조작하고 결과를 다시 View에 전달
  >
  > - View와 Presenter가 1:1 관계
  >   → 서로를 강하게 알고 있음 (예: Presenter가 View에 직접 결과 넘겨줌)
  > - View는 아무런 로직도 처리하지 않음 (최대한 단순한 UI 역할만)
  > - Presenter는 테스트가 쉬움 (View를 Mock으로 대체 가능)

---

#### ⭐ MVVM 패턴

- Cotroller가 View Model로 바뀐 ㅐ턴
- View Model : View를 더 추상화한 계층
- 커맨드와 데이터 바인딩을 가지는 것이 특징

  > **커맨드??**
  > 여러 가지 요소에 대한 처리를 하나의 액션으로 처리할 수 있게 하는 기법

  > **데이터 바인딩??**
  > 화면에 보이는 데이터와 웹 브라우저의 메모리 데이터를 일치시키는 기법
  > View Model을 변경하면 View가 변경 됨

- View와 View Model 사이의 양방향 데이터 바인딩을 지원
  → UI를 별도의 코드 수정 없이 재사용 가능, 단위 테스팅이 쉽다~!

  > **왜??별도의 코드 수정 없이?** > **ViewModel이 View랑 완전히 분리돼 있음**
  > → UI 코드와 로직 코드가 느슨하게 연결돼 있어서
  > 화면을 바꾸거나 스타일을 바꿔도, 비즈니스 로직(ViewModel)은 그대로 재사용 가능

  > **왜? 단위 테스팅이 왜 쉬움???**
  > ViewModel은 UI에 의존하지 않고, **순수한 로직만 포함돼 있음**
  > → ViewModel은 화면 그리는 코드가 없음 (DOM, HTML, 버튼 클릭 등 없음)
  > 그냥 클래스 안에 함수, 변수만 있음
  > 즉, 마치 함수 테스트하듯 테스트 가능

#### 📌 MVVM 패턴의 예, Vue.js

- 반응형이 특징인 FE 프레임워크
- 값 대입만으로도 변수가 변경됨
- 양방향 바인딩, html을 토대로 컴포넌트를 구축할 수 있다는 특징

---

## 📖 SECTION 1.2 프로그래밍 패러다임

#### ⭐ 프로그래밍 패러다임

- 프로그래머에게 **프로그래밍의 관점을 갖게 해주는 역할**을 하는 개발 방법론
- **객체 지향 프로그래밍** : 프로그램을 상호 작용하는 객체들의 집합으로 볼 수 있게 함
- **함수형 프로그래밍** : 상태 값을 지니지 않는 함수 값들의 연속으로 볼 수 있게 함
- 크게 **선언형, 명령형**으로 나뉨
- 함수형 ⊂ 선언형
- 객체지향, 절차지향 ⊂ 명령형

---

#### ⭐ 선언형 프로그래밍

- 선언형 프로그래밍 : '무엇을' 풀어내는가에 집중하는 패러다임

  > "프로그램은 함수로 이루어진 것이다."

#### 📌 함수형 프로그래밍

- 선언형 패러다임의 일종
- 작은 '순수 함수'들을 블록처럼 쌓아 로직을 구현하고 '고차 함수'를 통해 재사용성을 높인 프로그래밍 패러다임

```javascript
const list = [1, 2, 3, 4, 5, 11, 12]
const ret = list.reduce((max, num) => (num > max ? num : max), 0)
console.log(ret) // 12
```

→ reduce() : '배열'만 받아서 누적한 결괏값을 반환하는 순수 함수

> **순수 함수**
> 출력이 입력에만 의존하는 것

> **고차 함수**
> 함수가 함수를 값처럼 매개변수로 받아 로직을 생성할 수 있는 것

#### 📌 일급 객체

- 고차함수를 쓰기 위해서는 해당 언어가 일급 객체라는 특징을 가져야 함

1. 변수나 메서드에 함수를 할당할 수 있다
2. 함수 안에 함수를 매개변수로 담을 수 있다
3. 함수가 함수를 반환할 수 있다

---

#### ⭐ 객체지향 프로그래밍 (OOP, Object-Oriented Programming)

- 객체들의 집합으로 프로그램의 상호 작용을 표현
- 데이터를 객체로 취급하여 객체 내부에 선언된 메서드를 활용하는 방식

#### 📌 객체지향 프로그래밍의 특징

- **추상화** : 복잡한 시스템으로부터 핵심적이 개념 또는 기능을 간추려내는 것
- **캡슐화** : 객체의 속성과 메서드를 하나로 묶고 일부를 외부에 감추어 은닉하는 것
- **상속성** : 상위 클래스의 특성을 하위 클래스가 이어받아서 재사용하거나 추가, 확장하는 것
- **다형성** : 하나의 메서드나 클래스가 다양한 방법으로 동작하는 것. 오버로딩, 오버라이딩 등

#### 📌 오버로딩

- 같은 이름을 가진 메서드를 여러 개 두는 것

```javascript
class Person {

    public void eat(String a) {
        System.out.println("I eat " + a);
    }

    public void eat(String a, String b) {
        System.out.println("I eat " + a + " and " + b);
    }
}

public class CalculateArea {

    public static void main(String[] args) {
        Person a = new Person();
        a.eat("apple");
        a.eat("tomato", "phodo");
    }
}
/*
I eat apple
I eat tomato and phodo
*/
```

→ 매개변수의 개수에 따라 다른 함수가 호출 됨

#### 📌 오버라이딩

- 상위 클래스로부터 상속받은 메서드를 하위 클래스가 재정의하는 것

```javascript
class Animal {
    public void bark() {
        System.out.println("mumu! mumu!");
    }
}

class Dog extends Animal {
    @Override
    public void bark() {
        System.out.println("wal!!! wal!!!");
    }
}

public class Main {
    public static void main(String[] args) {
        Dog d = new Dog();
        d.bark();
    }
}
/*
wal!!! wal!!!
*/
```

→ 자식 클래스 기반으로 메서드가 재정의 됨

#### 📌 설계 원칙

- SOLID 원칙

1. SRP, Single Responsibility Principle : 단일 책임 원칙

- 모든 클래스는 각각 하나의 책임만 가져야 하는 원칙

2. OCP, Open Closed Principle : 개방-폐쇄 원칙

- 유지 보수 사항이 생긴다면 코드를 쉽게 확장할 수 있도록 하고 수정할 때는 닫혀 있어야 하는 원칙

3. LSP, Liskov Substitution Principle : 리스코프 치환 원칙

- 프로그램의 객체는 프로그램의 정확성을 깨뜨리지 않으면서 하위 타입의 인스턴스로 바꿀 수 있어야 함
- 부모 객체에 자식 객체를 넣어도 시스템이 문제 없이 돌아가게 만드는 것

4. ISP, Interface Segragation Principle : 인터페이스 분리 원칙

- 하나의 일반적인 인터페이스보다 구체적인 여러 개의 인터페이스르 만들어야 하는 원칙

5. DIP, Dependency Inversion Principle : 의존 역전 원칙

- 자신보다 변하기 쉬언 것에 의존하던 것을 추상화된 인터페이스나 상위 클래스를 두어 변하기 ㅜ시운 것의 변화에 영향받지 않게 하는 원칙
- 상위 계층은 하위 계층의 변화에 대한 구현으로부터 독립해야 함

---

#### ⭐ 절차형 프로그래밍

- 로직이 수행되어야 할 연속적인 계산 과정으로 구성
- 코드의 가독성이 좋으며 실행 속도가 빠름
- 계산이 많은 작업 등에 쓰임
- 모듈화하기가 어렵고 유지 보수성이 떠렁진다는 점

---

#### ⭐ 패러다임의 혼합

- Q. 어떤 패러다임이 가장 좋을까요??????
- A. 그런 건 업!! 따!! 비즈니스 로직이나 서비스의 특징을 고려해서 정하거라
