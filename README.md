# LinguaTranslate

LinguaTranslate is a polished, full-stack translation workspace built for clear writing across languages. It pairs a quiet editorial interface with a server-side translation boundary, so the browser never calls an external translation service directly.

The app currently uses [`@vitalets/google-translate-api`](https://www.npmjs.com/package/@vitalets/google-translate-api), a keyless Node.js helper that accesses the public Google Translate web interface. This means the project does not require Google Cloud billing, a Google Cloud project, or a translation API key. The helper is intended for pet projects and prototypes, and public translation services can rate-limit requests. If you need production reliability or full control, run a self-hosted [LibreTranslate](https://github.com/LibreTranslate/LibreTranslate) instance and replace the provider module without changing the frontend contract.

## Features

The interface includes the exact brand name **LinguaTranslate**, the hero heading **Break Language Barriers**, an accessible two-panel translation desk, 18 supported language choices, Auto Detect source language, a 5000-character input limit, Ctrl+Enter translation, loading states, inline error and success messages, copy feedback using the exact label **Copied!**, intelligent language swapping, clear controls, preserved line breaks, local translation history, and browser text-to-speech playback.

The backend validates every request, keeps provider calls on the server, returns sanitized JSON errors, short-circuits same-language requests, and aborts upstream requests after a 15-second timeout. No translation API key is referenced by the client bundle.

## Technology

| Layer | Technology |
| --- | --- |
| Frontend | React 19, Vite, TypeScript, Tailwind CSS 4, Lucide React |
| Backend | Node.js, Express, TypeScript |
| Translation | `@vitalets/google-translate-api` with no billing or API key |
| Local persistence | Browser `localStorage` for recent translation history only |
| Validation | Server-side request validation and Vitest coverage |

## Project structure

```text
lingua-translate/
├── client/
│   ├── index.html
│   └── src/
│       ├── components/
│       │   └── LanguageSelect.tsx
│       ├── pages/
│       │   └── Home.tsx
│       ├── App.tsx
│       └── index.css
├── server/
│   ├── controllers/
│   │   └── translationController.ts
│   ├── routes/
│   │   └── translationRoutes.ts
│   ├── services/
│   │   └── translationService.ts
│   └── _core/
│       └── index.ts
├── shared/
│   └── languages.ts
├── server/translation.test.ts
├── todo.md
├── package.json
└── README.md
```

The initialized project also contains the platform template's authentication and database scaffolding, but the LinguaTranslate translation flow is public and does not require authentication, database tables, or user records.

## Getting started

Install Node.js 20 or newer, open the project folder in VS Code, and install dependencies from the project root.

```bash
pnpm install
```

Start the frontend and backend together through the managed development server.

```bash
pnpm dev
```

Open the local URL printed by the development server. The Vite frontend and Express backend are served through the same process, and the frontend calls `/api/translate` without requiring a client-side environment file.

To run the automated checks:

```bash
pnpm check
pnpm test
pnpm build
```

## Translation API

### `POST /api/translate`

Request body:

```json
{
  "text": "Hello, how are you?",
  "sourceLanguage": "en",
  "targetLanguage": "ta"
}
```

Successful response:

```json
{
  "translatedText": "வணக்கம், எப்படி இருக்கிறீர்கள்?",
  "detectedLanguage": "en"
}
```

The `sourceLanguage` value may be `auto` or a supported language code. The `targetLanguage` value must be a supported language code other than `auto`.

Supported codes include `en`, `ta`, `hi`, `te`, `ml`, `kn`, `bn`, `mr`, `fr`, `es`, `de`, `it`, `pt`, `ja`, `ko`, `zh-CN`, `ar`, and `ru`.

Validation or provider failures use a sanitized JSON response such as:

```json
{
  "error": "The free translation service is temporarily rate-limited. Please try again shortly."
}
```

The endpoint returns `400` for invalid input, `429` for detected rate limits, `502` for an invalid upstream response, `503` for network unavailability, and `504` when the upstream request exceeds the timeout. Provider credentials, raw upstream messages, and stack traces are never returned to the browser.

## How the request flows

1. The user enters a passage and selects source and target languages in the React interface.
2. React sends only the passage and language codes to `POST /api/translate` on the same origin.
3. Express validates the request, enforces the 5000-character limit, and calls the translation helper on the server.
4. The server returns only the translated text and optional detected language to the browser.
5. React renders the result with `white-space: pre-wrap`, so line breaks are preserved.

The translation helper is not imported from any React component, Vite entry point, HTML file, or client-side environment file.

## Environment variables

No translation key or billing configuration is required. The project does not need a client `.env` file for translation. The platform scaffold may provide unrelated runtime variables for its hosting and development services; those values are not used as translation credentials.

If you later swap in a self-hosted LibreTranslate server, keep its base URL in a server-only environment variable and update `server/services/translationService.ts`. Do not expose provider URLs, keys, or credentials through `VITE_*` variables.

## Free-provider considerations

This implementation avoids Google Cloud billing and does not require an API key. It relies on an unofficial public translation interface, so it is appropriate for a portfolio project, local experimentation, and low-volume personal use. Public services may change, rate-limit, or become unavailable. The UI reports those cases as inline messages instead of showing browser alerts.

For a fully open-source deployment, install LibreTranslate locally or on infrastructure you control, then send the same `{ q, source, target }` data from the server provider module. LibreTranslate's public hosted service may require a key or have usage limits, so self-hosting is the no-billing option with the most control.

## GitHub

Create a repository, commit the project, and push it with the following commands:

```bash
git init
git add .
git commit -m "Build LinguaTranslate translation workspace"
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/lingua-translate.git
git push -u origin main
```

Do not commit `.env` files, local secrets, or machine-specific credentials. The existing `.gitignore` is retained from the project scaffold and should be reviewed before the first push.

## Current manual configuration

There is no mandatory translation configuration. The optional remaining step is replacing the placeholder GitHub URL in the footer or repository instructions if you have a project repository. If the free upstream service is rate-limited, retry later or swap the provider module for a self-hosted LibreTranslate endpoint.
