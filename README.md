# sybase

/*
start server
*/

[sybase@localhost ~]$ cd /u01/app/sybase/SYB15.7
[sybase@localhost SYB15.7]$ . SYBASE.sh
[sybase@localhost SYB15.7]$ cd $SYBASE/$SYBASE_ASE/install
[sybase@localhost install]$ startserver -f RUN_LNX_ASE157

/* connection to engine */

[sybase@localhost install]$ rlwrap isql -Usa -SLNX_ASE157 -w1000
Password: 

/* create new login */

1> create login my_login with password "passwordxy"
2> go
1> select * from syslogins
2> go
 suid        status accdate                         totcpu      totio       spacelimit  timelimit   resultlimit dbname                         name                           password                                                                                                                                                                                                                                                           language                       pwdate                          audflags    fullname                       srvname                        logincount procid      lastlogindate                   crdate                          locksuid    lockreason  lockdate                        lpid        crsuid      
 ----------- ------ ------------------------------- ----------- ----------- ----------- ----------- ----------- ------------------------------ ------------------------------ ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ ------------------------------ ------------------------------- ----------- ------------------------------ ------------------------------ ---------- ----------- ------------------------------- ------------------------------- ----------- ----------- ------------------------------- ----------- ----------- 
           1      0             Jun 27 2015  3:12PM          19       57631           0           0           0 master                         sa                             0xc0074eaa04cedd223151c04edb86e3a4ca1a6849a3a2ee52fe60f38962fc5f68eb60152ba17e472aca41                                                                                                                                                                             NULL                                       Jun 27 2015  3:12PM           0 NULL                           NULL                                 NULL        NULL             Aug 14 2015 10:31AM             Jun 27 2015  3:12PM        NULL        NULL                            NULL          -1        NULL 
           2      0             Jun 27 2015  3:12PM           0           0           0           0           0 sybsystemdb                    probe                          0xc0074f7ffab7984d30a02a755b821d0d43f3d500b7540e0d5a18fe8e47eb052f50f83cd3adda40a3cf84                                                                                                                                                                             NULL                                       Jun 27 2015  3:12PM           0 NULL                           NULL                                    0        NULL                            NULL             Jun 27 2015  3:12PM        NULL        NULL                            NULL          -1        NULL 
           3     10             Jun 27 2015  3:14PM           0           0           0           0           0 master                         jstask                         0xc007be09b54b1370df49bed01a1be92c79002a338808cc62e04b5af323c4aebc2130a77d26e1b1914aeb                                                                                                                                                                             NULL                                       Jun 27 2015  3:14PM           0 NULL                           NULL                                 NULL        NULL                            NULL             Jun 27 2015  3:14PM           1           0             Jun 27 2015  3:14PM        NULL           1 
           4      0             Jul 25 2015 10:23AM           0           7           0           0           0 master                         martinLogin                    0xc007123501247c8bd775356ff0fea53c266c0362d770c025e03623cef88d2416e35939824edef99a6992                                                                                                                                                                             NULL                                       Jul 25 2015 10:23AM           0 NULL                           NULL                                    0        NULL             Jul 25 2015  1:44PM             Jul 25 2015 10:23AM        NULL        NULL                            NULL        NULL        NULL 
           5      0             Aug 14 2015 10:32AM           0           0           0           0           0 master                         my_login                       0xc007824bcf9ccea66d2a3a4756bfa371506c4867e086d61464fcac0a4babd73f9038a66f9abd7e77438d                                                                                                                                                                             NULL                                       Aug 14 2015 10:32AM           0 NULL                           NULL                                 NULL        NULL                            NULL             Aug 14 2015 10:32AM        NULL        NULL                            NULL        NULL           1 

(5 rows affected)

/*log out*/

1> exit

/*log in a another user*/
[sybase@localhost install]$ rlwrap isql -Umy_login -SLNX_ASE157 -w1000
Password: 


1> select db_name()
2> select user_name()
3> go
                                
 ------------------------------ 
 master                         

(1 row affected)
                                
 ------------------------------ 
 guest                          

(1 row affected)
1> exit

