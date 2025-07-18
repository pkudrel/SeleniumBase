<!-- SeleniumBase Docs -->

## [<img src="https://seleniumbase.github.io/img/logo6.png" title="SeleniumBase" width="32">](https://github.com/seleniumbase/SeleniumBase/) Using Desired Capabilities

You can specify browser capabilities when running SeleniumBase tests on a remote Selenium Grid server (such as <a href="https://www.browserstack.com/automate/capabilities" target="_blank">BrowserStack</a> or <a href="https://saucelabs.com/products/platform-configurator" target="_blank">Sauce Labs</a>).

Sample run commands may look like this when run from the [SeleniumBase/examples/](https://github.com/seleniumbase/SeleniumBase/tree/master/examples) folder: (The browser is now specified in the capabilities file.)

```zsh
pytest test_demo_site.py --browser=remote --server=USERNAME:KEY@hub.browserstack.com --port=80 --cap_file=capabilities/sample_cap_file_BS.py
```

```zsh
pytest test_demo_site.py --browser=remote --server=USERNAME:KEY@ondemand.us-east-1.saucelabs.com --port=443 --protocol=https --cap_file=capabilities/sample_cap_file_SL.py
```

(Parameters: ``--browser=remote``, ``--server=SERVER``, ``--port=PORT``, and ``--cap_file=CAP_FILE.py``)

Here's an example desired capabilities file for BrowserStack using the newer SDK format in a `.yml` / `.yaml` file:

```yml
platforms:
  - browserName: safari
    osVersion: 17
    deviceName: iPhone 15 Pro Max
buildIdentifier: ${BUILD_NUMBER}
parallelsPerPlatform: 1
projectName: My Project
browserstackLocal: true
debug: true
networkLogs: true
```

Here's an example desired capabilities file for BrowserStack using the legacy JSONWP format in a `.py` file:

```python
desired_cap = {
    "browser": "Chrome",
    "os": "Windows",
    "os_version": "11",
    "browser_version": "latest",
    "browserstack.console": "info",
    "browserstack.debug": "true",
    "browserstack.networkLogs": "true",
    "browserstack.local": "true",
}
```

Here's an example desired capabilities file for Sauce Labs:

```python
capabilities = {
    "browserName": "chrome",
    "browserVersion": "latest",
    "platformName": "macOS 10.14",
    "sauce:options": {},
}
```

(Note that the browser is now being specified in the capabilities file, rather than with ``--BROWSER`` when using a **remote** Selenium Grid. If using a **local** Selenium Grid, specify the browser, eg: ``--firefox``.)

<div><b>You can generate specific desired capabilities using:</b></div>

<ul>
    <li><a href="https://www.browserstack.com/docs/automate/capabilities" target="_blank">BrowserStack desired capabilities</a></li>
    <li><a href="https://saucelabs.com/products/platform-configurator" target="_blank">Sauce Labs desired capabilities</a></li>
</ul>

<div><b>Parsing desired capabilities:</b></div>

SeleniumBase has a desired capabilities parser that can capture all lines from the specified file in the following formats:

```python
'KEY': 'VALUE'
'KEY': True
'KEY': False
caps['KEY'] = "VALUE"
caps['KEY'] = True
caps['KEY'] = False
```

(Each pair must be on a separate line. You can interchange single and double quotes.)

You can also swap ``--browser=remote`` with an actual browser, eg ``--browser=chrome``, which will combine the default SeleniumBase desired capabilities with those that were specified in the capabilities file when using ``--cap_file=FILE.py``. Capabilities will override other parameters, so if you set the browser to one thing and the capabilities browser to another, SeleniumBase will use the capabilities browser.

You'll need default SeleniumBase capabilities for:
* Using a proxy server (not the same as a Selenium Grid server)
* Downloading files to a desired folder
* Disabling some warnings on Chrome
* Overriding a website's Content Security Policy on Chrome
* Other possible reasons

You can also set browser desired capabilities from a command-line string. Eg:

```zsh
pytest test_swag_labs.py --cap-string='{"browserName":"chrome","name":"test1"}' --server="127.0.0.1" --browser=remote
```

(Enclose cap-string in single quotes. Enclose parameter keys in double quotes.)

If you pass ``"*"`` into the ``"name"`` field of ``--cap-string``, the name will become the test identifier. Eg:

```zsh
pytest my_first_test.py --cap-string='{"browserName":"chrome","name":"*"}' --server="127.0.0.1" --browser=chrome
```

Example name: ``"my_first_test.MyTestClass.test_basics"``

<h3>Using a local Selenium Grid</h3>

If using a local Selenium Grid with SeleniumBase, start up the Grid Hub and nodes first:

```zsh
sbase grid-hub start
sbase grid-node start
```

(The Selenium Server JAR file will be automatically downloaded for first-time Grid users. You'll also need Java installed to start up the Grid.)
