# DxLib를 이용한 미니게임

#### 제작기간: 2017.08.12~2017.09.20
#### 사용 기술: DxLib

<br/><br/>
스테이지 헤더함수는 게임에 쓰일 이미지와 이미지 출력함수들로 구성
출력 함수는 DrawGraph(x, y, imageName,TransFlag); 사용
```C++
class Stage {
private:
	int rest= LoadGraph("C:/Users/김동빈/Desktop/My Work/damagochi/image/PNG/playground.png");// 절대경로로 지정하여 타 PC에서 실행 시 문제 발생
       .
       .
public:
	void draw();
       .
       .
};
```

```C++
while (ScreenFlip() == 0 && ProcessMessage() == 0 && ClearDrawScreen() == 0 && gpUpdateKey() == 0){
  // Loop 실행 
}
```