//
[sybase@localhost install]$ cd /u01/app/sybase/SYB15.7/data
[sybase@localhost data]$ ls
empty	    mh_one_device1.dbf	   sybmgmtdb.dat  sysprocs.dat
master.dat  mh_one_device_log.dbf  sybsysdb.dat   tempdbdev.dat
[sybase@localhost data]$ mkdir disk1
[sybase@localhost data]$ mkdir disk2
[sybase@localhost data]$ mkdir disk3
[sybase@localhost data]$ mkdir disk4
[sybase@localhost data]$ mkdir disk_log
[sybase@localhost data]$ ll
total 344004
drwxrwxr-x. 2 sybase sybase      4096 Aug 14 10:49 disk1
drwxrwxr-x. 2 sybase sybase      4096 Aug 14 10:49 disk2
drwxrwxr-x. 2 sybase sybase      4096 Aug 14 10:49 disk3
drwxrwxr-x. 2 sybase sybase      4096 Aug 14 10:49 disk4
drwxrwxr-x. 2 sybase sybase      4096 Aug 14 10:49 disk_log
-rw-r--r--. 1 sybase sybase         0 Feb  8  2012 empty
-rw-r-----. 1 sybase sybase  62914560 Aug 14 10:41 master.dat
-rw-r-----. 1 sybase sybase  21463040 Aug 14 10:28 mh_one_device1.dbf
-rw-r-----. 1 sybase sybase   2146304 Aug 14 10:28 mh_one_device_log.dbf
-rw-r-----. 1 sybase sybase  78643200 Aug 14 10:28 sybmgmtdb.dat
-rw-r-----. 1 sybase sybase   6291456 Jun 27 15:12 sybsysdb.dat
-rw-r-----. 1 sybase sybase 180355072 Aug 14 10:28 sysprocs.dat
-rw-r-----. 1 sybase sybase 104857600 Aug 14 10:29 tempdbdev.dat
[sybase@localhost data]$ rlwrap isql -Usa -SLNX_ASE157 -w1000
Password: 
1> disk init name = 'my_device1', physname='/u01/app/sybase/SYB15.7/data/disk1/my_device1.dbf', size = '10M'
2> go
1> disk init name = 'my_device2', physname='/u01/app/sybase/SYB15.7/data/disk2/my_device2.dbf', size = '10M'
2> go
1> disk init name = 'my_device3', physname='/u01/app/sybase/SYB15.7/data/disk2/my_device3.dbf', size = '10M'
2> go
1> disk init name = 'my_device3', physname='/u01/app/sybase/SYB15.7/data/disk3/my_device3.dbf', size = '10M'
2> go
Msg 5162, Level 16, State 1:
Server 'LNX_ASE157', Line 1:
The maximum number 10 of configured devices has already been reached. Please reconfigure 'number of devices' to a larger value and retry disk initialization.
1> sp_dropdevice 'my_device3'
2> go
Device dropped.
(return status = 0)
1> disk init name = 'my_device3', physname='/u01/app/sybase/SYB15.7/data/disk3/my_device3.dbf', size = '10M'
2> go
1> disk init name = 'my_device4', physname='/u01/app/sybase/SYB15.7/data/disk4/my_device4.dbf', size = '10M'
2> go
Msg 5162, Level 16, State 1:
Server 'LNX_ASE157', Line 1:
The maximum number 10 of configured devices has already been reached. Please reconfigure 'number of devices' to a larger value and retry disk initialization.
1> select * from sysdevices
2> go
 low         high        status cntrltype name                           phyname                                                                                                                         mirrorname                                                                                                                      vdevno      crdate                          resizedate                      status2     
 ----------- ----------- ------ --------- ------------------------------ ------------------------------------------------------------------------------------------------------------------------------- ------------------------------------------------------------------------------------------------------------------------------- ----------- ------------------------------- ------------------------------- ----------- 
           0       30719      3         0 master                         /u01/app/sybase/SYB15.7/data/master.dat                                                                                         NULL                                                                                                                                      0             Jun 27 2015  3:12PM                            NULL           0 
           0       20000     16         2 tapedump1                      /dev/nst0                                                                                                                       NULL                                                                                                                                      0             Jun 27 2015  3:12PM                            NULL           0 
           0       20000     16         3 tapedump2                      /dev/nst1                                                                                                                       NULL                                                                                                                                      0             Jun 27 2015  3:12PM                            NULL           0 
           0       88063      2         0 sysprocsdev                    /u01/app/sybase/SYB15.7/data/sysprocs.dat                                                                                       NULL                                                                                                                                      1             Jun 27 2015  3:12PM                            NULL           1 
           0        3071      2         0 systemdbdev                    /u01/app/sybase/SYB15.7/data/sybsysdb.dat                                                                                       NULL                                                                                                                                      2             Jun 27 2015  3:12PM                            NULL           1 
           0       51199      2         0 tempdbdev                      /u01/app/sybase/SYB15.7/data/tempdbdev.dat                                                                                      NULL                                                                                                                                      3             Jun 27 2015  3:12PM                            NULL           1 
           0       38399      2         0 sybmgmtdev                     /u01/app/sybase/SYB15.7/data/sybmgmtdb.dat                                                                                      NULL                                                                                                                                      4             Jun 27 2015  3:14PM                            NULL           1 
           0       10479      2         0 mh_one_device1                 /u01/app/sybase/SYB15.7/data/mh_one_device1.dbf                                                                                 NULL                                                                                                                                      5             Jul 14 2015 11:39PM                            NULL           1 
           0        1047      2         0 mh_one_device_log              /u01/app/sybase/SYB15.7/data/mh_one_device_log.dbf                                                                              NULL                                                                                                                                      6             Jul 14 2015 11:39PM                            NULL           1 
           0        5119      2         0 my_device1                     /u01/app/sybase/SYB15.7/data/disk1/my_device1.dbf                                                                               NULL                                                                                                                                      7             Aug 14 2015 10:54AM                            NULL           1 
           0        5119      2         0 my_device2                     /u01/app/sybase/SYB15.7/data/disk2/my_device2.dbf                                                                               NULL                                                                                                                                      8             Aug 14 2015 11:12AM                            NULL           1 
           0        5119      2         0 my_device3                     /u01/app/sybase/SYB15.7/data/disk3/my_device3.dbf                                                                               NULL                                                                                                                                      9             Aug 14 2015 11:15AM                            NULL           1 

(12 rows affected)
1> sp_configure 'number of devices'
2> go
 Parameter Name                 Default              Memory Used Config Value         Run Value            Unit                 Type                 
 ------------------------------ -------------------- ----------- -------------------- -------------------- -------------------- -------------------- 
 number of devices                       10                  #14           10                   10         number               dynamic              

