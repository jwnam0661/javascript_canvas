충돌 감지 함수
-
> 이 모든 것을 시작하기 위해 우리는 모든 벽돌들을 순회하고 각 벽돌의 좌표를 공의 위치와 비교하는 충돌 감지 함수를 만들어야 합니다. 코드의 가독성을 향상시키기 위해 충돌 감지의 반복에서 사용할 벽돌 객체를 저장하는 b 변수를 정의할 것입니다.
```javascript
function collisionDetection() {
    for(var c=0; c<brickColumnCount; c++) {
        for(var r=0; r<brickRowCount; r++) {
            var b = bricks[c][r];
            // calculations
        }
    }
}
```
> 만약 공의 중앙이 어떤 벽돌의 범위 내에 있을 경우, 공의 방향을 바꾸게 됩니다. 공이 벽돌 안에 존재하려면, 아래 4가지 조건이 참이어야 합니다.

* 공의 x 좌표는 벽돌의 x 좌표보다 커야 한다.
* 공의 x 좌표는 벽돌의 x 좌표 + 가로 길이보다 작아야 한다.
* 공의 y 좌표는 벽돌의 y 좌표보다 커야 한다.
* 공의 y 좌표는 벽돌의 y 좌표 + 높이보다 작아야 한다.

> 이 조건을 코드로 작성해봅시다.
```javascript
function collisionDetection() {
    for(var c=0; c<brickColumnCount; c++) {
        for(var r=0; r<brickRowCount; r++) {
            var b = bricks[c][r];
            if(x > b.x && x < b.x+brickWidth && y > b.y && y < b.y+brickHeight) {
                dy = -dy;
            }
        }
    }
}
```
> 위 코드를 keyUpHandler() 함수 아래에 추가해봅시다.

충돌 후에 벽돌을 사라지게 만들기
-
> 위의 코드는 우리가 의도한대로 작동할 것이고 공은 방향을 바꿀 것입니다. 문제는 벽돌이 그대로 있다는 것입니다. 우린 이미 공이 부딪힌 벽돌을 제거하기 위한 방법을 고민해봐야 합니다. 우리는 화면에 있는 벽돌을 그릴지 결정하는 변수를 추가해서 이 문제를 해결할 수 있습니다, 벽돌을 초기화하는 코드에, status 속성을 각 벽돌 객체에 추가해봅시다. 아래와 같이 말입니다.
```javascript
var bricks = [];
for(var c=0; c<brickColumnCount; c++) {
    bricks[c] = [];
    for(var r=0; r<brickRowCount; r++) {
        bricks[c][r] = { x: 0, y: 0, status: 1 };
    }
}
```
> 다음으로 drawBricks 함수에서 벽돌들을 그리기 전에 status 속성을 확인해야 합니다. 만약 status가 1이라면 벽돌을 그리고, 만약 0이라면 이미 공이 치고간 벽돌이므로 우린 더이상 화면에 그릴 필요가 없습니다. 아래와 같이 drawBricks 함수를 수정해봅시다.
```javascript
function drawBricks() {
    for(var c=0; c<brickColumnCount; c++) {
        for(var r=0; r<brickRowCount; r++) {
            if(bricks[c][r].status == 1) {
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
}
```
충돌 감지함수에서 상태 추적 및 업데이트
-
> 이제 collisonDectection 함수에 벽돌 status 속성을 포함시켜야 합니다. 만약 벽돌이 활성 상태(status 1)이라면 충돌이 일어났는지 확인해야 합니다. 만약 충돌이 발생했다면 다시 그리지 않게 벽돌의 속성을 0으로 변경해야 합니다. collisionDetection 함수를 아래와 같이 수정합시다.
```javascript
function collisionDetection() {
    for(var c=0; c<brickColumnCount; c++) {
        for(var r=0; r<brickRowCount; r++) {
            var b = bricks[c][r];
            if(b.status == 1) {
                if(x > b.x && x < b.x+brickWidth && y > b.y && y < b.y+brickHeight) {
                    dy = -dy;
                    b.status = 0;
                }
            }
        }
    }
}
```
충돌 감지 활성화하기
-
> 마지막으로 할일은 draw함수에서 collisionDetection 함수를 호출하는 것입니다. 아래 코드를 draw 함수에, drawPaddle() 아래에 추가해봅시다.
```javascript
collisionDetection();
```
