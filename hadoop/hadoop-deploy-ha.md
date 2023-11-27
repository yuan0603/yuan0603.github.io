<!--
| Host Name     | Host IP Address | Name Node | ZKFailoverController (ZKFC) | Resource Manager | Data Node | Node Manager | Journal Node | ZooKeeper Quorum |
| :------------ | :-------------- | :-------: | :-------------------------: | :--------------: | :-------: | :----------: | :----------: | :--------------: |
| lab-master-01 | 192.168.90.11   | V         | V                           | V                |           |              |              |                  |
| lab-master-02 | 192.168.90.12   | V         | V                           | V                |           |              |              |                  |
| lab-slave-01  | 192.168.90.21   |           |                             |                  | V         | V            | V            | V                |
| lab-slave-02  | 192.168.90.22   |           |                             |                  | V         | V            | V            | V                |
| lab-slave-03  | 192.168.90.23   |           |                             |                  | V         | V            | V            | V                |
-->

| Host Name                    | lab-master-01   | lab-master-02   | lab-slave-01    | lab-slave-02    | lab-slave-03    |
| :--------------------------- | :-------------: | :-------------: | :-------------: | :-------------: | :-------------: |
| Host IP Address              | 192.168.90.11   | 192.168.90.12   | 192.168.90.21   | 192.168.90.22   | 192.168.90.23   |
| Name Node                    | V               | V               |                 |                 |                 |
| ZKFailoverController (ZKFC)  | V               | V               |                 |                 |                 |
| Resource Manager             | V               | V               |                 |                 |                 |
| Data Node                    |                 |                 | V               | V               | V               |
| Node Manager                 |                 |                 | V               | V               | V               |
| Journal Node                 |                 |                 | V               | V               | V               |
| ZooKeeper Quorum             |                 |                 | V               | V               | V               |

## HDFS High Availability Using the Quorum Journal Manager

## ResourceManager High Availability