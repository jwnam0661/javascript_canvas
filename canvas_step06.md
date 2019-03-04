벽돌에 대한 변수 설정하기
-
> 이번 학습의 모든 목표는 벽돌들을 위한 코드를 2차원 배열로 동작하는 반복문을 통해 제공하는 것입니다. 그러나 먼저 우리는 가로, 세로, 행, 열 등 벽돌에 대한 값을 정의할 몇몇 변수들을 설정해야 합니다. 지난 학습에서 작성한 코드에 아래 코드를 추가해봅시다.
```javascript
var brickRowCount = 3;
var brickColumnCount = 5;
var brickWidth = 75;
var brickHeight = 20;
var brickPadding = 10;
var brickOffsetTop = 30;
var brickOffsetLeft = 30;
```
> 우리는 벽돌 배열 행과 열의 수, 그것들의 가로, 세로길이, 각 벽돌이 서로 닿지 않을 정도의 간격과 벽돌이 캔버스의 모서리에 닿지 않게 할 오프셋 변수들을 정의했습니다.

> 우리는 2차원 배열에 벽돌을 담았습니다. 배열은 열 c, 행 r, 그리고 배열의 각 객체엔 화면에 벽돌을 그릴 위치를 나타낼 x, y 위치를 가지고 있습니다. 위에서 변수를 선언한 코드 뒤에 아래 코드를 추가해봅시다.
```javascript
var bricks = [];
for(var c=0; c<brickColumnCount; c++) {
    bricks[c] = [];
    for(var r=0; r<brickRowCount; r++) {
        bricks[c][r] = { x: 0, y: 0 };
    }
}
```
벽돌을 그리는 방법
-
> 이제 배열안의 모든 벽돌을 반복해서 화면에 그려줄 함수를 만들어봅시다. 코드는 아래와 같습니다.
```javascript
function drawBricks() {
    for(var c=0; c<brickColumnCount; c++) {
        for(var r=0; r<brickRowCount; r++) {
            bricks[c][r].x = 0;
            bricks[c][r].y = 0;
            ctx.beginPath();
            ctx.rect(0, 0, brickWidth, brickHeight);
            ctx.fillStyle = "#0095DD";
            ctx.fill();
            ctx.closePath();
        }
    }
}
```
> 다시 행, 열 반복을 통해 각 벽돌의 x, y 값을 설정하고, 캔버스에 brickWidth * brickHeight 크기의 벽돌들을 그립니다. 문제는 모든 벽돌들이 좌표 (0, 0) 위치해있다는 것입니다. 우리는 약간의 연산을 통해 각 벽돌의 x, y 값을 계산해야 합니다.
```javascript
var brickX = (c*(brickWidth+brickPadding))+brickOffsetLeft;
var brickY = (r*(brickHeight+brickPadding))+brickOffsetTop;
```
> brickX는 brickWidth + brickPadding에 c를 곱하고, brickOffsetLeft를 더한 값입니다. brickY는 변수 r, brickHeight, brickOffsetTop 변수를 사용한다는 것을 제외하곤 동일합니다. 이제 모든 벽돌들을 올바른 위치에, 알맞은 간격을 두고, 캔버스 모서리로부터 오프셋 값만큼의 거리를 둔 상태로 그릴수 있게되었습니다.

> brickX와 brickY 값을 (0, 0) 대신에 좌표 값으로 할당한 후에 drawBricks 함수의 마지막 버전은 아래와 같을 것입니다. 이 코드는 drawpaddle 함수 아래에 추가해봅시다.
```javascript
function drawBricks() {
    for(var c=0; c<brickColumnCount; c++) {
        for(var r=0; r<brickRowCount; r++) {
            var brickX = (c*(brickWidth+brickPadding))+brickOffsetLeft;
            var brickY = (r*(brickHeight+brickPadding))+brickOffsetTop;
            bricks[c][r].x = brickX;
            bricks[c][r].y = brickY;
            ctx.beginPath();
            ctx.rect(brickX, brickY, brickWidth, brickHeight);
            ctx.fillStyle = "#0095DD";
            ctx.fill();
            ctx.closePath();
        }
    }
}
```
실제 벽돌을 그리기
-
> 이 학습에서 마지막으로 할 일은 drawBrick함수를 호출하는 코드를 draw함수 어딘가에, 되도록이면 시작하는 부분에, 캔버스를 초기화하는 부분과 공을 그리는 사이에 추가하는 것입니다.  아래 코드를 drawBall() 코드 위에 추가해봅시다.
```javascript
drawBricks();
```
