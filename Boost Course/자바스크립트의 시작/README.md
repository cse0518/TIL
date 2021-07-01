___
# 자바스크립트의 시작
- [x] 오리엔테이션 (21.06.27)
<br><br>
- [x] 웹과 Javascript (21.07.01)
- [x] 퀴즈 (21.07.01)
<br><br>
- [ ] Javascript 제어문
- [ ] 퀴즈
<br><br>
- [ ] Javascript 함수
- [ ] 퀴즈
<br><br>
- [ ] Javascript 객체
- [ ] 퀴즈
<br><br>
- [ ] Javascript 활용
- [ ] 퀴즈
<br><br>
- [ ] 코스를 마치며
<br><br>
- [ ] 성적조회
___
<br>

## 웹과 Javascript
- 자바스크립트 문법 "< h3 style="color: yellow">Javascript < /h3>"
<span style="color: yellow">Javascript</span>

- div & span
  - div는 한줄 전체의 공간
  - span은 전체가 아닌 그 크기만큼

<span>span</span>은 이렇게 되고 <div>div</div>는 이렇게 됩니다.

- CSS ( class & id )
  - class는 같은 스타일이 지정될 그룹 (포괄적)
  - id는 특정한 태그만 스타일 지정
  - 우선순위는 태그 < class < id
  
<img width="250px" src="https://user-images.githubusercontent.com/60170616/124061592-1b0a9900-da6a-11eb-81ce-62933e1c485e.png"/>

- 해당 페이지 전체에 해당하는 css 수정
  - < input type="button" value="night" onclick="documnet.querySelector('body').style.backgroundColor = 'black';">
  - 버튼을 누르면 배경색이 검정으로 바뀐다.
  - document -> 해당 페이지의
  - querySelector('body') -> body 태그를 select
  - style.backgroundColor = 'black'; -> 배경색 스타일을 검정으로 수정