(1 row affected)
(return status = 0)
1> sp_configure 'number of devices', 15
2> go
 Parameter Name                 Default              Memory Used Config Value         Run Value            Unit                 Type                 
 ------------------------------ -------------------- ----------- -------------------- -------------------- -------------------- -------------------- 
 number of devices                       10                  #20           15                   15         number               dynamic              

(1 row affected)
Configuration option changed. ASE need not be rebooted since the option is dynamic.
Changing the value of 'number of devices' to '15' increases the amount of memory ASE uses by 8 K.
(return status = 0)
1> sp_configure 'number of devices'
2> go
 Parameter Name                 Default              Memory Used Config Value         Run Value            Unit                 Type                 
 ------------------------------ -------------------- ----------- -------------------- -------------------- -------------------- -------------------- 
 number of devices                       10                  #20           15                   15         number               dynamic              

(1 row affected)
(return status = 0)
1> disk init name = 'my_device4', physname='/u01/app/sybase/SYB15.7/data/disk4/my_device4.dbf', size = '10M'
2> go
1> disk init name = 'my_device_log', physname='/u01/app/sybase/SYB15.7/data/disk_log/my_device_log.dbf', size = '10M'
2> go
1> sp_helpdevice
2> go
 device_name                                                          physical_name                                                                                                                   description                                                                                                                                                                                                                                                                                                                                                                                                                              status                   cntrltype                            vdevno                   vpn_low                      vpn_high                         
 -------------------------------------------------------------------- ------------------------------------------------------------------------------------------------------------------------------- ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- ------------------------ ------------------------------------ ------------------------ ---------------------------- -------------------------------- 
 master                                                               /u01/app/sybase/SYB15.7/data/master.dat                                                                                         file system device, special, dsync on, directio off, default disk, physical disk, 60.00 MB, Free: 16.00 MB                                                                                                                                                                                                                                                                                                                                    3                           0                                 0                         0                         30719                         
 mh_one_device1                                                       /u01/app/sybase/SYB15.7/data/mh_one_device1.dbf                                                                                 file system device, special, dsync off, directio on, physical disk, 20.47 MB, Free: 14.47 MB                                                                                                                                                                                                                                                                                                                                                  2                           0                                 5                         0                         10479                         
 mh_one_device_log                                                    /u01/app/sybase/SYB15.7/data/mh_one_device_log.dbf                                                                              file system device, special, dsync off, directio on, physical disk, 2.05 MB, Free: 0.05 MB                                                                                                                                                                                                                                                                                                                                                    2                           0                                 6                         0                          1047                         
 my_device1                                                           /u01/app/sybase/SYB15.7/data/disk1/my_device1.dbf                                                                               file system device, special, dsync off, directio on, physical disk, 10.00 MB, Free: 10.00 MB                                                                                                                                                                                                                                                                                                                                                  2                           0                                 7                         0                          5119                         
 my_device2                                                           /u01/app/sybase/SYB15.7/data/disk2/my_device2.dbf                                                                               file system device, special, dsync off, directio on, physical disk, 10.00 MB, Free: 10.00 MB                                                                                                                                                                                                                                                                                                                                                  2                           0                                 8                         0                          5119                         
 my_device3                                                           /u01/app/sybase/SYB15.7/data/disk3/my_device3.dbf                                                                               file system device, special, dsync off, directio on, physical disk, 10.00 MB, Free: 10.00 MB                                                                                                                                                                                                                                                                                                                                                  2                           0                                 9                         0                          5119                         
 my_device4                                                           /u01/app/sybase/SYB15.7/data/disk4/my_device4.dbf                                                                               file system device, special, dsync off, directio on, physical disk, 10.00 MB, Free: 10.00 MB                                                                                                                                                                                                                                                                                                                                                  2                           0                                10                         0                          5119                         
 my_device_log                                                        /u01/app/sybase/SYB15.7/data/disk_log/my_device_log.dbf                                                                         file system device, special, dsync off, directio on, physical disk, 10.00 MB, Free: 10.00 MB                                                                                                                                                                                                                                                                                                                                                  2                           0                                11                         0                          5119                         
 sybmgmtdev                                                           /u01/app/sybase/SYB15.7/data/sybmgmtdb.dat                                                                                      file system device, special, dsync off, directio on, physical disk, 75.00 MB, Free: 0.00 MB                                                                                                                                                                                                                                                                                                                                                   2                           0                                 4                         0                         38399                         
 sysprocsdev                                                          /u01/app/sybase/SYB15.7/data/sysprocs.dat                                                                                       file system device, special, dsync off, directio on, physical disk, 172.00 MB, Free: 0.00 MB                                                                                                                                                                                                                                                                                                                                                  2                           0                                 1                         0                         88063                         
 systemdbdev                                                          /u01/app/sybase/SYB15.7/data/sybsysdb.dat                                                                                       file system device, special, dsync off, directio on, physical disk, 6.00 MB, Free: 0.00 MB                                                                                                                                                                                                                                                                                                                                                    2                           0                                 2                         0                          3071                         
 tapedump1                                                            /dev/nst0                                                                                                                       unknown device type, disk, dump device                                                                                                                                                                                                                                                                                                                                                                                                       16                           2                                 0                         0                         20000                         
 tapedump2                                                            /dev/nst1                                                                                                                       unknown device type, tape,        625 MB, dump device                                                                                                                                                                                                                                                                                                                                                                                        16                           3                                 0                         0                         20000                         
 tempdbdev                                                            /u01/app/sybase/SYB15.7/data/tempdbdev.dat                                                                                      file system device, special, dsync off, directio on, physical disk, 100.00 MB, Free: 0.00 MB                                                                                                                                                                                                                                                                                                                                                  2                           0                                 3                         0                         51199                         

