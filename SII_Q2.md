SII Second Stage Assessment Question 2
--------------------------------------

Introduction
------------
In this document, I will be describing one of the bugs I solved for Mozilla Balrog.

Installation
------------
1. Clone the Balrog repository from Github
    ```
    git clone https://github.com/mozilla-releng/balrog
    ```

2. Install Docker and docker-compose


Code
----
Specific Pull Request discussed in this document: https://github.com/mozilla-releng/balrog/pull/182/


Bug Report: https://bugzilla.mozilla.org/show_bug.cgi?id=1310213

Usage
-----

One of the test I wrote for deleting rule is [testAddScheduledChangeExistingDeletingRule](https://github.com/mozilla-releng/balrog/blob/12ff286c81264273f3a4ccb83602669bf79c457a/auslib/test/admin/views/test_rules.py#L852)
 
1. In order to run the specific test, replace [L29](https://github.com/mozilla-releng/balrog/blob/main/tox.ini#L29) in tox.ini with

    ```
    {posargs:py.test -n auto --cov-config=tox.ini --cov-append --cov=auslib --cov-report term-missing test/admin/views/test_rules.py::TestRuleScheduledChanges::testAddScheduledChangeExistingDeletingRule}
    ```
    *Note: Without this step, every test case will be run.*
    
2. Install tox
    ```
    pip install tox
    ```
3. Run tox
    ```
    tox
    ```
Other work for Balrog
---------------------

My other Pull Requests to Balrog: https://github.com/mozilla-releng/balrog/pulls?q=is%3Apr+is%3Aclosed+author%3Aninadbhat
