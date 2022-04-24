# JSON, Javascript Object

JS Object: JS Engine 메모리 안에 있는 데이터 구조

JSON(JavaScript Object Notation): 객체의 내용을 기술하기 위한 text파일(확장자명 .json)

클라이언트, 서버 HTTP 통신 시에는 `JSON`으로 송수신한다.

### 자바스크립트에 내장된 Convert 해주는 method.

> JSON to JS Object Method: JSON.parse()<br>
> JS Object to JSON: JSON.stringify()

```javscript
const str=`{"data":[{"name":"joo","info":["man","322"]}]}`;

const obj = {data:[{name:'joo',info:['man','322']}]};
```

- JSON에선 모든 키를 따옴표로 묶는다.
- JSON type은 string이다.
- JSON은 함수를 값으로 할당할 수 없다.

## Why Use JSON?

본래는 자바스크립트 언어로부터 파생되어 자바스크립트의 구문 형식을 따르지만 언어 독립형 데이터 포맷이다. 즉, 프로그래밍 언어나 플랫폼에 독립적이므로, 구문 분석 및 JSON 데이터 생성을 위한 코드는 수많은 프로그래밍 언어에서 쉽게 이용할 수 있다.(출처:위키백과)

JSON은 문법적으로 JavaScript Object와 유사하기 때문에 자바스크립트 프로그램에서 쉽게 JavaScript Object, JSON으로 서로 변환이 쉽다. 또한 JSON의 string으로만 이루어진 포맷이므로 컴퓨터, 어느 프로그래밍언어에서 다루기 쉽다는 장점이 있다.
