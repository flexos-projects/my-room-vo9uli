---
id: 007-technical
title: Technical Architecture
description: The technology stack, key packages, and architectural decisions for the My Room project.
type: doc
subtype: core
status: draft
sequence: 7
tags:
  - tech-stack
  - architecture
  - frontend
  - backend
createdAt: "2023-10-27T10:00:00Z"
updatedAt: "2023-10-27T10:00:00Z"
---

# Technical Architecture Specification

This document outlines the technical stack and architectural plan for building the My Room application. The choices are guided by the need for rapid development, scalability, a rich interactive frontend, and a serverless backend.

## 1. Core Technology Stack

*   **Framework:** **Nuxt 4**
    *   **Rationale:** Nuxt provides a powerful, opinionated framework on top of Vue 3. Its file-based routing, auto-imports, and module ecosystem will significantly accelerate development. Server-side rendering (SSR) capabilities can be leveraged for the public-facing **Landing Page** for better SEO, while the core application can run as a Single Page Application (SPA) for a faster, app-like experience.

*   **Backend & Database:** **Firebase**
    *   **Rationale:** Firebase offers a comprehensive suite of tools that perfectly match our needs, allowing us to build a robust application without managing server infrastructure.
    *   **Firestore (Database):** A NoSQL, document-based database that is highly scalable and offers real-time data synchronization capabilities. Its flexible data model is ideal for storing user-generated content like room layouts. The schema is detailed in the [Database Schema](./004-database.md) document.
    *   **Firebase Authentication (Auth):** Provides a secure, easy-to-implement authentication system with built-in support for email/password and social providers like Google. It handles all the complexity of user session management.
    *   **Firebase Hosting (Hosting):** Offers a fast, secure, and reliable global CDN for our static assets and Nuxt application. It integrates seamlessly with the rest of the Firebase ecosystem.
    *   **Firebase Storage (File Storage):** Will be used to store user-uploaded assets (in future versions) and application assets like 3D models (`.gltf`) and furniture icons (`.svg`).

## 2. Frontend Architecture & Key Packages

*   **State Management:** **Pinia**
    *   **Rationale:** Pinia is the official, lightweight state management library for Vue. Its simple, intuitive API is perfect for managing global state, such as the current user's authentication status, application settings, and the state of the active design project on the **Room Designer Canvas**.

*   **2D/3D Canvas Rendering:**
    *   **Three.js:** The definitive library for 3D graphics in the browser. It will be used to render the 3D visualization of the room, including walls, furniture models, lighting, and camera controls (orbital/first-person).
    *   **Konva.js:** A 2D HTML5 canvas library. It will be used for the 2D top-down floor plan view. Konva excels at handling complex shapes, layers, and user interactions like dragging, dropping, and resizing, which is perfect for our canvas needs.

*   **Routing:** **Vue Router** (via Nuxt)
    *   **Rationale:** Nuxt's file-based routing system, built on top of Vue Router, will handle all page navigation. Dynamic routes like `/designer/:roomId` will be used to load specific room projects.

*   **Utility Libraries:**
    *   **VueUse:** A comprehensive collection of Vue Composition API utilities. It will be used for tasks like tracking window size, handling local storage, and simplifying event listeners.
    *   **@nuxtjs/color-mode:** For implementing light/dark mode themes in the future, building on the design system.

## 3. API Integrations & Data Flow

*   **Primary API:** The entire application will communicate directly with the Firebase suite of services via the Firebase Web SDK.
    *   **Data Flow:** When a user logs in, the frontend subscribes to their `rooms` in Firestore. When they edit a design on the canvas, the local state (managed by Pinia) is updated in real-time. On 'Save', this state is serialized into the `layoutData` object and written back to the corresponding Firestore document.
    *   **Security:** Firestore Security Rules will be the primary mechanism for protecting user data. Rules will be written to ensure that a user can only read and write documents where the `userId` field matches their own authenticated UID.

*   **AI Layout Assistant API:**
    *   **Initial Implementation (MVP):** The AI assistant will start as a client-side, rules-based engine. The logic will be written in TypeScript and will execute directly in the user's browser. It will analyze the room's aspect ratio and apply a set of predefined interior design heuristics (e.g., clearance rules, zoning principles).
    *   **Future Enhancement:** For more advanced suggestions, we may integrate a third-party AI service or a custom model hosted on a serverless function (e.g., Firebase Cloud Functions). In this scenario, the client would send the room dimensions and furniture list to the function via an HTTPS request, and the function would return a set of layout suggestions.

## 4. Deployment & DevOps

*   **Hosting:** The Nuxt application will be deployed to Firebase Hosting.
*   **CI/CD:** A continuous integration and deployment pipeline will be set up using GitHub Actions. On every push to the `main` branch, the workflow will automatically build the Nuxt project and deploy it to Firebase Hosting, ensuring a streamlined and reliable release process.
*   **Environment Variables:** Sensitive keys (like Firebase API keys) will be managed using environment variables (`.env` files) and configured within the Nuxt build process and GitHub Actions secrets.