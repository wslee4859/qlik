/*------------ 21.02.01 코드 임시 처리  -------------------------------------------------------------*/



STORE [$(vTableName)] INTO [lib://Source\CHP\Master/$(vTableName).qvd];            //MCUSTOMER 을 MCUSTOMER.qvd로 저장

Let vTableRows=NoOfRows('$(vTableName)');            //처리건수
Let vTableFields=NoOfFields('$(vTableName)');        //필드건수

Drop Table [$(vTableName)];                          //MCUSTOMER Drop하여 메모리 초기화

Let vEndTime=now();                                   //작업완료시간

'''
/*----- MCUSTOMER 작업 로그 저장------------------------------------------------------------------*/

LoadStats:                                          
Load
    $(i) AS SEQ,                                      //작업순번
    '$(vTabDiv)' AS Division,                         //테이블구분
    '$(vTableName)' AS TableName,                     //테이블명
    '$(vSourceTable)' AS SourceName,                  //Source Name
    '$(vTableDesc)' As TableDesc,                     //테이블한글명
    '$(vStartTime)' AS StartTime,                     //작업시작시간
    '$(vEndTime)' AS EndTime,                         //작업완료시간
    '$(vDB)' As DBName,                               //DB 명
    $(vTableRows) AS Rows,                           //적재건수
    $(vTableFields) AS Fields,                      //적재필드수 (테이블컬럼수+텍스트처리건수)
    $(vTextColCnt) AS TextFields,                   //텍스트처리건수
    'Y' as LastWork,                                  //최신작업여부
    '$(vStart)' As WorkStartTime                     //Application 시작시간
AutoGenerate(1);

/*----- MCUSTOMER 작업 로그 저장 완료 --------------------------------------------------------------*/



disconnect;



/******************** 에러 체크 ****************************************/
Let vScriptErrorCount=$(ScriptErrorCount);    //에러가 발생한 건수
Trace '$(vScriptErrorCount)';

If  $(vScriptErrorCount)>0 then
    Call ErrorProcess;
    
End If

'''
