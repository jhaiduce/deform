# Contributing

All projects under the Pylons Projects, including this one, follow the guidelines established at [How to Contribute](https://pylonsproject.org/community-how-to-contribute.html), [Coding Style and Standards](https://pylonsproject.org/community-coding-style-standards.html), and [Pylons Project Documentation Style Guide](https://docs.pylonsproject.org/projects/docs-style-guide/).

You can contribute to this project in several ways.

* [File an Issue on GitHub](https://github.com/Pylons/deform/issues)
* Fork this project and create a branch with your suggested change. When ready, submit a pull request for consideration. [GitHub Flow](https://guides.github.com/introduction/flow/index.html) describes the workflow process and why it's a good practice. When submitting a pull request, sign [CONTRIBUTORS.txt](https://github.com/Pylons/deform/blob/master/CONTRIBUTORS.txt) if you have not yet done so. Also update [CHANGES.txt](https://github.com/Pylons/deform/blob/master/CHANGES.txt) with a change log entry.
* Join the [IRC channel #pyramid on irc.freenode.net](https://webchat.freenode.net/?channels=pyramid).


## Discuss proposed changes first

To propose a change, open an issue on GitHub for discussion, preferably before starting any coding.
Ask the authors' opinion how the issue should be approached, as this will make it easier to merge the pull request later.


## Documentation

All features must be documented with code samples in narrative documentation, API documentation, and in [deformdemo](https://github.com/Pylons/deformdemo).


## Using tox

We use [tox](https://tox.readthedocs.io/en/latest/install.html) to manage virtual environments, run tests across all supported versions of Python, build docs, perform lint checks, and perform test coverage.

See `tox.ini` at the root of the repository for current supported environments.
You can run tests in a specific environment by specifying it on the command line, or multiple environments as comma-separated values.

    tox -e py38,docs


## Unit tests

All features must be covered by unit tests, so that test coverage stays at 100%.


## Functional tests

All features must be covered by functional tests and have example usage.
We use the following for running functional tests.

* Firefox latest
* geckodriver
* [Selenium 3.x](https://pypi.org/project/selenium/)
* [gettext](https://www.gnu.org/software/gettext/)
* [tox](https://tox.readthedocs.io/en/latest/)
* [deformdemo](https://github.com/pylons/deformdemo)

If you add or change a feature that reduces test coverage or causes a functional test to fail, then you also must submit a pull request to the [deformdemo](https://github.com/pylons/deformdemo) repository to go along with your functional test change to Deform.

For functional tests, tox runs the shell script [run-selenium-tests.bash](https://github.com/Pylons/deform/blob/master/run-selenium-tests.bash), located at the root of the Deform repository.
See its comments for a description.

To run functional tests.

    tox -e functional3

Stop on error.

    tox -e functional3 -- -x

Run only the last failed tests.

    tox -e functional3 -- --failed

Run a single test.

    tox -e functional3 -- deformdemo.test:CheckedInputWidgetWithMaskTests

    tox -e functional3 -- deformdemo.test:SequenceOfMaskedTextInputs.test_submit_one_filled


### Preparing a functional testing environment

To avoid conflicts with other possibly installed versions of applications, we suggest that you install the following applications into your local checkout of Deform.
We will assume that you put your projects in your user directory, although you can put them anywhere.

    cd ~/projects/deform/


#### Set an environment variable

Set an environment variable to add your local checkout of Deform to your `PATH`.
It must to be set before running tox or nosetest, otherwise Firefox or Chrome will not start and will return an error message of `'geckodriver' executable needs to be in PATH.`.

    export PATH=~/projects/deform:$PATH


#### Firefox latest

##### macOS

[Download the latest version of Firefox for your platform](https://www.mozilla.org/en-US/firefox/all/).

Open the `.dmg` (macOS), and drag the Firefox icon to:

    ~/projects/deform/

##### Linux (Debian)

Use cURL or wget.
See the [Firefox download README.txt](https://ftp.mozilla.org/pub/firefox/releases/latest/README.txt) for instructions.
For example on Linux:

    cd ~/projects/deform/
    wget -O firefox-latest.tar.bz2 \
    "https://download.mozilla.org/?product=firefox-latest&os=linux64&lang=en-US"

Decompress the downloaded file.

    tar -xjf firefox-latest.tar.bz2

#### geckodriver

Install the [latest release of geckodriver](https://github.com/mozilla/geckodriver/releases).

    # macOS
    wget https://github.com/mozilla/geckodriver/releases/download/v0.26.0/geckodriver-v0.26.0-macos.tar.gz
    tar -xzf geckodriver-v0.26.0-macos.tar.gz

    # Linux (Debian)
    wget https://github.com/mozilla/geckodriver/releases/download/v0.26.0/geckodriver-v0.26.0-linux64.tar.gz
    tar -xzf geckodriver-v0.26.0-linux64.tar.gz


#### gettext

The functional tests require the installation of the GNU `gettext` utilities, specifically `msgmerge` and `msgfmt`.
Use your package manager to install these requirements.

##### macOS
 
Use [Homebrew](https://brew.sh/):

    brew install gettext
    brew link gettext --force

If you ever have problems building packages, you can always unlink it.

    brew unlink gettext

##### Linux (Debian)

    apt-get install gettext
    apt-get install gettext-base


#### Selenium

Selenium is installed automatically by tox via `pip install -e .["testing"]`.


### Testing on Chrome or Chromium

Tests might not run correctly on Chrome due to various timing issues.
Some effort was put forth to fix this many years ago, but it was a never ending swamp.
However, [the situation might have improved recently](https://developers.google.com/web/updates/2017/04/headless-chrome).

If you accept the challenge, we welcome pull requests to this contributing guide, along with tests to support testing on Chrome.
The following are some clues to get you started.

- To use google-chrome or Chromium, download the web browser and respective webdriver, [chromiumdriver](https://chromedriver.chromium.org/downloads).
  These would be used instead of Firefox and geckodriver.
- Set the `WEBDRIVER` environment variable to chromiumdriver instead of geckodriver.
- Profit! Fun! World domination!
