Windows PowerShell
Copyright (C) 2016 Microsoft Corporation. All rights reserved.

PS C:\Users\AOlson> docker run -it --rm -v c:\Users\AOlson\w205:/w205 midsw205/base:latest bash
root@1c28c49bbbc8:~# curl -L -o assessment-attempts-20180128-121051-nested.json https://goo.gl/GGRvoh
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   393    0   393    0     0    373      0 --:--:--  0:00:01 --:--:--   376
100   333    0   333    0     0    204      0 --:--:--  0:00:01 --:--:--   740
root@1c28c49bbbc8:~# git clone https://github.com/mids-w205-crook/activity-06-ajolson89.git
Cloning into 'activity-06-ajolson89'...
Username for 'https://github.com': ajolson89
Password for 'https://ajolson89@github.com':
remote: Counting objects: 5, done.
remote: Compressing objects: 100% (5/5), done.
remote: Total 5 (delta 0), reused 5 (delta 0), pack-reused 0
Unpacking objects: 100% (5/5), done.
Checking connectivity... done.
root@1c28c49bbbc8:~# cd activity-06-ajolson89/
root@1c28c49bbbc8:~/activity-06-ajolson89# docker-compose up -d
bash: docker-compose: command not found
root@1c28c49bbbc8:~/activity-06-ajolson89# ls
README.md                                        docker-compose.yml       htmartin-history.txt
assessment-attempts-20180128-121051-nested.json  htmartin-annotations.md
root@1c28c49bbbc8:~/activity-06-ajolson89# exit
exit
PS C:\Users\AOlson> cd w205
PS C:\Users\AOlson\w205> cd .\activity-06-ajolson89\
PS C:\Users\AOlson\w205\activity-06-ajolson89> docker-compose up -d
Creating activity06ajolson89_zookeeper_1 ... done
Creating activity06ajolson89_mids_1      ... done
Creating activity06ajolson89_zookeeper_1 ...
Creating activity06ajolson89_kafka_1     ... done
PS C:\Users\AOlson\w205\activity-06-ajolson89> docker-compose logs zookeeper | grep -i binding
grep : The term 'grep' is not recognized as the name of a cmdlet, function, script file, or operable program. Check
the spelling of the name, or if a path was included, verify that the path is correct and try again.
At line:1 char:33
+ docker-compose logs zookeeper | grep -i binding
+                                 ~~~~
    + CategoryInfo          : ObjectNotFound: (grep:String) [], CommandNotFoundException
    + FullyQualifiedErrorId : CommandNotFoundException

PS C:\Users\AOlson\w205\activity-06-ajolson89> docker-compose ps
             Name                          Command            State                    Ports
-------------------------------------------------------------------------------------------------------------
activity06ajolson89_kafka_1       /etc/confluent/docker/run   Up      29092/tcp, 9092/tcp
activity06ajolson89_mids_1        /bin/bash                   Up      8888/tcp
activity06ajolson89_zookeeper_1   /etc/confluent/docker/run   Up      2181/tcp, 2888/tcp, 32181/tcp, 3888/tcp
PS C:\Users\AOlson\w205\activity-06-ajolson89> docker-compose exec kafka kafka-topics --create --topic foo --partitions 1 --replication-factor 1 --if-not-exists --zookeeper zookeeper:32181
Created topic "foo".
PS C:\Users\AOlson\w205\activity-06-ajolson89> docker-compose exec kafka kafka-topics --describe --topic foo --zookeeper zookeeper:32181
Topic:foo       PartitionCount:1        ReplicationFactor:1     Configs:
        Topic: foo      Partition: 0    Leader: 1       Replicas: 1     Isr: 1
PS C:\Users\AOlson\w205\activity-06-ajolson89> docker-compose exec mids bash -c "cat /w205/assessment-attempts-20180128-121051-nested.json | jq '.[]' -c | kafkacat -P -b kafka:29092 -t foo && echo 'Produced 100
messages.'"
cat: /w205/assessment-attempts-20180128-121051-nested.json: No such file or directory
Produced 100 messages.
PS C:\Users\AOlson\w205\activity-06-ajolson89> docker-compose exec kafka kafka-console-consumer --bootstrap-server kafka:29092 --topic foo --from-beginning --max-messages 5

