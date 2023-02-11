# compilation-context

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
