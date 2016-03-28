
# 자바스트립트의 Math 객체(Object)

자바스크립트로 개발을 해본 개발자라면, ```Math.abs```를 이용하여 절대값을 구하거나 ```Math.round```를 이용하여 반올림을 하는 등의 경험은 대부분 있을 것이다.
이와 같이 자바스크립트에서는 수식을 처리하기 위해 Math객체를 사용한다.
이번 글에서는 Math객체의 특징과 주의해야할 부분, 그리고 활용 방법에 대해 이야기 하겠다.
<br><br>

### Math객체는 생성자가 아니다

Math객체는 다른 글로벌 객체외 다르게 생성자가 아니다. 다시말해, new 연산자를 사용하여 객체를 만들 수 없다.
하지만 Math객체의 모든 메소드(methods)와 프로퍼티(properties)는 static이기 때문에, Math객체에 바로 접근하여 사용 가능하다. 
다음은 Date와 Math의 사용 방법의 차이를 보여주는 예제이다.

```javascript
// Date
var date = new Date();
date.getTime();

// Math
var absValue = Math.abs(-20);

```

Math객체가 제공하는 프로퍼티는 ```Math.PI```, ```Math.E```와 같이 수학식에서 사용하는 상수들로 구성되어있다.<br>

자세한 내용은 [MDN Math](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Math),
[MSDN Math](https://msdn.microsoft.com/ko-kr/library/b272f386(v=vs.94).aspx)등의 문서를 참조하길 바란다.
<br><br>

### 삼각연산에서의 각도

Math 객체는 다양한 삼각함수들(Math.sin, Math.cos,...)을 지원한다. 이들을 사용하면 파이 그래프 호의 좌표나 회전된 상자의 높이 등을 계산할 수 있다.
이들은 공통적으로 각도 값을 인자로 받게 되는데, 이때 입력하는 각도는 우리가 흔히 사용하는 도(degree)가 아닌 라디안(radian) 값을 필요로 한다.

> ##### 도(degree)와 라디안(radian)
> 도는 우리가 흔하게 사용하는 각도를 말하며 원을 360등분한 60분법을 사용한다.<br>
> 라디안은 원의 반지름과 같은 길이를 갖는 호에 대응하는 중심각의 크기를 나타내는 호도법을 사용한다.<br><br>
>![degree](https://upload.wikimedia.org/wikipedia/commons/thumb/f/f3/%C4%90%E1%BB%99_%28g%C3%B3c%29-Degree_%28angle%29.jpg/220px-%C4%90%E1%BB%99_%28g%C3%B3c%29-Degree_%28angle%29.jpg)
>  &nbsp;  &nbsp;  &nbsp;  &nbsp;  &nbsp;  &nbsp;  &nbsp;  &nbsp;  &nbsp;
>![radian](https://upload.wikimedia.org/wikipedia/commons/thumb/1/15/Angle_radian.svg/200px-Angle_radian.svg.png)<br>
> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; 도(degree)
> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;
> &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; 라디안(radian)


도 정보만 알고있는 경우 라디안으로 변환해야 하는데, 다음과 같은 변환공식을 사용해 변환할 수 있다.

```javascript
var degree = 30;
var radian = degree * Math.PI / 180;
```

역으로 변환하고 싶으면 다음과 같이 처리하면 된다.
```javascript
var radian = 0.5235987755982988;
var degree = radian * 180 / Math.PI;
```

Math객체의 삼각함수들을 사용하려면 도(degree)가 아닌 라디안(radian)을 사용해야 함을 잊지 말자.
<br><br>

### 소수점 이하 버리기

Math 객체의 라운딩 함수(```Math.ceil```, ```Math.round```, ```Math.floor```)들은 당연한 이야기지만 수학의 처리방식을 따른다.<br>
예를 들어, ```Math.floor```(버림)는 양수 1.12대해서는 1 이라는 결과값을 -1.12에 대해서는 -2라는 결과값을 주게 된다.
이는 ```Math.ceil```, ```Math.round```에도 동일하게 적용된다.

```javascript
// 올림
console.log(Math.ceil(1.12)); // 2;
console.log(Math.ceil(-1.12)); // -1;

// 반올림
console.log(Math.round(1.12)); // 1;
console.log(Math.round(-1.12)); // -2;

// 버림
console.log(Math.floor(1.12)); // 1;
console.log(Math.floor(-1.12)); // -2;
```

이와 같은 이유로 인해 Math객체를 이용하여 소수점 이하 버리기를 하고 싶은 경우에는 음수, 양수 여부에 따라서 달리 처리해주어야 한다.

```javascript
function truncateDecimal (value) {
    var int;

    if (value < 0) {
        int = Math.ceil(value);
    } else {
        int = Math.ceil(value);
    }
    
    return int;
}

console.log(truncateDecimal(3.14159)); // 3
console.log(truncateDecimal(-3.14159)); // -3
```

es2015(es6)에서는 이와 같은 기능을 지원하는 Math.trunc를 제공한다.
```javascript
console.log(Math.trunc(3.14159)); // 3
console.log(Math.trunc(-3.14159)); // -3
```

parseInt()에 radix 10을 사용하면 Math.trunc와 동일한 결과를 얻을 수 있다.
```javascript
console.log(parseInt(-3.14159, 10)); // -3
```
그러나 parseInt는 문자열(string)을 숫자(number)로 변환하는 용도로 쓰이고 있기 때문에,
입력값이 숫자인 경우에는 불필요한 문자열 형변환을 하게된다. (숫자 --> 문자열 --> 숫자)<br>
이는 아래와 같은 의도하지 않은 결과가 나타나게 하기도 한다.

```javascript
var num = 1000000000000000000000;
parseInt(num, 10) // 1;
```
1. parseInt 과정에서 num값이 문자열로 변함
  * 1000000000000000000000 --> '1e+21'
1. 문자열 '1e+21'로 parseInt 수행
  * '1e+21' --> 1
<br><br>


### 소수점 n번째 자리까지 표시하기

개발을 하다보면 숫자형을 유지한채 소수점 n번째 자리까지만 소수점을 표시해야 하는 경우가 있는데,<br>
이때 ```Math.round```와 ```Math.pow```를 이용하여 처리할 수 있다.

```javascript

function truncateDecimalUntilLimit(number, limit) {
    var rounded = Math.pow(10, limit);
    
    return Math.round(number * rounded) / rounded;
}

console.log(truncateDecimalUntilLimit(1.4142, 2)); // 1.41
```

```toFixed```와 ```parseFloat```을 이용하는 방법도 있다.

```javascript
console.log(parseFloat(1.4142.toFixed(2))); // 1.41
```

Math객체를 활용하는것이 형변환을 하지 않기 때문에, toFixed를 사용하는 것 보다 성능면에서는 이득을 준다.
다음은 두가지 방법의 성능 테스트 예제다.<br>
http://jsperf.com/truncate-decimal-until-limit

### 배열의 min, max 구하기

자바스크립트에서 배열을 사용하다보면 배열중에 최대값이나 최소값을 얻고 싶은 경우가 있다.<br>
이런 경우에는 ```Math.min```, ```Math.max```를 활용할 수 있다.

```Math.min```, ```Math.max```는 전달된 인자들 중 최대값과 최소값을 구할 수 있는 함수이다.
```javascript
var min = Math.min(5, 10),
    max = Math.max(5, 10);

console.log(min) // 5
console.log(max) // 10
```

이 두 함수를 apply를 이용하여 호출하면, 배열의 min, max를 구할 수 있다.
```javascript

var arr = [10, 3, 2, 8, 4, 6, 5],
    min = Math.min.apply(null, arr),
    max = Math.max.apply(null, arr);
    
console.log(min) // 2
console.log(max) // 10
```

### 원하는 범위의 랜덤 숫자 구하기

```Math.random```이라는 함수는 0 ~ 1 사이의 숫자를 랜덤하게 반환한다.<br>
```Math.random```과 ```Math.floor``를 이용하면, 원하는 범위의 랜덤 숫자를 구할 수 있다.

```javascript

function getRandom(start, end) {
    return start + (Math.floor(Math.random() * (end - start + 1)));
}

getRandom(1, 10); // 1 ~ 10사이의 random 숫자 반환
```




### 결론

지금까지 Math객체와 몇가지 활용 방법에 대해 이야기해 보았다. 요약하면 다음과 같다.

* Math객체는 생성자가 아니며, Math객체의 메소드와 프로퍼티는 static이다.
* Math객체에서 제공하는 메소드 들을 사용할 때에는 사용방법을 정확하게 알고 사용해야 한다.
* 숫자형을 다룰 때에는 형변환이 필요없는 Math객체를 활용하는 것이 좋다.
* Math객체를 활용하면 자주사용하는 기능에 대해 간단하게 구현할 수 있다.

여기에 정리된 내용이, Math객체를 사용하는 분들에게 조금이라도 도움이 되길 바란다.

<br><br>


##### 참고
* https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math
* https://msdn.microsoft.com/ko-kr/library/b272f386(v=vs.94).aspx
* http://www.w3schools.com/jsref/jsref_obj_math.asp
* https://pawelgrzybek.com/rounding-and-truncating-numbers-in-javascript/
* http://darkpgmr.tistory.com/26
* https://ko.wikipedia.org/wiki/%EB%9D%BC%EB%94%94%EC%95%88
* 나라얀 프루스티, ECMAScript 6 길들이기(2106)



이 외에도 새롭게 추가된 메소드들이 궁금하다면 [MDN Math](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Math)를 참고하길 바란다.



