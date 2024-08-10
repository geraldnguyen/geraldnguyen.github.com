+++
title = 'Behavior Driven Development (BDD) with Selenium in Python'
subtitle = 'A beginner guide'
date = 2024-05-20T08:00:00-07:00
tags = [ "Programming", "Python", "Selenium", "BDD", "Tutorial"]
+++

This is a continuation from my previous article where we learned the basic of Selenium and how to program it in Python to perform UI automation testing.

[Getting started on UI automation testing using Selenium with Python](https://levelup.gitconnected.com/getting-started-on-ui-automation-testing-using-selenium-with-python-b4903dad5a74?source=post_page-----58390fcc0325--------------------------------)

{{< relativelink "/posts/2023/02/five-minimum-viable-interview-questions-series" >}}

Basics of BDD
=============

Behavior Driven Development is a popular software development discipline. As its name suggests, BDD project is guided by definition and verification of _behaviors_. A common form of _behavior_ can be expressed in the form of

> **given** &lt;some pre-requisites&gt;

> **when** &lt;some actions happen&gt;

> **then** &lt;some results to verify&gt;

This form of expression is popular enough that people created a language called [Gherkin](https://cucumber.io/docs/gherkin/) to express and extend it. Gherkin organizes a software’s behaviors into one or more `.feature` files, each may contain one or more scenarios in the _given…when…then_ form.

Example:

```
# Source: https://cucumber.io/docs/gherkin/reference/#example  
Feature: Guess the word  
  
  # The first example has two steps  
  Scenario: Maker starts a game  
    When the Maker starts a game  
    Then the Maker waits for a Breaker to join  
  
  # The second example has three steps  
  Scenario: Breaker joins a game  
    Given the Maker has started a game with the word "silky"  
    When the Breaker joins the Maker's game  
    Then the Breaker must guess a word with 5 characters
```
{{< figure src="1_ZWQCCWcTUsb9DoEVUAwcTQ.webp" caption="Gherkin as a language?" >}}


BDD in Software Development and Testing
---------------------------------------

Software development team uses BDD to describe software behaviors in simple, natural and consistent language. Its main functions are thus a documentation and communication tool across all disciplines and stakeholders such as end users, product managers, developers, testers.

Arguably, BDD is often considered a special form of Test Driven Development (TDD) and is most popular among software testers or developers who develop automation tests.

Implementing the Behavior in BDD
--------------------------------

Each BDD expression has a corresponding implementation. Take the second scenario above for example:

When a scenario read `Given the Maker has started a game with the word “silky”`, there are setup codes executing to create a new gameboard with the specified word “silky”

```
@given('the Maker has started a game with the word "silky"')  
def step_impl(context):  
    context.game = Game("silky")
```

The implementation can be very elaborated or can be just as simple as doing nothing

```
@when(u'the Breaker joins the Maker\'s game')  
def step_impl(context):  
    pass
```

It’s an error if we miss an implementation. Suppose we forgot to implement the last expression `Then the Breaker must guess a word with 5 characters` , we will receive the following error:

{{< figure src="1_TV-yoUTewIRnchjXC2oTRw.webp" caption="Missing implementation error" >}}


BDD with Selenium in Python: Overview
=====================================

Dependencies
------------

In Python, we can use [Behave](https://behave.readthedocs.io/en/latest/) to run BDD tests. It will pick up the `.features` file, their implementation and execute them accordingly.

```
pip install behave
```

We also need [Selenium](https://www.selenium.dev/) to launch and control Chrome

```
pip install selenium
```

Organization
------------

We will organize our files in the following structure:


```
Root project folder    
  |-- features  
        |-- steps  
        |    |-- a_feature_implementation.py  
        |-- environment.py  
        |-- a_feature.feature
```

*   Keep all `.feature` files under a single folder named `features`
*   Keep all implementation `.py` files under a subfolder named `steps` below `features`
*   If multiple features or scenarios shared the same setup, we can keep the setup (and cleanup) codes inside an `environment.py` file directly under the `features` folder

Write Feature files and implement the steps
-------------------------------------------

See the demo in the next section

Execute the test
----------------

See the demo in the next section

BDD with Selenium in Python: Demo
=================================

We will demonstrate how to describe and implement the 2 tests in the [previous article](https://medium.com/gitconnected/getting-started-on-ui-automation-testing-using-selenium-with-python-b4903dad5a74) in BDD style.

Quick Recap
-----------

*   The 2 test cases are implemented in 2 function `test_tc1_get_python_org_title(self)` and `test_tc2_search_getting_started(self)` respectively.
*   We use Selenium’s Chrome web driver to control the Chrome browser, inspect its content and simulate user actions. We define `setUp(self)` and `tearDown(self)` functions to handle the setting up and cleaning up of test’s fixture i.e. the Chrome web driver.
*   We use Python’s `unittest` library to run and assert desired results

```
import unittest  
from selenium import webdriver  
from selenium.webdriver.common.by import By  
  
class TestPythonOrg(unittest.TestCase):  
    def setUp(self):  
        self.driver = webdriver.Chrome()  
  
    def tearDown(self):  
        self.driver.quit()  
  
    def test_tc1_get_python_org_title(self):  
        driver = self.driver  
        driver.get("https://www.python.org")  
  
        print(driver.title)  
        self.assertEqual("Welcome to Python.org", driver.title)  
  
  
    def test_tc2_search_getting_started(self):  
        driver = self.driver  
  
        driver.get("https://www.python.org")  
  
        search_input_field = driver.find_element(By.ID, "id-search-field")  
        search_input_field.clear()  
        search_input_field.send_keys("getting started in Python")  
  
        search_go_button = driver.find_element(By.ID, "submit")  
        search_go_button.click()  
  
        print("current url: " + driver.current_url)  
        self.assertEqual("https://www.python.org/search/?q=getting+started+in+Python&submit=", driver.current_url)  
  
        results = driver.find_elements(By.CSS_SELECTOR, ".list-recent-events li")  
        print(f"number of results: {len(results)}")  
        self.assertGreater(len(results), 0)  
  
if __name__ == '__main__':  
    unittest.main()
```

BDD Scenarios
-------------

We can express the previous test cases as the following 2 scenarios. We place them under a single feature here but you can have them in separate `.feature` files.

```
# file: lauch_python_org.feature  
  
Feature: Launch and Search Python.org  
  
    Scenario: Launch Python.org  
      Given we have selenium installed  
       When we launch https://www.python.org in chrome  
       Then we obtain title "Welcome to Python.org"  
  
    Scenario: Search Getting started in Python  
      Given we have selenium installed  
       When we launch https://www.python.org in chrome  
        And we search "getting started in Python"  
       Then we arrive at search page  
        And we obtain some result
```

In both scenario:

*   We make an explicit assumption that we have Selenium installed → the `Given`
*   Then we open [https://www.python.org](https://www.python.org./) in Chrome → the `When`. Scenario 2 perform an extra action (search “getting started in Python”) → the `And` following a `When`
*   Each scenario performs their own validation of the obtained result → the `Then`. Scenario 1 validate that the page’s title is “Welcome to Python.org” while Scenario 2 asserts two conditions: that the browser arrived at the search page, and that it contains some search results → the `Then` and the `And` following that Then

We can observe that `And` has flexible meanings depending on where it is used. It takes after the keyword it follows:

*   Following a `Given`, it specifies an additional pre-requisite
*   Following a `When`, it specifies an additional action
*   Following a `Then`, it specifies an additional assertion

Implementing the behaviors
--------------------------

Since our 2 test scenarios requires the use of Selenium to launch and control Chrome browser — so-called test’s fixture, we’ll create an `environment.py` to set that up.

*   The `selenium_browser_chrome(context)` is responsible for setting Selenium’s Chrome `webdriver` and save its reference to the the `context.browser` attribute. It returns that attribute and `yield` the execution to Behave’s runtime to execute all the tests. When all tests finish, it gains back control and shutdown the Chrome browser by calling the `.quit()` method
*   The `before_all(context)` method runs once at the beginning. It tells Behave to call `selenium_browser_chrome` method to setup the fixture.

```
# file: features/environment.py  
from behave import fixture, use_fixture  
from selenium import webdriver  
  
@fixture  
def selenium_browser_chrome(context):  
    context.browser = webdriver.Chrome()  
    yield context.browser  
    context.browser.quit()  
  
def before_all(context):  
    use_fixture(selenium_browser_chrome, context)
```

We implement the remaining behaviors as below:

*   We create a file `features/steps/lauch_python_org.py` to contain our implementation
*   We import necessary codes from Behave and Selenium at the start of the file
*   For the `Given`, the `environment.py` already setup Selenium for us. We can just `pass` it, or we can explicitly assert that calling `assert hasattr(context, ‘browser’)`
*   We obtain the Chrome webdriver by the `context.browser` expression
*   Using the Chrome webdriver, we open [https://www.python.org,](https://www.python.org,/) examine its title, search “Getting started in Python”… just like we did in the [previous article](https://medium.com/gitconnected/getting-started-on-ui-automation-testing-using-selenium-with-python-b4903dad5a74).

```
# file: features/steps/lauch_python_org.py  
  
from behave import *  
from selenium.webdriver.common.by import By  
  
@given('we have selenium installed')  
def step_impl(context):  
    assert hasattr(context, 'browser')  
  
@when('we launch https://www.python.org in chrome')  
def step_impl(context):  
    context.browser.get("https://www.python.org")  
  
@then('we obtain title "Welcome to Python.org"')  
def step_impl(context):  
    assert context.browser.title == "Welcome to Python.org"  
  
@when('we search "getting started in Python"')  
def step_impl(contex):  
    browser = contex.browser  
    search_input_field = browser.find_element(By.ID, "id-search-field")  
    search_input_field.clear()  
    search_input_field.send_keys("getting started in Python")  
  
    search_go_button = browser.find_element(By.ID, "submit")  
    search_go_button.click()  
  
@then('we arrive at search page')  
def step_impl(context):  
    assert context.browser.current_url == "https://www.python.org/search/?q=getting+started+in+Python&submit="  
  
@then('we obtain some result')  
def step_impl(context):  
    results = context.browser.find_elements(By.CSS_SELECTOR, ".list-recent-events li")  
    assert  len(results) > 0
```

Execute the test
----------------

Depending on how you setup your project, it can be as simple as typing `behave` into the terminal prompt (in PyCharm virtual environment setup), or as complicated as locating where `behave` is installed in order to execute it.

If you have run `pip install behave` previously, you can also type `python -m behave` to execute the test

```
PS ...\bdd-getting-started> behave  
DevTools listening on ws://127.0.0.1:59978/devtools/browser/9ab3805c-031d-45cc-bd58-b601e1979e50  
Feature: Launch and Search Python.org # features/lauch_python_org.feature:1  
  Scenario: Launch Python.org                       # features/lauch_python_org.feature:3  
    Given we have selenium installed                # features/steps/launch_python_org.py:5  
    When we launch https://www.python.org in chrome # features/steps/launch_python_org.py:8  
    Then we obtain title "Welcome to Python.org"    # features/steps/launch_python_org.py:12  
  Scenario: Search Getting started in Python        # features/lauch_python_org.feature:8  
    Given we have selenium installed                # features/steps/launch_python_org.py:5  
    When we launch https://www.python.org in chrome # features/steps/launch_python_org.py:8  
    And we search "getting started in Python"       # features/steps/launch_python_org.py:16  
    Then we arrive at search page                   # features/steps/launch_python_org.py:26  
    And we obtain some result                       # features/steps/launch_python_org.py:30  
1 feature passed, 0 failed, 0 skipped  
2 scenarios passed, 0 failed, 0 skipped  
8 steps passed, 0 failed, 0 skipped, 0 undefined  
Took 0m1.844s
```

Conclusion
==========

We have briefly learn what Behavior-Driven development is, how a software development team uses it, and how we can do so with [Behave](https://behave.readthedocs.io/), [Selenium](https://www.selenium.dev/) in Python.

For the demo, we converted our [previous project](https://levelup.gitconnected.com/getting-started-on-ui-automation-testing-using-selenium-with-python-b4903dad5a74), which contains 2 simple Selenium-based UI automation tests, into 2 BDD scenarios. You can get the sample codes from the GitHub link below

[GitHub - geraldnguyen/bdd-getting-started: Behavior Driven Development (BDD) with Selenium in…](https://github.com/geraldnguyen/bdd-getting-started?source=post_page-----58390fcc0325--------------------------------)

