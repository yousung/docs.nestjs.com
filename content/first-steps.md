### 첫 단계

이 문서에서 Nest의 **기본 핵심**을 배우게 됩니다. Nest 어플리케이션의 필수 구성 요소를 배우기 위해서 CRUD를 만들어 볼 것입니다.

#### 언어
우리는 [TypeScript](https://www.typescriptlang.org/)를 사랑하지만, 무엇보다도 [Node.js](https://nodejs.org/en/)를 사랑합니다. 그것이 우리가 TypeScript 와 **순수 JavaScript**를 호환하는 이유입니다. Nest는 최신 Javascript의 기능을 활용하므로, 바닐라 자바스크립트를 활용하기 위해 [Babel](https://babeljs.io/) 컴파일러가 필요합니다.

우리가 제공하는 대부분의 예제는 TypeScript 를 사용하지만, **코드조각** 항상 바닐라 자바스크립트와 전환 할 수 있습니다. (각 파트에 오른쪽 상단에 있는 언어 버튼을 클릭하여 토글하면됩니다).

``역주 : 이 메뉴얼이 배포 되었을 때에  Javascript 토글버튼은 아직 구현되지 않았을 수도 있습니다. ``

#### 전체조건

[Node.js](https://nodejs.org/) (>= 10.13.0, v13 제외) 가 운영체제에 설치 되어있는지 확인하십시오.

#### 설정

새 프로젝트를 [Nest CLI](/cli/overview)로 설정하는 것이 간단합니다. [npm](https://www.npmjs.com/) 으로 당신의 OS에서 터미널로 다음 명령을 통해 새 프로젝트를 만들 수 있습니다.

```bash
$ npm i -g @nestjs/cli
$ nest new project-name
```

`project` 디렉토리에는, node modules과 몇 가지 기본 파일들이 생성됩니다, 그리고 `src/` 디렉토리에는 여러개의 코어 파일이 채워집니다.

<div class="file-tree">
  <div class="item">src</div>
  <div class="children">
    <div class="item">app.controller.spec.ts</div>
    <div class="item">app.controller.ts</div>
    <div class="item">app.module.ts</div>
    <div class="item">app.service.ts</div>
    <div class="item">main.ts</div>
  </div>
</div>

다음은 이러한 핵심파일에 대한 간략한 개요입니다:

|                          |                                                                                                                     |
| ------------------------ | ------------------------------------------------------------------------------------------------------------------- |
| `app.controller.ts`      | 단일 경로가 있는 기본 컨트롤러.                                                                             |
| `app.controller.spec.ts` | 컨트롤러 유닛 테스트.                                                                                  |
| `app.module.ts`          | 애플리케이션의 루트 모듈 입니다.                                                                                 |
| `app.service.ts`         | 단일 메서드가 있는 기본 서비스.                                                                               |
| `main.ts`                | 핵심 기능을 `NestFactory` 을 사용하여 Nest 어플리케이션 인스턴스를 생성하는 엔트리 파일입니다 |

여기 `main.ts` 에는 어플리케이션의 **bootstrap** 비동기 함수를 포함합니다.:

```typescript
@@filename(main)

import { NestFactory } from '@nestjs/core';
import { AppModule } from './app.module';

async function bootstrap() {
  const app = await NestFactory.create(AppModule);
  await app.listen(3000);
}
bootstrap();
@@switch
import { NestFactory } from '@nestjs/core';
import { AppModule } from './app.module';

async function bootstrap() {
  const app = await NestFactory.create(AppModule);
  await app.listen(3000);
}
bootstrap();
```

Nest 어플리케이션 인스턴스를 생성하기 위해, `NestFactory` 클래스를 사용합니다. `NestFactory` 는 응용프로그램 인스턴스를 생성하기 위해 몇가지 Static 메서드를 제공합니다. `create()` 메서드는 `INestApplication` 인터페이스를 충족하기 위한 응용프로그램을 반환합니다. 이 객체는 다음 장에서 설명하는 일련의 메서드들을 제공합니다.  위의 `main.ts` 의 HTTP 인바운드 요청을 기다릴 수 있도록 HTTP 리스너를 시작하기만 하면 됩니다.

Nest CLI로 개발자가 권장하는 규칙에 맞게 초기 프로젝트를 생성할 수 있습니다.

<app-banner-courses></app-banner-courses>

#### 플랫폼

Nest 는 플랫폼에 구애받지 않는 프레임워크를 목표합니다. 플랫폼의 독립성을 통해 개발자가 여러 유형의 응용프로그램에 활용할 수 있는 재사용 가능한 논리 부품을 만들 수 있습니다. Technically, Nest는 기술적으로 모든 Node HTTP 프레임워크에 작동하는 어덥터를 생성합니다. 두가지의 HTTP 플랫폼 [express](https://expressjs.com/) 와 [fastify](https://www.fastify.io)를 지원합니다. 당신은 여기서 당신에게 가장 필요한 것을 선택하면 됩니다.

|                    |                                                                                                                                                                                                                                                                                                                                    |
| ------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `platform-express` | [Express](https://expressjs.com/) 는 Node에서 가장 잘 알려진 미니멀 프레임워크입니다. 치열한 테스트, 커뮤니티에서 만들어진 운영환경에 잘 준비된 라이브러리들이 있습니다. 여기선 `@nestjs/platform-express` 기본 패키지로 사용합니다. 많은 사용자가 Express 를 잘 사용하고 있으며, 이를 사용하기 위해 따로 활성화할 필요는 없습니다. |
| `platform-fastify` | [Fastify](https://www.fastify.io/) 는 최적의 효율성과 속도를 제공하기 위한 고성능 그리고 낮은 오버 헤드 프레임워크입니다. [here](/techniques/performance) 에서 사용방법을 확인하십시오.                                                                                                                                  |

어떤 플랫폼을 사용하든 자체 어플리케이션 인터페이스를 노출합니다  이들은 각각 `NestExpressApplication` 와 `NestFastifyApplication` 로 표시됩니다.

아래의 예제와 같이 `NestFactory.create()` 메서드 유형을 전달하면, `app` 객체는 플랫폼에서만 사용할 수 있는 메서드를 갖게 됩니다. 그러나 실제로 기본 플랫폼이 **필요** 없는 경우가 **아니라면** 유형을 지정을 따로 지정할 필요는 없습니다.

```typescript
const app = await NestFactory.create<NestExpressApplication>(AppModule);
```

#### 응용프로그램 실행

설치 프로세스가 완료되면, 다음 명령을 실행하여 HTTP 요청을 수신하는 애플리케이션을 시작할 수 있습니다.

```bash
$ npm run start
```

이 명령은 `src/main.ts` 파일에 정의된 PORT에서 수신대기 중인 HTTP 서버로 앱을 시작합니다. 애플리케이션이 실행되면 브라우저를 열고 `http://localhost:3000/` 로 이동해보십시오. 당신은 `Hello World!` 글귀를 볼 수 있을 것입니다.
