### 모듈

모듈은 `@Module()` 라는 데코레이터 주석이 달린 클래스입니다. `@Module()` 데코레이터는 **Nest** 가 응용프로그램 구조를 구성하는데 사용하는 메타데이터를 제공합니다.

<figure><img src="https://docs.nestjs.com/assets/Modules_1.png" /></figure>

각 응용 프로그램에는 하나 이상의 모듈인 **루트 모듈**이 있습니다. 루트 모듈은 Nest가 응용 프로그램 그래프를 빌드하기 위해 사용하는 시작점입니다. Nest는 모듈과 공급자(이하 provider) 관계 및 종속성을 해결하는데 사용하는 내부 데이터 구조입니다. 아주 작은 응용 프로그램에는 이론적으로 루트 모듈만 있을 수 있지만 일반적인 경우는 아닙니다. 구성 요소를 효과적으로 구성하려면 모듈을 **강력히 권장**합니다. 따라서 대부분의 애플리케이션에서 결과 아키텍처는 여러 모듈을 사용하며 각 모듈은 밀접하게 관련된 **기능** 세트를 캡슐화합니다.

`@Module()` 데코레이터 속성이 모듈을 설명하는 단일 객체를 취합니다.

<table>
  <tr>
    <td><code>providers</code></td>
    <td>Nest 주입자에 의해 인스턴스화 되고, 이 모듈에서 공유 될 수 있는 공급자</td>
  </tr>
  <tr>
    <td><code>controllers</code></td>
    <td>이 모듈에 정의되어 인스턴스화 되어야하는 컨트롤러 세트</td>
  </tr>
  <tr>
    <td><code>imports</code></td>
    <td>이 모듈에 필요한 provider를 가져오는 목록</td>
  </tr>
  <tr>
    <td><code>exports</code></td>
    <td>이 모듈에서 제공하며 다른 모듈에서 가져올 수 있는 <code>providers</code> 목록</td>
  </tr>
</table>

``역자주 : exports 부분을 제대로 해석 도움 필요합니다.``

이 모듈은 기본적으로 provider에게 **캡슐화** 됩니다. This means that it's impossible to inject providers that are neither directly part of the current module nor exported from the imported modules. Thus, you may consider the exported providers from a module as the module's public interface, or API.

#### 기능 모듈 ( feature module )

