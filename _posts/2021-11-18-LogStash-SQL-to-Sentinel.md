---
layout: post
title:  "Use LogStash to copy SQL table with audit logs data to Microsoft Sentinel"
excerpt_separator: <!--more-->
---
The goal of this experiment was to get a LogStash instance running in Docker to copy on-premise SQL Server data to Sentinel. 
If your application is still emitting data only to a SQL table and no other sources are available, this is a trick to get data into Sentinel until a permanent solution has been created.

This is a first experiment, so no production ready code:
```
 docker run --rm -it -p5044:5044 --env xpack.monitoring.enabled=false -v /mnt/c/Users/MyUserName/pipeline/:/usr/share/logstash/pipeline/ -d docker.elastic.co/logstash/logstash:7.10.0 sh -c "logstash-plugin install microsoft-logstash-output-azure-loganalytics ; /usr/local/bin/docker-entrypoint"
``` 

<!--more-->
### What is LogStash
LogStash is part of the Elastic/LogStash/Kibana (ELK), but is also usable as a standalone tool to copy data from almost any system to any other system.
So we are not limited to using Elastic as the destination, but we will be using Microsoft Sentinel in this case.

### So what are we starting
We'll start docker with some defaults and port forwarding to be able to reach the machine
```
docker run --rm -it -p5044:5044 
```

If you want to run this without monitoring, use the following to disable monitoring and hide some Elastic errors regarding server not found 
``` 
--env xpack.monitoring.enabled=false 
```

Then we need to supply a local LogStash config path, create a *pipeline* folder in your user path
```
-v /mnt/c/Users/MyUserName/pipeline/:/usr/share/logstash/pipeline/ 
```
Fire up the docker image (please check for newer versions)
```
 -d docker.elastic.co/logstash/logstash:7.10.0 
```

And last, to modify the installed plugins, we add a custom shell to install the plugin (currently not installing the SQL plugin)
```
sh -c "logstash-plugin install microsoft-logstash-output-azure-loganalytics ; /usr/local/bin/docker-entrypoint"
```

### The config file
```
input { 
    stdin { } 
    # We need to add the SQL / JDBC plugin here
}
filter {
  dissect {
    mapping => {
      "message" => "%{MyEvent},%{MyUser},%{MyOperation}"
    }
  }
}
output {
    microsoft-logstash-output-azure-loganalytics {
        workspace_id => "12345678-abcd-ef12-0123-abcdef12312" # <your workspace id>
        workspace_key => "YoUr123223/WoRkSpAce45676KeY123HeRe==" # <your workspace key>
        custom_log_table_name => "MyLogStash"
      }
  stdout { codec => rubydebug }
}
```

### Prerequisites
 - Azure Sentinel up and running
 - Log Analytics Workspace ID and key
 - Running Docker instance
 - SQL Server with login

### Resources
[Connect LogStash](https://docs.microsoft.com/en-us/azure/sentinel/connect-logstash)
