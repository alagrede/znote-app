# Playwright demo

**npm package installation**
`npm i -S playwright`

Open browser and **take a screenshot**
```js
const { chromium } = require('playwright');

const browser = await chromium.launch();
const page = await browser.newPage();
await page.goto('https://playwright.dev/');

const results = await page.$("[class*=heroTitle]");
const text = await results.evaluate(element => element.innerText);
print(text); // Get text from title
await page.screenshot({ path: '/Users/alagrede/example.png' });
//open("/Users/alagrede/example.png");
await browser.close();
```


## Links
- [Automated Headless Browser scripting in Node.js with Playwright](https://www.twilio.com/blog/automated-headless-browser-scripting-in-node-js-with-playwright)
- [Playwright Library](https://playwright.dev/docs/api/class-playwright)