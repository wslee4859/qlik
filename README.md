
# script Error 체크 로직
[참고](https://help.qlik.com/ko-KR/sense/February2022/Subsystems/Hub/Content/Sense_Hub/Scripting/ErrorVariables/ScriptErrorCount.htm)
* ScriptErrorCount

이 오류 변수는 현재 스크립트 실행 도중 오류를 일으킨 문의 총 수를 반환합니다. 이 변수는 스크립트 실행이 시작될 때 항상 0으로 초기화됩니다.

```script
/******************** 에러 체크 ****************************************/
Let vScriptErrorCount=$(ScriptErrorCount);    //에러가 발생한 건수
Trace '$(vScriptErrorCount)';

If  $(vScriptErrorCount)>0 then
    Call ErrorProcess;
    
End If
```

ErrorProcess 를 아래 새로운 스크립트로 선언해서 사용
```script
Sub ErrorProcess

/*********** SMS 데이터 Insert ************************/  

Let _now = date('$(vStartTime)','YYYY-MM-DD hh:mm:ss');

SMS수신자:
LOAD
    SEQ,
    구분,
    수신자
FROM [lib://Source/CHP/Master/SMS수신자.qvd]
(qvd);

Let CNT = NoOfRows('SMS수신자');
Let v발신자=Peek('수신자', 0, 'SMS수신자');

LIB CONNECT TO 'kakaoTalk';
For k=1 to $(CNT)-1
     Let v수신자=Peek('수신자', $(k), 'SMS수신자');      
     SQL exec kakaoTalk.dbo.kakaosend_js
'$(v수신자)',
'$(v발신자)',
'[MIS BATCH]
[$(_now) - $(vScriptError)] batch error',
'LMSG_20201126142120853559',
'MMS',
'[MIS BATCH]
[$(_now) - $(vScriptError)] batch error'
;

Next k
DisConnect;       
/*********** SMS 데이터 Insert 완료 ************************/  
End Sub
```
