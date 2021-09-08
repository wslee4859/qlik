# pick match 로 if, else if, else 문 구현하기. 

### Match 함수 
대/소문자가 구분됨

* wildmatch : 와일드 카드 
* mixmatch : 대소문자 구분 x 
___________________________________

특정 파라미터의 값일 땐 정상, 나머지 일 경우에는 default 가 되도록 구현 

MATCH 함수 특성상 파라미터가 일치 하지 않으면 0을 return 하므로 return 된 값에 +1 을 하면 defualt 로 PICK 에서 1 위치에 값이 PICK 됨.

### Example 
```
# A 는 비정상
# R 은 진행중
# 나머지 STATUS 는 정상

LOAD 
,PICK(MATCH(STATUS,'A','R' )+1,'정상','비정상','진행중') 

```
