# AdvancePlaywrightFramework

AdvancePlaywrightFramework is a Playwright + TypeScript automation framework for the TTACart web application. It is designed to help teams build scalable, maintainable UI test automation using a modular structure, reusable fixtures, environment-aware configuration, and page object abstractions.

## Highlights

- Playwright-based UI automation with TypeScript
- Page Object Model for login, inventory, cart, and checkout flows
- Reusable custom fixtures for application state setup
- Environment-variable-driven configuration
- Data generation for realistic credentials and customer data
- Structured logging and custom HTML reporting
- Support for both simple login tests and end-to-end checkout scenarios

## Project structure

```text
.
├── .github/                    GitHub workflow files
├── docs/                      Project documentation
├── rules/                     Rule and standards documents
├── src/
│   ├── config/                Environment and credential helpers
│   ├── fixtures/              Custom Playwright fixtures and state setup
│   ├── pages/                 Page Object Model classes
│   ├── testdata/              JSON and data files used in tests
│   ├── tests/
│   │   ├── e2e/               End-to-end flow specs
│   │   ├── login/             Login-focused specs
│   │   └── example.spec.ts   Basic example smoke test
│   └── utils/
│       ├── CustomReporter.ts  Custom HTML report generator
│       ├── DataGenerator.ts   Faker-based test data creation
│       ├── logger.ts          Winston logger setup
│       ├── UtilElementLocator.ts  Stable locator wrapper
│       └── visualStep.ts      Screenshot-aware step helper
├── .gitignore
├── package.json
├── package-lock.json
├── playwright.config.ts
├── tsconfig.json
├── README.md
├── logs/                      Log files generated at runtime
├── reports/                   JSON run summaries
├── playwright-report/         Playwright HTML output
├── tta-report/                Custom TTA HTML report output
└── node_modules/              Installed dependencies
```

## Prerequisites

- Node.js 18 or later
- npm
- Playwright browser binaries installed

## Setup

```bash
npm install
npx playwright install --with-deps
```

## Environment configuration

The framework loads environment values from `.env` and resolves the application base URL in `playwright.config.ts`.

Common variables:

```bash
BASE_URL=
QA_BASE_URL=
STG_BASE_URL=
PROD_BASE_URL=
DEV_BASE_URL=
API_BASE_URL=
TTA_ENV=qa
STANDARD_USER=standard_user
TTA_SECRET=tta_secret
```

If no variables are provided, the framework falls back to the QA TTACart URL:

- `https://app.thetestingacademy.com`

## Running tests

Run the full suite:

```bash
npx playwright test
```

Run a single login spec:

```bash
npx playwright test src/tests/login/Login.spec.ts --project=chromium
```

Run the checkout end-to-end flow:

```bash
npx playwright test src/tests/e2e/e2e-checkout.spec.ts --project=chromium
```

Open the built-in HTML report:

```bash
npx playwright show-report
```

Open the custom TTA HTML report if generated:

```bash
npx playwright test
# then open the HTML file generated under tta-report/
```

## Example usage

### Login page object

```ts
import { LoginPage } from '@pages/LoginPage';

const loginPage = new LoginPage(page);
await loginPage.open();
await loginPage.loginAs('standard_user', 'tta_secret');
```

### Fixture-based test setup

```ts
import { test } from '@fixtures/test-base';

test('login and navigate to inventory', async ({ loginPage, inventoryPage }) => {
  await loginPage.open();
  await loginPage.loginAs('standard_user', 'tta_secret');
  await inventoryPage.assertLoaded();
});
```

## Included capabilities

- Login automation using TTACart data-test selectors
- Inventory page interactions, cart validation, and checkout flow coverage
- Fixture-driven state preparation such as `validLogin`, `invalidLogin`, and `loginWithInventory`
- Data generation using Faker for credentials and customer checkout info
- Custom logs, screenshots, and HTML test reporting to speed up debugging

This repository is built for extension: new screen objects, fixtures, and API layers can be added without disrupting the current test architecture.
