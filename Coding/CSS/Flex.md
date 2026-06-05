# Flex

## 1. Flex란?
`flex`는 HTML 요소들을 가로 또는 세로 방향으로 쉽게 배치하기 위해 사용하는 CSS 레이아웃 방식이다.
<br>
부모 요소에 `display: flex;`를 적용하면, 그 안에 있는 자식 요소들을 원하는 방향과 간격으로 정렬할 수 있다.
```css
.container{
    display: flex;
}
```
```html
<div class="container">
    <div class="item">1</div>
    <div class="item">2</div>
    <div class="item">3</div>
</div>
```
위 코드에서 `.container`는 flex를 적용하는 부모 요소이고, `.item`들은 정렬 대상이 되는 자식 요소이다.

---

## 2. Flex를 사용하는 이유
`flex`를 사용하면 요소 배치를 훨씬 쉽게 처리할 수 있다.
<br>
주로 아래와 같은 상황에서 사용한다.
- 메뉴를 가로로 정렬할 때
- 버튼 여러 개를 일정 간격으로 배치할 때
- 박스를 가운데 정렬할 때
- 카드 목록을 한 줄 또는 여러 줄로 배치할 때
- 화면 크기에 따라 요소를 유연하게 배치할 때

예전에는 `float`,`position`,`margin`등을 조합해야 했지만 flex를 사용하면 더 간단하게 정렬할 수 있다.

---

## 3. Flex 속성 종류

Flex 속성은 크게 **부모 요소에 작성하는 속성**과 **자식 요소에 작성하는 속성**으로 나눌 수 있다.

부모 요소는 자식 요소들을 감싸는 컨테이너 역할을 한다.
따라서 정렬 방향, 줄바꿈, 전체 정렬 방식 같은 속성은 부모 요소에 작성한다.

자식 요소는 실제로 배치되는 대상이다.
따라서 각 자식의 크기 비율, 순서, 개별 정렬 방식은 자식 요소에 작성한다.

```html
<div class="container">
  <div class="item">1</div>
  <div class="item">2</div>
  <div class="item">3</div>
</div>
```

```css
.container {
  display: flex;
  justify-content: center;
}

.item {
  flex: 1;
}
```

위 코드에서 `.container`에는 부모 속성을 작성하고, `.item`에는 자식 속성을 작성한다.

---

### 부모 요소에 작성하는 Flex 속성

부모 요소에 작성하는 대표적인 속성은 아래와 같다.

```css
.container {
  display: flex;
  flex-direction: row;
  justify-content: center;
  align-items: center;
  flex-wrap: wrap;
  gap: 20px;
}
```

---

### 1) display: flex

```css
display: flex;
```

`display: flex;`는 해당 요소를 flex 컨테이너로 만드는 속성이다.

이 속성을 부모 요소에 적용해야 자식 요소들이 flex 규칙에 따라 배치된다.

```css
.container {
  display: flex;
}
```

```text
기본 결과

[1] [2] [3]
```

`display: flex;`를 적용하면 자식 요소들은 기본적으로 가로 방향으로 배치된다.

---

### 2) flex-direction

```css
flex-direction: row;
 row | column | row-reverse | column-reverse 
```

`flex-direction`은 내부 요소들의 정렬 방향을 정하는 속성이다.

| 값                | 설명                       |
| ---------------- | ------------------------ |
| `row`            | 기본값, 가로 방향으로 왼쪽에서 오른쪽 정렬 |
| `column`         | 세로 방향으로 위에서 아래 정렬        |
| `row-reverse`    | 가로 방향으로 오른쪽에서 왼쪽 정렬      |
| `column-reverse` | 세로 방향으로 아래에서 위 정렬        |

```css
.container {
  display: flex;
  flex-direction: row;
}
```

```text
row

[1] [2] [3]
```

```css
.container {
  display: flex;
  flex-direction: column;
}
```

```text
column

[1]
[2]
[3]
```

```css
.container {
  display: flex;
  flex-direction: row-reverse;
}
```

```text
row-reverse

[3] [2] [1]
```

---

### 3) justify-content

```css
justify-content: center;
 flex-start | center | flex-end | space-between | space-around | space-evenly 
```

`justify-content`는 주축 방향으로 자식 요소들을 정렬하는 속성이다.

기본값인 `flex-direction: row;` 상태에서는 가로 방향 정렬을 담당한다.

