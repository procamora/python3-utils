# python3-utils

This repository contains a set of libraries that I use in other projects

## Installation

Installation can be done through the _pip3_ command:

```bash
pip3 install procamora-utils --user
```

You can also update the library with:

```bash
python3 -m pip install --user --upgrade procamora-utils
```


## Libraries


- logger
- interface_sqlite
- ip
- ping
- client_ssh




## logger

Library to run the logging library with colors according to the type of log.


### Basic Usage


To use this class the first thing to do is import the library:


```python
import logging
from procamora_utils.logger import get_logging

logger: logging = get_logging(verbose=False, name='test')

logger.debug('hi')
logger.info('hi')
logger.warning('hi')
logger.error('hi')
logger.critical('hi')
```








## interface_sqlite

This library provides an easy way to manage a sqlite database. To do this, use the _sqlite3_ library to connect to the database and the _logging_ library to display information about errors and debugging.

### Basic Usage

To use this class the first thing to do is import the library:

```python
from procamora_utils.logger import get_logging
from procamora_utils.interface_sqlite import *
```

The _interface_sqlite_ file when doing an _import *_ we are importing three functions, these are:



    __all__ = ['conection_sqlite', 'execute_script_sqlite', 'dump_database']



#### conection_sqlite

This function is responsible for carrying out the main SQL operations, such as: _SELECT_, _INSERT_, _UPDATE_ or _DELETE_.


An example of some of these functions would be:


```python
from pathlib import Path
from typing import List, Dict, Text, Any
from procamora_utils.interface_sqlite import *

db: Path = Path('database.db')
def select_all_hosts() -> List[Dict[Text, Any]]:
    query: Text = "SELECT * FROM Hosts"
    response_query: List[Dict[Text, Any]] = conection_sqlite(db, query, is_dict=True)
    return response_query

def update_host_offline(date: Text):
    query: Text = f"UPDATE Hosts SET active=0 WHERE date <> '{date}';"
    conection_sqlite(db, query)
```


##### Parameterized query

This library allows the Parameterized of sql queries, here is an example of how to use it

```python
from pathlib import Path
from typing import List, Dict, Text, Any, Tuple
from procamora_utils.interface_sqlite import *
db: Path = Path('database.db')

query: Text = "SELECT * FROM table1 WHERE value=?"
params: Tuple = ('Python',)
response_query: List[Dict[Text, Any]] = conection_sqlite(db, query, query_params=params, is_dict=True)
```



#### execute_script_sqlite


This function allows you to run a script or dump that you receive in string format. With this function, databases could be created.


```python
from pathlib import Path
from typing import Text
from procamora_utils.interface_sqlite import *

db: Path = Path('database.db')
dump: Text = '''INSERT INTO() VALUES();
INSERT INTO() VALUES();'''
execute_script_sqlite(db, dump)
```

#### dump_database


This function allows you to perform a complete dump of the database.




```python
from pathlib import Path
from procamora_utils.interface_sqlite import *

db: Path = Path('database.db')
response = dump_database(db)
```



## ip

This library provides a high-level abstraction for storing an IP address or FQDN. In case of entering a wrong value it will raise an exception.

### Basic Usage

```python
import logging
from typing import Text

from procamora_utils.ip import IP
from procamora_utils.logger import get_logging
logger: logging = get_logging(False, 'ip')


fqdn: Text = 'google.es'
ip: IP = IP(fqdn=fqdn)
logger.info(ip.get_addr())

ipv4: Text = '192.168.1.1'
ip: IP = IP(ip=ipv4)
logger.info(ip.get_addr())
```



## ping

This library provides an easy way to ping using the _ping_ command provided by the operating system. The reason for using the operating system command is so that you do not need to be root to send an ICMP packet.

### Basic Usage


To use this class the first thing to do is import the library:


```python
import logging
from typing import List
from procamora_utils.ip import IP
from procamora_utils.ping import ping
from procamora_utils.logger import get_logging
logger: logging = get_logging(False, 'ping')

ips: List[IP] = [IP(ip="127.0.0.1"), IP(ip="192.168.0.103"), IP(fqdn="google.es"), IP(fqdn="google.false")]
for ip in ips:
    texto: bool = ping(ip)
    if texto:
        logger.info("{} up".format(ip.get_addr()))
    else:
        logger.info("{} down".format(ip.get_addr()))
```



## client_ssh


This library provides a high-level abstraction for storing an IP address or FQDN. In case of entering a wrong value it will raise an exception.

### Basic Usage

```python
import logging
from typing import Text

from procamora_utils.ip import IP
from procamora_utils.logger import get_logging
logger: logging = get_logging(False, 'ip')


fqdn: Text = 'google.es'
ip: IP = IP(fqdn=fqdn)
logger.info(ip.get_addr())

ipv4: Text = '192.168.1.1'
ip: IP = IP(ip=ipv4)
logger.info(ip.get_addr())
```



## ping

This library provides an easy way to use the _ssh_ command, allowing you to connect unattended over SSH and thus facilitating process automation tasks.

### Basic Usage


To use this class the first thing to do is import the library:


```python
from procamora_utils.logger import get_logging, logging
from procamora_utils.client_ssh import create_arg_parser, ClientSSH
logger: logging = get_logging(False, 'ping')

args = create_arg_parser()

ssh: ClientSSH = ClientSSH(args.ip, args.port, args.verbose)
output = ssh.execute_command(user=args.user, password=args.pwd, cert=args.key, sudo=args.sudo)
print(output)
```





