# DxLib를 이용한 미니게임

#### 제작기간: 2017.08.12~2017.09.20(코드 파일 소실됨;;)
#### 사용 기술: DxLib
<p align="center">
<img width="30%" src="https://user-images.githubusercontent.com/33209821/229362931-01c82709-2d6c-4935-a958-b5cfd2de90e1.png"/>
<img width="30%" src="https://user-images.githubusercontent.com/33209821/229362924-c22bf067-4f44-4bad-9382-3b02ec06689f.png"/>
	<br/>
<img width="30%" src="https://user-images.githubusercontent.com/33209821/229362930-00913b13-2e97-4820-a791-f9a9d0b0e1bf.png"/>
<img width="30%" src="https://user-images.githubusercontent.com/33209821/229362926-1cba4e11-9bfa-4847-8299-d57e51c0d8ff.png"/>
</p>
<br/>

```C++
class Stage {
private:
	int rest= LoadGraph("C:/Users/김동빈/Desktop/My Work/damagochi/image/PNG/playground.png");// 리소스를 절대경로로 설정하여 타 PC에서 실행 불가
}
```

#### 설명
 - 단일 객체를 생성한 뒤 메뉴 루프와 실제 게임루프로 이루어진 루프를 실행한다.
 - 메뉴 루프에서는 키보드 입력에 따라 start_page의 y값에 변화를 주어 분기를 나눈다.
 - 시작화면에서 start버튼에서 엔터를 누르면 게임이 시작된다.
 - 캐릭터의 위치는 키보드로 조작 가능하고 특정 범위에서만 이동이 가능하다.
 - 화면에 게임에 필요한 리소스들을 출력하고 플레이어를 제외한 구조물들은 위치가 고정되어 있기에 blakcone.cpp에 각 구조물에 대한 충돌처리가 구현되어있다.
 - tired가 70이상이라면 하단에 위치한 던전에 출입이 불가능하고 왼쪽 상당에 위치한 휴식처에서 엔터를 누를 시 회복 가능하다.
 - 던전에 입장하면 몬스터의 위치, 이동방향이 랜덤하게 정해지는데 플레이어와 충돌하거나 좌표가 맵 밖으로 설정된다면 다시 설정한다. 
 - 몬스터가 이동 중 벽 또는 플레이어와 충돌한다면 이동방향을 랜덤으로 다시 부여받는다.
 
```C++
if ((sli.check(black)||sli.check()) == TRUE) { sli.getran_s(); }
```

 - 전투 중 방향키와 함께 스페이스바를 누를 시 bullet이 발사 된다.
 - bullet 발사 방식은 trigger와 a라는 변수로 이루어지는데 trigger은 방향을, a는 bullet의 발사 위치를 설정한다.
 
 ```C++
 if (trigger == 0) {
		if (CheckHitKey(KEY_INPUT_RIGHT) && CheckHitKey(KEY_INPUT_SPACE)) { trigger = 1; a = 1; ChangeVolumeSoundMem(60, bul_sound); PlaySoundMem(bul_sound, DX_PLAYTYPE_BACK);
		}
	.
	.
	.
if (a == 1) {
	x_b = k.x1 + 23;
	y_b = k.y1 + 30;// 위치를 플레이어의 위치로 설정
	a = 0;
	}

 ```
 
 - bullet이 발사되지 않을 시 기본 위치는 (-1000,-1000)으로 플레이에 지장이 없다. 
 - 몬스터도 bullet을 발사하여 공격한다. 이는 플레이어와 몬스터 사이 각도를 구해 공격방향이 결정된다.
 
 ```C++
 if (bul_trigger == 0) {
		sb_x = s_x; // bullet의 위치 조정
		sb_y = s_y;
		length = s_x - b.x1-30; // 플레이어와 몬스터 간 x축 거리
		height = s_y - b.y1-30; // 플레이어와 몬스터 간 y축 거리
		hypo = sqrt(pow(length, 2) + pow(height, 2)); // length와 height로 이루어진 삼각형의 빗변 길이
		bul_trigger = 1;
	}
 
 ```
 - 몬스터의 bullet과 플레이어가 충돌할 시 플레이어의 hp를 10 감소시키고  trigger가 0으로 되어 위 코드가 실행된다.
 - 몬스터의 bullet이 벽과 충돌할 시 똑같이 위 코드가 실행된다.
 -몬스터와 플레이어가 충돌한다면 hp가 5 감소되고 몬스터의 이동 방향으로 튕겨진다.
 - 일정 스테이지가 지난다면 몬스터의 종류가 바뀐다.
 - hp가 0이하로 감소 시 자동으로 던전에서 추방되고 tired가 70 증가한다.
 - 푸드존에 가면 hp회복이 가능하고 입장 시 음식의 위치가 랜덤으로 부여된다. 
 - 음식의 종류는 2가지로 hp회복 정도가 다르며 한번에 1개만 출력된다.
 
 ```C++
 void Eat::getran() {
	m_a = GetRand(100);// 음식의 종류를 결정 할 변수
	x = GetRand(420);  // 음식의 위치
	y = GetRand(420);
}
```
- 플레이어가 음식과 충돌한다면 위 코드를 실행하여 상태를 바꾼다.
- 음식을 먹을 시 tired가 증가한다.
 - esc를 누른다면 즉시 게임이 종료된다.
