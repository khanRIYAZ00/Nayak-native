# Nayak Native

Nayak Native is a mobile chat application built with Expo and React Native for a legal assistant experience. It lets users ask legal questions, maintain chat sessions, authenticate as guests, and interact with a backend API that handles session management and conversation responses.

This project is the client-facing application for the Nayak ecosystem and provides a polished conversational UI with persistent session state, dark/light theming, and a sidebar-based history interface.

## Overview

The app is designed to provide a fast and intuitive legal Q&A experience on mobile and web. It includes:

- A modern chat interface for user and assistant messages
- Session-aware conversation flow
- Guest authentication and token persistence
- History loading from an external backend
- Dark and light theme switching
- Sidebar navigation for new chats and previous conversations
- Expo Router-based screen navigation

## Key Features

- Conversational legal assistant UI
- Real backend integration through a REST API layer
- Session creation and management via secure storage
- Persistent authentication using Expo Secure Store
- Multi-platform support through Expo (Android, iOS, web)
- Type-safe app structure with TypeScript support
- Lightweight, responsive, native-style design

## Tech Stack

- React Native
- Expo SDK
- Expo Router
- TypeScript
- SecureStore for local session and auth persistence
- React Native Safe Area Context
- Ionicons for UI icons

## Project Structure

```text
Nayak-native/
├── .claude/                 # Project-specific Claude configuration
├── .vscode/                 # VS Code editor settings
├── assets/                  # App icons, splash assets, images
├── scripts/
│   └── reset-project.js     # Expo project reset helper
├── src/
│   ├── app/
│   │   ├── _layout.tsx      # App root layout
│   │   ├── chat.tsx         # Main chat screen
│   │   └── index.tsx        # App entry redirect
│   ├── components/
│   │   ├── ChatView.tsx     # Message rendering and chat list UI
│   │   ├── InputBar.tsx     # Compose and send message field
│   │   └── Sidebar.tsx      # Drawer/history navigation UI
│   ├── constants/
│   │   └── theme.ts         # Shared theme config
│   ├── hooks/
│   │   ├── use-color-scheme.ts
│   │   ├── use-color-scheme.web.ts
│   │   └── use-theme.ts
│   ├── lib/
│   │   ├── api.js           # Backend API client
│   │   └── storage.js      # Secure local storage wrappers
│   ├── theme/
│   │   └── colors.js
│   ├── global.css
│   └── ...
├── .gitignore
├── AGENTS.md
├── CLAUDE.md
├── LICENSE
├── README.md
├── app.json
├── package.json
├── package-lock.json
├── tsconfig.json
└── ...
```

## Core Application Flow

The app starts from the index route and immediately redirects to `/chat` after the splash screen is dismissed.

From there, the chat screen does the following:

1. Reads stored user/session data from secure storage
2. Validates or creates an auth token
3. Reuses a saved session ID or creates a new backend session
4. Loads the chat history for that session
5. Accepts new user input and sends it to the backend
6. Displays assistant responses in the chat list

## Authentication and Session Logic

The app uses Expo Secure Store to manage:

- `auth_token`
- `nayak_session_id`
- `nayak_user`

If there is no valid token, the app triggers a guest-login flow. That login returns an authentication token and user metadata. The app then creates or reuses a chat session for that user and keeps it persisted locally.

## Backend API Integration

The app communicates with a backend exposed by the `src/lib/api.js` client. The current configuration points to:

```javascript
const API_BASE_URL = 'http://192.168.1.48:8000'
```

This is a local network URL, so it is intended for development on the same network as the backend server. If your backend runs on another machine or port, update this value before running the app.

### Available API Calls

The client includes the following endpoints:

- `POST /api/auth/login`
- `POST /api/auth/register`
- `POST /api/auth/guest-login`
- `GET /api/auth/verify?token=...`
- `POST /api/session`
- `POST /api/new-chat`
- `GET /api/history?session_id=...`
- `POST /api/command`

The app sends structured JSON payloads for chat messages and keeps the session ID attached to each request.

## Screens and UI Components

### Chat Screen

The main app screen is located at `src/app/chat.tsx` and handles:

- App initialization
- Authentication and session creation
- Message sending and response handling
- Error banners and loading states
- Sidebar and profile overlays
- Theme toggling

### Chat View

`src/components/ChatView.tsx` renders user and assistant messages with a styled bubble layout and lightweight markdown-style formatting for assistant text.

### Input Bar

`src/components/InputBar.tsx` provides the message composer with a send button, keyboard handling, and disabled/loading states.

### Sidebar

`src/components/Sidebar.tsx` implements the side drawer that allows users to:

- Start a new chat
- View prior conversation entries
- Select an old conversation
- Open a profile section
- Toggle dark mode
- Clear the session

## Getting Started

### Prerequisites

Before you begin, ensure the following are installed:

- Node.js (recommended LTS)
- npm
- Expo CLI (optional but useful)
- An Android emulator, iOS simulator, or a physical device
- A running Nayak backend service for real API responses

### Install dependencies

```bash
npm install
```

### Start the app

```bash
npx expo start
```

### Run on specific platforms

```bash
npx expo start --android
npx expo start --ios
npx expo start --web
```

## Development Notes

### Expo Router

This project uses `expo-router` with file-based routing. The app root is under `src/app` and navigation is handled through the Expo router entry point configured in `package.json`:

```json
"main": "expo-router/entry"
```

### Styling

The project combines native React Native styles and a dedicated theme structure. Color themes are defined directly within `src/app/chat.tsx`, enabling both dark and light visual modes.

### Secure Storage

Sensitive client data is stored using `expo-secure-store`, which is appropriate for auth tokens and session state in mobile environments.

## Configuration

### App metadata

The application metadata is defined in `app.json` and includes:

- app name: `nayak-native`
- app slug: `nayak-native`
- scheme: `nayaknative`
- splash configuration
- adaptive icons for Android
- web favicon configuration

### Environment-specific backend URL

To connect to a different backend endpoint, edit the `API_BASE_URL` constant in `src/lib/api.js`:

```javascript
const API_BASE_URL = 'http://YOUR_BACKEND_IP:PORT'
```

If you are developing with a simulator or emulator, make sure the backend is accessible from that host and that your network access rules permit communication.

## Troubleshooting

### Backend not reachable

If the app shows an error such as “Could not reach the Nayak backend,” verify:

- the backend is running
- the backend port is correct
- the device or emulator can access the host machine or LAN IP
- the URL in `src/lib/api.js` matches your backend environment

### Session issues

If chat history or session actions fail:

- remove saved local storage data from app state or device
- restart the app
- confirm the backend session endpoints are working

### Web or mobile build issues

If you run into Expo-specific problems:

```bash
npx expo install
npx expo start --clear
```

Also ensure that your local Node version is compatible with the Expo version used by the project.

## Scripts

The available scripts in `package.json` are:

```bash
npm start
npm run android
npm run ios
npm run web
npm run reset-project
npm run lint
```

## License

This project is licensed under the MIT License. See the `LICENSE` file for full details.

## Summary

Nayak Native is a mobile-first legal assistant client designed to provide a clean and responsive AI-powered experience. It combines Expo, React Native, secure mobile storage, and a REST backend to deliver a session-aware conversation app that feels native and modern.

For local development, ensure your backend is running and the API URL in `src/lib/api.js` matches your environment. Once configured, the app is ready to start conversations, persist sessions, and provide a legal assistant experience on mobile and web.
