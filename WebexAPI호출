

```
LIB CONNECT TO 'REST_webex_mis_notice (custom_qlikadmin)';

Let _now = date(now(),'YYYY-MM-DD hh:mm:ss');
LET vScriptError = 'Error Script 발생 테스트 ';
LET vText = '----Task Failed-----\n Task: $(vTask) \n Start Time(KST): $(_now) \n Error: $(vScriptError)';


let vRequestBody = '{"roomId": "697f0f60-5ffe-11ed-ac68-0702890de197","text": "$(vText)"}';
let vRequestBody = replace(vRequestBody,'"', chr(34)&chr(34));


RestConnectorMasterTable:
SQL SELECT 
	"id",
	"roomId",
	"roomType",
	"text",
	"personId",
	"personEmail",
	"created"
FROM JSON (wrap on) "root"
WITH CONNECTION(      
      BODY "$(vRequestBody)"
    );
;

[root]:
LOAD	[id],
	[roomId],
	[roomType],
	[text],
	[personId],
	[personEmail],
	[created]
RESIDENT RestConnectorMasterTable;


DROP TABLE RestConnectorMasterTable;

DisConnect;

/*********** Webex API 호출 완료  ************************/
```
