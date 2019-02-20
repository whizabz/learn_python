# Useful Python Libraries

A list of useful Python libraries

Credit: https://github.com/judy2k/command-line-talk

## Command Line Parsing Libraries
* Built-in
  - getopt 
  - optparse
  - argparse
* 3rd Party
	- docopt
	- click
	- clint
	- compago

## Input Output

### Logging
```
logging.basicConfig(level=logging.WARNING, format="%(msg)s")

if options.verbose: logging.getLogger().setLevel(logging.DEBUG)

LOG = logging.getLogger('logtest') 

LOG.debug('Running main')
LOG.info('Everything is okay')
LOG.warning('EVERYTHING HAS GONE WRONG!')
```

### Terminal Input or Piped Input?

Sample code:
```
from sys import stdin, stdout, stderr

print "Piped input:", not stdin.isatty()
print "Piped output:", not stdout.isatty()
print "Piped error:", not stderr.isatty()
```

Output:
```
$ ./isatty.py 

Piped input: False
Piped output: False
Piped error: False  

$ ./isatty.py | cat 

Piped input: False
Piped output: True
Piped error: False 

$ echo 'Hello' | ./isatty.py | cat 

Piped input: True
Piped output: True
Piped error: False
```

### Color

Library: `Colorama`
PyPi Documentation: https://pypi.org/project/colorama/

```
from colorama import Fore, Back, Style  

print Fore.RED + 'some red text'  
print Back.GREEN + 'and with a green background' print Style.BRIGHT + 'and in bright text',
print Fore.RESET + Back.RESET + Style.RESET_ALL print 'back to normal now'
```

**Problem with this:**
When captured in a log file, color-related characters are also seen.
```
$ ./colour > saved_data.txt
  
$ vim saved_data.txt  
^[[31msome red text

^[[42mand with a green background ^[[1mand in bright text ^[[39m^[[49m^[[0m back to normal now
```

*Solution:*

Set `strip=True`
```
import colorama

if not sys.stdout.isatty():
    colorama.init(strip=True)

print Fore.RED + 'some red text'  
print Back.GREEN + ‘and a green background'
```
## User Credentials

```
import getpass 

username = getpass.getuser()  
password = getpass.getpass()  
  
print ( “You are {username}, and you should never use the password ‘{password}’ again!”.format(
username=username,
password=password
)
```
Here, `getpass.getuser()` can get the current user from the system.

## Progress

Library: `progressbar2`

PyPi Documentation: https://pypi.org/project/progressbar2/

Sample Code:
```
CommandLinePython

from progressbar import * import time  
  
progress = ProgressBar()  
for i in progress(range(80)):

 time.sleep(0.01) 
```

Output:
```
# 100% |#######################################|
```

**Widgets:**

Sample Code:
```
widgets = ['Loading: ', Percentage(), ' ', Bar(),
           ' ', ETA(), ' ', FileTransferSpeed()] 

pbar = ProgressBar(widgets=widgets, maxval=20000).start() 

for i in range(20000): pbar.update(i)
 time.sleep(.005)
pbar.finish()
```
Output:
```
# Loading:   9% |#                                      | ETA:  0:01:42 177.08  B/s
```

## Configuration

* Built-in:
	- ini
	- Environment Variables
	- json
	- csv
	- xml
	- Apple plist
* 3rd Party:
	- yaml
	- Java Properties

### ini Files

```
from ConfigParser import SafeConfigParser from os.path import dirname, join, expanduser
  
INSTALL_DIR = dirname(__file__)  
  
config = SafeConfigParser()  
config.read([
	join(INSTALL_DIR, 'defaults.ini'), expanduser(‘~/.tool.ini’), 'config.ini'
])
```

Examples:
```
print config.get('server', ‘host’)
=> www.ninjarockstar.guru

print config.getint('server', ‘port')
=> 5000

print config.get('server', ‘url’)
=> http://www.ninjarockstar.guru:5000/
```

## Signals

```
import signal  

signal.siginterrupt(signal.SIGINFO, False)  

signal.signal(signal.SIGINFO, mysiginfofunc)
```

**KeyboardInterrupt:**
```
def main():
	try:
		time.sleep(5)
	except KeyboardInterrupt:
		pass

if __name__ == '__main__':
	main()
```

## Code Structure & Packaging

Structure:
```
mytool-project/
  setup.py
  mytool
  mytoollib/ 
 __init__.py
   __main__.py 
 mytool.py
   utils.py
```

setuptools:
```
# setup.py
setup(name = 'mytool',
    version = '2.0',
    url = ‘http://mytool.ninjarockstar.guru/',
    license = 'BSD License',
    author = ‘Mark Smith',
    author_email = ‘judy@judy.co.uk’,
    description = ‘A tool with little purpose.',
    keywords = 'utils',
    packages = ‘mytoollib’,
    scripts = [‘mytool’]
    platforms = ‘any')
```

## Exit Codes

```
# Normal termination exits with 0
# Uncaught exceptions exit with 1
# ... or you can explicitly exit: sys.exit(exit_code)
```
