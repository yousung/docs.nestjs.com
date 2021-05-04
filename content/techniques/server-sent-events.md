### Server-Sent Events

Server-Sent Events (SSE) 클라이언트가 HTTP 연결을 통해 서버에서 자동 업데이트를 받을 수 있는 서버 PUSH 기술입니다. 각 알림은 한쌍의 줄 바꿈으로 끝나는 텍스트 블록을 전송합니다 (더 배우려면 [here](https://developer.mozilla.org/en-US/docs/Web/API/Server-sent_events)).

#### 사용법

(**콘트롤러 클레스** 내에서 등록된 경로)에서 `@Sse()` 데코레이터를 등록하여 SSE 를 활성화 할 수 있습니다.

```typescript
@Sse('sse')
sse(): Observable<MessageEvent> {
  return interval(1000).pipe(map((_) => ({ data: { hello: 'world' } })));
}
```

> info **힌트** `@Sse()` 데코레이터는 `@nestjs/common`, 안에  `Observable`, `interval`, `and map` 와 같이 있는 `rxjs`  패키지입니다.

> warning **경고** SSE는 `Observable` 스트림을 반환합니다.

위의 예제에서 `sse` 실시간 업데이트를 전파할 수 있는 경로를 정의했습니다. 이러한 이벤트는 [EventSource API](https://developer.mozilla.org/en-US/docs/Web/API/EventSource)를 사용하여 수신할 수 있습니다..

`sse` 메서드는 `MessageEvent`를 `Observable` 반환하여 여러개 방출합니다.  (이 예제에서는 `MessageEvent`를 매초마다 새로 방출합니다).  `MessageEvent` 객체는 인터페이스와 일치하도록 준수 해야합니다.

```typescript
export interface MessageEvent {
  data: string | object;
  id?: string;
  type?: string;
  retry?: number;
}
```

이를 통해 클라이언트 어플리케이션에서 `EventSource` 생성하여, `/sse` 경로 (`@Sse()` 데코레이터 경로와 일치하는 endpoint ) 생성자 인수로 전달 할수 있습니다.

`EventSource` 인스턴스는 `text/event-stream` 형식으로 HTTP 서버에 지속적으로 연결합니다, 연결된 연결을 닫을 때에는 `EventSource.close()` 을 사용합니다.

연결이 열리면, 서버에서 들어오는 메세지는 이벤트형식으로 들어옵니다. 들어오는 메세지에 이벤트 필드가 있는 경우, 트리거된 이벤트는 이벤트 필드와 동일합니다. 이벤트 필드가 없으면 일반 `message` 이벤트가 발행됩니다 ([source](https://developer.mozilla.org/en-US/docs/Web/API/EventSource)).

```javascript
const eventSource = new EventSource('/sse');
eventSource.onmessage = ({ data }) => {
  console.log('New message', JSON.parse(data));
};
```

#### 예제

작업된 예제는 [here](https://github.com/nestjs/nest/tree/master/sample/28-sse) 에서 확인할 수 있습니다.
