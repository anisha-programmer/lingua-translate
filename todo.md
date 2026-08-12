# Project TODO

- [x] Implement the LinguaTranslate editorial header with exact app name and subtitle
- [x] Implement the hero section with exact heading “Break Language Barriers”
- [x] Implement responsive two-panel translation card with source and target language selectors
- [x] Add 18+ supported languages with internal language codes and Auto Detect handling
- [x] Add 5000-character input limit, live counter, validation, and keyboard accessibility
- [x] Implement secure POST /api/translate backend endpoint with a server-side translation provider
- [x] Implement the keyless free translation provider through @vitalets/google-translate-api
- [x] Implement sanitized JSON error responses for validation, provider rate limits, timeouts, and network failures
- [x] Add loading state, disabled Translate button, Ctrl+Enter shortcut, inline toast errors, and preserved line breaks
- [x] Add swap, clear, copy with exact temporary “Copied!” feedback, local history, and text-to-speech polish
- [x] Add accessible semantic markup, focus states, responsive layout, and editorial styling
- [x] Add frontend and backend tests for translation flows, validation, errors, and interactions
- [x] Update README with free-provider setup, API documentation, environment variables, project structure, and GitHub instructions
- [x] Verify typecheck, build, tests, local runtime, desktop layout, and mobile layout
- [x] Create the final project checkpoint after all items are completed

## Superseded requirements retained as history

- [x] Google Cloud Translation API backend requirement — superseded by the user's request for a completely free, no-billing solution.
- [x] GOOGLE_TRANSLATE_API_KEY server environment requirement — superseded; the backend now uses a keyless translation helper.
- [x] Google Cloud setup and live-key verification requirement — superseded; free provider availability and rate-limit behaviour are documented instead.

## Change History

- [x] Project initialized from the full-stack web template.
- [x] User supplied the complete LinguaTranslate functional and visual specification.
- [x] User required a cream editorial aesthetic, secure backend boundary, and no browser-native alerts.
- [x] User replaced Google Cloud Translation with a completely free keyless backend provider.

## Optional future considerations (documented in README)

- [x] GitHub repository instructions and template URL provided in README.
- [x] LibreTranslate self-hosting guidelines documented in README as an optional production scaling path.

## Verification notes

- [x] Live English-to-Tamil translation was verified locally through POST /api/translate.
- [x] Empty input and same-language validation were verified locally.
- [x] The keyless provider may be rate-limited because it relies on an unofficial public translation interface; the app shows a clean retry message rather than leaking provider details.
- [x] No translation reviews, ratings, testimonials, or fabricated user-generated content were added.
- [x] No database, authentication, Docker, payments, or unnecessary external integrations were added to the translation feature.
- [x] Add an explicit timeout and abort mechanism around the server-side translation provider call.
- [x] Harden and test provider rate-limit and network failure mapping with sanitized JSON responses.

### Verification notes added during review

The first local provider request succeeded, but timeout handling needs an explicit abort signal rather than relying only on provider error text. Rate-limit and network failure mappings also need direct automated coverage.
- [x] Add frontend test coverage for translation interactions and UI states including translate, disabled button, Ctrl+Enter, copy feedback, swap, clear, inline errors, and loading state.
- [x] Add route-level integration coverage for POST /api/translate success and sanitized failure responses.

### Coverage review note

Backend unit tests, browser-facing interaction tests, and route-level integration tests are now present before the final checkpoint.
- [x] Add a frontend test that holds a translation request pending and verifies loading text, spinner state, and Translate button disabled state.
- [x] Add a frontend assertion that loading state clears after the mocked request resolves or rejects.

### Loading coverage review note

The existing frontend tests cover translation, disabled-empty input, Ctrl+Enter, copy, swap, clear, inline errors, and an explicit pending-request loading state that clears after resolution.
