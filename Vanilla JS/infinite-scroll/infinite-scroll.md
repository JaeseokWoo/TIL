# Infinite Scroll

Vanilla JS로 무한 스크롤을 구현할 일이 생겨 구현 방법에 대해서 알아보았다.

구현 방법에는 스크롤 이벤트와 인터섹션 옵저버를 이용한 방법을 공부하였다.

## 1. Scroll event

첫 번째는 방법은 스크롤 이벤트이다.

스크롤이 마지막에 오면 요소를 추가하는 식으로 구현할 수 있다.

![https://media.vlpt.us/images/dev-tinkerbell/post/9d5e3def-4fc5-4208-9b36-39de7b3dcb0e/velog.png](https://media.vlpt.us/images/dev-tinkerbell/post/9d5e3def-4fc5-4208-9b36-39de7b3dcb0e/velog.png)

```jsx
/*
	window.innerHeight: 사용자가 보고 있는 브라우저의 높이
	window.scrollTY: 스크롤 y값
	document.body.offsetHeight: 페이지 전체의 높이
*/

document.addEventListener('scroll', () => {
	if((window.innerHeight + window.scrollY) >= document.body.offsetHeight) {
		// 요소 추가하는 코드 ex)
		// const li = document.createElement('li');
		// ul.appendChild(li);
	}
});
```

요소가 추가되는 동작은 잘되지만 스크롤이 예민해서 그런지 제대로 작동을 안하는 듯 하다.

## 2. intersection observer

인터섹션 옵저버는 대상을 등록시켜 그 대상이 상위 요소 혹은 최상위 document인 viewport와의 교차 영역에 대한 변화를 감지할 수 있도록 하는 방법이다.

### 간단한 문법

1. 인터섹션 옵저버 생성
    
    ```jsx
    const io = new IntersectionObserver(callback, options);
    ```
    
2. 감시할 요소 설정
    
    ```jsx
    io.observe(target);
    ```
    
3. callback 함수
    
    ```jsx
    new IntersectionObserver((entries, observer) => {}, options)
    ```
    

### methods

- observe(): 대상 요소 관찰
- unobserve(): 대상 요소 관찰 중지
- disconnect(): intersectionObserver 인스턴스가 관찰하는 모든 요소의 관찰 중지

```jsx
const io = new IntersectionObserver((entry, observer) => {
	const ioTarget = entry[0].target;
	if (entry[0].isIntersecting) { // viewport에 target이 보이면
		io.unobserve(ioTarget); // 현재 target 감시 취소
		
		//새로운 요소 추가 ex)
		// const li = document.createElement('li');
		// ul.appendChild(li); 
		io.observe(li); // 새로 추가한 요소 감시
	}
});

io.observe(target);
```

첫 번째의 Scroll event보다 안정적으로 요소가 출력되는 것을 확인할 수 있었다. 

## 참조

[바닐라 JS로 무한스크롤 구현을 위한 베이직 공부](https://velog.io/@dev-tinkerbell/%EB%AC%B4%ED%95%9C%EC%8A%A4%ED%81%AC%EB%A1%A4-%EA%B5%AC%ED%98%84%EB%B0%A9%EB%B2%95)

## 추가 공부(Intersection Observer)

[Intersection Observer - 요소의 가시성 관찰](https://heropy.blog/2019/10/27/intersection-observer/)