^CProcessed a total of 0 messages
PS C:\Users\AOlson\w205\activity-06-ajolson89> docker-compose exec mids bash -c "cat c:\Users\AOlson\w205\activity-06-ajolson89\assessment-attempts-20180128-121051-nested.json | jq '.[]' -c | kafkacat -P -b kafk
a:29092 -t foo && echo 'Produced 100 messages.'"
cat: 'c:UsersAOlsonw205activity-06-ajolson89assessment-attempts-20180128-121051-nested.json': No such file or directory
Produced 100 messages.
PS C:\Users\AOlson\w205\activity-06-ajolson89> docker-compose exec mids bash -c "cat c:\\Users\\AOlson\\w205\\activity-06-ajolson89\\assessment-attempts-20180128-121051-nested.json | jq '.[]' -c | kafkacat -P -b
 kafka:29092 -t foo && echo 'Produced 100 messages.'"
cat: 'c:\Users\AOlson\w205\activity-06-ajolson89\assessment-attempts-20180128-121051-nested.json': Invalid argument
Produced 100 messages.
PS C:\Users\AOlson\w205\activity-06-ajolson89> docker-compose exec mids bash -c "cat /w205/assessment-attempts-20180128-121051-nested.json | jq '.[]' -c | kafkacat -P -b kafka:29092 -t foo && echo 'Produced 100
messages.'"
parse error: Invalid numeric literal at line 1, column 6
Produced 100 messages.
PS C:\Users\AOlson\w205\activity-06-ajolson89> docker-compose exec mids bash -c "cat /w205/assessment-attempts-20180128-121051-nested.json | jq '.[]' -c | kafkacat -P -b kafka:29092 -t foo && echo 'Produced 100
messages.'"
parse error: Invalid numeric literal at line 1, column 6
Produced 100 messages.
PS C:\Users\AOlson\w205\activity-06-ajolson89> docker-compose exec mids bash -c "cat /w205/assessment-attempts-20180128-121051-nested.json | jq '.[]' -c | kafkacat -P -b kafka:29092 -t foo && echo 'Produced 100
messages.'"
parse error: Invalid numeric literal at line 1, column 6
Produced 100 messages.
PS C:\Users\AOlson\w205\activity-06-ajolson89> docker-compose exec kafka kafka-topics --describe --topic foo --zookeeper zookeeper:32181
Topic:foo       PartitionCount:1        ReplicationFactor:1     Configs:
        Topic: foo      Partition: 0    Leader: 1       Replicas: 1     Isr: 1
