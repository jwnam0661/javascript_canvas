게임 오버 기능 적용하기
-
> '게임 오버' 기능을 만들어봅시다. 아래에 세 번째 학습에서 작성한, 벽에서 공을 반사시키는 코드의 일부가 있습니다.
```javascript
if(x + dx > canvas.width-ballRadius || x + dx < ballRadius) {
    dx = -dx;
}

if(y + dy > canvas.height-ballRadius || y + dy < ballRadius) {
    dy = -dy;
}
```
> 사면 모두에서 공을 튕겨내지 말고 왼쪽, 위쪽, 오른쪽, 세 면에만 적용해봅시다. 아래쪽 면에 닿는 순간 게임은 끝납니다. 우리는 공이 밑면에 충돌하는 순간 "게임 오버" 상태로 바뀌게 하기 위해 두 번째 if 블록을 수정할 것입니다. 우선 경고 메시지를 보여주고 페이지를 리로딩해서 게임을 다시 시작하게 할 것입니다. 두번째 if 블록을 아래와 같이 수정해봅시다.
```javascript
if(y + dy < ballRadius) {
    dy = -dy;
} else if(y + dy > canvas.height-ballRadius) {
    alert("GAME OVER");
    document.location.reload();
}
```
Paddle은 공을 튕겨내야지
-
> 마지막 일은 공과 패들 사이의 충돌 감지같은, 공을 게임 화면으로 되돌아가게 튕겨내는 기능을 만드는 것입니다. 가장 쉬운 방법은 공의 중심이 패들의 내부에 있는지 확인하는 것이다. 위에서 수정한 코드를 약간 고쳐봅시다.
```javascript
if(y + dy < ballRadius) {
    dy = -dy;
} else if(y + dy > canvas.height-ballRadius) {
    if(x > paddleX && x < paddleX + paddleWidth) {
        dy = -dy;
    }
    else {
        alert("GAME OVER");
        document.location.reload();
    }
}
```
> 공이 캔버스의 밑면에 닿는 순간, 공이 패들의 안쪽에 있는지 확인해야 합니다. 만약 그렇다면, 우리가 기대하는대로 공은 튕겨져야 합니다. 그게 아니라면, 게임의 전과 같이 끝나야 합니다.
