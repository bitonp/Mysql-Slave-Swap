# Mysql-Slave-Swap
Issued under MIT License by Peter Colclough (biton@compuserve.com). Please read the License information in this repo for more information.
	
PHP script to swap a slave from one Master to another master (that could be a slave to the first). Performs a complete swap in around 2-3 seconds.

An early version of a php script that allows you to swap a currently replicating slave from one master to another. You can also chain this slave to another slave, 
so long as the slave you are chaining to is acting as a master, with replication logs being generated. A master can be a slave in this instance, and not be written to.
For example:
                    Main Master
                        |
                    Slave master 
                        |
                      Slave
In the above situation this script will allow you to move 'slave' from 'slave master' to 'master' freeing up the slave master for maintenance. It will also cope with swapping back the other way, from 'Main Master' to 'Slave Master'.

This script is called by:

sh>/path/to/php swapSlaveV1.cli [slave] [Main Master] [Slave Master] 
The script requires that the slave is currently attached to one of the master machines, and that you have the mysqli libraries available. 
For testing, it is suggested you comment out lines 229 to 236 and give it a trial run. Those lines actually swap the slave between machines. 
Tested and used against EC2 instances (30 as at today) , and this script did the operation in between 2 - 6s, depending on how busy the slave was. I still need to run a 
check after the event to make sure there were no 'Duplicate Key Errors' (1602), so it is advisable to have a second terminal open to check on successful completion. 
If it does not complete, with a 1602 error, simply keep issuing a :
        mysql>Stop Slave; SET GLOBAL sql_slave_skip_counter = 1; Start Slave;
Until the problem goes away. 

Enjoy.



