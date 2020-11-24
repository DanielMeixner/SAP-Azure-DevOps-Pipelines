# Necessary changes to enable karma testing

## 1. Edit package.json

In the HTML5 webapp the following dependencies need to be added to the **devDependencies** in the [package.json](HTML5Module/package.json):

```json
    "karma": "^5.0.4",
    "karma-chrome-launcher": "^3.1.0",
    "karma-coverage": "^2.0.2",
    "karma-ui5": "^2.1.0"
```

In addition two **scripts** need to be added to the [package.json](HTML5Module/package.json):

```json
    "test": "npm run karma-ci",
    "karma-ci": "karma start karma-ci.conf.js
```

The test script is called by the pipeline and thus needs to be defined. It in turn calls the script defintion from the [karma-ci.conf.js file](HTML5Module/karma-ci.conf.js), which is described in the next section.

## 2. Karma configuration

Two files need to be added to the HTML5 webapp dir: [karma.conf.js file](HTML5Module/karma.conf.js) and [karma-ci.conf.js](HTML5Module/karma-ci.conf.js). They describe how the tests should be run using a custom webdriver launcher to connect to a remote Chrome browser.

## 3. Add test page

To run both unit and integration tests the corresponfing testpages are added to a testsuite. The testsuite files need to be added to the `webapp/test` dir as can be seen with [HTML5Module/webapp/test/testsuite.qunit.html](HTML5Module/webapp/test/testsuite.qunit.html) and [HTML5Module/webapp/test/testsuite.qunit.js](HTML5Module/webapp/test/testsuite.qunit.js).

Now the tests should be able to run in our ui5 pipeline.
