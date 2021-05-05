# 소개
Nest (NestJS)는 효율적이고 확장 가능한 [Nodejs](https://nodejs.org/ko/)의 서버-사이드 프레임워크입니다.
progressive JavaScript를 사용하고 [TypeScript](https://www.typescriptlang.org/)를 완전히 지원 하며,
(순수 JavaScript 로 코딩 할 수 있음) OOP (Object Oriented Programming), FP (Functional Programming) 및 FRP (Functional Reactive Programming) 요소의 결합체입니다.

Nest는 내부적으로 Express(기본값)와 서버-사이드 프레임워크 를 사용하며 선택적으로 [Fastify](https://github.com/fastify/fastify)) 를 사용하도록 구성 할 수도 있습니다 !

Nest는 Node.js의 Express/Fastify 프레임워크의 공용 라이브러리를 추상적으로 제공하지만, API를 개발자들에게 직접적으로 공개하기도 합니다. 이를 통해 개발자들은 플랫폼(NestJS)에서 사용가능한 서드파티 모듈을 자유롭게 사용할 수 있습니다.

### 철학
최근 몇 년 동안 Node.js 덕분에 JavaScript는 프론트 및 백엔드 애플리케이션 모두를위한 웹의 "lingua franca"가 되었습니다.
이로 인해 Angular , React 및 Vue 와 같은 멋진 프로젝트가 생겨서 개발자 생산성이 향상되고 빠르고 테스트 가능하며 확장 가능한 프런트 엔드 애플리케이션을 만들 수 있습니다.
그러나 Node (및 서버 측 JavaScript)를 위한 훌륭한 라이브러리, 도우미 및 도구가 많이 존재하지만 이들 중 어느 것도 아키텍처 의 주요 문제를 효과적으로 해결하지 못 하였습니다.

`역자주 : lingua franca 는 사용언어가 다른 사람들끼리 의사 소통을 위해 만든 공통어`

Nest는 개발자과 팀에게 고도의 테스트가 가능하고, 확장 가능하며 느슨한 결합 및 쉬운 유지보수에 용이 ​​한 애플리케이션을 만들 수있는 즉시 사용 가능한 애플리케이션 아키텍처를 제공합니다.
이 아키텍처는 Angular에서 크게 영감을 받았습니다.

### 설치
시작하려면 [Nest CLI](https://docs.nestjs.com/cli/overview)로 시작하거나 스타터 프로젝트를 복제 할 수 있습니다 (둘 다 동일한 결과를 생성 함).

Nest CLI 로 프로젝트 시작하려면, 다음 명령어를 실행하세요.
이렇게하면 새 프로젝트 디렉토리가 생성되고 초기 핵심 Nest 파일 및 지원 모듈로 디렉토리가 채워져 프로젝트의 기존 기본 구조가 생성됩니다.
처음 사용하는 사용자에게는 Nest CLI 로 새 프로젝트를 만드는 것이 좋습니다. 이 접근 방식은 첫 번째 단계 에서 계속할 것 입니다.

```bash
$ npm i -g @nestjs/cli
$ nest new project-name
```

### 대안
또는 `Git` 을 사용하여 TypeScript 스타터 프로젝트를 설치하려면 다음을 수행하십시오 .
```bash
$ git clone https://github.com/nestjs/typescript-starter.git project
$ cd project
$ npm install
$ npm run start
```

브라우저를 다음 `http://localhost:3000/.` 주소로 이동합니다.

스타터 프로젝트의 JavaScript 특징을 설치하려면 위의 명령 순서에서 `javascript-starter.git` 을 사용합니다.
npm (또는 yarn )을 사용하여 코어 및 지원 파일을 설치하여 처음부터 수동으로 새 프로젝트를 만들 수도 있습니다 .
물론이 경우에는 프로젝트 상용구 파일을 직접 생성해야합니다.

```bash
$ npm i --save @nestjs/core @nestjs/common rxjs reflect-metadata
```
