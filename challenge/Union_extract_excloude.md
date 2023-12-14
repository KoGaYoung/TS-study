## 문제
<img width="666" alt="image" src="https://github.com/KoGaYoung/TS-study/assets/36693355/33c584fa-7c05-425c-a7ae-d535ca84063b">

## 풀이
~~~typescript
// Extract<T, U> 는 T와 U가 공유하는 타입들을 추출합니다.
type type1 = string | number | boolean;
type type2 = number | boolean;

type commonType = Extract<type1, type2>; // number | boolean

// (구성요소 뽑아내기) 풀이
import {Equal, Expect} from "../../helper";

export type Event =
    | {
    type: "click";
    event: MouseEvent;
}
    | {
    type: "focus";
    event: FocusEvent;
}
    | {
    type: "keydown";
    event: KeyboardEvent;
};

type ClickEvent = Extract<Event, {type: 'click'}>;

type tests = [Expect<Equal<ClickEvent, { type: "click"; event: MouseEvent }>>];
~~~

## 문제 
<img width="629" alt="image" src="https://github.com/KoGaYoung/TS-study/assets/36693355/4ca02a16-6f44-4d83-b819-e9aa1476a322">

## 풀이
~~~typescript
import { Equal, Expect } from '../../helper';

export type Event =
  | {
      type: 'click';
      event: MouseEvent;
    }
  | {
      type: 'focus';
      event: FocusEvent;
    }
  | {
      type: 'keydown';
      event: KeyboardEvent;
    };

type NonKeyDownEvents = Exclude<Event, { type: 'keydown' }>;

// keydown과 click, focus를 포함해야함. keydown은 포함하지 말아야함
type tests = [
  Expect<
    Equal<
      NonKeyDownEvents,
      | { type: 'click'; event: MouseEvent }
      | { type: 'focus'; event: FocusEvent }
    >
  >
];

~~~