PS C:\Users\AOlson\w205\activity-06-ajolson89> docker-compose exec mids bash -c "cat /w205/assessment-attempts-20180128-121051-nested.json"
<?xml version="1.0" encoding="UTF-8"?>
<Error><Code>AccessDenied</Code><Message>Request has expired</Message><Expires>2018-02-16T16:34:49Z</Expires><ServerTime>2018-02-25T16:53:57Z</ServerTime><RequestId>255E82BA500316B9</RequestId><HostId>Khpb5Qu7Yv
wVNWALaCf+QC2MxkSS+Eqon/xGYVfHwQSVRCUlFTMW46M6kdoSKjyESXtuFo6XLHg=</HostId></Error>
PS C:\Users\AOlson\w205\activity-06-ajolson89> docker-compose down
Stopping activity06ajolson89_kafka_1     ... done
Stopping activity06ajolson89_zookeeper_1 ... done
Stopping activity06ajolson89_mids_1      ... done
Removing activity06ajolson89_kafka_1     ... done
Removing activity06ajolson89_zookeeper_1 ... done
Removing activity06ajolson89_mids_1      ... done
Removing network activity06ajolson89_default
PS C:\Users\AOlson\w205\activity-06-ajolson89> docker-compose up -d
Creating activity06ajolson89_zookeeper_1 ... done
Creating activity06ajolson89_mids_1      ... done
Creating activity06ajolson89_zookeeper_1 ...
Creating activity06ajolson89_kafka_1     ... done
PS C:\Users\AOlson\w205\activity-06-ajolson89> docker-compose exec kafka kafka-topics --describe --topic foo --zookeeper zookeeper:32181
PS C:\Users\AOlson\w205\activity-06-ajolson89> docker-compose exec mids bash -c "cat /w205/assessment-attempts-20180128-121051-nested.json"
<?xml version="1.0" encoding="UTF-8"?>
<Error><Code>AccessDenied</Code><Message>Request has expired</Message><Expires>2018-02-16T16:34:49Z</Expires><ServerTime>2018-02-25T16:53:57Z</ServerTime><RequestId>255E82BA500316B9</RequestId><HostId>Khpb5Qu7Yv
wVNWALaCf+QC2MxkSS+Eqon/xGYVfHwQSVRCUlFTMW46M6kdoSKjyESXtuFo6XLHg=</HostId></Error>
PS C:\Users\AOlson\w205\activity-06-ajolson89> docker-compose down
Stopping activity06ajolson89_kafka_1     ... done
Stopping activity06ajolson89_mids_1      ... done
Stopping activity06ajolson89_zookeeper_1 ... done
Removing activity06ajolson89_kafka_1     ... done
Removing activity06ajolson89_mids_1      ... done
Removing activity06ajolson89_zookeeper_1 ... done
Removing network activity06ajolson89_default
PS C:\Users\AOlson\w205\activity-06-ajolson89> docker run -it --rm -v c:\Users\AOlson\w205:/w205 midsw205/base:latest bash
root@af6a6fd81d96:~# curl -L -o assessment-attempts-20180128-121051-nested.json https://goo.gl/f5bRm4
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   391    0   391    0     0    457      0 --:--:-- --:--:-- --:--:--   457
100 9096k  100 9096k    0     0  2309k      0  0:00:03  0:00:03 --:--:-- 3912k
root@af6a6fd81d96:~# exit
exit
PS C:\Users\AOlson\w205\activity-06-ajolson89> docker-compose up -d
Creating activity06ajolson89_zookeeper_1 ... done
Creating activity06ajolson89_mids_1      ... done
Creating activity06ajolson89_zookeeper_1 ...
Creating activity06ajolson89_kafka_1     ... done
PS C:\Users\AOlson\w205\activity-06-ajolson89> docker-compose exec kafka kafka-topics --create --topic foo --partitions 1 --replication-factor 1 --if-not-exists --zookeeper zookeeper:32181
Error while executing topic command : Replication factor: 1 larger than available brokers: 0.
[2018-02-25 18:26:42,303] ERROR org.apache.kafka.common.errors.InvalidReplicationFactorException: Replication factor: 1 larger than available brokers: 0.
 (kafka.admin.TopicCommand$)
PS C:\Users\AOlson\w205\activity-06-ajolson89> docker-compose exec kafka kafka-topics --describe --topic foo --zookeeper zookeeper:32181
PS C:\Users\AOlson\w205\activity-06-ajolson89> docker-compose ps
             Name                          Command            State                    Ports
-------------------------------------------------------------------------------------------------------------
activity06ajolson89_kafka_1       /etc/confluent/docker/run   Up      29092/tcp, 9092/tcp
activity06ajolson89_mids_1        /bin/bash                   Up      8888/tcp
activity06ajolson89_zookeeper_1   /etc/confluent/docker/run   Up      2181/tcp, 2888/tcp, 32181/tcp, 3888/tcp
PS C:\Users\AOlson\w205\activity-06-ajolson89> docker-compose exec kafka kafka-topics --create --topic foo --partitions 1 --replication-factor 1 --if-not-exists --zookeeper zookeeper:32181
Created topic "foo".
PS C:\Users\AOlson\w205\activity-06-ajolson89> docker-compose exec kafka kafka-topics --describe --topic foo --zookeeper zookeeper:32181
Topic:foo       PartitionCount:1        ReplicationFactor:1     Configs:
        Topic: foo      Partition: 0    Leader: 1       Replicas: 1     Isr: 1
