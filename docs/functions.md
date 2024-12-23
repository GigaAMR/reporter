Testomat functions gives you more flexibility in reporting and make your reports more powerful.

### Usage example

```javascript
import testomat from '@testomatio/reporter'; // or const testomat = require('@testomatio/reporter');

test('my test', async () => {
  testomat.artifact('path/to/file');
});
```

Or import only required functions:

```javascript
import { artifact, log, meta } from '@testomatio/reporter'; // or const { artifact, log } = require('@testomatio/reporter');

test('my test', async () => {
  meta('ISSUE', 'MY-123');
  await page.login();
  log`I was logged in with user ${user}`;
  artifact(await saveScreenshot());
  assert(something);
});
```

After you import and invoke `testomat`, autocompletion will help you to find the right function.

### Available functions

- [artifact](#artifact)
- [log](#log)
- [step](#step)
- [meta (key:value)](#meta)

### Artifact

Adds file to the test report (text, image, video, etc.)

```javascript
import { artifact } from '@testomatio/reporter';

test('my test', async () => {
  const pathToFile = await saveScreenshot();
  artifact(pathToFile);
});
```

> Artifacts, generated by testrunners (like screenshots/videos by Playwright) are uploaded automatically, you don't need to add them manually using artifact function. Just setup [S3 bucket](./artifacts.md).

### Log

Similar to [step](#step) function, intended to log any additional info to the test report (including text, arrays, complex objects).

##### Usage examples:

```javascript
testomat.log('your message');
testomat.log(`your message ${variable}`);
testomat.log('your message', variable1, variable2);
```

```javascript
const testomat = require('@testomatio/reporter');
test('Your test @T12345678', async () => {
  await page.login(user);
  testomat.log`I was logged in with user ${user}`;
  assert(loggedIn);

  log`I was logged in with user ${user}`; // < shorter syntax
});
```

### Step

Adds step to the test report. Step is a human-readable description of the test action. It is used to describe the test flow. This function is similar to [log](#log) function, but looks differently in the report.

```javascript
const testomat = require('@testomatio/reporter');
describe('Your suite @S12345678', () => {
  test('Your test @T12345678', async () => {
    await page.login();
    testomat.step`Login successful`;
    assert(something);
  });
});
```

### Meta

Meta information is a key:value pair(s), which is used to add additional information to the test report. E.g. browser, environment, etc.

```js
import { meta } from '@testomatio/reporter';

test('my test', () => {
  // use it inside tests as key, value
  meta('browser', 'chrome');

  // or use it as an object
  meta({
    browser: 'chrome',
    server: 'staging',
  });
})
```

Or in CommonJS style:

```javascript
const { meta } = require('@testomatio/reporter');

test('Your test @T12345678', async () => {
  await page.login();
  testomat.meta({
    browser: 'chrome',
    testType: 'smoke',
  });
  assert(something);
});
```

---

Supported frameworks:

- 🟢 CodeceptJS
- 🟢 Cucumber
- 🟢 Jest
- 🟢 Mocha
- 🟢 Playwright
- 🟢 WDIO (everything, except artifacts)
