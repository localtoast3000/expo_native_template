# 📱 expo_native_template

A robust, typed Expo app template built with `expo-router`, environment-based configuration, and a strong focus on native platform readiness and localization (i18n).

---

## 🚀 Quick Start

1. **Install dependencies**

   ```bash
   yarn install
   ```

2. **Start the app (with i18n + env setup)**

   ```bash
   yarn start
   ```

---

## 🧱 Native Prebuild & Renaming the Project

To clean and regenerate the native directories (`android` and `ios`), run:

```bash
yarn build:native
```

Which executes:

```bash
rm -rf ./android && rm -rf ./ios && yarn dlx expo prebuild
```

---

### 🔄 Changing the Project Name

If you need to rename the project after cloning or starting from the base template, follow these steps:

1. **Update `package.json`:**

```json
{
  "name": "your_project_name"
}
```

2. **Update `app.json`:**

```json
{
  "name": "your_project_name",
  "slug": "your_project_name",
  "scheme": "yourprojectscheme",
  "ios": {
    "bundleIdentifier": "com.yourproject.identifier"
  },
  "android": {
    "package": "com.yourproject.identifier"
  }
}
```

> ⚠️ Make sure your `name`, `slug`, and identifiers match your branding and distribution requirements.

3. **Rebuild native directories:**

```bash
yarn build:native
```

This will regenerate the `android` and `ios` directories with the new configuration.

---

## 🌍 Localization (i18n)

This project uses `i18next-resources-for-ts` to statically type your translation resources.

Run this manually:

```bash
yarn merge-i18n-resources
yarn type-i18n-resources
```

These are also triggered automatically by:

```bash
yarn prepare
```

---

## 🔐 Environment Management

Environment variables are stored in the **root** of the project (not in `src`) and should be placed in the following files:

```
.env.development
.env.production
.env.test
```

All files must declare **the same variable names** (with environment-specific values).  
Variables **must** be prefixed with `EXPO_PUBLIC_` to be accessible in the app.

Example:

```env
EXPO_PUBLIC_ENVIRONMENT=production
EXPO_PUBLIC_SHOW_TEST=true
```

---

### 🛠️ Generate Typed Env Map

To generate a typed environment config (`src/env.ts`) usable in your app, run:

```bash
yarn generate-env-ts
```

This script reads from `.env.production` (make sure it exists before running), and outputs an `Env` object where:

- `EXPO_PUBLIC_` is stripped from the keys
- Values are typed as:
  - `'development' | 'production' | 'test'` → if the variable is `EXPO_PUBLIC_ENVIRONMENT`
  - `boolean` → if the value is `true` or `false`
  - `string` → otherwise

### 🧪 Example Output

```ts
export const Env = {
  ENVIRONMENT: process.env.EXPO_PUBLIC_ENVIRONMENT as
    | "development"
    | "production"
    | "test",
  SHOW_TEST: JSON.parse(process.env.EXPO_PUBLIC_SHOW_TEST as string) as boolean,
  BASE_URL: process.env.EXPO_PUBLIC_BASE_URL as string
};
```

This is also triggered automatically by:

```bash
yarn prepare
```

---

## 🧰 Tooling

This project uses [`mise`](https://mise.jdx.dev/) for managing all runtime environments.

- **All tooling versions** are defined in `.mise.toml`, including:
  - **Java**: `zulu-17`
  - **Ruby**: `2.7.6` with `bundler` installed post-install
  - **Node**: `22.2.0` with `corepack` enabled
- **Environment variables** (e.g., `NODE_ENV`, `USE_FRAMEWORKS`) are also managed via `.mise.toml` under the `[env]` section.
- **Xcode-specific settings** are configured in `.xcode.env.local` to support local development workflows.

---

## 📦 Build & Deploy

- **Android Release:** `yarn android:release`
- **iOS Pods Install:** `yarn ios:pods`

---

## 📚 Learn More

- [Expo Docs](https://docs.expo.dev)
- [expo-router Docs](https://expo.github.io/router/)
- [i18next](https://www.i18next.com/)