(14 rows affected)
(return status = 0)
1> sp_diskdefault my_device1, defaulton
2> go
(return status = 0)
1> sp_diskdefault my_device2, defaulton
2> go
(return status = 0)
1> sp_diskdefault master, defaultoff
2> go
(return status = 0)
1> sp_helpdevice
2> go
 device_name                                                          physical_name                                                                                                                   description                                                                                                                                                                                                                                                                                                                                                                                                                              status                   cntrltype                            vdevno                   vpn_low                      vpn_high                         
 -------------------------------------------------------------------- ------------------------------------------------------------------------------------------------------------------------------- ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- ------------------------ ------------------------------------ ------------------------ ---------------------------- -------------------------------- 
 master                                                               /u01/app/sybase/SYB15.7/data/master.dat                                                                                         file system device, special, dsync on, directio off, physical disk, 60.00 MB, Free: 16.00 MB                                                                                                                                                                                                                                                                                                                                                  2                           0                                 0                         0                         30719                         
 mh_one_device1                                                       /u01/app/sybase/SYB15.7/data/mh_one_device1.dbf                                                                                 file system device, special, dsync off, directio on, physical disk, 20.47 MB, Free: 14.47 MB                                                                                                                                                                                                                                                                                                                                                  2                           0                                 5                         0                         10479                         
 mh_one_device_log                                                    /u01/app/sybase/SYB15.7/data/mh_one_device_log.dbf                                                                              file system device, special, dsync off, directio on, physical disk, 2.05 MB, Free: 0.05 MB                                                                                                                                                                                                                                                                                                                                                    2                           0                                 6                         0                          1047                         
 my_device1                                                           /u01/app/sybase/SYB15.7/data/disk1/my_device1.dbf                                                                               file system device, special, dsync off, directio on, default disk, physical disk, 10.00 MB, Free: 10.00 MB                                                                                                                                                                                                                                                                                                                                    3                           0                                 7                         0                          5119                         
 my_device2                                                           /u01/app/sybase/SYB15.7/data/disk2/my_device2.dbf                                                                               file system device, special, dsync off, directio on, default disk, physical disk, 10.00 MB, Free: 10.00 MB                                                                                                                                                                                                                                                                                                                                    3                           0                                 8                         0                          5119                         
 my_device3                                                           /u01/app/sybase/SYB15.7/data/disk3/my_device3.dbf                                                                               file system device, special, dsync off, directio on, physical disk, 10.00 MB, Free: 10.00 MB                                                                                                                                                                                                                                                                                                                                                  2                           0                                 9                         0                          5119                         
 my_device4                                                           /u01/app/sybase/SYB15.7/data/disk4/my_device4.dbf                                                                               file system device, special, dsync off, directio on, physical disk, 10.00 MB, Free: 10.00 MB                                                                                                                                                                                                                                                                                                                                                  2                           0                                10                         0                          5119                         
 my_device_log                                                        /u01/app/sybase/SYB15.7/data/disk_log/my_device_log.dbf                                                                         file system device, special, dsync off, directio on, physical disk, 10.00 MB, Free: 10.00 MB                                                                                                                                                                                                                                                                                                                                                  2                           0                                11                         0                          5119                         
 sybmgmtdev                                                           /u01/app/sybase/SYB15.7/data/sybmgmtdb.dat                                                                                      file system device, special, dsync off, directio on, physical disk, 75.00 MB, Free: 0.00 MB                                                                                                                                                                                                                                                                                                                                                   2                           0                                 4                         0                         38399                         
 sysprocsdev                                                          /u01/app/sybase/SYB15.7/data/sysprocs.dat                                                                                       file system device, special, dsync off, directio on, physical disk, 172.00 MB, Free: 0.00 MB                                                                                                                                                                                                                                                                                                                                                  2                           0                                 1                         0                         88063                         
 systemdbdev                                                          /u01/app/sybase/SYB15.7/data/sybsysdb.dat                                                                                       file system device, special, dsync off, directio on, physical disk, 6.00 MB, Free: 0.00 MB                                                                                                                                                                                                                                                                                                                                                    2                           0                                 2                         0                          3071                         
 tapedump1                                                            /dev/nst0                                                                                                                       unknown device type, disk, dump device                                                                                                                                                                                                                                                                                                                                                                                                       16                           2                                 0                         0                         20000                         
 tapedump2                                                            /dev/nst1                                                                                                                       unknown device type, tape,        625 MB, dump device                                                                                                                                                                                                                                                                                                                                                                                        16                           3                                 0                         0                         20000                         
 tempdbdev                                                            /u01/app/sybase/SYB15.7/data/tempdbdev.dat                                                                                      file system device, special, dsync off, directio on, physical disk, 100.00 MB, Free: 0.00 MB                                                                                                                                                                                                                                                                                                                                                  2                           0                                 3                         0                         51199                         