PS C:\Users\AOlson\w205\activity-06-ajolson89> docker-compose exec mids bash -c "cat /w205/assessment-attempts-20180128-121051-nested.json | jq '.[]' -c | kafkacat -P -b kafka:29092 -t foo && echo 'Produced 100
messages.'"
Produced 100 messages.
PS C:\Users\AOlson\w205\activity-06-ajolson89> docker-compose exec kafka kafka-console-consumer --bootstrap-server kafka:29092 --topic foo --from-beginning --max-messages 5
{"keen_timestamp":"1516717442.735266","max_attempts":"1.0","started_at":"2018-01-23T14:23:19.082Z","base_exam_id":"37f0a30a-7464-11e6-aa92-a8667f27e5dc","user_exam_id":"6d4089e4-bde5-4a22-b65f-18bce9ab79c8","seq
uences":{"questions":[{"user_incomplete":true,"user_correct":false,"options":[{"checked":true,"at":"2018-01-23T14:23:24.670Z","id":"49c574b4-5c82-4ffd-9bd1-c3358faf850d","submitted":1,"correct":true},{"checked":
true,"at":"2018-01-23T14:23:25.914Z","id":"f2528210-35c3-4320-acf3-9056567ea19f","submitted":1,"correct":true},{"checked":false,"correct":true,"id":"d1bf026f-554f-4543-bdd2-54dcf105b826"}],"user_submitted":true,
"id":"7a2ed6d3-f492-49b3-b8aa-d080a8aad986","user_result":"missed_some"},{"user_incomplete":false,"user_correct":false,"options":[{"checked":true,"at":"2018-01-23T14:23:30.116Z","id":"a35d0e80-8c49-415d-b8cb-c21
a02627e2b","submitted":1},{"checked":false,"correct":true,"id":"bccd6e2e-2cef-4c72-8bfa-317db0ac48bb"},{"checked":true,"at":"2018-01-23T14:23:41.791Z","id":"7e0b639a-2ef8-4604-b7eb-5018bd81a91b","submitted":1,"c
orrect":true}],"user_submitted":true,"id":"bbed4358-999d-4462-9596-bad5173a6ecb","user_result":"incorrect"},{"user_incomplete":false,"user_correct":true,"options":[{"checked":false,"at":"2018-01-23T14:23:52.510Z
","id":"a9333679-de9d-41ff-bb3d-b239d6b95732"},{"checked":false,"id":"85795acc-b4b1-4510-bd6e-41648a3553c9"},{"checked":true,"at":"2018-01-23T14:23:54.223Z","id":"c185ecdb-48fb-4edb-ae4e-0204ac7a0909","submitted
":1,"correct":true},{"checked":true,"at":"2018-01-23T14:23:53.862Z","id":"77a66c83-d001-45cd-9a5a-6bba8eb7389e","submitted":1,"correct":true}],"user_submitted":true,"id":"e6ad8644-96b1-4617-b37b-a263dded202c","u
ser_result":"correct"},{"user_incomplete":false,"user_correct":true,"options":[{"checked":false,"id":"59b9fc4b-f239-4850-b1f9-912d1fd3ca13"},{"checked":false,"id":"2c29e8e8-d4a8-406e-9cdf-de28ec5890fe"},{"checke
d":false,"id":"62feee6e-9b76-4123-bd9e-c0b35126b1f1"},{"checked":true,"at":"2018-01-23T14:24:00.807Z","id":"7f13df9c-fcbe-4424-914f-2206f106765c","submitted":1,"correct":true}],"user_submitted":true,"id":"951943
31-ac43-454e-83de-ea8913067055","user_result":"correct"}],"attempt":1,"id":"5b28a462-7a3b-42e0-b508-09f3906d1703","counts":{"incomplete":1,"submitted":4,"incorrect":1,"all_correct":false,"correct":2,"total":4,"u
nanswered":0}},"keen_created_at":"1516717442.735266","certification":"false","keen_id":"5a6745820eb8ab00016be1f1","exam_name":"Normal Forms and All That Jazz Master Class"}
{"keen_timestamp":"1516717377.639827","max_attempts":"1.0","started_at":"2018-01-23T14:21:47.505Z","base_exam_id":"37f0a30a-7464-11e6-aa92-a8667f27e5dc","user_exam_id":"2fec1534-b41f-4419-b741-79d372f05cbe","seq
uences":{"questions":[{"user_incomplete":false,"user_correct":true,"options":[{"checked":false,"id":"62feee6e-9b76-4123-bd9e-c0b35126b1f1"},{"checked":true,"at":"2018-01-23T14:22:04.301Z","id":"7f13df9c-fcbe-442
4-914f-2206f106765c","submitted":1,"correct":true},{"checked":false,"id":"2c29e8e8-d4a8-406e-9cdf-de28ec5890fe"},{"checked":false,"id":"59b9fc4b-f239-4850-b1f9-912d1fd3ca13"}],"user_submitted":true,"id":"9519433
1-ac43-454e-83de-ea8913067055","user_result":"correct"},{"user_incomplete":true,"user_correct":false,"options":[{"checked":true,"at":"2018-01-23T14:22:24.525Z","id":"7e0b639a-2ef8-4604-b7eb-5018bd81a91b","submit
ted":1,"correct":true},{"checked":false,"correct":true,"id":"bccd6e2e-2cef-4c72-8bfa-317db0ac48bb"},{"checked":false,"id":"a35d0e80-8c49-415d-b8cb-c21a02627e2b"}],"user_submitted":true,"id":"bbed4358-999d-4462-9
596-bad5173a6ecb","user_result":"missed_some"},{"user_incomplete":false,"user_correct":false,"options":[{"checked":true,"at":"2018-01-23T14:22:39.396Z","id":"c185ecdb-48fb-4edb-ae4e-0204ac7a0909","submitted":1,"
correct":true},{"checked":true,"at":"2018-01-23T14:22:35.081Z","id":"77a66c83-d001-45cd-9a5a-6bba8eb7389e","submitted":1,"correct":true},{"checked":false,"id":"85795acc-b4b1-4510-bd6e-41648a3553c9"},{"checked":t
rue,"at":"2018-01-23T14:22:48.197Z","id":"a9333679-de9d-41ff-bb3d-b239d6b95732","submitted":1}],"user_submitted":true,"id":"e6ad8644-96b1-4617-b37b-a263dded202c","user_result":"incorrect"},{"user_incomplete":tru
e,"user_correct":false,"options":[{"checked":false,"correct":true,"id":"d1bf026f-554f-4543-bdd2-54dcf105b826"},{"checked":false,"correct":true,"id":"f2528210-35c3-4320-acf3-9056567ea19f"},{"checked":true,"at":"2
018-01-23T14:22:55.494Z","id":"49c574b4-5c82-4ffd-9bd1-c3358faf850d","submitted":1,"correct":true}],"user_submitted":true,"id":"7a2ed6d3-f492-49b3-b8aa-d080a8aad986","user_result":"missed_some"}],"attempt":1,"id
":"5b28a462-7a3b-42e0-b508-09f3906d1703","counts":{"incomplete":2,"submitted":4,"incorrect":1,"all_correct":false,"correct":1,"total":4,"unanswered":0}},"keen_created_at":"1516717377.639827","certification":"fal
se","keen_id":"5a674541ab6b0a0001c6e723","exam_name":"Normal Forms and All That Jazz Master Class"}
{"keen_timestamp":"1516738973.653394","max_attempts":"1.0","started_at":"2018-01-23T20:22:22.584Z","base_exam_id":"4beeac16-bb83-4d58-83e4-26cdc38f0481","user_exam_id":"8edbc8a8-4d26-4292-a5af-ae3f246cb09f","seq
uences":{"questions":[{"user_incomplete":false,"user_correct":false,"options":[{"checked":true,"at":"2018-01-23T20:22:39.089Z","id":"c315723f-4753-4f8f-932e-d9d7085fed28","submitted":1},{"checked":true,"at":"201
8-01-23T20:22:27.250Z","id":"f298bc63-9075-45bc-8b95-9c8c979bf779","submitted":1,"correct":true},{"checked":false,"correct":true,"id":"302fa72e-3949-46a1-9dd9-668fac9c1fc1"}],"user_submitted":true,"id":"b9ff2e88
-cf9d-4bd4-bcea-c2673779425c","user_result":"incorrect"},{"user_incomplete":false,"user_correct":true,"options":[{"checked":true,"at":"2018-01-23T20:22:41.102Z","id":"60b45b0b-f20c-42eb-8b26-61ef690fd1ba","submi
tted":1,"correct":true},{"checked":false,"id":"a5a583b6-9c3a-4147-afbb-d6fbfc516c85"},{"checked":true,"at":"2018-01-23T20:22:42.988Z","id":"a3e31a57-6407-40f1-9808-c7f2ed8e7f95","submitted":1,"correct":true},{"c
hecked":true,"at":"2018-01-23T20:22:44.447Z","id":"aae4e562-49bf-46ca-808a-5300ac4b08d1","submitted":1,"correct":true}],"user_submitted":true,"id":"bec23e7b-4870-49f7-92c1-1e0a016ef7c9","user_result":"correct"},
{"user_incomplete":false,"user_correct":true,"options":[{"checked":false,"id":"ac290741-c86c-491a-9872-72213e947a70"},{"checked":true,"at":"2018-01-23T20:22:46.048Z","id":"a9d65a6d-0fdf-4518-b1a0-900d9248f123","
submitted":1,"correct":true}],"user_submitted":true,"id":"1ba75b31-64a4-4bd3-9646-25aa3b73505f","user_result":"correct"},{"user_incomplete":false,"user_correct":true,"options":[{"checked":false,"id":"cb8e8ecb-5a
24-4712-ae46-81bc5b858c3b"},{"checked":false,"id":"429b03ec-bda9-4d4a-bd7f-e2d208cbb5fc"},{"checked":true,"at":"2018-01-23T20:22:50.811Z","id":"1cdc931d-dbfb-4aea-bcb8-57be503bb649","submitted":1,"correct":true}
,{"checked":true,"at":"2018-01-23T20:22:52.423Z","id":"088b406e-1fa6-419f-8193-d41c29ee4d57","submitted":1,"correct":true}],"user_submitted":true,"id":"1f7c5def-904b-4834-8519-639a3b22a496","user_result":"correc
t"}],"attempt":1,"id":"b370a3aa-bf9e-4c10-848a-8ecacbd1d93e","counts":{"incomplete":0,"submitted":4,"incorrect":1,"all_correct":false,"correct":3,"total":4,"unanswered":0}},"keen_created_at":"1516738973.653394",
"certification":"false","keen_id":"5a67999d3ed3e300016ef0f1","exam_name":"The Principles of Microservices"}
{"keen_timestamp":"1516738921.1137421","max_attempts":"1.0","started_at":"2018-01-23T20:21:10.833Z","base_exam_id":"4beeac16-bb83-4d58-83e4-26cdc38f0481","user_exam_id":"c0ee680e-8892-4e64-a7f2-bb576a665dc5","se
quences":{"questions":[{"user_incomplete":false,"user_correct":true,"options":[{"checked":false,"id":"429b03ec-bda9-4d4a-bd7f-e2d208cbb5fc"},{"checked":true,"at":"2018-01-23T20:21:16.528Z","id":"088b406e-1fa6-41
9f-8193-d41c29ee4d57","submitted":1,"correct":true},{"checked":false,"id":"cb8e8ecb-5a24-4712-ae46-81bc5b858c3b"},{"checked":true,"at":"2018-01-23T20:21:20.089Z","id":"1cdc931d-dbfb-4aea-bcb8-57be503bb649","subm
itted":1,"correct":true}],"user_submitted":true,"id":"1f7c5def-904b-4834-8519-639a3b22a496","user_result":"correct"},{"user_incomplete":true,"user_correct":false,"options":[{"checked":true,"at":"2018-01-23T20:21
:31.682Z","id":"aae4e562-49bf-46ca-808a-5300ac4b08d1","submitted":1,"correct":true},{"checked":false,"id":"a5a583b6-9c3a-4147-afbb-d6fbfc516c85"},{"checked":false,"correct":true,"id":"a3e31a57-6407-40f1-9808-c7f
2ed8e7f95"},{"checked":true,"at":"2018-01-23T20:21:34.849Z","id":"60b45b0b-f20c-42eb-8b26-61ef690fd1ba","submitted":1,"correct":true}],"user_submitted":true,"id":"bec23e7b-4870-49f7-92c1-1e0a016ef7c9","user_resu
lt":"missed_some"},{"user_incomplete":false,"user_correct":true,"options":[{"checked":true,"at":"2018-01-23T20:21:39.798Z","id":"a9d65a6d-0fdf-4518-b1a0-900d9248f123","submitted":1,"correct":true},{"checked":fal
se,"id":"ac290741-c86c-491a-9872-72213e947a70"}],"user_submitted":true,"id":"1ba75b31-64a4-4bd3-9646-25aa3b73505f","user_result":"correct"},{"user_incomplete":true,"user_correct":false,"options":[{"checked":fals
e,"id":"c315723f-4753-4f8f-932e-d9d7085fed28"},{"checked":false,"correct":true,"id":"302fa72e-3949-46a1-9dd9-668fac9c1fc1"},{"checked":true,"at":"2018-01-23T20:21:59.075Z","id":"f298bc63-9075-45bc-8b95-9c8c979bf
779","submitted":1,"correct":true}],"user_submitted":true,"id":"b9ff2e88-cf9d-4bd4-bcea-c2673779425c","user_result":"missed_some"}],"attempt":1,"id":"b370a3aa-bf9e-4c10-848a-8ecacbd1d93e","counts":{"incomplete":
2,"submitted":4,"incorrect":0,"all_correct":false,"correct":2,"total":4,"unanswered":0}},"keen_created_at":"1516738921.1137421","certification":"false","keen_id":"5a6799694fc7c70001034706","exam_name":"The Princ
iples of Microservices"}
{"keen_timestamp":"1516737000.212122","max_attempts":"1.0","started_at":"2018-01-23T19:48:42.477Z","base_exam_id":"6442707e-7488-11e6-831b-a8667f27e5dc","user_exam_id":"e4525b79-7904-4050-a068-27969b01f6bd","seq
uences":{"questions":[{"user_incomplete":false,"user_correct":false,"options":[{"checked":true,"at":"2018-01-23T19:48:59.018Z","id":"5a3ba547-6bd8-11e6-9cfa-a8667f27e5dc","submitted":1},{"checked":true,"at":"201
8-01-23T19:48:52.771Z","id":"53b3130a-6bd8-11e6-808b-a8667f27e5dc","submitted":1,"correct":true},{"checked":true,"at":"2018-01-23T19:48:56.446Z","id":"4cf30168-6bd8-11e6-8826-a8667f27e5dc","submitted":1,"correct
":true},{"checked":false,"id":"4f4b0ee9-71f2-476e-b045-23d72f8b0bfc"}],"user_submitted":true,"id":"620c924f-6bd8-11e6-bcbd-a8667f27e5dc","user_result":"incorrect"},{"user_incomplete":false,"user_correct":true,"o
ptions":[{"checked":false,"id":"ec558988-76ac-11e6-9f14-a8667f27e5dc"},{"checked":true,"at":"2018-01-23T19:49:24.216Z","id":"dc7c8cc8-76ac-11e6-a12a-a8667f27e5dc","submitted":1,"correct":true},{"checked":false,"
id":"ce3f6b30-76ac-11e6-935d-a8667f27e5dc"},{"checked":true,"at":"2018-01-23T19:49:28.815Z","id":"d5390790-76ac-11e6-a4c9-a8667f27e5dc","submitted":1,"correct":true}],"user_submitted":true,"id":"f4bc7618-76ac-11
e6-a6c9-a8667f27e5dc","user_result":"correct"},{"user_incomplete":false,"user_correct":true,"options":[{"checked":false,"id":"28e7b459-6bd8-11e6-8def-a8667f27e5dc"},{"checked":true,"at":"2018-01-23T19:49:44.656Z
","id":"1fa888ba-6bd8-11e6-85c8-a8667f27e5dc","submitted":1,"correct":true}],"user_submitted":true,"id":"42d6d026-6bd8-11e6-bd61-a8667f27e5dc","user_result":"correct"},{"user_incomplete":false,"user_correct":tru
e,"options":[{"checked":false,"id":"72588554-6bd8-11e6-8bdb-a8667f27e5dc"},{"checked":false,"id":"7bb18719-6bd8-11e6-9a07-a8667f27e5dc"},{"checked":false,"id":"d1452429-4c17-4ac5-a266-a2ae70748e56"},{"checked":t
rue,"at":"2018-01-23T19:49:56.912Z","id":"692c0975-6bd8-11e6-b682-a8667f27e5dc","submitted":1,"correct":true}],"user_submitted":true,"id":"8372371c-6bd8-11e6-82fc-a8667f27e5dc","user_result":"correct"}],"attempt
":1,"id":"04a192c1-4f5c-4ac1-91df-3f175996af99","counts":{"incomplete":0,"submitted":4,"incorrect":1,"all_correct":false,"correct":3,"total":4,"unanswered":0}},"keen_created_at":"1516737000.212122","certificatio
n":"false","keen_id":"5a6791e824fccd00018c3ff9","exam_name":"Introduction to Big Data"}
Processed a total of 5 messages
PS C:\Users\AOlson\w205\activity-06-ajolson89> docker-compose down
Stopping activity06ajolson89_kafka_1     ... done
Stopping activity06ajolson89_zookeeper_1 ... done
Stopping activity06ajolson89_mids_1      ... done
Removing activity06ajolson89_kafka_1     ... done
Removing activity06ajolson89_zookeeper_1 ... done
Removing activity06ajolson89_mids_1      ... done
Removing network activity06ajolson89_default
PS C:\Users\AOlson\w205\activity-06-ajolson89>