| 값               | 설명                         |
| --------------- | -------------------------- |
| `flex-start`    | 시작 지점에 붙여서 정렬              |
| `center`        | 가운데 정렬                     |
| `flex-end`      | 끝 지점에 붙여서 정렬               |
| `space-between` | 양 끝에 붙이고 요소 사이 간격을 동일하게 배치 |
| `space-around`  | 각 요소의 양쪽 간격을 동일하게 배치       |
| `space-evenly`  | 요소 사이와 양 끝 간격을 모두 동일하게 배치  |

```css
.container {
  display: flex;
  justify-content: center;
}
```

```text
center

|---------------- container ----------------|
                 [1] [2] [3]
```

```css
.container {
  display: flex;
  justify-content: space-between;
}
```

```text
space-between

|---------------- container ----------------|
[1]                 [2]                 [3]
```

---

### 4) align-items

```css
align-items: center;
 stretch | flex-start | center | flex-end | baseline 
```

`align-items`는 교차축 방향으로 자식 요소들을 정렬하는 속성이다.

기본값인 `flex-direction: row;` 상태에서는 세로 방향 정렬을 담당한다.

| 값            | 설명                 |
| ------------ | ------------------ |
| `stretch`    | 기본값, 부모 높이에 맞게 늘어남 |
| `flex-start` | 위쪽 정렬              |
| `center`     | 세로 가운데 정렬          |
| `flex-end`   | 아래쪽 정렬             |
| `baseline`   | 글자 기준선에 맞춰 정렬      |

```css
.container {
  display: flex;
  align-items: center;
  height: 200px;
}
```

```text
align-items: center

|---------------- container ----------------|
|                                           |
|                 [1] [2] [3]               |
|                                           |
```

---

### 5) flex-wrap

```css
flex-wrap: nowrap;
 nowrap | wrap | wrap-reverse 
```

`flex-wrap`은 자식 요소들이 부모 영역을 넘어갈 때 줄바꿈을 할지 정하는 속성이다.

| 값              | 설명                   |
| -------------- | -------------------- |
| `nowrap`       | 기본값, 줄바꿈하지 않음        |
| `wrap`         | 공간이 부족하면 다음 줄로 내려감   |
| `wrap-reverse` | 공간이 부족하면 반대 방향으로 줄바꿈 |

```css
.container {
  display: flex;
  flex-wrap: wrap;
}
```

```text
wrap

|------------- container -------------|
[1] [2] [3] [4]
[5] [6]
```

---

### 6) gap

```css
gap: 20px;
```

`gap`은 자식 요소들 사이의 간격을 설정하는 속성이다.

```css
.container {
  display: flex;
  gap: 20px;
}
```

```text
gap: 20px

[1]    [2]    [3]
```

예전에는 요소 사이 간격을 만들 때 `margin`을 사용했지만, flex에서는 `gap`을 사용하면 더 간단하게 간격을 줄 수 있다.

---

### 7) align-content

```css
align-content: center;
 stretch | flex-start | center | flex-end | space-between | space-around 
```

`align-content`는 여러 줄이 생겼을 때, 줄 전체를 교차축 방향으로 어떻게 배치할지 정하는 속성이다.

단, `flex-wrap: wrap;`처럼 여러 줄이 생긴 상황에서 의미가 있다.

| 값               | 설명                             |
| --------------- | ------------------------------ |
| `stretch`       | 기본값, 줄들이 부모 높이를 채우도록 늘어남       |
| `flex-start`    | 여러 줄을 위쪽에 정렬                   |
| `center`        | 여러 줄을 가운데 정렬                   |
| `flex-end`      | 여러 줄을 아래쪽에 정렬                  |
| `space-between` | 첫 줄과 마지막 줄을 양 끝에 배치하고 사이 간격 동일 |
| `space-around`  | 각 줄 주변 간격을 동일하게 배치             |

```css
.container {
  display: flex;
  flex-wrap: wrap;
  align-content: center;
  height: 300px;
}
```

```text
align-content: center

|------------- container -------------|
|                                     |
|        [1] [2] [3]                  |
|        [4] [5] [6]                  |
|                                     |
```

---

### 자식 요소에 작성하는 Flex 속성

자식 요소에 작성하는 대표적인 속성은 아래와 같다.

```css
.item {
  flex: 1;
  order: 1;
  align-self: center;
}
```

---

### 8) flex-grow

```css
flex-grow: 1;
```

`flex-grow`는 부모 요소 안에 남는 공간이 있을 때, 자식 요소가 그 공간을 얼마나 차지할지 정하는 속성이다.

기본값은 `0`이다.

