### Qlik Sense Tasks 의 결과내용을 Repository 에서 조회할 수 있는 테이블 정보 및 쿼리정보

* starttime, endtime 의 경우 timezone 고려하지 않은 UTC 시간임.

```sql
SELECT 
"TaskID", 
task."Name" as TaskName,
app."Name" as AppName,
task."Enabled" as Enabled,  
"ExecutionID", 
result."Status", 
"StatusName",
"StartTime", 
"StopTime", 
"Duration", 
"ExecutingNodeName", 
"ScriptLogLocation"
FROM public."ExecutionResults" as result
left join public."ReloadTasks" as task
on result."TaskID" = task."ID"
left join public."Apps" as app
on result."AppID" = app."ID"
left join (select '0' as "Status", 'NeverStarted' as "StatusName" union
select 1 as "Status", 'Triggered' as "StatusName" union
select 2 as "Status", 'Started' as "StatusName" union
select 3 as "Status", 'Queued' as "StatusName" union
select 4 as "Status", 'AbortInitiated' as "StatusName" union
select 5 as "Status", 'Aborting' as "StatusName" union
select 6 as "Status", 'Aborted' as "StatusName" union
select 7 as "Status", 'FinishedSuccess' as "StatusName" union
select 8 as "Status", 'FinishedFail' as "StatusName" union
select 9 as "Status", 'Skipped' as "StatusName" union
select 10 as "Status", 'Retry' as "StatusName" union
select 11 as "Status", 'Error' as "StatusName" union
select 12 as "Status", 'Reset') as status
on result."Status" = status."Status"
```
