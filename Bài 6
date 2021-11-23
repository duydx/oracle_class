## Mục tiêu

- Tạo listener mới với port khác mặc định
- Stop|Start Listener.
- Kết nối vào database bằng Oracle Client

## Thực hiện

- Set các biến môi trường

export ORACLE_SID=+ASM
export ORACLE_HOME=/u01/app/11.2.0/grid
export PATH=$PATH:$ORACLE_HOME/bin

- Sửa listener với port khác

vi /u01/app/11.2.0/grid/network/admin/listener.ora

Sửa lại thành:

LISTENER =
(DESCRIPTION_LIST =
(DESCRIPTION =
(ADDRESS = (PROTOCOL = TCP)(HOST = oralab)(PORT = 1522))
)
)

Thêm đoạn sau:

SID_LIST_LISTENER =
(SID_LIST =
(SID_DESC =
(SID_NAME = +ASM)
(ORACLE_HOME = /u01/app/11.2.0/grid)
)
(SID_DESC =
(GLOBAL_DBNAME = orcl)
(SID_NAME = orcl)
(ORACLE_HOME = /u01/app/oracle/product/11.2.0/dbhome_1)
)
)

- Sửa listener của Database

export ORACLE_SID=orcl
export ORACLE_HOME= /u01/app/oracle/product/11.2.0/dbhome_1
export PATH=$PATH:$ORACLE_HOME/bin

vi /u01/app/oracle/product/11.2.0/dbhome_1/netword/admin/listener.ora

LISTENER =
(
    DESCRIPTION_LIST =
        (DESCRIPTION =
        (ADDRESS = (PROTOCOL = TCP)(HOST = oralab)(PORT = 1522))
      )
)
SID_LIST_LISTENER =
(SID_LIST =
(SID_DESC =
(SID_NAME = +ASM)
(ORACLE_HOME = /u01/app/11.2.0/grid)
)

(SID_DESC =
(GLOBAL_DBNAME = orcl)
(SID_NAME = orcl)
(ORACLE_HOME = /u01/app/oracle/product/11.2.0/dbhome_1)
)
)

** Stop Listener **

su – grid
srvctl stop listener

** Start listener **

srvctl start listener

** Kiểm tra lại trạng thái listener**

lsnrctl status

- Cài đặt Oracle Client

Vào đường dẫn của Oracle Client Home (thư mục cài đặt Oracle Client)

Ví dụ: C:\\app\Administrator\product\11.2.0\client_1\NETWORK\ADMIN

Mở file tnsname.ora, thêm vào đoạn sau:

ORCL =
(DESCRIPTION =
(ADDRESS = (PROTOCOL = TCP)(HOST = 192.168.2.10)(PORT = 1521))
(CONNECT_DATA =
(SERVER = DEDICATED)
(SERVICE_NAME = orcl)
)
)

- Kết nối với Oracle Database bằng công cụ Oracle SQL Developer

+ Bằng cách EZConnect
+ Bằng Local Naming

- Tạo thêm 1 database tên là orcl_test bằng dbca

- Connect vào database orcl_test tạo user ten_ban/oracle123

export ORACLE_SID=orcl_test
export ORACLE_HOME=/u01/app/oracle/11.2.0/product/dbhome_1
export PATH=$PATH:$ORACLE_HOME/bin
create user ten_ban identifiedby oracle123;
grant connect, resource to ten_ban;
connect / as sysdba
create table ten_ban.dbuser as select * from dba_user;

- Connect vào database orcl, sử dụng user scott tạo 1 dblink như sau:

export ORACLE_SID=orcl
export ORACLE_HOME=/u01/app/oracle/11.2.0/product/dbhome_1
export PATH=$PATH:$ORACLE_HOME/bin
sqlplus / as sysdba
CREATE DATABASE LINK to_orcl_test
CONNECT TO ten_ban IDENTIFIED BY oracle123
USING '(ADDRESS = (PROTOCOL = TCP)(HOST = 192.168.2.10)(PORT =
1521))
(CONNECT_DATA =
(SERVER = DEDICATED)
(SERVICE_NAME = orcl_test)
)';

- Select bảng ten_ban.dbuser từ database orcl

select * from ten_ban.dbuser@to_orcl_test;
