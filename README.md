# expo-typed-routes-outside-app-windows

On Windows, a file added outside the app directory while `npx expo start` runs becomes a typed route. This repository is `npx create-expo-app@latest` (the default template: Expo 58.0.21, `expo-router` 58.0.28, `@expo/router-server` 58.0.19, typed routes on, the app in `src/app`), with `npx expo install --fix` run once.

## Steps

Windows, npm, the development server only.

1. In PowerShell, clone and install:
   ```powershell
   git clone https://github.com/YevheniiKotyrlo/expo-typed-routes-outside-app-windows
   cd expo-typed-routes-outside-app-windows
   npm install
   ```
2. Start the dev server and wait for `Waiting on http://localhost:8081`:
   ```powershell
   npx expo start
   ```
3. In a second PowerShell window, in the same folder, add a component outside `src/app`:
   ```powershell
   Set-Content src/components/extra-card.tsx 'export function ExtraCard() { return null; }'
   ```
4. Read the routes in `.expo/types/router.d.ts`:
   ```powershell
   Select-String -Path .expo/types/router.d.ts -Pattern 'pathname: `[^`]*`' -AllMatches | ForEach-Object { $_.Matches.Value } | Sort-Object -Unique
   ```

Expected: `src/components/extra-card.tsx` is outside the app directory, so the routes stay `/`, `/_sitemap` and `/explore`.

What I see on Windows 11 with Node 26.12.1: the routes gain `/../components/extra-card`.
