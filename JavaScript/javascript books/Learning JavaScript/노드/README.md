# Node

2009년까지 자바스크립트는 거의 브라우저 스크립트 언어로만 쓰였다. 라이언 달은 자바스크립트를 서버에서 사용할 목적으로 노드를 만들었다.
서버에서 자바스크립트를 쓸 수 있게 된 것은 사고방식을 빠꿀 필요 없이 일관된 언어를 쓸 수 있다는 의미이고, 다른 전문가에게 의존할 필요가 줄어든다는 의미이며, 아마 가장 중요한 것이겠지만 서버와 클라이언트에서 같은 코드를 쓸 수 있다는 의미이다. 노드는 원래 웹 애플리케이션 개발을 목적으로 만들어졌지만, 서버에서 쓰이게 되면서 데스크톱 애플리케이션 개발이나 시스템 스크립트 같은 영역으로 확장되었다.

## 20.1 노드의 기초

브라우저 기반 자바스크립트는 브라우저에만 해당하는 API를 사용한다. 가장 명확한 특징은, 노드에는 DOM이 없다는 것이다. 물론 HTML이 없기 때문이다. 마찬가지로, 노드에만 해당되고 브라우저에는 존재하지 않는 API도 있다. 운영체제와 파일시스템 지원 같은 일부 기능은 보안상의 이유로 브라우저에서는 사용할 수 없다. 해커가 브라우저에 침투해 파일을 지울 수 있게 된다면 어떤 참사가 발생할지는 굳이 말하지 않아도 알 것이다. 그 밖에 웹 서버를 만드는 기능 같은 것은 브라우저에는 아무 쓸모 없다.
무엇이 자바스크립트이고 무엇이 API의 일부인지 구분할 수 있어야 한다. 브라우저 기반 크드만 작성하던 개발자라면 window 와 document가 자바스크립트의 일부라고 생각할 수 있지만, window와 document는 브라우저 환경에서 제공하는 API이다.

## 20.2 모듈

모듈은 패키지를 만들고 코드를 네임스페이스로 구분하는 메커니즘이다. 네임스페이스는 이름 충돌을 방지하는 방법이다. 예를 들어 아만다와 타일러가 모두 calculate함수를 만들었는데, 두 함수를 그냥 복사해서 당신의 프로그램에 넣는다면 나중에 붙여 넣은 함수가 처음에 붙여 넣은 함수를 덮어쓴다. 네임스페이스는 아만다가 만든 calculate, 타일러가 만든 calculate 하는 식으로 구분할 수 있는 방법을 제공한다.
Module은 노드에서 모듈을 구현하기 위해 만든 특별한 객체이다. exports프로퍼티에 무엇을 할당하든 모듈은 그것을 내보낸다.
모듈은 어떤 타입의 값이든 내보낼 수 있다. 그럴 이유는 별로 없지만, 원한다면 문자열이나 숫자 같은 원시 값을 내보낼 수도 있다. 보통은 모듈 하나에 여러 함수를 저장하고, 그 함수를 메서드로 포함하는 객체를 내보내는 것이 일반적이다. 모듈은 단순히 일반적인 객체를 내보내는것 뿐이고, 그 객체에 함수 프로퍼티가 있을 뿐이다. 이 방식은 널리 쓰이므로, 특별한 변수 exports를 사용하는 단축 문법이 따로 있다. exports를 사용한 단축 문법은 객체를 내보 낼 때에만 쓸 수 있다. 함수나 기타 다른 값을 내보낼 때는 반드시 module.exports를 써야한다. 또 두 문법을 섞어 쓸 수 없다. 모듈하나에 한가지 문법만 써야한다.

## 20.4 함수 모듈을 통한 모듈 커스터마이징

모듈은 대부분 객체를 내보내지만, 이따금 함수 하나만 내보낼 때도 있다. 함수 하나만 내보내는 경우는 그 모듈의 함수를 즉시 호출하려는 의도로 만들 때가 대부분이다. 이런 경우 사용하려는 것은 그 함수가 아니라 함수의 반환값이다.
달리 말해, 모듈이 내보내는 함수가 아니라 그 함수가 반환하는 것을 쓰게 한 것이다. 이런 패턴은 모듈 일부 커스터마이즈하거나, 주변 컨텍스트에서 정보를 얻어야 할 때 주로 사용한다. 실제 npm 패키지 debug 를 예로 들어보자

```javascript
const debug = require("debug")("main"); // 모듈이 반환하는 함술르 즉시 호출 할 수 있다.
debug("starting"); // 디버그가 활성화되어 있으면 "main starting +0ms라는 로그를 남긴다."
```

