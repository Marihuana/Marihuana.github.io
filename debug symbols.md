# Why?

빌드 과정에서 디버깅 정보가 제거 되기 때문에 디버깅 정보가 포함된 Native-Debug-Symbols 파일을 생성하여 서버에 등록시켜줘야함.

---

### 1. Sentry-cli(Command Line Interface) 설치

깃허브 혹은 brew 등을 이용해 설치할 수 있다

- IOS
```bash
brew install getsentry/tools/sentry-cli
```

- Window
```bash
curl -sL https://sentry.io/get-cli/ | bash
```

이후 다음 명령어를 통해 설치됐는지 확인할 수 있다.

```bash
sentry-cli --help
```

참고 : [Sentry Docs]([https://docs.sentry.io/product/cli/installation/](https://docs.sentry.io/product/cli/installation/))

---

### 2. Android native-debug-symbols 파일 생성

build.config 내의 buildConfig 속성에 아래 내용을 추가한 이후 빌드를 합니다.

```groovy
ndk { debugSymbolLevel 'FULL'}
```

이후 해당 프로젝트의 app/build/outputs 로 이동하면 native-debug-symbols 폴더가 생성된 것을 볼 수 있다.

---

### 3. debug symbol 파일 등록

위 2번 과정을 거쳐 해당 폴더로 이동하면 zip 파일을 볼 수 있는데 해당 파일의 압축을 해제 후 다음 명령어를 통해 서버에 등록하도록 하자

```
sentry-cli upload-dif -o <소속회사명> -p <프로젝트명> <압축 해제한 파일들이 있는 폴더 경>
```

다음과 같은 결과를 볼 수 있다.

```
> Found 2 debug information files 
> Prepared debug information files for upload 
> Uploaded 2 missing debug information files > File processing complete:

PENDING 1ddb3423-950a-3646-b17b-d4360e6acfc9 (MyApp; x86\_64 executable)  
PENDING 1ddb3423-950a-3646-b17b-d4360e6acfc9 (MyApp; x86\_64 debug companion)
```

참고 : [Sentry Docs](https://docs.sentry.io/product/cli/dif/#uploading-files)

> Authentication 관련 오류가 난다면 Auth Token을 등록하여야 한다.