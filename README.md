# Smart Wallet

Smart Wallet is a TypeScript monorepo for a digital wallet platform that combines a browser extension, a backend service layer, and shared wallet/security packages. The project is organized as a Yarn workspace and is designed to support wallet operations, key management, fraud/risk evaluation, and extensibility across client and server components.

## Overview

The codebase is structured around a single product vision: provide users with a secure, extensible wallet experience across web-connected surfaces. The project currently contains:

- Chrome extension client for wallet interaction in the browser
- Backend API layer for service orchestration
- Shared TypeScript packages for wallet logic, key management, validation, and schema types
- A mobile app scaffold for future cross-platform expansion
- Cloud and infrastructure directories for deployment support

The repository name is `smart-wallet`, and the extension app is marketed as `Sarwallet` in its manifest configuration.

## Project goals

The project aims to provide a foundation for:

- Secure wallet management
- Browser-based wallet access through a Chrome extension
- Key lifecycle management and support for web authentication primitives
- Risk scoring and policy checks for transactions or wallet actions
- Shared contracts and reusable types across services and clients
- Infrastructure readiness for deployment and scaling

## Architecture summary

The monorepo follows a modular workspace design. The root package config uses Yarn workspaces to allow each subproject to share dependencies while remaining logically separated.

### Root configuration

The root `package.json` declares:

- A private monorepo setup
- Shared workspace resolution for `apps/*`, `packages/*`, and `backend`
- A root build command that runs builds for all workspace packages
- A root dev script that starts the backend development server

This gives the project a clean developer workflow for working across frontend, backend, and shared libraries without having to manually wire each package together.

## Folder structure

### `smart-wallet/apps/extension`

This is the browser extension implementation.

- `public/manifest.json` declares a Chrome Manifest V3 extension
- `public/popup/index.html` serves the popup UI entry point
- `src/background/background.ts` contains the extension background script boilerplate
- `src/content/contentScript.ts` is a content script entry point triggered on webpages
- `src/popup/Popup.tsx` renders the popup UI for the wallet
- `src/popup/index.tsx` mounts the React popup into the document root

The extension has permissions for:

- storage
- activeTab
- scripting

It is configured to inject a content script on all URLs and open a popup UI when the user clicks the extension action.

### `smart-wallet/backend`

This is the backend service package.

- It is built with Express and TypeScript
- It uses `ts-node-dev` for local development
- It includes `axios`, `dotenv`, `uuid`, and TypeScript support packages

The service is planned to serve as the API layer for wallet workflows, backend verification, or orchestration between client apps and infrastructure services.

### `smart-wallet/packages/core-wallet`

This package is intended to contain wallet business logic and core blockchain or wallet-related operations.

It depends on:

- `@circle-fin/w3s-pw-web-sdk`
- `axios`
- `uuid`

This suggests the project is designed to support broader wallet functionality such as account abstraction, wallet orchestration, or blockchain SDK integrations.

### `smart-wallet/packages/key-manager`

This package is focused on signing, authentication, and key lifecycle management.

It includes dependencies for:

- WebAuthn browser integration
- Server-side WebAuthn support
- Crypto utilities

This indicates the project is designed with secure authentication and credential handling in mind, which is crucial for modern digital wallets.

### `smart-wallet/packages/risk-engine`

This package is dedicated to fraud and risk evaluation logic.

It uses Zod for validation and schema modeling, which is a strong sign that the system wants runtime validation and structured policy checks around transactional or behavioral risks.

The risk engine is likely intended to evaluate events or decisions before wallet actions are executed.

### `smart-wallet/packages/shared-types`

This is the shared schema and type package for common contracts used by multiple parts of the system.

It also uses Zod, making it a natural place to centralize validation schemas for payloads, wallet structures, API contracts, and domain models.

### `smart-wallet/apps/mobile`

The mobile app folder exists as a workspace target, but it is not yet richly implemented. It is clearly intended as a future client surface alongside the browser extension.

## Technology stack

The project uses a modern TypeScript-first stack:

- TypeScript
- React for browser popup UI
- Vite for extension build tooling
- Express for backend APIs
- Yarn workspaces for monorepo orchestration
- Zod for validation
- WebAuthn libraries for authentication flows
- Chrome extension APIs for browser integration

## Security and trust model

The architecture reflects a wallet system with security-conscious design.

Notable aspects include:

- Key management in a dedicated module
- WebAuthn dependencies for strong authentication
- Risk-engine package for policy checks and validation
- Shared type contracts to reduce interface drift between packages and services
- Manifest V3 browser extension security model

This is a strong foundation for building secure client-side wallet experiences with explicit validation layers.

## Development workflow

The root workspace is intended to support a simple workflow:

1. Install workspace dependencies with Yarn.
2. Build packages and applications with the root build script.
3. Run the backend with the root dev command or direct package scripts.
4. Develop UI, risk logic, and wallet logic in their relevant packages.

The current configured commands are:

- Root build: `yarn workspaces run build`
- Root dev: `yarn workspace backend dev`
- Extension build: `yarn workspace extension build`
- Backend build: `yarn workspace backend build`

## Infrastructure and deployment

The repository includes:

- `infra/docker`
- `infra/github-workflows`
- `infra/k8s`
- `infra/terraform`

These directories indicate the project is intended to be deployable in modern cloud environments, with containerization, CI/CD workflows, Kubernetes manifests, and infrastructure-as-code support.

## Current implementation status

This is an early-stage monorepo foundation rather than a completed production wallet product. The codebase already establishes a clear architectural direction and package boundaries, but many business flows are still scaffolded or intentionally minimal.

The extension popup currently exposes a basic placeholder UI:

- title: "Smart Wallet"
- message: "Welcome to your wallet extension popup!"

The backend and packages are similarly structured as foundation modules rather than fully implemented product features.

## Expected product direction

Based on the code structure, the project is moving toward a digital wallet platform with a browser extension front end and a supporting backend. Likely future capabilities include:

- wallet balance and activity tracking
- transaction signing and approval flows
- secure key storage and recovery flows
- fraud detection and risk-based checks
- API integrations for external wallet services or blockchain networks
- a mobile client that mirrors the extension experience

## Conclusion

Smart Wallet is a modular, TypeScript-driven wallet platform monorepo designed around security, governance, and extensibility. It has a clear separation of concerns across browser extension, backend orchestration, risk evaluation, key management, and shared typing. The project is in a strong architectural starting state and is ready for further feature implementation and product development.

This repository captures the core foundation of a modern wallet ecosystem, with enough structure to evolve into a real multi-surface digital wallet product.
