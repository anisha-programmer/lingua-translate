# Linguage Translate

## Break Language Barriers 🌍

LinguaTranslate is a full-stack translation workspace designed to help users translate text across multiple languages through a clean and responsive interface.

The application uses a React frontend and a Node.js/Express backend. Translation requests are processed through the server, keeping the external translation service away from the client-side application.

## 🛠️ Tech Stack

| Technology                      | Purpose                                                                       |
| ------------------------------- | ----------------------------------------------------------------------------- |
| **React 19**                    | Builds the interactive translation interface                                  |
| **Vite**                        | Provides fast frontend development and build tooling                          |
| **TypeScript**                  | Adds type safety and structured development                                   |
| **Tailwind CSS 4**              | Creates the responsive and modern UI design                                   |
| **Lucide React**                | Provides interface icons                                                      |
| **Node.js**                     | Runs the backend JavaScript environment                                       |
| **Express.js**                  | Creates the backend server and translation API                                |
| **TypeScript (Backend)**        | Provides type-safe backend development                                        |
| **Google Translate API Helper** | Performs the actual text translation through `@vitalets/google-translate-api` |
| **localStorage**                | Stores recent translation history locally in the user's browser               |
| **Vitest**                      | Used for automated testing and validation                                     |

## ⚙️ How It Works

LinguaTranslate follows a simple **Frontend → Backend → Translation Service → Frontend** workflow.

```text
User enters text
       ↓
Select Source Language
       ↓
Select Target Language
       ↓
React Frontend
       ↓
POST /api/translate
       ↓
Express Backend
       ↓
Request Validation
       ↓
Translation Service
       ↓
Google Translate Helper
       ↓
Translated Text
       ↓
React displays result
```

### 1. User Input

The user enters text into the translation workspace and selects the source and target languages.

The application supports **18 languages** and also provides an **Auto Detect** option for the source language.

### 2. Frontend Processing

The React application collects:

* Text
* Source language
* Target language

It then sends these details to the backend through the `/api/translate` endpoint.

The browser does **not** directly call the external translation service.

### 3. Backend Validation

The Express server receives the request and validates the input.

It checks:

* Text availability
* Supported source language
* Supported target language
* 5000-character input limit

Invalid requests are rejected with a structured JSON error response.

### 4. Translation

After validation, the backend sends the request to the translation service using `@vitalets/google-translate-api`.

The translation service processes the text and returns the translated result.

### 5. Result

The backend sends the translated text back to the React frontend.

The interface then displays the translated result while preserving the original line breaks.

### 6. Translation History

Recent translations are stored using the browser's **localStorage**.

This allows the user to keep a local history without requiring a database or user account.


## 🎯 Project Purpose

LinguaTranslate demonstrates how **React, TypeScript, Node.js, Express, and translation services** can be combined to create a practical full-stack language translation application.

The project focuses on a clean user experience while keeping translation-service communication on the server side.
