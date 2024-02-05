POST:   http://localhost:8086/api/login



{
    "bankcode":"MSC",
    "teller":"zdv",
    "password":"sd",
    "branchno":"001",
    "terminal":"ds"
}

Response
[
    {
        "bankcode": "dfg",
        "loginstatus": "Success",
        "errorcode": "",
        "errordesc": "",
        "sessionid": "CB5AAA23E4062C8C599E67CC1C063E00",
        "token": null
    }
]





[pfms@dcpfmsnoncbs Single-AV-File-Upload]$ mv XML.jar Single-AV-File-Upload.jar
[pfms@dcpfmsnoncbs Single-AV-File-Upload]$ java -jar Single-AV-File-Upload.jar
17:37:15.894 [main] INFO com.sl.pp.AppMain - AppMain.main() :: Execution Started
17:37:15.902 [pool-2-thread-1] INFO com.sl.pp.service.XmlService - InsertProcess::XmlService Started
17:37:15.909 [pool-2-thread-1] INFO com.sl.pp.service.XmlProcessingImpl - XmlProcessingImpl.getXMLFiles() :: total xml files present 5
17:37:15.909 [pool-2-thread-1] INFO com.sl.pp.service.XmlService - XmlService.insertProcess() :: [CSFCBMAVREQ030220242.xml, CSFCBMAVREQ050220241.xml, CSFCBMAVREQ050220242.xml, CSFCBMAVREQ050220243.xml, CSFCBMAVREQ050220244.xml]
17:37:16.435 [pool-2-thread-1] ERROR com.sl.pp.service.XmlService - XmlService.insertProcess() ::: exception while callable thread call java.util.concurrent.ExecutionException: java.lang.NoSuchMethodError: java.util.stream.Stream.toList()Ljava/util/List;
java.util.concurrent.ExecutionException: java.lang.NoSuchMethodError: java.util.stream.Stream.toList()Ljava/util/List;
at java.util.concurrent.FutureTask.report(FutureTask.java:122)
at java.util.concurrent.FutureTask.get(FutureTask.java:192)
at com.sl.pp.service.XmlService.insertProcess(XmlService.java:72)
at com.sl.pp.service.XmlService.performXmlService(XmlService.java:25)
at com.sl.pp.AppMain.lambda$0(AppMain.java:22)
at java.util.concurrent.Executors$RunnableAdapter.call(Executors.java:511)
at java.util.concurrent.FutureTask.runAndReset(FutureTask.java:308)
at java.util.concurrent.ScheduledThreadPoolExecutor$ScheduledFutureTask.access$301(ScheduledThreadPoolExecutor.java:180)
at java.util.concurrent.ScheduledThreadPoolExecutor$ScheduledFutureTask.run(ScheduledThreadPoolExecutor.java:294)
at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1149)
at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:624)
at java.lang.Thread.run(Thread.java:750)
Caused by: java.lang.NoSuchMethodError: java.util.stream.Stream.toList()Ljava/util/List;
at com.sl.pp.service.XmlProcessingImpl.convertAccountToInsertString(XmlProcessingImpl.java:174)
at com.sl.pp.service.XmlProcessingImpl.insertXmlData(XmlProcessingImpl.java:104)
at com.sl.pp.service.XmlService.insertionThread(XmlService.java:106)
at com.sl.pp.service.XmlService.lambda$0(XmlService.java:61)
at java.util.concurrent.FutureTask.run(FutureTask.java:266)
... 3 more
17:37:16.437 [pool-2-thread-1] INFO com.sl.pp.service.XmlService - InsertProcess::XmlService Ended
17:37:16.437 [pool-2-thread-1] INFO com.sl.pp.service.XmlService - UpdateProcess::XmlService Started
17:37:16.437 [pool-2-thread-1] INFO com.sl.pp.service.XmlProcessingImpl - XmlProcessingImpl.getXMLFiles() :: total xml files present 0
17:37:16.437 [pool-2-thread-1] INFO com.sl.pp.service.XmlService - XmlService.updateProcess() :: []
17:37:16.438 [pool-2-thread-1] INFO com.sl.pp.service.XmlService - UpdateProcess::XmlService Ended
17:39:15.899 [pool-2-thread-1] INFO com.sl.pp.service.XmlService - InsertProcess::XmlService Started
17:39:15.907 [pool-2-thread-1] INFO com.sl.pp.service.XmlProcessingImpl - XmlProcessingImpl.getXMLFiles() :: total xml files present 5
17:39:15.908 [pool-2-thread-1] INFO com.sl.pp.service.XmlService - XmlService.insertProcess() :: [CSFCBMAVREQ030220242.xml, CSFCBMAVREQ050220241.xml, CSFCBMAVREQ050220242.xml, CSFCBMAVREQ050220243.xml, CSFCBMAVREQ050220244.xml]
17:39:16.100 [pool-2-thread-1] ERROR com.sl.pp.service.XmlService - XmlService.insertProcess() ::: exception while callable thread call java.util.concurrent.ExecutionException: java.lang.NoSuchMethodError: java.util.stream.Stream.toList()Ljava/util/List;
java.util.concurrent.ExecutionException: java.lang.NoSuchMethodError: java.util.stream.Stream.toList()Ljava/util/List;
at java.util.concurrent.FutureTask.report(FutureTask.java:122)
at java.util.concurrent.FutureTask.get(FutureTask.java:192)
at com.sl.pp.service.XmlService.insertProcess(XmlService.java:72)
at com.sl.pp.service.XmlService.performXmlService(XmlService.java:25)
at com.sl.pp.AppMain.lambda$0(AppMain.java:22)
at java.util.concurrent.Executors$RunnableAdapter.call(Executors.java:511)
at java.util.concurrent.FutureTask.runAndReset(FutureTask.java:308)
at java.util.concurrent.ScheduledThreadPoolExecutor$ScheduledFutureTask.access$301(ScheduledThreadPoolExecutor.java:180)
at java.util.concurrent.ScheduledThreadPoolExecutor$ScheduledFutureTask.run(ScheduledThreadPoolExecutor.java:294)
at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1149)
at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:624)
at java.lang.Thread.run(Thread.java:750)
Caused by: java.lang.NoSuchMethodError: java.util.stream.Stream.toList()Ljava/util/List;
at com.sl.pp.service.XmlProcessingImpl.convertAccountToInsertString(XmlProcessingImpl.java:174)
at com.sl.pp.service.XmlProcessingImpl.insertXmlData(XmlProcessingImpl.java:104)
at com.sl.pp.service.XmlService.insertionThread(XmlService.java:106)
at com.sl.pp.service.XmlService.lambda$0(XmlService.java:61)
at java.util.concurrent.FutureTask.run(FutureTask.java:266)
... 3 more
17:39:16.102 [pool-2-thread-1] INFO com.sl.pp.service.XmlService - InsertProcess::XmlService Ended
17:39:16.103 [pool-2-thread-1] INFO com.sl.pp.service.XmlService - UpdateProcess::XmlService Started
17:39:16.103 [pool-2-thread-1] INFO com.sl.pp.service.XmlProcessingImpl - XmlProcessingImpl.getXMLFiles() :: total xml files present 0
17:39:16.103 [pool-2-thread-1] INFO com.sl.pp.service.XmlService - XmlService.updateProcess() :: []
17:39:16.104 [pool-2-thread-1] INFO com.sl.pp.service.XmlService - UpdateProcess::XmlService Ended