위 예제에서 debug모듈이 반환한 것을 즉시 호출 했으므로 debug모듈이 함수를 반환한 것을 알 수 있고, 반환값인 함수 역시 함수를 반환하며 최종적으로 반환된 함수는 첫 번째 함수에 넘긴 문자열을 '기억' 한다는 걸 알 수 있다. 요컨대 우리는 main이란 문자열을 사용하도록 debug모듈을 커스터마이즈 한 것이다.

노드는 노드앱을 실행 할 때 어떤 모듈이든 단 한번만 임포트 한다. 성능,메모리사용량, 관리 편의성 등에서 모듈은 한번만 임포트하는 것이 낫다.

## 20.5 파일 시스템 접근

path.join은 운영체제에 따라 디렉터리 구분자를 알맞게 사용하므로 이메서드를 사용하길 권한다.
디렉터리에 어떤 파일이 있는지 알아보려면 fs.readdir을 사용한다.

## 20.6 process

실행 중인 노드 프로그램은 모두 process 변수에 접근 할 수 있다.
이 변수는 해당 프로그램에 관한 정보를 담고 있으며 실행 자체를 컨트롤 할 수 있다.예를들어서 애플리케이션이 치명적인 에러를 만나서, 계속 실행하지 않는 편이 좋거나 더 실행해도 의미가 없는 상황이라면(이런 에러를 fatal error 라고 한다.) process.exit을 호출해서 즉시 실행을 멈출 수 있다.
숫자형 종료코드를 쓰면 프로그램이 성공적으로 종료됐는지 에러가 있었는지 외부 스크립트에서도 알 수 있다. 보통 에러없이 프로그램을 끝냈을 때는 종료코드 0을 사용한다. 종료코드가 0이 아니라면 에러가 있었다는 뜻이다.
process 객체를 통해 프로그램에 전달된 명령줄 매개변수 배열에 접근 할 수도 있다. 노드 어플리케이션을 실행할 때 명령줄에서 매개변수를 지정할 수 있다. 예를 들어서 텍스트 파일을 처리하는 프로그램에 파일 이름을 매개변수로 넘기고 각파일이 몇 행인지 출력한다고 하자.

```javascript
node linecount.js file1.txt file2.txt file3.txt
```

명령줄 매개변수는 process.argv 배열에 저장된다.
첫 번째 요소는 인터프리터, 즉 소스 파일을 해석한 프로그램, 두 번째 요소는 실행중인 프로그램의 전체 경로이며, 나머지 요소는 프로그램에 전달된 매개변수이다. 여기서 거를 때 Array.slice 를 사용할 수 있다.
process.env를 통해 환경 변수에 접근 할 수도 있다. 환경 변수는 시스템 변수이며 주로 명령줄 프로그램에서 사용한다. 대부분의 유닉스 시스템에서 export VAR_NAME=value 명령으로 환경변수를 설정할 수 있다. 환경변순느 보통 전부 대문자로 표기한다. 윈도우에서는 set VAR_NAME=value라는 명령을 사용한다.환경 변수를 사용하면 프로그램을 실행할 때마다 명령줄에서 매개변수로 지정할 필요가 없다.
예를 들어 프로그램에서 디버그 정보를 기록할지 아닌지를 환경 변수로 컨트롤하려 한다고 하자. 환경변수 DEBUG를 1로 설정하면 디버그 정보를 기록하고 다른값은 무엇이든 디버그 정보를 기록하지 않는다.

```javascript
const debug = process.env.DEBUG === "1" ? console.log : function () {};
debug("Visible only if environment variable DEBUG is set!");
```

이 예제에서 debug 함수는 환경 변수 DEBUG가 1이면 console.log의 별칭이 되고, 그렇지 않다면 null함수가 된다. null함수를 만들지 않으면 debug가 정의 되지 않는 경우가 생기고 에러가 날 것이다.

현재 작업 디렉터리의 기본값은 프로그램을 실행한 디렉터리이다.(프로그램이 존재하는 디렉터리가 아니다.) process.cwd에는 현재 작업디렉터리가 저장되며, process.chdir로 현재 작업 디렉터리를 바꿀 수 있다. 예를 들어서 프로그램에서 현재 작업 디렉터리를 출력한다음, 프로그램이 저장된 디렉터리로 현재 작업 디렉터리를 바꾸려면 다음과 같이 할 수 있다.

