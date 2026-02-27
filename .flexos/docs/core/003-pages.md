---
id: 003-pages
title: Page & Component Architecture
description: An overview of the application's pages, their routes, and key components.
type: doc
subtype: core
status: draft
sequence: 3
tags:
  - pages
  - architecture
  - frontend
createdAt: "2023-10-27T10:00:00Z"
updatedAt: "2023-10-27T10:00:00Z"
---

# Page & Component Architecture

This document describes the primary pages of the My Room application, their routes, and their high-level component structure. This serves as a blueprint for the frontend development using Nuxt 4.

## 1. Landing Page (`/`)

*   **Route:** `/`
*   **Auth:** Public (accessible to all visitors).
*   **Description:** This is the marketing and entry point for the application. Its goal is to clearly communicate the value proposition—especially for users with challenging spaces—and convert visitors into registered users.
*   **Component Breakdown:**
    *   **`TheHeader`**: Contains the 'My Room' logo, navigation links ('Features', 'Inspiration'), and 'Login'/'Sign Up' CTAs.
    *   **`HeroSection`**: Above-the-fold content featuring the tagline: "Design your dream bedroom, effortlessly visualize, and optimize every inch." It will include a compelling visual, perhaps a short animation of a narrow room being transformed, and a primary CTA: "Start Designing for Free".
    *   **`FeatureHighlights`**: A section with 3-4 cards, each dedicated to a core feature like the **Interactive Room Canvas**, **AI-Powered Layout Assistant**, and **Customizable Furniture Library**. Each card will have an icon, a short title, and a brief description.
    *   **`HowItWorks`**: A simple 3-step visual guide: 1. Input Dimensions, 2. Drag, Drop & Design, 3. Get AI Suggestions.
    *   **`TestimonialGallery`**: A visual grid showcasing successful designs of narrow or small bedrooms created with the tool. This provides social proof and inspiration.
    *   **`TheFooter`**: Standard footer with links to social media, terms of service, and contact information.

## 2. Authentication Page (`/auth`)

*   **Route:** `/auth`
*   **Auth:** Public (for non-authenticated users).
*   **Description:** A clean, focused page for user signup, login, and password recovery. The design will be minimal to reduce friction.
*   **Component Breakdown:**
    *   **`AuthForm`**: A single, stateful component that can toggle between three modes: Login, Sign Up, and Forgot Password.
        *   **Login View:** Email and password fields, a 'Login' button, and a 'Sign in with Google' option.
        *   **Sign Up View:** Name, email, and password fields, a 'Create Account' button, and the Google option.
        *   **Forgot Password View:** A single email field to send the reset link.

## 3. Onboarding Flow (`/onboarding`)

*   **Route:** `/onboarding`
*   **Auth:** Private (redirected here after first signup).
*   **Description:** A modal-based or multi-step page flow that guides new users through the initial setup.
*   **Component Breakdown:**
    *   **`OnboardingWizard`**: A component that manages the state of the multi-step process.
        *   **Step 1: Welcome**: A friendly welcome message.
        *   **Step 2: RoomDimensionsInput**: A form prompting the user for their first room's length and width, with metric/imperial options. It will be pre-filled with the example 2.65m x 6m to guide them.
        *   **Step 3: CanvasTutorial**: A brief, interactive overlay pointing out key UI elements on a simplified version of the designer.
        *   **Step 4: ProjectNameInput**: A final step to name their project before being redirected to the full designer.

## 4. My Projects Dashboard (`/projects`)

*   **Route:** `/projects`
*   **Auth:** Private.
*   **Description:** The user's home base after logging in. It displays all their saved designs and allows for project management.
*   **Component Breakdown:**
    *   **`ProjectGrid`**: The main component that fetches and displays the user's `rooms` from Firestore.
    *   **`ProjectCard`**: A reusable component for each project. It displays a thumbnail (`thumbnail_url`), the project name, and the last updated date. On hover, it reveals an options menu (kebab icon) with 'Edit', 'Duplicate', and 'Delete' actions.
    *   **`CreateProjectButton`**: A prominent button that initiates the flow for creating a new room design.

## 5. Room Designer Canvas (`/designer/:roomId`)

*   **Route:** `/designer/:roomId`
*   **Auth:** Private.
*   **Description:** The core interactive workspace of the application. This is the most complex page, orchestrating multiple features.
*   **Component Breakdown:**
    *   **`DesignerLayout`**: A main layout component that structures the page into key areas.
        *   **`TopBar`**: Displays the editable project name. Contains 'Save', 'Undo', 'Redo', and 'Share' buttons.
        *   **`LeftSidebar`**: Houses the **`FurnitureLibrary`** component, with its search, filter, and category views.
        *   **`MainCanvas`**: The central area containing the Three.js/Konva canvas for the **`room-canvas`** feature.
        *   **`RightSidebar`**: A context-aware inspector panel. It's hidden by default and appears when a user selects a furniture item on the canvas, showing its properties (dimensions, color) for editing.
    *   **`AIAssistantButton`**: A floating action button that, when clicked, opens the **`AISuggestionsModal`** to display layout options.

## 6. Inspiration Feed (`/inspiration`)

*   **Route:** `/inspiration`
*   **Auth:** Private (could be made public to attract users).
*   **Description:** A curated, visually-driven feed to inspire users, sourced from the `design_inspirations` collection.
*   **Component Breakdown:**
    *   **`FilterBar`**: Contains buttons or dropdowns to filter inspirations by tags like 'narrow room', 'minimalist', etc.
    *   **`InspirationGrid`**: A masonry or uniform grid of images from the collection.
    *   **`InspirationModal`**: When a user clicks an image, a modal opens to show a larger view, the design's description, and associated tags.

## 7. Settings Page (`/settings`)

*   **Route:** `/settings`
*   **Auth:** Private.
*   **Description:** A standard settings page for account management and preferences.
*   **Component Breakdown:**
    *   **`SettingsTabs`**: Navigation to switch between different settings sections.
    *   **`AccountForm`**: A form to update user's display name and password.
    *   **`PreferencesForm`**: Contains controls to switch units (`preferences.units`) and manage notifications (`preferences.notifications_enabled`).
    *   **`LogoutButton`**: A clear button to end the user's session.