(14 rows affected)
(return status = 0)
1> create database myDB  on my_device1 = '8M', my_device2 = '8M'
2> log on my_device_log
3> go
CREATE DATABASE: allocating 2048 logical pages (8.0 megabytes) on disk 'my_device1' (2048 logical pages requested).
CREATE DATABASE: allocating 2048 logical pages (8.0 megabytes) on disk 'my_device2' (2048 logical pages requested).
CREATE DATABASE: allocating 1536 logical pages (6.0 megabytes) on disk 'my_device_log' (1536 logical pages requested).
Database 'myDB' is now online.
1> select * from sysdatabases
2> go
 name                           dbid   suid        status version logptr      crdate                          dumptrdate                      status2 audflags    deftabaud   defvwaud    defpraud    def_remote_type def_remote_loc                                                                                                                                                                                                                                                                                                                                                status3     status4     audflags2                          spare durability  lobcomp_lvl inrowlen dcompdefaultlevel 
 ------------------------------ ------ ----------- ------ ------- ----------- ------------------------------- ------------------------------- ------- ----------- ----------- ----------- ----------- --------------- ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- ----------- ----------- ---------------------------------- ----- ----------- ----------- -------- ----------------- 
 master                              1           1      0       1        4634             Jun 27 2015  3:12PM             Jun 27 2015  3:14PM  -32768           0           0           0           0            NULL NULL                                                                                                                                                                                                                                                                                                                                                               131072       16384 NULL                                NULL           1           0     NULL                 1 
 model                               3           1      0       1         921             Jun 27 2015  3:12PM             Jun 27 2015  3:12PM  -32768           0           0           0           0            NULL NULL                                                                                                                                                                                                                                                                                                                                                               131072       16384 NULL                                NULL           1           0     NULL                 1 
 tempdb                              2           1     12       1         980             Aug 14 2015 10:28AM             Aug 14 2015 11:34AM  -32768           0           0           0           0            NULL NULL                                                                                                                                                                                                                                                                                                                                                               131072      540672 NULL                                NULL           6           0     NULL                 1 
 sybsystemdb                     31513           1      0       1         918             Jun 27 2015  3:12PM             Jun 27 2015  3:12PM  -32768           0           0           0           0            NULL NULL                                                                                                                                                                                                                                                                                                                                                               131072       16384 NULL                                NULL           1           0     NULL                 1 
 sybsystemprocs                  31514           1      8       1       43737             Jun 27 2015  3:12PM             Aug 14 2015 11:29AM  -32768           0           0           0           0            NULL NULL                                                                                                                                                                                                                                                                                                                                                               131072       16384 NULL                                NULL           1           0     NULL                 1 
 sybmgmtdb                       31515           1     12       1       10441             Jun 27 2015  3:14PM             Jun 27 2015  5:22PM  -32768           0           0           0           0            NULL NULL                                                                                                                                                                                                                                                                                                                                                               131072       16384 NULL                                NULL           1           0     NULL                 1 
 mh_one                              4           1      0       1        1542             Jul 14 2015 11:44PM             Jul 14 2015 11:44PM       0           0           0           0           0            NULL NULL                                                                                                                                                                                                                                                                                                                                                               131072       16384 NULL                                NULL           1           0     NULL                 1 
 myDB                                5           1      0       1        4102             Aug 14 2015 11:38AM             Aug 14 2015 11:38AM       0           0           0           0           0            NULL NULL                                                                                                                                                                                                                                                                                                                                                               131072       16384 NULL                                NULL           1           0     NULL                 1 

(8 rows affected)
1> sp_helpdb
2> go
 name                                                     db_size                                              owner                dbid                 created                                          durability                                   lobcomplvl                               inrowlen                         status                                                                                                                                                                                                                                                                                                                                                               
 -------------------------------------------------------- ---------------------------------------------------- -------------------- -------------------- ------------------------------------------------ -------------------------------------------- ---------------------------------------- -------------------------------- -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- 
 master                                                         26.0 MB                                        sa                       1                Jun 27, 2015                                     full                                                  0                                   NULL                         mixed log and data                                                                                                                                                                                                                                                                                                                                                   
 mh_one                                                          8.0 MB                                        sa                       4                Jul 14, 2015                                     full                                                  0                                   NULL                         no options set                                                                                                                                                                                                                                                                                                                                                       
 model                                                           6.0 MB                                        sa                       3                Jun 27, 2015                                     full                                                  0                                   NULL                         mixed log and data                                                                                                                                                                                                                                                                                                                                                   
 myDB                                                           22.0 MB                                        sa                       5                Aug 14, 2015                                     full                                                  0                                   NULL                         no options set                                                                                                                                                                                                                                                                                                                                                       
 sybmgmtdb                                                      75.0 MB                                        sa                   31515                Jun 27, 2015                                     full                                                  0                                   NULL                         select into/bulkcopy/pllsort, trunc log on chkpt, mixed log and data                                                                                                                                                                                                                                                                                                 
 sybsystemdb                                                    12.0 MB                                        sa                   31513                Jun 27, 2015                                     full                                                  0                                   NULL                         mixed log and data                                                                                                                                                                                                                                                                                                                                                   
 sybsystemprocs                                                172.0 MB                                        sa                   31514                Jun 27, 2015                                     full                                                  0                                   NULL                         trunc log on chkpt, mixed log and data                                                                                                                                                                                                                                                                                                                               
 tempdb                                                        106.0 MB                                        sa                       2                Aug 14, 2015                                     no_recovery                                           0                                   NULL                         select into/bulkcopy/pllsort, trunc log on chkpt, mixed log and data, allow wide dol rows                                                                                                                                                                                                                                                                            

