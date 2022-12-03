# 목차

- [flex sizing](#flex-sizing)
- [flex shorthand](#flex-shorthand)

# **flex sizing**

## **flex-basis**

요소가 한 줄로 늘어서 있을 때 `flex-basis`가 **너비의 기준**이 된다. `flex-basis`**는 main axis인 가로에 걸쳐있기** 때문이다. `flex-basis`는 요소가 배치될 때의 최초 크기이다. **main axis의 방향에 따라 width이기도 하고 height이기도** 하다.

<img src="https://user-images.githubusercontent.com/87808288/205209905-622ae648-4313-4069-a59d-ccacfae81f53.png" width="60%">  

```css
item1 {
  background: #ef9a9a;
  flex-basis: 60%;
}

item2 {
  background: #ce93d8;
  flex-basis: 30%;
}

item3 {
  background: #90caf9;
  flex-basis: 10%;
}
```

`flex-basis` 속성에서 `auto`와 함께 자주 사용하는 속성값이 `0`이다. `flex-basis` 속성의 값을 0px, 0% 등으로 설정할 수 있다.

`flex-basis`의 기본값으로 `auto`가 사용된다. `auto`는 **해당 아이템의 width 값을 사용**하는데 width를 **따로 설정하지 않으면 컨텐츠의 크기**가 된다.

```css
.item {
	flex-basis: auto; /* 기본값 */
	/* flex-basis: 0; */
	/* flex-basis: 50%; */
	/* flex-basis: 300px; */
	/* flex-basis: 10rem; */
	/* flex-basis: content; */
}
```

아래의 코드와 이미지를 살펴보면, 원래 width가 100px이 안되는 AAA와 CCC는 100px로 늘어나고, **원래 100px이 넘는 BBB는 그대로 유지**된다.

```css
.item {
	flex-basis: 100px;
}
```

<img src="https://user-images.githubusercontent.com/87808288/205210012-3c9bf324-9995-4426-ba27-1037af092ff3.png" width="90%">

원래 width를 설정하면 100px을 넘는 BBB도 100px로 맞춰진다.

## **flex-grow**

`flex-grow`는 **공간이 남아 있을 때, 요소가 그 공간을 얼마나 차지할지** 정하게 된다. `flex-grow`와 `flex-shrink`는 단위가 없다.

![https://user-images.githubusercontent.com/87808288/179657635-d4fa7489-0b00-4d53-ba29-b81099315e4c.png](https://user-images.githubusercontent.com/87808288/179657635-d4fa7489-0b00-4d53-ba29-b81099315e4c.png)

```css
#container {
  background-color: #003049;
  width: 90%;
  height: 500px;
  margin: 0 auto;
  border: 5px solid #003049;
  display: flex;
  flex-direction: row;
  justify-content: center;
}

#container div {
  width: 100px;
  height: 100px;
}
```

![https://user-images.githubusercontent.com/87808288/179657864-5f151300-ad2f-4871-ad06-05662074e29b.png](https://user-images.githubusercontent.com/87808288/179657864-5f151300-ad2f-4871-ad06-05662074e29b.png)

```css
#container {
  background-color: #003049;
  width: 90%;
  height: 500px;
  margin: 0 auto;
  border: 5px solid #003049;
  display: flex;
  flex-direction: row;
  justify-content: center;
}

#container div {
  width: 100px;
  height: 100px;
}

#container div:nth-of-type(1) {
  flex-grow: 1;
}
```

위의 이미지와 같이 `div:nth-of-type(1)`을 사용하여 `flex-grow`를 사용하면첫 번째 `<div />`의 크기를 늘려 **화면을 꽉 채울 수** 있게 된다.

![https://user-images.githubusercontent.com/87808288/179658206-c44f25fb-f568-4618-b8cc-8997bde935e4.png](https://user-images.githubusercontent.com/87808288/179658206-c44f25fb-f568-4618-b8cc-8997bde935e4.png)

```css
#container {
  background-color: #003049;
  width: 90%;
  height: 500px;
  margin: 0 auto;
  border: 5px solid #003049;
  display: flex;
  flex-direction: row;
  justify-content: center;
}

#container div {
  width: 100px;
  height: 100px;
  flex-grow: 1;
}
```

**모든** `<div />`에게 `flex-grow`를 `1`을 설정하면 **공간을 균등하게** 차지하게 된다. 창을 줄이더라도 비율은 유지된다. 하지만 이렇게 무한정으로 늘어나는 것도 좋은 것은 아니다. 그래서 **최대 넓이와 최소 넓이를 설정할 수** 있다.

```css
#container {
  background-color: #003049;
  width: 90%;
  height: 500px;
  margin: 0 auto;
  border: 5px solid #003049;
  display: flex;
  flex-direction: row;
  justify-content: center;
}

#container div {
  width: 100px;
  height: 100px;
  max-width: 200px;
  min-width: 100px;
  flex-grow: 1;
}
```

![https://user-images.githubusercontent.com/87808288/179659259-21cd3b85-7813-4074-8112-859865cdea6e.png](https://user-images.githubusercontent.com/87808288/179659259-21cd3b85-7813-4074-8112-859865cdea6e.png)

```css
#container {
  background-color: #003049;
  width: 90%;
  height: 500px;
  margin: 0 auto;
  border: 5px solid #003049;
  display: flex;
  flex-direction: row;
  justify-content: center;
}

#container div {
  width: 100px;
  height: 100px;
}

#container div:nth-of-type(1) {
  flex-grow: 1;
}

#container div:nth-of-type(5) {
  flex-grow: 2;
}
```

`flex-grow`는 다른 element에 여러 숫자를 부여할 수 있다. 첫 번째 `<div />`에는 `1`을 부여하고 두 번째 `<div />`에는 `2`를 부여하면 위의 이미지와 같이 **1 : 2의 비율로** 나타내지는 것을 볼 수 있다.

🔥 **원리**

<img src="https://user-images.githubusercontent.com/87808288/205210171-62370136-b846-43d6-beec-a479413514ed.png" width="70%">

위의 이미지는 레이아웃의 너비보다 **아이템(content)들의 width 값이 더 적어** 여백이 남게 된 것이다. 그래서 위에서 배운 `flex-grow`를 사용하면 남은 여백 공간 만큼을 채울 수 있게 된다. 아래의 코드와 이미지를 살펴보자.

```css
.item:nth-child(1) {
  flex-grow: 1;
}
.item:nth-child(2) {
  flex-grow: 1;
}
.item:nth-child(3) {
  flex-grow: 0;
}
.item:nth-child(4) {
  flex-grow: 2;
}
```

<img src="https://user-images.githubusercontent.com/87808288/205210344-39fc5cd3-70e7-4197-850e-cd7dad07febc.png" width="70%">

flex-grow를 사용하여 여백을 모두 채웠다!

<img src="https://user-images.githubusercontent.com/87808288/205210432-1b858961-1b36-4ec9-98ce-fc26947fbc4f.png" width="80%">

flex-grow 1 하나가 45px을 가지게 되었다.(flex-grow가 2라면 90px!)

flex box 안의 아이템들에 적용된 `flex-grow` 속성 값의 합(4)을 구한다. 그리고 남은 **여백을 위에서 구한 합으로 나누게** 된다(`45px`).(패딩 값은 제외한다.) 그렇게 `flex-grow` **속성 값만큼** `45px`**을 더하여** 각각의 `content` 너비가 결정되는 것이다.

## **flex-shrink**

`flex-grow`와는 반대로 **레이아웃 너비보다 아이템들의 너비 합이 커지면 그 차이 만큼 줄어드는** 기능이다. `flex-shrink` 속성은 flex-box에 `flex-wrap: wrap;`을 부여한 경우는 적용되지 않는다. 왜냐하면 `wrap`의 상황에서는 레이아웃을 넘기는 상황이 발생되지 않기 때문이다.

🔥 **원리**

```html
<div class="layout">
  <div class="flexbox">
    <div class="item">content1</div>
    <div class="item">content2</div>
    <div class="item">content3</div>
    <div class="item">content4</div>
  </div>
</div>
```

```css
.layout{
    max-width: 600px;
    margin: 0 auto;
    padding: 0;
}

.flexbox{
    display: flex;
    flex-wrap: nowrap;
    gap: 0;
    padding: 10px;
    background-color: #f0f0f0;
}

.item{
    min-height: 150px;
    flex-basis: 200px;
}

.item:nth-child(1) {
  flex-shrink: 1;
}
.item:nth-child(2) {
  flex-shrink: 0;
}
.item:nth-child(3) {
  flex-shrink: 1;
}
.item:nth-child(4) {
  flex-shrink: 2;
}
```

<img src="https://user-images.githubusercontent.com/87808288/205210525-1bb77fe0-ee53-4aa9-8aa3-d418889c79a2.png" width="70%">

.flexbox의 크기를 item들이 넘어섰다.

<img src="https://user-images.githubusercontent.com/87808288/205210591-23a571e7-14e7-498f-ab0c-e1e91c79dccf.png" width="70%">

flex-shrink를 설정한 후 .flexbox의 width 이하의 크기에서는 이렇게 설정한 만큼 줄어든다.

<img src="https://user-images.githubusercontent.com/87808288/205210672-88d8c828-e32e-4643-ade8-32cf2263335a.png" width="90%">

이렇게 초과한 여백(`800px - 580px = 220px`)을 `flex-shrink`의 합으로 나누고 `flex-shrink`의 값만큼 아이템의 `width`를 줄여준다.

# **flex shorthand**

`flex`라는 속기법이 있다. `flex`는 위의 **3가지 방법을 한 번에 표기**하는 방법이다.

```css
main .sidebar {
  background-color: purple;
  /* **flex-grow**, **shrink**, **basis** */
  flex: 1 2 300px;
  border: 2px solid white;
}
```

## flex: 1;

`flex: 1`도 자주 사용하는 속성이다. 이것의 의미는 아래의 코드와 같다.

```css
flex-grow: 1;
flex-shrink: 1;
flex-basis: 0%
```

`flex-basis`가 `0%`이므로 **점유 크기를 0**으로 만든 후 **화면 비율에 따라 유연하게 늘어나거나 줄어들게** 된다. 이러한 특성을 이용하여 **footer를 하단에 고정**할 때도 사용된다.

> 💡 **참고자료**  
*어포스트: [CSS 플렉스박스(flex) flex-grow와 flex-shrink 속성의 완벽 이해](https://blogpack.tistory.com/863)  
1분코딩: [이번에야말로 CSS Flex를 익혀보자](https://studiomeal.com/archives/197)*
>