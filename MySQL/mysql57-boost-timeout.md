## How to solve "Timeout was reached" when downloading MySQL boost file

### Problem Definition
1. I was trying to install the MySQL boost file using the command below. 


```shell
vldb@vldb:~/mysql-5.7.33$ cmake -DDOWNLOAD_BOOST=ON -DWITH_BOOST=/home/vldb/mysql-5.7.33 -DCMAKE_INSTALL_PREFIX=/usr/local/mysq
```

2. Error Occured
```shell
...
-- [download 78% complete]
-- [download 79% complete]
-- [download 80% complete]
-- [download 81% complete]
-- [download 82% complete]
-- [download 83% complete]
-- Download failed, error: 28;"Timeout was reached"
CMake Error at cmake/boost.cmake:194 (MESSAGE):
  You can try downloading
  http://sourceforge.net/projects/boost/files/boost/1.59.0/boost_1_59_0.tar.gz
  manually using curl/wget or a similar tool, or increase the value of
  DOWNLOAD_BOOST_TIMEOUT (which is now 600 seconds)
Call Stack (most recent call first):
  CMakeLists.txt:548 (INCLUDE)


-- Configuring incomplete, errors occurred!
See also "/home/vldb/mysql-5.7.33/CMakeFiles/CMakeOutput.log".
See also "/home/vldb/mysql-5.7.33/CMakeFiles/CMakeError.log".

```

### Problem Solving
1. Make the directory to place the boost file in mysql-5.7.33 directory.

```shell
vldb@vldb:~/mysql-5.7.33$ mkdir boost

```

2. Move in to boost file.

```shell
vldb@vldb:~/mysql-5.7.33$ cd boost
```

3. Download boost file manually.

```shell
vldb@vldb:~/mysql-5.7.33/boost$ wget http://sourceforge.net/projects/boost/files/boost/1.59.0/boost_1_59_0.tar.gz

--2023-05-17 22:31:19--  http://sourceforge.net/projects/boost/files/boost/1.59.0/boost_1_59_0.tar.gz
Resolving sourceforge.net (sourceforge.net)... 104.18.10.128, 104.18.11.128, 2606:4700::6812:b80, ...
Connecting to sourceforge.net (sourceforge.net)|104.18.10.128|:80... connected.
HTTP request sent, awaiting response... 301 Moved Permanently
Location: https://sourceforge.net/projects/boost/files/boost/1.59.0/boost_1_59_0.tar.gz [following]
--2023-05-17 22:31:20--  https://sourceforge.net/projects/boost/files/boost/1.59.0/boost_1_59_0.tar.gz
Connecting to sourceforge.net (sourceforge.net)|104.18.10.128|:443... connected.
HTTP request sent, awaiting response... 301 Moved Permanently
Location: https://sourceforge.net/projects/boost/files/boost/1.59.0/boost_1_59_0.tar.gz/ [following]
--2023-05-17 22:31:20--  https://sourceforge.net/projects/boost/files/boost/1.59.0/boost_1_59_0.tar.gz/
Reusing existing connection to sourceforge.net:443.
HTTP request sent, awaiting response... 301 Moved Permanently
Location: https://sourceforge.net/projects/boost/files/boost/1.59.0/boost_1_59_0.tar.gz/download [following]
--2023-05-17 22:31:20--  https://sourceforge.net/projects/boost/files/boost/1.59.0/boost_1_59_0.tar.gz/download
Reusing existing connection to sourceforge.net:443.
HTTP request sent, awaiting response... 302 Found
Location: https://downloads.sourceforge.net/project/boost/boost/1.59.0/boost_1_59_0.tar.gz?ts=gAAAAABkZNcpv_x54j7f52XTqgPzG2kGtsjGA2gIg5jnNGU6SJcLS0Aam-9XjB3gq3qyaR-zo2FLRyIE52LvhARvDYhuSkWvKA%3D%3D&use_mirror=jaist&r= [following]
--2023-05-17 22:31:21--  https://downloads.sourceforge.net/project/boost/boost/1.59.0/boost_1_59_0.tar.gz?ts=gAAAAABkZNcpv_x54j7f52XTqgPzG2kGtsjGA2gIg5jnNGU6SJcLS0Aam-9XjB3gq3qyaR-zo2FLRyIE52LvhARvDYhuSkWvKA%3D%3D&use_mirror=jaist&r=
Resolving downloads.sourceforge.net (downloads.sourceforge.net)... 204.68.111.105
Connecting to downloads.sourceforge.net (downloads.sourceforge.net)|204.68.111.105|:443... connected.
HTTP request sent, awaiting response... 302 Found
Location: https://jaist.dl.sourceforge.net/project/boost/boost/1.59.0/boost_1_59_0.tar.gz [following]
--2023-05-17 22:31:22--  https://jaist.dl.sourceforge.net/project/boost/boost/1.59.0/boost_1_59_0.tar.gz
Resolving jaist.dl.sourceforge.net (jaist.dl.sourceforge.net)... 150.65.7.130, 2001:df0:2ed:feed::feed
Connecting to jaist.dl.sourceforge.net (jaist.dl.sourceforge.net)|150.65.7.130|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 83709983 (80M) [application/x-gzip]
Saving to: ‘boost_1_59_0.tar.gz’

boost_1_59_0.tar.gz                   60%[========================================>                            ]  48.06M   121KB/s    eta 4m 46s 

```

4. Execute cmake command.
```shell
vldb@vldb:~/mysql-5.7.33$ cmake -DWITH_BOOST=/home/vldb/mysql-5.7.33/boost -DCMAKE_INSTALL_PREFIX=/use/local/mysql
# format: cmake -DWITH_BOOST=/path/to/boost -DCMAKE_INSTALL_PREFIX=/path/to/dir
```