(1 row affected)
(return status = 0)
1> select *syslogin
2> go
Msg 102, Level 15, State 181:
Server 'LNX_ASE157', Line 1:
Incorrect syntax near 'syslogin'.
1> select * syslogin
2> go
Msg 102, Level 15, State 181:
Server 'LNX_ASE157', Line 1:
Incorrect syntax near 'syslogin'.
1> select * from syslogins
2> go
 suid        status accdate                         totcpu      totio       spacelimit  timelimit   resultlimit dbname                         name                           password                                                                                                                                                                                                                                                           language                       pwdate                          audflags    fullname                       srvname                        logincount procid      lastlogindate                   crdate                          locksuid    lockreason  lockdate                        lpid        crsuid      
 ----------- ------ ------------------------------- ----------- ----------- ----------- ----------- ----------- ------------------------------ ------------------------------ ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ ------------------------------ ------------------------------- ----------- ------------------------------ ------------------------------ ---------- ----------- ------------------------------- ------------------------------- ----------- ----------- ------------------------------- ----------- ----------- 
           1      0             Jun 27 2015  3:12PM          19       66681           0           0           0 master                         sa                             0xc0074eaa04cedd223151c04edb86e3a4ca1a6849a3a2ee52fe60f38962fc5f68eb60152ba17e472aca41                                                                                                                                                                             NULL                                       Jun 27 2015  3:12PM           0 NULL                           NULL                                 NULL        NULL             Aug 14 2015 10:52AM             Jun 27 2015  3:12PM        NULL        NULL                            NULL          -1        NULL 
           2      0             Jun 27 2015  3:12PM           0           0           0           0           0 sybsystemdb                    probe                          0xc0074f7ffab7984d30a02a755b821d0d43f3d500b7540e0d5a18fe8e47eb052f50f83cd3adda40a3cf84                                                                                                                                                                             NULL                                       Jun 27 2015  3:12PM           0 NULL                           NULL                                    0        NULL                            NULL             Jun 27 2015  3:12PM        NULL        NULL                            NULL          -1        NULL 
           3     10             Jun 27 2015  3:14PM           0           0           0           0           0 master                         jstask                         0xc007be09b54b1370df49bed01a1be92c79002a338808cc62e04b5af323c4aebc2130a77d26e1b1914aeb                                                                                                                                                                             NULL                                       Jun 27 2015  3:14PM           0 NULL                           NULL                                 NULL        NULL                            NULL             Jun 27 2015  3:14PM           1           0             Jun 27 2015  3:14PM        NULL           1 
           4      0             Jul 25 2015 10:23AM           0           7           0           0           0 master                         martinLogin                    0xc007123501247c8bd775356ff0fea53c266c0362d770c025e03623cef88d2416e35939824edef99a6992                                                                                                                                                                             NULL                                       Jul 25 2015 10:23AM           0 NULL                           NULL                                    0        NULL             Jul 25 2015  1:44PM             Jul 25 2015 10:23AM        NULL        NULL                            NULL        NULL        NULL 
           5      0             Aug 14 2015 10:32AM           0           4           0           0           0 master                         my_login                       0xc007824bcf9ccea66d2a3a4756bfa371506c4867e086d61464fcac0a4babd73f9038a66f9abd7e77438d                                                                                                                                                                             NULL                                       Aug 14 2015 10:32AM           0 NULL                           NULL                                 NULL        NULL             Aug 14 2015 10:39AM             Aug 14 2015 10:32AM        NULL        NULL                            NULL        NULL           1 

(5 rows affected)
1> select db_name()
2> go
                                
 ------------------------------ 
 master                         

(1 row affected)
1> myDB..sp_changedbowner my_login
2> go
Warning: The stored procedure 'sp_thresholdaction' may not execute; check database owner's threshold authorization.
DBCC execution completed. If DBCC printed error messages, contact a user with System Administrator (SA) role.
Database owner changed.
(return status = 0)
1> sp_helpdb
2> go
 name                                                     db_size                                              owner                            dbid                 created                                          durability                                   lobcomplvl                               inrowlen                         status                                                                                                                                                                                                                                                                                                                                                               
 -------------------------------------------------------- ---------------------------------------------------- -------------------------------- -------------------- ------------------------------------------------ -------------------------------------------- ---------------------------------------- -------------------------------- -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- 
 master                                                         26.0 MB                                        sa                                   1                Jun 27, 2015                                     full                                                  0                                   NULL                         mixed log and data                                                                                                                                                                                                                                                                                                                                                   
 mh_one                                                          8.0 MB                                        sa                                   4                Jul 14, 2015                                     full                                                  0                                   NULL                         no options set                                                                                                                                                                                                                                                                                                                                                       
 model                                                           6.0 MB                                        sa                                   3                Jun 27, 2015                                     full                                                  0                                   NULL                         mixed log and data                                                                                                                                                                                                                                                                                                                                                   
 myDB                                                           22.0 MB                                        my_login                             5                Aug 14, 2015                                     full                                                  0                                   NULL                         no options set                                                                                                                                                                                                                                                                                                                                                       
 sybmgmtdb                                                      75.0 MB                                        sa                               31515                Jun 27, 2015                                     full                                                  0                                   NULL                         select into/bulkcopy/pllsort, trunc log on chkpt, mixed log and data                                                                                                                                                                                                                                                                                                 
 sybsystemdb                                                    12.0 MB                                        sa                               31513                Jun 27, 2015                                     full                                                  0                                   NULL                         mixed log and data                                                                                                                                                                                                                                                                                                                                                   
 sybsystemprocs                                                172.0 MB                                        sa                               31514                Jun 27, 2015                                     full                                                  0                                   NULL                         trunc log on chkpt, mixed log and data                                                                                                                                                                                                                                                                                                                               
 tempdb                                                        106.0 MB                                        sa                                   2                Aug 14, 2015                                     no_recovery                                           0                                   NULL                         select into/bulkcopy/pllsort, trunc log on chkpt, mixed log and data, allow wide dol rows                                                                                                                                                                                                                                                                            

