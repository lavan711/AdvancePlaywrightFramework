# AdvancePlaywrightFramework

AdvancePlaywrightFramework is a Playwright + TypeScript automation framework built for the TTACart application. It follows a clean Page Object Model structure, includes environment-aware configuration, custom reporting, reusable utilities, and a ready-to-run login spec.

## Features

- Playwright test setup with browser/device configuration
- Page Object Model for maintainable UI automation
- Custom logging using Winston
- Reusable locator abstraction for stable element interactions
- Faker-based data generation helpers
- Custom HTML report generation for test automation results
- Environment-driven base URL selection via `BASE_URL` / `TTA_ENV`

## Project structure

```text
.
├── .github/                 GitHub workflow files
├── docs/                   Project docs and notes
├── rules/                  Rule/configuration notes
├── src/
│   ├── pages/              Page Object Model classes
│   ├── tests/              Playwright test specs
│   └── utils/              Logger, locators, reporter, generators
├── .gitignore
├── package.json
├── package-lock.json
├── playwright.config.ts    Playwright configuration and env handling
├── tsconfig.json
├── README.md
├── logs/                   Runtime logs (created during execution)
├── reports/                Summary reports (created during execution)
└── playwright-report/      HTML Playwright report output
```

## Prerequisites

- Node.js 18 or later
- npm
- Playwright browsers installed

## Setup

```bash
npm install
npx playwright install --with-deps
```

## Environment configuration

The framework resolves the application URL from environment variables in `playwright.config.ts`.

Supported variables:

```bash
BASE_URL=
QA_BASE_URL=
STG_BASE_URL=
PROD_BASE_URL=
DEV_BASE_URL=
API_BASE_URL=
TTA_ENV=qa
```

If no environment variables are set, the default QA URL is used:

- `https://app.thetestingacademy.com`

## Running tests

Run the full suite:

```bash
npx playwright test
```

Run a single spec:

```bash
npx playwright test src/tests/Login.spec.ts --project=chromium
```

Open the HTML report after execution:

```bash
npx playwright show-report
```

## Example test flow

The repository includes a login flow using the `LoginPage` page object:

```ts
const loginPage = new LoginPage(page);
await loginPage.open();
await loginPage.loginAs('standard_user', 'tta_secret');
```

This project is structured to be extended with additional page objects, fixtures, and API test layers as the suite grows.