`CatsController` 과 `CatsService` 같은 응용프로그램 도메인에 속합니다. 밀접하게 관련되어 있으므로, feature module 하는 것이 좋습니다. 기능 모듈은 단순히 특정 기능과 관련된 코드를 체계적이고 명확하게 경계합니다. 이를 특히 팀의 규모가 커짐에 따라 복잡성을 관리하는 [SOLID](https://en.wikipedia.org/wiki/SOLID) 원칙에 따라 개발하는데 도움이 됩니다.

여기 `CatsModule` 을 만들었습니다.

```typescript
@@filename(cats/cats.module)
import { Module } from '@nestjs/common';
import { CatsController } from './cats.controller';
import { CatsService } from './cats.service';

@Module({
  controllers: [CatsController],
  providers: [CatsService],
})
export class CatsModule {}
```

> **힌트** CLI를 사용하여 모듈을 생성하려면 `$ nest g module cats` 명령을 실행하기만 하면 됩니다.
> 
``역자주 : 여기서 g는 generate의 약자이다``

위에서 우리는 `cats.module.ts` 파일에 `CatsModule` 모듈을 정의하고 `cats` 디렉토리에 파일을 옮겼습니다. 마지막으로 해야할 일은 루트 모듈로 (`app.module.ts` 파일에 정의된 `AppModule`) 가져 오는 것입니다.

```typescript
@@filename(app.module)
import { Module } from '@nestjs/common';
import { CatsModule } from './cats/cats.module';

@Module({
  imports: [CatsModule],
})
export class AppModule {}
```

디렉토리 구조는 다음과 같습니다.:

<div class="file-tree">
  <div class="item">src</div>
  <div class="children">
    <div class="item">cats</div>
    <div class="children">
      <div class="item">dto</div>
      <div class="children">
        <div class="item">create-cat.dto.ts</div>
      </div>
      <div class="item">interfaces</div>
      <div class="children">
        <div class="item">cat.interface.ts</div>
      </div>
      <div class="item">cats.controller.ts</div>
      <div class="item">cats.module.ts</div>
      <div class="item">cats.service.ts</div>
    </div>
    <div class="item">app.module.ts</div>
    <div class="item">main.ts</div>
  </div>
</div>

#### 공유 모듈 (Shared modules)

`Nest`에서 모듈은 기본적으로 **singletons** 이므로, 여러 모듈간에 동일한 provider 인스턴스에 손쉽게 공유할 수 있습니다.

<figure><img src="https://docs.nestjs.com/assets/Shared_Module_1.png" /></figure>

모든 모듈은 자동으로 **shared module**이 됩니다. 한번 생성해두면 모든 모듈은 재사용할 수 있습니다. `CatsService` 인스턴스를 공유하고 싶다고 상상해보십시오.이를 위해서 우리는 먼저 `CatsService` 공급자를  `exports` 배열에 넣어서 **export** 해야 합니다.

```typescript
@@filename(cats.module)
import { Module } from '@nestjs/common';
import { CatsController } from './cats.controller';
import { CatsService } from './cats.service';

@Module({
  controllers: [CatsController],
  providers: [CatsService],
  exports: [CatsService]
})
export class CatsModule {}
```

이제 `CatsService`는 `CatsModule` 에서 접근할 수 있으며, 이를 가져오는 다른 모든 모듈과 동일한 인스턴스를 공유합니다.

<app-banner-enterprise></app-banner-enterprise>

#### Module re-exporting

위에서 볼 수 있듯이 모듈은 내부 provider 들을 내보낼 수 있습니다. 또한 가져온 모듈을 다시 내보낼 수 있습니다. 아래 예제에서 `CommonModule` 은 `CoreModule`에서 가져오고, 내보냄으로써 이를 가져오는 다른 모듈에서도 사용할 수 있습니다.

```typescript
@Module({
  imports: [CommonModule],
  exports: [CommonModule],
})
export class CoreModule {}
```

#### Dependency injection (의존성 주입)

모든 클래스는 provider 로 **주입** 할 수 있습니다. (예 : 구성목적):

```typescript
@@filename(cats.module)
import { Module } from '@nestjs/common';
import { CatsController } from './cats.controller';
import { CatsService } from './cats.service';

@Module({
  controllers: [CatsController],
  providers: [CatsService],
})
export class CatsModule {
  constructor(private catsService: CatsService) {}
}
@@switch
import { Module, Dependencies } from '@nestjs/common';
import { CatsController } from './cats.controller';
import { CatsService } from './cats.service';

@Module({
  controllers: [CatsController],
  providers: [CatsService],
})
@Dependencies(CatsService)
export class CatsModule {
  constructor(catsService) {
    this.catsService = catsService;
  }
}
```

그러나 [circular dependency](/fundamentals/circular-dependency)로 인해 클래스 자체를 provider로 주입할수 없습니다.

#### Global modules (전역 모듈)

모든 곳에서 동일한 모듈을 가져와야하는 경우 힘들수 있습니다. nest와 달리 [Angular](https://angular.io)는 `providers` 전역 범위에서 등록할 수 있습니다. 일단 정의되면 어디서나 사용 할수 있습니다. 그러나 Nest는 모듈 범위에서 캡슐화 합니다. 캡슐화 모듈을 가져오지 않는 이상 다른 곳에서는 사용할 수 없습니다.

모든 곳에서 사용할 수 있는 provider를 만드려면 (예, helpers, database connections, etc.), `@Global()` 데코레이터를 이용하여 **전역** 모듈을 만드십시오.
``역자주 : ??? ``

```typescript
import { Module, Global } from '@nestjs/common';
import { CatsController } from './cats.controller';
import { CatsService } from './cats.service';

@Global()
@Module({
  controllers: [CatsController],
  providers: [CatsService],
  exports: [CatsService],
})
export class CatsModule {}
```

`@Global()` 데코레이터는 모듈을 전역 범위로 만듦니다. 전역 모듈은 일반적으로 루트 또는 코어 모듈로 **한번만** 등록해야 합니다. 위 예제에서는 `CatsService` provider를 어디에나 사용할 수 있게 되었으므로, `CatsModule` imports 배열로 가져올 필요가 없습니다.

> **힌트** 모든 것을 글로벌하게 하는 것은 좋은 설계가 아닙니다. 필요한 사용구의 양을 줄이기 위해 글로벌 모듈을 사용할수 있습니다. `imports` 배열은 일반적으로 소비자가 모듈의 API를 사용하는 가장 기본적인 방법입니다.

#### Dynamic modules (동적 모듈)

Nest 에는 **dynamic modules** 이라는 강력한 기능이 있습니다. 이 기능을 사용하면 provider 를 동적으로 등록할고, 사용자가 지정 가능한 모듈을 쉽게 만들수 있습니다.[여기](/fundamentals/dynamic-modules)에서 동적 모듈을 광범위하게 다룹니다. 이 장에서는 모듈 소개를 완료하기 위해서 간략한 개요만 제공합니다.

다음은 `DatabaseModule` 에 대한 동적 모듈의 예제입니다.

```typescript
@@filename()
import { Module, DynamicModule } from '@nestjs/common';
import { createDatabaseProviders } from './database.providers';
import { Connection } from './connection.provider';

@Module({
  providers: [Connection],
})
export class DatabaseModule {
  static forRoot(entities = [], options?): DynamicModule {
    const providers = createDatabaseProviders(options, entities);
    return {
      module: DatabaseModule,
      providers: providers,
      exports: providers,
    };
  }
}
@@switch
import { Module } from '@nestjs/common';
import { createDatabaseProviders } from './database.providers';
import { Connection } from './connection.provider';

@Module({
  providers: [Connection],
})
export class DatabaseModule {
  static forRoot(entities = [], options?) {
    const providers = createDatabaseProviders(options, entities);
    return {
      module: DatabaseModule,
      providers: providers,
      exports: providers,
    };
  }
}
```

> 안내 **Hint** `forRoot()` method 동적 모듈을 동기식 또는 비동기식으로 (`Promise`를 통해) 반환 할 수 있습니다.

This module defines the `Connection` provider by default (in the `@Module()` decorator metadata), but additionally - depending on the `entities` and `options` objects passed into the `forRoot()` method - exposes a collection of providers, for example, repositories. Note that the properties returned by the dynamic module **extend** (rather than override) the base module metadata defined in the `@Module()` decorator. That's how both the statically declared `Connection` provider **그리고** the dynamically generated repository providers are exported from the module.

전역 범위에 동적 모듈을 등록하려면 `global` 속성을 `true`로 설정합니다.

```typescript
{
  global: true,
  module: DatabaseModule,
  providers: providers,
  exports: providers,
}
```

> **경고** 위에서 언급했듯이 모든 것을 글로벌하게 만드는 것은 **좋은 디자인**은 아닙니다.

`DatabaseModule` 는 다음과 같은 방식으로 import 할 수 있다.

```typescript
import { Module } from '@nestjs/common';
import { DatabaseModule } from './database/database.module';
import { User } from './users/entities/user.entity';

@Module({
  imports: [DatabaseModule.forRoot([User])],
})
export class AppModule {}
```

동적으로 re export 하려면 exports 배열에서  `forRoot()` 메서드 호출을 생략할 수 있다.

```typescript
import { Module } from '@nestjs/common';
import { DatabaseModule } from './database/database.module';
import { User } from './users/entities/user.entity';

@Module({
  imports: [DatabaseModule.forRoot([User])],
  exports: [DatabaseModule],
})
export class AppModule {}
```

[Dynamic modules](/fundamentals/dynamic-modules) 여기에서 동적 모듈을 더 상세하게 다룹니다.
[예제](https://github.com/nestjs/nest/tree/master/sample/25-dynamic-modules).