(1 row affected)
(return status = 0)
1> create table myDB..t1 (s1 varchar(10))
2> go
1> use myDB
2> go
1> select db_name()
2> go
                                
 ------------------------------ 
 myDB                           

(1 row affected)
1> sp_hepl 't1'
2> go
Msg 2812, Level 16, State 5:
Server 'LNX_ASE157', Line 1:
Stored procedure 'sp_hepl' not found. Specify owner.objectname or use sp_help to check whether the object exists (sp_help may produce lots of output).
1> sp_help 't1'
2> go
 Name             Owner                Object_type                                  Object_status                                        Create_date                                                                  
 ---------------- -------------------- -------------------------------------------- ---------------------------------------------------- ---------------------------------------------------------------------------- 
 t1               dbo                  user table                                    -- none --                                          Aug 14 2015 11:54AM                                                          

(1 row affected)
 Column_name                                  Type                         Length                   Prec             Scale                Nulls                Not_compressed                                           Default_name                                     Rule_name                            Access_Rule_name                                                 Computed_Column_object                                                                   Identity                                 
 -------------------------------------------- ---------------------------- ------------------------ ---------------- -------------------- -------------------- -------------------------------------------------------- ------------------------------------------------ ------------------------------------ ---------------------------------------------------------------- ---------------------------------------------------------------------------------------- ---------------------------------------- 
 s1                                           varchar                          10                   NULL              NULL                    0                             0                                           NULL                                             NULL                                 NULL                                                             NULL                                                                                              0                               
Object does not have any indexes.
No defined keys for this object.
 name             type                                     partition_type                                           partitions                               partition_keys                                           
 ---------------- ---------------------------------------- -------------------------------------------------------- ---------------------------------------- -------------------------------------------------------- 
 t1               base table                               roundrobin                                                        1                               NULL                                                     
 
 partition_name                                           partition_id                                     compression_level                                                    pages                row_count                            segment                      create_date                                                                  
 -------------------------------------------------------- ------------------------------------------------ -------------------------------------------------------------------- -------------------- ------------------------------------ ---------------------------- ---------------------------------------------------------------------------- 
 t1_576002052                                                576002052                                     none                                                                     1                        0                            default                      Aug 14 2015 11:54AM                                                          
 
 Partition_Conditions                                                             
 -------------------------------------------------------------------------------- 
 NULL                                                                             
 
 Avg_pages   Max_pages   Min_pages   Ratio(Max/Avg)              Ratio(Min/Avg)              
 ----------- ----------- ----------- --------------------------- --------------------------- 
           1           1           1                    1.000000                    1.000000 
Table LOB compression level 0
Lock scheme Allpages
The attribute 'exp_row_size' is not applicable to tables with allpages lock scheme.
The attribute 'concurrency_opt_threshold' is not applicable to tables with allpages lock scheme.
 
 exp_row_size reservepagegap fillfactor max_rows_per_page identity_gap ascinserts  
 ------------ -------------- ---------- ----------------- ------------ ----------- 
            0              0          0                 0            0           0 

(1 row affected)
 concurrency_opt_threshold optimistic_index_lock dealloc_first_txtpg 
 ------------------------- --------------------- ------------------- 
                         0                     0                   0 
(return status = 0)
1> 


/*enter data to the table t1*/

1> insert into t1 (s1) values ('A')
2> insert into t1 (s1) values ('B')
3> insert into t1 (s1) values ('C')
4> go
(1 row affected)
(1 row affected)
(1 row affected)
1> select * from myDB.dbo.t1
2> go
 s1         
 ---------- 
 A          
 B          
 C          

(3 rows affected)

/*set non automatic transactions (manual commits)*/
1> set chained on
2> go

1> insert into t1 (s1) values ('F')
2> go
(1 row affected)

/*the table will be locked before "commit" command
you can see locs on the syslocks table*/

1> select * from master..syslocks
2> go
 id          dbid   page        type   spid   class                          fid    context row    loid        partitionid nodeid 
 ----------- ------ ----------- ------ ------ ------------------------------ ------ ------- ------ ----------- ----------- ------ 
   576002052      5           0      3     17 Non Cursor Lock                     0       0      0          34           0   NULL 
   576002052      5           0      4     14 Non Cursor Lock                     0       0      0          28           0   NULL 
   576002052      5         825    261     17 Non Cursor Lock                     0       0      0          34           0   NULL 

2> commit
3> go


/*Create new login and new user*/

