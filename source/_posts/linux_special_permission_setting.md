title: Linux 中特殊权限为设置
date: 2013-02-12 18:13:16
tags: suid 
categories: linux
---

###粘着位

1)粘着位的作用

linux的文件或目录时，有时会遇到“t”为。“t”代表粘着位，如果在一个目录上出现“t”位，这就意味着该目录中的文件只有其属主才可以删除，即使某个同组用户具有和属主同等的权限.

2)设置粘着位

将相应的权限位之前的那一位设置为1

例如：

![设置粘着位][linux-suid-001-007]

3）如果一个目录是可写的，而且设置了粘着位，那么在下面的条件中至少有一个满足的情况下，这个目录中的文件就可以被删除或者重命名：

>文件的所有者是这个用户
>
>目录的所有者是这个用户
>
>文件对于这个用户是可写的
>
>用户是超级用户

###suid/guid

1)suid/guid定义：

suid意味着如果某个用户对属于自己的shell脚本设置了这种权限，那么其他用户在执行这一脚本时也会具有其属主的相应权限。于是，如果跟用户的某一个脚本设置了这样的权限，那么其他普通用户在执行它的期间也同样具有根用户的权限。同样的原则也适用于guid，执行相应脚本的用户将具有该文件所属用户组中用户的权限。

2)查看搜索设有suid和guid的命令：
 
有相当一些Linux命令也设置了suid和guid。如果想找出这些命令，可以进入/bin或/sbin目录，执行下面的命令：

	$ls -l |grep ‘^…s’
	上面的命令是用来查找suid文件的；
	$ls -l |grep ‘^…s..s’
	上面的命令是用来查找suid文件的； 

![查看搜索设有suid和guid的命令][linux-suid-002-007]


3）设置suid/guid

设置suid是将相应的权限位之前的那一位设置为4；如果希望设置guid，那么就将相应的权限位之前的那一位设置为2；如果希望两者都置位，那么将相应的权限位之前的那一位设置为4+2=6.

一旦设置了这一位，一个s将出现在x的位置上。注意：在设置suid或guid的同时，相应的执行权限位必须要被设置。例如，如果希望设置guid，那么必须要让该用户组具有执行权限。

例如：将文件test.sh设置suid，它当前所具有的权限为rwx rw- r–(741),需要使用chmod命令时在该权限数字的前面加上一个4，即chmod 4741,这将使该文件的权限变为rws rw- r– 

![设置suid/guid][linux-suid-003-007]

注意：当没有执行权限而设置suid/guid时将变为S

[linux-suid-001-007]: /image/linux/linux-suid-001-007.png
[linux-suid-002-007]: /image/linux/linux-suid-002-007.png
[linux-suid-003-007]: /image/linux/linux-suid-003-007.png