# Setting up code coverage with Cypress and Next.js on a repo

I am writing tests for an open source application in [this](https://github.com/pramam/refine) repo, forked from [github.com/pankod/refine](https://github.com/pankod/refine).

The tests are being written for an example app in this repo, fineFoods. fineFoods is a restaurant delivery app with a client and an admin application. One can place orders from a menu in the client app, and the admin app allows one to accept the order, process it and deliver it.

The client app and the admin app can be seen [here](https://refine.dev/demo/).

Here's a live version of the client app: [https://example.refine.dev/](https://example.refine.dev/).
Here's a live version of the admin app: [https://example.admin.refine.dev/](https://example.admin.refine.dev/).

Watch [this](https://youtu.be/C8g5X4vCZJA) video in the [cypress documentation](https://docs.cypress.io/guides/tooling/code-coverage) to get context on how to get started. Specifically, we need to:
- instrument the code
- run cypress tests
- see code coverage results

## Instrumenting the client to show Cypress code coverage

All commands below are run in the directory `<repo>/examples/fineFoods/client`

1. run `npm install` and then run `npm run start` to make sure the client application comes up.
2. install cypress 

   $npm install cypress --save-dev
2. Run `npx cypress open` to make sure the example tests run.
3. The sample cypress tests automatically installed by cypress go to a different application outside of the "refine" repo. So it is not possible to instrument that code. We'll have to write a test. Here's a simple test on the refine fineFoods client application:
```
//cypress/integration/3-finefoods/order.spec.js
/// <reference types="cypress" />
describe("Order an item", () => {
    beforeEach(() => {
        cy.visit("localhost:3001");
    });
    it("Click explore button", () => {
        cy.get("button").contains("Explore menu").click();
    });
});
```
4. Run `npx cypress open`. This will bring up a cypress window, run the test 3-finefoods/order.spec.js to make sure the test passes.
5. Add the `istanbul` instrumenting plugin to `./babelrc`:
```
    {
        "presets": [
          "next/babel"
        ],
        "plugins": [
           [
            "import",
            {
                "libraryName": "antd",
                "style": true
            }
           ],
           "istanbul"
       ]
    }
```
6. Install the `istanbul` [plugin] (https://github.com/istanbuljs/babel-plugin-istanbul)

   $npm install --save-dev babel-plugin-istanbul
7. Now we need to check that our cypress test is instrumented. After you run the cypress test, in the window in which the Cypress test ran, open dev tools.
   
