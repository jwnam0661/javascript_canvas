공을 치기 위한 paddle 정의
-
> 먼저, 우리는 공을 치기 위한 paddle이 필요합니다. 이를 위해 몇가지 변수들을 정의합시다. 코드 상단에 다른 변수들과 함께 아래 변수들을 추가하세요:
```javascript
var paddleHeight = 10;
var paddleWidth = 75;
var paddleX = (canvas.width-paddleWidth)/2;
```
> 여기에서 paddle의 높이와 너비, 그리고  x 축 위에 시작 지점을 정의합니다. paddle을 스크린에 그리는 함수를 만듭시다. drawBall() 함수 아래에 다음 코드를 추가하세요:
```javascript
function drawPaddle() {
    ctx.beginPath();
    ctx.rect(paddleX, canvas.height-paddleHeight, paddleWidth, paddleHeight);
    ctx.fillStyle = "#0095DD";
    ctx.fill();
    ctx.closePath();
}
```
유저의 paddle 컨트롤
-
> paddle은 우리가 원하는 곳 어디든 그릴 수 있지만, 사용자의 컨트롤에 반응해야 합니다. — 키보드 컨트롤을 구현합시다. 다음 내용이 필요합니다.:

* 왼쪽 혹은 오른쪽 컨트롤 버튼이 눌렸는지 확인하는 두개의 변수
* keydown 과 keyup 이벤트 리스너 — 버튼이 눌렸을 때 paddle의 움직임을 조종할 수 있는 코드가 실행되어야 합니다.
* 버튼이 눌렸을 때  keydown 과 keyup 이벤트를 핸들링하는 두개의 함수
* paddle이 왼쪽 혹은 오른쪽으로 움직이는 기능

> 버튼을 누르는 것은 boolean 변수로 정의하고 초기화 합니다. 아래 코드를 변수 선언 부분에 추가하세요. :
```javascript
var rightPressed = false;
var leftPressed = false;
```
> 처음에는 컨트롤 버튼이 눌려지지 않은 상태이므로 두개의 기본값은 false 입니다. 키가 눌렸음을 인식하기 위해, 이벤트 리스너를 설정합니다. 자바스크립트 하단에 setInterval() 바로 위에 아래 코드를 추가합니다.:
```javascript
document.addEventListener("keydown", keyDownHandler, false);
document.addEventListener("keyup", keyUpHandler, false);
```
> 키보드 중 어떤 키 하나가 눌려서 keydown 이벤트가 발생하면, keyDownHandler() 함수가 실행됩니다. 두번째 리스너에도 같은 패턴이 적용됩니다: 키에서 손을 때면 keyup 이벤트가 keyUpHandler() 함수를 실행합니다 . addEventListener() 아래에 다음 코드를 추가하세요:
```javascript
function keyDownHandler(e) {
    if(e.keyCode == 39) {
        rightPressed = true;
    }
    else if(e.keyCode == 37) {
        leftPressed = true;
    }
}

function keyUpHandler(e) {
    if(e.keyCode == 39) {
        rightPressed = false;
    }
    else if(e.keyCode == 37) {
        leftPressed = false;
    }
}
```
> 키를 누르면 변수에 정보가 저장됩니다. 각 경우에 관련된 변수가 true 로 설정됩니다. 키에서 손을 때면, 변수값은 false로 되돌아갑니다.

> 두 함수 모두 e 변수로 표시되는 이벤트를 파라미터로 사용합니다. 이것으로 유용한 정보를 얻을 수 있습니다: keyCode 는 눌려진 키에 대한 정보를 가지고 있습니다. 예를 들어 키 코드 37 은 왼쪽 방향키이고 39 는 오른쪽 방향키 입니다. 만약에 왼쪽 방향키를 누르면, leftPressed 변수가 true 로 설정되고, 왼쪽 방향키에서 손을 때면 leftPressed 변수가 false로 설정됩니다. 오른쪽 방향키와 rightPressed 변수에도 동일한 패턴이 적용됩니다.

Paddle 이동 로직
-
> 이제 우리는 눌려진 키, 이벤트 리스너, 관련된 함수에 대한 정보를 저장할 변수를 가지고 있습니다. 이제 실제 코드를 사용하여 이것들을 사용하고 paddle을 화면에서 움직여봅시다. draw() 함수에서, 각각의 프레임이 렌더링 될때마다 왼쪽이나 오른쪽 방향키가 눌려졌는지 확인합니다. 코드는 아래와 같습니다:
```javascript
if(rightPressed) {
    paddleX += 7;
}
else if(leftPressed) {
    paddleX -= 7;
}
```
> 만약 왼쪽 방향키를 누르면, paddle은 좌측으로 7픽셀 움직이고, 오른쪽 방향키를 누르면, 우측으로 7픽셀 움직입니다. 잘 작동하지만, 키를 너무 오래 누르고 있으면 paddle이 캔버스 밖으로 사라집니다. 아래처럼 코드를 수정해서 paddle이 캔버스 안에서만 움직이도록 개선합니다:
```javascript
if(rightPressed && paddleX < canvas.width-paddleWidth) {
    paddleX += 7;
}
else if(leftPressed && paddleX > 0) {
    paddleX -= 7;
}
```
> paddleX 의 위치는 캔버스 왼쪽 끝 0 위치와 오른쪽 canvas.width-paddleWidth 에서 움직입니다.

> 위의 코드를 draw() 함수 아래쪽에 추가합니다.

> 이제 paddle이 화면에서 실제로 그려지도록 draw() 함수 안에서 drawPaddle() 을 호출합니다.  draw() 함수 안에  drawBall() 아래에 다음 코드를 추가합니다:
```javascript
drawPaddle();
```
