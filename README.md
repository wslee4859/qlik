
# Script Error 체크 로직
[참고](https://help.qlik.com/ko-KR/sense/February2022/Subsystems/Hub/Content/Sense_Hub/Scripting/ErrorVariables/ScriptErrorCount.htm)
* ScriptErrorCount

이 오류 변수는 현재 스크립트 실행 도중 오류를 일으킨 문의 총 수를 반환합니다. 이 변수는 스크립트 실행이 시작될 때 항상 0으로 초기화됩니다.

* ErrorMode   

이 오류 변수는 스크립트 실행 도중 오류가 발견되었을 때 Qlik Sense에서 실행할 동작을 결정합니다.
>ErrorMode=1	기본 설정입니다. 스크립트 실행이 정지되며 사용자에게 동작을 묻는 메시지가 표시됩니다(배치 모드가 아닐 경우).
>ErrorMode =0	Qlik Sense에서 오류를 그냥 무시하고 다음 스크립트 문의 스크립트를 실행을 계속합니다.   
>ErrorMode =2	
Qlik Sense에서 사용자에게 사전에 동작을 묻는 메시지를 표시하지 않고 "스크립트 실행에 실패했습니다..."라는 오류 메시지를 트리거합니다.



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