| 값      | 설명                   |
| ------ | -------------------- |
| `0`    | 남는 공간을 차지하지 않음       |
| `1` 이상 | 남는 공간을 비율에 따라 나누어 가짐 |

```css
.item1 {
  flex-grow: 1;
}

.item2 {
  flex-grow: 2;
}
```

```text
flex-grow

|------------- container -------------|
[ item1      ][ item2                ]
```

`item1`은 남는 공간을 1만큼, `item2`는 2만큼 차지한다.
따라서 `item2`가 `item1`보다 더 넓어진다.

---

### 9) flex-shrink

```css
flex-shrink: 1;
```

`flex-shrink`는 부모 공간이 부족할 때 자식 요소가 얼마나 줄어들지 정하는 속성이다.

기본값은 `1`이다.

| 값      | 설명         |
| ------ | ---------- |
| `0`    | 줄어들지 않음    |
| `1`    | 기본 비율로 줄어듦 |
| `2` 이상 | 더 많이 줄어듦   |

```css
.item1 {
  flex-shrink: 0;
}

.item2 {
  flex-shrink: 1;
}
```

`item1`은 줄어들지 않고, `item2`는 공간이 부족하면 줄어든다.

---

### 10) flex-basis

```css
flex-basis: 200px;
```

`flex-basis`는 자식 요소의 기본 크기를 정하는 속성이다.

```css
.item {
  flex-basis: 200px;
}
```

`flex-direction: row;`일 때는 기본 너비처럼 동작하고, `flex-direction: column;`일 때는 기본 높이처럼 동작한다.

---

### 11) flex

```css
flex: 1 1 200px;
```

`flex`는 아래 세 속성을 한 번에 작성하는 단축 속성이다.

```css
flex: flex-grow flex-shrink flex-basis;
```

예를 들어 아래 코드는:

```css
.item {
  flex: 1 1 200px;
}
```

다음과 같은 의미이다.

```css
.item {
  flex-grow: 1;
  flex-shrink: 1;
  flex-basis: 200px;
}
```

실무에서는 자식 요소들이 같은 너비를 가지도록 만들 때 아래처럼 자주 사용한다.

```css
.item {
  flex: 1;
}
```

```text
flex: 1

|------------- container -------------|
[    1    ][    2    ][    3    ]
```

자식 요소들이 같은 비율로 공간을 나누어 가진다.

---

### 12) order

```css
order: 1;
```

`order`는 HTML 작성 순서와 다르게 화면에 표시되는 순서를 바꾸는 속성이다.

기본값은 `0`이다.

```css
.item1 {
  order: 2;
}

.item2 {
  order: 1;
}

.item3 {
  order: 3;
}
```

```text
HTML 순서:  [1] [2] [3]
화면 순서:  [2] [1] [3]
```

`order` 값이 낮을수록 먼저 배치된다.

---

### 13) align-self

```css
align-self: flex-end;
 auto | flex-start | center | flex-end | stretch | baseline 
```

`align-self`는 특정 자식 요소 하나만 교차축 방향으로 다르게 정렬할 때 사용한다.

부모에 `align-items`가 있어도, 특정 자식에게 `align-self`를 주면 해당 요소만 따로 정렬할 수 있다.

```css
.container {
  display: flex;
  align-items: flex-start;
  height: 200px;
}

.item2 {
  align-self: flex-end;
}
```

```text
align-self

|------------- container -------------|
[1]        [3]


     [2]
```

`item2`만 아래쪽으로 정렬된다.

---

## 4. 자주 사용하는 Flex 조합

### 1) 가로 가운데 정렬

```css
.container {
  display: flex;
  justify-content: center;
}
```

### 2) 세로 가운데 정렬

```css
.container {
  display: flex;
  align-items: center;
}
```

### 3) 가로 + 세로 완전 가운데 정렬

```css
.container {
  display: flex;
  justify-content: center;
  align-items: center;
}
```

### 4) 요소 사이 간격 동일하게 배치

```css
.container {
  display: flex;
  justify-content: space-between;
}
```

### 5) 카드 목록 줄바꿈 배치

```css
.container {
  display: flex;
  flex-wrap: wrap;
  gap: 20px;
}
```

---

## 5. 한줄요약

`flex`는 부모 요소에 `display: flex;`를 적용한 뒤, 부모 속성으로 전체 정렬 방향과 배치를 정하고, 자식 속성으로 각 요소의 크기·순서·개별 정렬을 조절하는 CSS 레이아웃 방식이다.