1> exit
[sybase@localhost data]$ rlwrap isql -Usa -SLNX_ASE157 -w1000
Password: 
1> create login druhylogin with password "password"
2> go
1> select * from syslogins
2> go
 suid        status accdate                         totcpu      totio       spacelimit  timelimit   resultlimit dbname                         name                           password                                                                                                                                                                                                                                                           language                       pwdate                          audflags    fullname                       srvname                        logincount procid      lastlogindate                   crdate                          locksuid    lockreason  lockdate                        lpid        crsuid      
 ----------- ------ ------------------------------- ----------- ----------- ----------- ----------- ----------- ------------------------------ ------------------------------ ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ ------------------------------ ------------------------------- ----------- ------------------------------ ------------------------------ ---------- ----------- ------------------------------- ------------------------------- ----------- ----------- ------------------------------- ----------- ----------- 
           1      0             Jun 27 2015  3:12PM          28       70253           0           0           0 master                         sa                             0xc0074eaa04cedd223151c04edb86e3a4ca1a6849a3a2ee52fe60f38962fc5f68eb60152ba17e472aca41                                                                                                                                                                             NULL                                       Jun 27 2015  3:12PM           0 NULL                           NULL                                 NULL        NULL             Aug 14 2015  1:23PM             Jun 27 2015  3:12PM        NULL        NULL                            NULL          -1        NULL 
           2      0             Jun 27 2015  3:12PM           0           0           0           0           0 sybsystemdb                    probe                          0xc0074f7ffab7984d30a02a755b821d0d43f3d500b7540e0d5a18fe8e47eb052f50f83cd3adda40a3cf84                                                                                                                                                                             NULL                                       Jun 27 2015  3:12PM           0 NULL                           NULL                                    0        NULL                            NULL             Jun 27 2015  3:12PM        NULL        NULL                            NULL          -1        NULL 
           3     10             Jun 27 2015  3:14PM           0           0           0           0           0 master                         jstask                         0xc007be09b54b1370df49bed01a1be92c79002a338808cc62e04b5af323c4aebc2130a77d26e1b1914aeb                                                                                                                                                                             NULL                                       Jun 27 2015  3:14PM           0 NULL                           NULL                                 NULL        NULL                            NULL             Jun 27 2015  3:14PM           1           0             Jun 27 2015  3:14PM        NULL           1 
           4      0             Jul 25 2015 10:23AM           0           7           0           0           0 master                         martinLogin                    0xc007123501247c8bd775356ff0fea53c266c0362d770c025e03623cef88d2416e35939824edef99a6992                                                                                                                                                                             NULL                                       Jul 25 2015 10:23AM           0 NULL                           NULL                                    0        NULL             Jul 25 2015  1:44PM             Jul 25 2015 10:23AM        NULL        NULL                            NULL        NULL        NULL 
           5      0             Aug 14 2015 10:32AM           0           4           0           0           0 master                         my_login                       0xc007824bcf9ccea66d2a3a4756bfa371506c4867e086d61464fcac0a4babd73f9038a66f9abd7e77438d                                                                                                                                                                             NULL                                       Aug 14 2015 10:32AM           0 NULL                           NULL                                 NULL        NULL             Aug 14 2015 10:39AM             Aug 14 2015 10:32AM        NULL        NULL                            NULL        NULL           1 
           6      0             Aug 14 2015  1:24PM           0           0           0           0           0 master                         druhylogin                     0xc007b54833bfe2304715ec1af6681db5b997a1a6b3978bee07c9c8d18e2a98a5135f60260dd14652a8c1                                                                                                                                                                             NULL                                       Aug 14 2015  1:24PM           0 NULL                           NULL                                 NULL        NULL                            NULL             Aug 14 2015  1:24PM        NULL        NULL                            NULL        NULL           1 

(6 rows affected)

1> myDB..sp_adduser druhylogin, druhyuser
2> go
New user added.
(return status = 0)
1> select * from myDB..sysusers
2> go
 suid        uid         gid         name                           environ                                                                                                                                                                                                                                                         
 ----------- ----------- ----------- ------------------------------ --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- 
           5           1           0 dbo                            NULL                                                                                                                                                                                                                                                            
          -2           0           0 public                         NULL                                                                                                                                                                                                                                                            
          -2       16384       16384 sa_role                        NULL                                                                                                                                                                                                                                                            
          -2       16385       16385 sso_role                       NULL                                                                                                                                                                                                                                                            
          -2       16386       16386 oper_role                      NULL                                                                                                                                                                                                                                                            
          -2       16387       16387 sybase_ts_role                 NULL                                                                                                                                                                                                                                                            
          -2       16388       16388 navigator_role                 NULL                                                                                                                                                                                                                                                            
          -2       16389       16389 replication_role               NULL                                                                                                                                                                                                                                                            
          -2       16390       16390 dtm_tm_role                    NULL                                                                                                                                                                                                                                                            
          -2       16391       16391 ha_role                        NULL                                                                                                                                                                                                                                                            
          -2       16392       16392 mon_role                       NULL                                                                                                                                                                                                                                                            
          -2       16393       16393 js_admin_role                  NULL                                                                                                                                                                                                                                                            
          -2       16394       16394 messaging_role                 NULL                                                                                                                                                                                                                                                            
          -2       16395       16395 js_client_role                 NULL                                                                                                                                                                                                                                                            
          -2       16396       16396 js_user_role                   NULL                                                                                                                                                                                                                                                            
          -2       16397       16397 webservices_role               NULL                                                                                                                                                                                                                                                            
          -2       16398       16398 keycustodian_role              NULL                                                                                                                                                                                                                                                            
           6           3           0 druhyuser                      NULL                                                                                                                                                                                                                                                            

(18 rows affected)

/*log in  as druhylogin*/

1> exit
[sybase@localhost data]$ rlwrap isql -Udruhylogin -SLNX_ASE157 -w1000
Password: 

1> select user_name()
2> select db_name()
3> go
                                
 ------------------------------ 
 guest                          

(1 row affected)
                                
 ------------------------------ 
 master                         

(1 row affected)

/*switch DB to myDB*/

1> use myDB
2> select db_name()
3> select user_name()
4> go
                                
 ------------------------------ 
 myDB                           

(1 row affected)
                                
 ------------------------------ 
 druhyuser                      

(1 row affected)



