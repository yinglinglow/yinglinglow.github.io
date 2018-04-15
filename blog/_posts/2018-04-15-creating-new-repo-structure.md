---
layout: post
title: "Creating new repo structure"
date: 2018-04-15
---

Now I'm quite lucky to be involved in designing the repo structure from scratch at work.

Sharing some interesting insights here:

*1) Coding a common library and sharing that across projects.*

Now this doesn't sound too complicated (I mean, who doesn't do this right?) but it ain't that easy after all when there are some constraints, namely: my main.py is not at the root directory level. if it were, things will be really simple - just import! But because we need each project to be self-contained within each of its folder, AND we didn't want to hard code the path in (other teams may use this in the future) things get complicated (or so i thought).

This is my repo structure.

```python
python_scripts
├── lib
│   └── talk.py          # def sayhello(x): return(f'Hi, {x}!')
│   
├── proj_1
│   └── main.py      # from lib.talk import sayhello
│                    # print(sayhello('Person A')
│
└── proj_2           # import lib.talk
```

if you just do this, it throws an error, ModuleNotFoundError. 

the below is a fairly good solution.

```python
import sys, os
path = os.path.abspath('../') # if main.py is two levels in, use '../..' instead

if path not in sys.path:
    sys.path.append(path)

# Now import
import lib
```

caveats:
- i need to include this at the top of every .py file i have that imports from the common library. if only there was a neater way...
- i need to run it at the project folder level (which is fine for me, but of course if there is an option to run it at any level that would be the best lol - i'm greedy i know)

i tried configuring `__init__.py` but doesn't seem to work for me.
changing the PYTHONPATH apparently works but it becomes difficult when another team (of non-developers) want to run the program and they aren't too savvy with PYTHONPATH and stuff...

well, i'll have to take this as the solution in the meantime.

---

on top of this, pytest becomes complicated.

This is my repo structure.

```python
python_scripts
├── lib
│   └── talk.py             # def sayhello(x): return(f'Hi, {x}!')
│   
├── proj_1
│   └── main.py             # from lib.talk import sayhello
│                           # print(sayhello('Person A')
│
├── proj_2
│   └── main.py             # from lib.talk import sayhello
│                           # print(sayhello('Person B')
│
├── tests   
│   ├── lib  
│   │   └── test_talk.py    # from lib.talk import sayhello
│   └── proj_1              # def test_sayhello(x):
│       └── test_main.py    #   assert test_sayhello('Person A') == 'Hi, Person A!'
│                           # test_sayhello()
│    
└── test_setup.py           
```

now if i just run test_setup, it throws an error - even if i include the snippet within the tests.
this is because i am running pytest from the root directory i believe... in any case, below is the solution:

```python
# place this code in test_setup.py

import sys, os
path = os.path.abspath('')

if path not in sys.path:
    sys.path.append(path)
```

when pytest is run from the top-level directory, `test_setup.py` is run since it begins with "test".
the code appends the current directory name of `test_setup.py` (which is the root directory in this case) to the system path within the test environment.

in fact, in this way all my test files will not need to include the earlier snippet when import from the lib.

i tried to do the same for the project folders with `__init__.py` to no avail... i think it's because of where i'm calling the files from. oh wells, it don't matter.

caveat:
- somehow i can't run my tests individually using this T.T it only works if i run all of them...
(i know `pytest test_setup.py test\lib\test_sayhello.py` is supposed to work but... it doesn't T.T and i don't know why!!)

---

on a side note, i made the foolish mistake of not naming my test functions with a 'test_' prefix and my tests didn't run for the longest time.

be sure that even within your test files, you name your functions with a 'test_' prefix!!

---

on another note, i just learnt what configuration files are (and the difference in using json and YAML for it). basically, it replaces your argparse and stuff.

also, YAML is used for readability and you can comment on it. json is for greater ease of use across languages (or so i heard).

---

for logging, just `import logger`