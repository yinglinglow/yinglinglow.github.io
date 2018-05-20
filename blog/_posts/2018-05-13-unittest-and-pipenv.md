---
layout: post
title: "unittest and pipenv"
date: 2018-05-13
---

due to my inability to install pytest (LOL) i have resorted to the inbuilt unittest to do my TDD lol

---

now, how do we go about doing this?

the most important thing - write your test first, and watch it FAIL.

then, write code to make it pass.

---


now, step by step with pipenv:
1) git clone your repo
2) pipenv install
3) create requirements.txt file
4) pipenv install -r path/to/requirements.txt
5) pipenv shell (this enters the shell of pipenv. to exit the shell, type 'exit')

in the repo, create your app.py file, and two other folders: tests, and fixtures. fixtures contains fake data on which your tests will run.

inside tests folder, create a test_app.py file with the below code:

```python

import unittest

class Test_A_col_abcd(unittest.TestCase):

    def test_A_col_a(self):
        self.assertEqual('foo'.upper(), 'FOO')

    def test_A_col_b(self):
        self.assertTrue('FOO'.isupper())
        self.assertFalse('Foo'.isupper())

    def test_A_col_c(self):
        s = 'hello world'
        self.assertEqual(s.split(), ['hello', 'world'])
        # check that s.split fails when the separator is not a string
        with self.assertRaises(TypeError):
            s.split(2)

    def test_A_col_d(self):
        s = 'hello world'
        self.assertEqual(s.split(), ['hello', 'world'])
        # check that s.split fails when the separator is not a string
        with self.assertRaises(TypeError):
            s.split(2)

if __name__ == '__main__':
    unittest.main()

```

to run the tests, make sure you are in the pipenv shell before typing 'python tests/test_app.py'.

it should reflect 4 tests ran (passed in the above case!)

---

in a more practical scenario:

- you need to refer to your fixtures in another folder.

```python

import pathlib
import unittest
import sys
import pandas as pd
from pandas.util.testing import assert_frame_equal

path = str(pathlib.Path(file).parents[1]) # generate path to two folders up
if path not in sys.path:
    sys.path.append(path)
from main import create_df

class Test_main(unittest.TestCase):
    def test_table1(self):
        path = str(pathlib.Path(__file__).parents[1]) # generates path to proj_root
        t_filepath = f'{path}/fixtures/xxx.csv'
        t= create_df(t_filepath)

        t_test= pd.read_csv(f'{path}/fixtures/xxx.csv', dtype={'col1': str})
        assert_frame_equal(t.head(), t_test)

```