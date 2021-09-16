# 차원(Dimension) 에 조건 Condition 넣어서 조건에 따라 표시 컨트롤

EX ) 시스템이 SAP 인 배치명만 Chart 에 표시 


> 1. 차원필드에 if 문으로 SAP 인것만 조건을 걸면, SAP가 아닌것들의 배치명이 NULL 로 표시됨. 
>``` 
>=if([시스템]='SAP',[배치명])
>```
> 2. 차원 조건에 NULL 값 표시를 해제 
