---
title: zabbix监控tomcat错误日志
date: 2018-03-30 11:25:32
categories: "开发"
tags:
	- 文本编辑器
	- 脚本语言
	- Tomcat
	- Sed
	- Vim

---

1 定义好2个监控项（1一个统计错误个数，一个统计错误日志内容）

vim /etc/zabbix/zabbix\_agentd.d/tomcat-params.conf (我的路径在这里， 根据agent自己的路径填写)

UserParameter=tomcat.errorLog,/etc/zabbix/sh/monitor\_tomcat\_error\_log.sh

UserParameter=tomcat.errorCount,/etc/zabbix/sh/count\_tomcat\_error\_log.sh

2 编写监控脚本

vim /etc/zabbix/sh/count\_tomcat\_error\_log.sh（统计错误日志条数）

\#!/bin/bash

LAST\_MINUTES=$(date -d ‘-1 minute’ +%H%M%S)

LOG\_NUM=0

LOG\_PATH=”/data/log/tomcat8/test.$(date +%Y-%m-%d).mon”

if \[\[ ! -f “$LOG\_PATH” \]\];then

echo “ZBX\_NOTSUPPORTED”

exit 1

fi

while read line;do

if \[\[ “$line” =~ ^\[0-9\]\{2\}:\[0-9\]\{2\}:\[0-9\]\{2\} \]\];then

date\_time=$(echo $line | grep -E -o “\[0-9\]\{2\}:\[0-9\]\{2\}:\[0-9\]\{2\}” | tr -d ‘:’)

date\_time=$(echo $date\_time | sed ‘s/^0//’)

LAST\_MINUTES=$(echo $LAST\_MINUTES | sed ‘s/^0//’)

if \[\[ “$date\_time” -gt “$LAST\_MINUTES” \]\];then

((LOG\_NUM++))

else

break

fi

fi

done < <(tac $LOG\_PATH)

echo -n $LOG\_NUM

chmod 777 /etc/zabbix/sh/count\_tomcat\_error\_log.sh记得给脚本权限

vim /etc/zabbix/sh/monitor\_tomcat\_error\_log.sh（统计错误日志内容，这样不需要上服务器查看错误日志）

\#!/bin/bash

LAST\_MINUTES=$(date -d ‘-1 minute’ +%H%M%S)

LOG\_NUM=0

MAX\_LOG\_RECORD=3

MAX\_LOG=30

LOG\_CONTENT=””

LOG\_PATH=”/data/log/tomcat8/test.$(date +%Y-%m-%d).mon”

while read line;do

if \[\[ “$line” =~ ^\[0-9\]\{2\}:\[0-9\]\{2\}:\[0-9\]\{2\} \]\];then

date\_time=$(echo $line | grep -E -o “\[0-9\]\{2\}:\[0-9\]\{2\}:\[0-9\]\{2\}” | tr -d ‘:’)

date\_time=$(echo $date\_time | sed ‘s/^0//’)

LAST\_MINUTES=$(echo $LAST\_MINUTES | sed ‘s/^0//’)

if \[\[ “$date\_time” -gt “$LAST\_MINUTES” \]\];then

((LOG\_NUM++))

\[\[ “$LOG\_NUM” -gt “$MAX\_LOG\_RECORD” \]\] || log\_entry=”$line $log\_entry”

\[\[ “$LOG\_NUM” -gt “$MAX\_LOG\_RECORD” \]\] || LOG\_CONTENT=”$LOG\_CONTENT $log\_entry”

else

break

fi

log\_entry=””

else

\[\[ “$LOG\_NUM” -gt “$MAX\_LOG\_RECORD” \]\] || log\_entry=”$line $log\_entry”

fi

if \[\[ “$LOG\_NUM” -gt “$MAX\_LOG” \]\];then

break

fi

done < <(tac $LOG\_PATH)

\[\[ “$LOG\_NUM” -gt “$MAX\_LOG” \]\] && echo -n -e “$LOG\_CONTENT” | sed ‘s\#^\#<br />\#’

chmod 777 /etc/zabbix/sh/monitor\_tomcat\_error\_log.sh（记得给脚本权限）

3 在zabbix 里面添加监控项

第一张是统计个数， 第二张是统计内容

![zabbix监控tomcat错误日志][zabbix_tomcat]

![zabbix监控tomcat错误日志][zabbix_tomcat 1]


[zabbix_tomcat]: /pro/os/crawler/2ME6-F3IV-7ZY2.jpg
[zabbix_tomcat 1]: /pro/os/crawler/MAAF-V2BZ-BJNR.jpg
 *  **原文作者：** Linux云计算
 *  **原文链接：** https://www.toutiao.com/item/6538573739972362760/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许可协议。转载请注明出处。