```javascript
cosole.log(`Current directory: ${process.cwd()}`)
process.chdir(__dirname)
console.log(`New current directory: ${process.cwd()})
```

## 20.7 운영체제

os모듈은 프로그램을 실행하는 컴퓨터의 운영체제에 관한 정보를 제공한다. 다음 예제는 os모듈에서 접근할 수 있는 정보중 가장 유용한 것들이다.

```javascript
const os = require("os");

console.log("Hostnmae: " + os.hostname());
console.log("OS type: " + os.type()); // window
console.log("OS platform: " + os.platform());
console.log("Totlal Memory: " + (os.totalmem() / 1e6).toFixed(1) + "MB");
```

## 20.8 자식 프로세스

child_process 모듈은 애플리케이션에서 다른 프로그램을 실행할 때 사용한다. 실행할 프로그램은 다른 노드 프로그램, 실행파일, 다른언어로 만든스크립트여도 상관없다.
child_process 모듈에서 제공하는 주요함수는 exec, execFile, fork 이다. fs와 마찬가지로 이들 함수에는 동기적 버전이 있다. exec는 운영체제에서 지원하는 실행 파일은 무엇이든 실행할 수 있다. exec는 운영체제의 명령줄이나 다름없는 셀을 호출한다. 따라서 명령줄에서 실행할 수 있는것은 무엇이든 exec를 통해 실행할 수 있다.
execFile은 셀을 통하지 않고 실행파일을 직접실행하므로 메모리와 자원관리 면에서 좀더효율적이지만, 그만큼 더 주의해야 한다. fork는 다른 노드 스크립트를 실행할 때 사용한다.(물론 exec로도 실행할 수 있다.)

```javascript
const exec = require("child_process").exec;
exec("dir", function (err, stdout, stderr) {
  if (err) return console.error("Error executing dir");
  stdout = stdout.toString(); // Buffer를 문자열로 바꿈
  console.log(stdout);
  stderr = stderr.toString();
  if (stderr !== "") {
    console.error("error:");
    console.error(stderr);
  }
});
```

exec는 셸을 호출하므로 dir 실행파일이 존재하는 경로를 따로 지정할 필요는 없다. 일반적으로 시스템 셸에서 바로 실행할 수 없고 전체 경로를 써야하는 외부 프로그램을 실행한다면 exec에서도 전체 경로를 지정해야한다.

## 20.9 스트림

스트림은 노드에서 중요한 개념이다. 스트림은 스트림 형태의 데이터를 다루는 객체이다. 스트림이란 단어는 흐름이란 느낌이 있고 흐름은 시간이 지나면서 일어나는 일이므로 비동기적으로 이루어질 것이라는 느낌이든다.

스트림에는 읽기 스트림, 쓰기스트림, 이중스트림이 있다. 스트림의 예로 사용자의 타이핑, 클라이언트와 통신하는 웹서비스 등을 들 수 있다. 파일을 읽고 쓰는 작업은 스트림을 통하지 않아도 가능하지만, 파일 작업 역시 스트림을 통할 때가 많다.
인코딩은 데이터를 해석 할 때에만 필요하다.

## 20.10 웹서버

노드 웹서버의 핵심은 들어오는 요청에 모두 응답하는 콜백함수이다. 이 함수는 매개변수로 IncomingMessage 객체 (보통 req라고 한다.)와 ServerResponse객체(보통 res라 한다.)를 받는다. IncomingMessage객체에는 요청받은 URL, 보낸 헤더, 바디에 들어있던 데이터등 HTTP요청에 관한 모든 정보가 들어있다. ServerResponse객체에는 클라이언트(보통 브라우저)에 보낼 응답을 컨트롤하는 프로퍼티와 메서드가 들어있다.
ServerResponse객체는 쓰기 스트림 인터페이스이며 이를 통해 데이터를 클라이언트에 보낸다. ServerResponse객체가 쓰기 스트림이므로 파일을 보내기도 쉽다. 파일 읽기스트림을 만들어 HTTP응답에 파이프로 연결하기만 하면된다. 예를 들어서 웹사이트에서 favicon.ico파일을 사용한다면 파비콘 요청을 감지하고 파일을 바로 보낼 수 있다.

```javascript
const server = http.createServer(function (req, res) {
  if (req.method === "GET" && req.url === "/favicon.ico") {
    const fs = require("fs");
    fs.createReadStream("favicon.ico");
    fs.pipe(res); // end 대신 사용할 수 있다.
  } else {
    console.log(`${req.method} ${req.url}`);
    res.end("Hello world");
  }
});
```
