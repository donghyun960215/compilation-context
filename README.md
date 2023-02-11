# TypeScript Compiler

## compileOnSave
```plaintext
"compilOnSave": true or false (default false)  
파일을 세이브하면 컴파일 하겠다.
```

## extends
```plaintext
"extends": "./base.json"
파일 (상대)경로명 : string  

//in PROJECT/base.json
{
  "compilerOptions": {
    "strict": true
  }
}
//in PROJECT/tsconfig.json
{
  "extends": "./base.json",
}
```

# files. include, exclude
```plaintext
@ 셋다 설정이 없으면, 전부다 컴파일
@files
  상대 혹은 절대 경로의 리스트 배열입니다.
  exclude 보다 강한 설정입니다.
@include, exclude
  glob 패턴(마치.gitignire)
  include
      exclude 보다 약한 설정입니다.
      *같은걸 사용하면, .ts/ .tsx/ .d.ts만 include (allowJS)
  exclude
      설정 안하면 4가지(node_modules, bower_components, jspm_packges,
      outDir>)를 default로 제외합니다.
      <outDir>은 항상 제외합니다.(include에 있어도)
```

# compileOpstions

## typeRoots, types
```plaintext
* TypeScript 2.0 부터 사용 가능해진 내장 type definition 시스템  

* 아무 설정을 안하면?
  node_modules/@types 라는 모든 경로를 찾아서 사용

* typeRoots 를 사용하면 ?
  배열 안에 들어있는 경로들 아래서만 가져옵니다.

* types 를 사용하면 ?
  배열 안의 모듈 혹은 ./node_modules/@types/ 안의 모듈 이름에서 찾아옵니다.  
  []빈 배열을 넣은다는건 이 시스템을 이용하지 않겠다는 것입니다.

* typeRoots 와 types 를 같이 사용하지 않습니다.
```

## target, lib
```plaintext
* target
  빌드의 결과물을 어떤 버전으로 할 것이냐  
  지정을 안하면 es3 입니다.  

* lib
  기본 type definition 라이브러리를 어떤 것을 사용할 것이냐  
  lib 를 지정하지 않을 때,
    target 이 'es3'이고, 디폴트로 lib.d.ts를 사용합니다.
    target 이 'es5'이면, 디폴트로 din, es5, scripthost를 사용합니다.
    target 이 'es6'이면, 디폴트로 dom, es6 dom.iterable, scripthost를 사용합니다.  
  lib를 지정하면 그 lib 배열로만 라이브러리를 사용합니다.
    빈 [] => 'no definition found 어쩌구'
```

## strict
```plaintext
* nolmplicitAny
  명시적이지 않게 any 타입을 사용하여, 표현식과 선언에 사용하면, 에러를 발생.
    타입스크립트가 추론을 실패한 경우, any 가 맞으면, any 라고 지정하라
    아무것도 쓰지 않으면, 에러를 발생
    이 오류를 해결하면, any 라고 지정되어 있지 않은 경우는 any 가 아닌 것이다.(타입 추론이 되었으므로)

* nolmplicitThis
  명시적이지 않게 any타입을 사용하여, this 표현식에 사용하면, 에러를 발생합니다.  
  첫번째 매개변수 자리에 this를 놓고, this에 대한 타입을 어떤 것이라도 표현하지 않으면, nolmplicitAny가 오류를 발생시킨다.    
  JavaScript 에서는 매개변수에 this 를 넣으먄, 이미 예약된 키워드라 SyntaxError 를 발생시킨다.    
  call/ apply / bind 와 같이 this 를 대체하여 함수 콜을 하는 용도로도 쓰입니다.  
  그래서 this 를 any로 명시적으로 지정하는 것은 합리적입니다.(문론 구체적인 사용처가 있는 경우 타입을 표현하기도 합니다.)  

* strictNullChecks
  strictNullChecks 모드에서는
    null 및 undefined 값이 모든 유형의 도메인에 속하지 않으며, 그자신을 타입 으로 가지거나, any 일 경우에만 할당이 가능합니다.  
    한 가지 예외는 undefined 에 void 할당 가능
  strictNullChecks 를 적용하지 않으면,
    모든 타입은 null, undefined 값을 가질 수 있습니다.
    sting 으로 타입을 지정해도, null 혹은 undefined 값을 할당할 수 있다는 것입니다.
  strictNullChecks 를 적용하면,
    모든 타입은 null, undefined 값을 가질 수 없고, 가지려면 union type 을 이용하여 직접 명시 해야 합니다.
    any 타입은 null 과 undefined 를 가집니다. 예외적으로 void 타입의 경우 undefined 를 가집니다.

*strictFunctionTypes
  함수 타입에 대한 bivariant 매개변수 검사를 비활성화 합니다.

*stictPropertyInitialization
  정의되지 않은 클래스의 속성이 생성자에서 초기화되었는지 확인해야합니다.
  이 옵션을 사용하려면 stritNullChecks를 사용하도록 설정해야 합니다.

*stictBindCallApply
  bind, call, apply 에 대한 더 엄격한 검사 수행
  Function 의 내장 함수인 bind/call/apply 를 사용할 때, 엄격하게 체크하도록 하는 옵션입니다.
  bind 는 해당 함수안에서 사용할 this 와 인자를 설정해주는 역할을 하고, call 과 apply 는 this 
  와 인자를 설정한 후, 실행까지 합니다.
  call 과 apply 는 인자를 설정하는 방식에서 차이점이 있습니다.
    call은 함수의 인자를 여러 인자의 나열로 넣어서 사용하고, apply 는 모든 인자를 배열 하나로 넣어서 시용합니다.

* alwaysStrict
  각 소스 파일에 대해 JavaScript 의 mode 로 코드를 분석하고, "엄격하게 사용" 을 해제합니다.
  컴파일된 JavaScript 파일에 "use strict" 추가 됨
```
