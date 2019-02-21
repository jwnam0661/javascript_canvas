게임의 HTML
-
> HTML문서 구조는 꽤 간단합니다. 게임은 <canvas> 엘리먼트에 렌더링됩니다. 텍스트 에디터로 새로운 HTML 문서를 생성하여 index.html로 저장하고, 아래 코드를 추가하세요.
```javascript
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8" />
    <title>Gamedev Canvas Workshop</title>
    <style>
    	* { padding: 0; margin: 0; }
    	canvas { background: #eee; display: block; margin: 0 auto; }
    </style>
</head>
<body>

<canvas id="myCanvas" width="480" height="320"></canvas>

<script>
	// JavaScript 코드가 여기에 들어갈 것입니다.
</script>

</body>
</html>
```
> <head> 에는 문자셋이 정의되어 있고, <title>과 몇가지 기본적인 CSS가 있습니다. <body>는 <canvas> 와 <script>를 포함하고 있습니다. <canvas>에는 게임이 렌더되고, <script>에는 JavaScript가 들어갑니다. <canvas>엘리먼트는 쉽게 참조하기 위해 id로 myCanvas를 갖고 있고, 480픽셀의 길이와 320픽셀의 높이를 갖도록 되어있습니다. 우리가 이 튜토리얼에서 작성하게될 모든 JavaScript 코드는 <script>와  </script> 태그 사이에 들어가게 됩니다.

캔버스 기본
-
> 실제로 <canvas>엘리먼트 위에 그래픽을 렌더링하기 위해서는 JavaScript로 참조할 수 있게 만들어야 합니다. 다음 코드를 여러분의 <script> 태그 다음에 추가하세요..
```javascript
var canvas = document.getElementById("myCanvas");
var ctx = canvas.getContext("2d");
```
> <canvas> 엘리먼트에 대한 참조를 canvas 변수에 저장하였습니다. 그러고 나서는 캔버스에 그리기 위해 실질적으로 사용되는 도구인 2D rendering context를 ctx 변수에 저장하고 있습니다.

> 캔버스에 빨간색 네모를 그리는 짧은 예제 코드를 작성해봅시다. 바로 직전의 JavaScript 코드 아래에 다음 코드를 추가하고 index.html을 브라우저에서 열어 보세요.
```javascript
ctx.beginPath();
ctx.rect(20, 40, 50, 50);
ctx.fillStyle = "#FF0000";
ctx.fill();
ctx.closePath();
```
> beginPath()와 closePath()메소드 사이에 모든 명령어가 들어갑니다. 우리는 rect()를 이용해서 직사각형을 정의했는데, 처음 두 값들은 캔버스의 좌상단 모서리로 부터의 좌표를 의미하고, 나머지 두 값은 직사각형의 너비와 높이를 의미합니다. 위 코드에서 직사각형은 캔버스 좌측에서 20픽셀 떨어져있고, 캔버스 상단에서 40픽셀만큼 아래로 떨어져 있습니다. 그리고 너비와 높이는 각각 50픽셀로 설정되어 완벽한 정사각형으로 그려지고 있습니다. fillStyle은  fill()메소드에서 칠해질 색상 값을 갖게 됩니다. 위 코드에서는 사각형을 빨간색으로 칠하고 있습니다.

> 직사각형만 그릴 수 있는 것은 아닙니다. 이번에는 초록색 원을 그려보겠습니다. 아래의 코드를 여러분의 JavaScript의 마지막에 추가하고, 저장한 이후에 페이지를 새로고침 해보세요.
```javascript
ctx.beginPath();
ctx.arc(240, 160, 20, 0, Math.PI*2, false);
ctx.fillStyle = "green";
ctx.fill();
ctx.closePath();
```
> 위 코드에서 볼 수 있듯이beginPath()와 closePath()메소드가 다시 나왔습니다. 그 사이에, 가장 중요한 부분인 arc() 메소드가 있습니다. 이 메소드는 6개의 파라미터를 갖습니다.

• 원의 중심을 가리키는 x와 y좌표
• 원의 반지름
• 시작 각도와 끝 각도(원을 그릴 때 시작과 끝이되는 각도이며, 라디안 값입니다.)
• 그리는 방향(false를 넣으면 시계방향으로 그려집니다. 기본 값이나 true를 넣으면 반 시계방향으로 그려집니다.) 이 파라미터는 옵션입니다.

> fillStyle속성은 이전과 조금 달라 보이는데, 이는 CSS에서 색상을 표현하는 여러가지 방법 중 하나입니다. 색상을 표현할 때, 16진수로 색상값을 표현하거나, 색상 키워드를 사용하거나, rgba() 함수를 사용하거나 그외에 다른 색상 메소드를 사용할 수 있습니다.

> fill()을 사용해서 원에 색상을 채울 수 있다면,stroke()를 이용하면 원의 외곽선에 색상을 부여할 수 있습니다.
```javascript
ctx.beginPath();
ctx.rect(160, 10, 100, 40);
ctx.strokeStyle = "rgba(0, 0, 255, 0.5)";
ctx.stroke();
ctx.closePath();
```
> 위 코드는 비어있는 파란색 외곽선으로 된 원을 그립니다. rgba() 함수의 알파 채널 값 때문에 파란색은 반투명하게 표현됩니다.
