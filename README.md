# shot-scraper

[![PyPI](https://img.shields.io/pypi/v/shot-scraper.svg)](https://pypi.org/project/shot-scraper/)
[![Changelog](https://img.shields.io/github/v/release/simonw/shot-scraper?include_prereleases&label=changelog)](https://github.com/simonw/shot-scraper/releases)
[![Tests](https://github.com/simonw/shot-scraper/workflows/Test/badge.svg)](https://github.com/simonw/shot-scraper/actions?query=workflow%3ATest)
[![License](https://img.shields.io/badge/license-Apache%202.0-blue.svg)](https://github.com/simonw/shot-scraper/blob/master/LICENSE)

Tool for taking automated screenshots

## Installation

Install this tool using `pip`:

    pip install shot-scraper

This tool depends on Playwright, which first needs to install its own dedicated browser.

Run `shot-scraper install` once to install that:
```
% shot-scraper install
Downloading Playwright build of chromium v965416 - 117.2 Mb [====================] 100% 0.0s 
Playwright build of chromium v965416 downloaded to /Users/simon/Library/Caches/ms-playwright/chromium-965416
Downloading Playwright build of ffmpeg v1007 - 1.1 Mb [====================] 100% 0.0s 
Playwright build of ffmpeg v1007 downloaded to /Users/simon/Library/Caches/ms-playwright/ffmpeg-1007
```
## Taking a screenshot

To take a screenshot of a web page and write it to `screenshot.png` run this:

    shot-scraper https://datasette.io/ -o screenshot.png

If you omit the `-o` the screenshot PNG binary will be output by the tool, so you can pipe it or redirect it to a file:

    shot-scraper https://datasette.io/ > datasette.png

Screenshots default to being 1280px wide and as long as needed to capture the full page.

To take a screenshot of a specific element on the page, use `--selector` with its CSS selector:

    shot-scraper https://simonwillison.net/ -s '#bighead' -o bighead.png

You can use custom JavaScript to modify the page after it has loaded (after the 'onload' event has fired) but before the screenshot is taken using the `--javascript` option:

    shot-scraper https://simonwillison.net/ -o simonwillison-pink.png \
      --javascript "document.body.style.backgroundColor = 'pink';"

## Taking multiple screenshot

You can configure multiple screenshots using a YAML file. Create a file called `shots.yml` that looks like this:

```yaml
- output: example.com.png
  url: http://www.example.com/
- output: w3c.org.png
  url: https://www.w3.org/
```
Then run the tool like so:

    shot-scraper multi shots.yml

This will create two image files, `example.com.png` and `w3c.org.png`, containing screenshots of those two URLs.

To take a screenshot of just the area of a page defined by a CSS selector, add `selector` to the YAML block:

```yaml
- output: bighead.png
  url: https://simonwillison.net/
  selector: "#bighead"
```
To execute JavaScript after the page has loaded but before the screenshot is taken, add a `javascript` key:
```yaml
- output: bighead-pink.png
  url: https://simonwillison.net/
  selector: "#bighead"
  javascript: |
    document.body.style.backgroundColor = 'pink'
```
## Development

To contribute to this tool, first checkout the code. Then create a new virtual environment:

    cd shot-scraper
    python -m venv venv
    source venv/bin/activate

Or if you are using `pipenv`:

    pipenv shell

Now install the dependencies and test dependencies:

    pip install -e '.[test]'

To run the tests:

    pytest
