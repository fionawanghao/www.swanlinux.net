title: Linux 系统后台执行命令解析
date: 2013-02-12 18:13:16
tags: crontab at
categories: linux
---

###名词解析

cron ：系统调度进程，可以使用它在每天的非高峰负荷时间段运行作业，或在一周或一月中的不同时段运行

at : at命令 使用它在一个特定的时间运行一些特殊的作业，或在晚一些的非负荷高峰时间段或高峰负荷时间运行

& 使用它在后台运行一个占用时间不长的进程

Nohup 使用它在后台运行一个命令，即使在用户退出是也不受影响 

###cron和crontab 

1.每个用户都可以有自己的crontab文件，但在一个较大的系统中，系统管理员一般会禁止这些文件，而只在整个系统保留一个这样的文件。系统管理员是通过cron.deny和cron.allow这两个文件来禁止或允许用户拥有自己的crontab文件。

2.crontab的域

为了能够在特定的时间运行作业，需要了解crontab文件每个条目中各个域的意义和格式。下面就是这些域：

>第1列        分钟1~59
>第2列        小时1~23(0表示子夜)
>第3列        日1~31
>第4列        月1~12
>第5列        星期0~6(0表示星期天)
>第6列        要运行的命令

下面是crontab的格式：

>分<>时<>日<>月<>星期<>要运行的命令

其中<>表示空格

在这些域中，可以用横杠 – 来表示一个时间范围，例如你希望星期一至星期五运行某个作业，那么可以在星期域使用1-5来表示。还可以在这些域中使用逗号“,”，例如你希望星期一和星期四运行某个作业，只需要使用1,4来表示。可以用星号\*来表示连续的时间段。如果你对某个表示时间的域没有特别的限定，也应该在该域填入*。该文件的每一个条目必须含有5个时间域，而且每一个之间要用空格分割。该文件中所有的注释行要在行首用#来表示。


3.crontab命令选项

crontab 命令的一般形式为：

>Crontab [-u user] -e -l -r

其中：

>-u 用户名
>-e 编辑crontab文件
>-l 列出crontab文件中的内容
>-r 删除crontab文件

如果使用自己的名字登录，就不用使用-u选项，因为在执行crontab命令时，该命令能够知道当前的用户。

4.crontab应用案例

1)每晚的21：30运行/apps/bin目录下的cleanup.sh

    31 21 * * * /apps/bin/cleanup.sh

2)每月1、10、22日的4：45运行/apps/bin目录下的backup.sh

    45 4 1,10,22 * * /apps/bin/backup.sh

3)每周六、周日的1：10运行一个find命令

    10 1 * * 6,0 /bin/find -name “core” -exec rm {} \;

4)在每天18：00至23：00之间每隔30分钟运行/apps/bin目录下的dbcheck.sh

    0,30 18-23 * * * /apps/bin/dbcheck.sh

5)每星期六的11：00 pm运行/apps/bin目录下的qtrend.sh

    0 23 * * 6 /apps/bin/qtrend.sh 

###at命令

at命令将会保留所有当前的环境变量，包括路劲，不像crontab只提供缺省的环境。该作业的所有输出都将一电子邮件的形式发送给用户，除非你对输出进行了重定向。

和crontab一样，根用户可以通过/etc目录下的at.allow和at.deny文件来控制那些用户可以使用at命令，那些用户不行。

at命令的基本形式为：

>at [-f script] [-m -l -r] [time] [date]

其中：

>-f script 是所要提交的脚本或命令
>-l列出当前等待运行的作业。atq命令具有相同的作用
>-r清除作业。为了清除某个作业，还要提供相应的作业标识(ID)；有些UNIX变体只接受atrm作为清除命令
>-m 作业完成后给用户发邮件

注意：at 后添加的时间格式   如：2012-12-25 01：23  则是01：23  122512 

![at][linux-cron-001-010]

###&提交后台执行命令 

1.&提交的格式：

  命令 &

2.查看后台执行的进程

  jobs

3.杀死后台运行进程

  kill %n

  其中n是用jobs查看的id

4.前台后台任务切换

  1)由前台到后台

  必须前暂停任务CRLT+Z

  jobs查看任务ID

  bg n

  其中n为任务ID

  2)由后台到前台
  fg n 

![fg][linux-cron-002-010]

###nohup命令

如果你正在运行一个进程，而且你觉得在退出账户是该进程还不会结束，那么可以使用nohug命令。该命令可以在你退出账户之后继续运行相应的进程。Nohup就是不挂起的意思

该命令的一般形式为：

>nohup command &


[linux-cron-001-010]: /image/linux/linux-cron-001-010.png
[linux-cron-002-010]: /image/linux/linux-cron-002-010.png
