---
id: 002-features
title: Core Features Specification
description: A detailed breakdown of the primary features and their functionalities for My Room.
type: doc
subtype: core
status: draft
sequence: 2
tags:
  - features
  - specification
  - product
createdAt: "2023-10-27T10:00:00Z"
updatedAt: "2023-10-27T10:00:00Z"
---

# Core Features Specification

This document outlines the functional requirements for the core features of the My Room application. Refer to the [Database Schema](./004-database.md) for data model details.

### 1. Interactive Room Canvas (2D/3D) `(id: room-canvas)`

**Priority:** P0

**Description:** This is the heart of the application, a dynamic canvas where users bring their room to life. It must be responsive, intuitive, and provide immediate visual feedback.

**Functionality:**
*   **Dimension Input:** On project creation (or via an editor on the canvas), users must be able to input room width, length, and height with precision (e.g., 2.65m, 6.0m). The canvas will dynamically resize to these dimensions.
*   **View Modes:** A prominent toggle will allow users to seamlessly switch between:
    *   **2D Top-Down View:** An architectural floor plan view. This is the primary mode for accurate placement, with a grid overlay and snapping guides. Measurements for walls and clearances between objects will be displayed dynamically.
    *   **3D View:** A realistic, textured view. Users can choose between an orbital camera (to view the room from the outside) and a first-person 'walkthrough' mode (to experience the space from within).
*   **Architectural Elements:** Users can drag-and-drop doors (with configurable swing direction) and windows onto the walls of the canvas. These elements will intelligently 'cut' through the walls in the 3D view.
*   **Controls:** The canvas will support standard interactions: drag-and-drop for placing items, click-and-drag for moving, and rotational handles for orienting objects. Zooming (scroll wheel) and panning (middle-mouse drag) will be supported in both views.

### 2. Customizable Furniture & Decor Library `(id: furniture-library)`

**Priority:** P0

**Description:** A comprehensive and easy-to-navigate library of furniture and decor items that users can add to their canvas. The data for this will be sourced from the `furniture_catalog` collection.

**Functionality:**
*   **UI:** The library will be presented in a sidebar panel on the **Room Designer Canvas** page. It will feature a search bar and a hierarchical category browser (e.g., Beds > Double Beds, Storage > Wardrobes).
*   **Customization:** When an item is selected from the library and placed on the canvas, a context-sensitive inspector panel will appear. For items marked as `resizable: true`, this panel will allow users to input exact width, length, and height dimensions. It will also offer options for changing colors and materials (where applicable).
*   **Filtering:** Users can filter the library by category, style tags (e.g., 'minimalist', 'Vietnamese'), and special attributes like 'narrow-space friendly'.
*   **Favorites:** Users can 'star' or 'favorite' items, which will be collected in a dedicated 'Favorites' tab for quick access.

### 3. AI-Powered Layout Assistant `(id: ai-layout-assistant)`

**Priority:** P1

**Description:** This is the unique value proposition of My Room. The AI assistant provides intelligent, context-aware layout suggestions, specifically optimized for challenging room shapes.

**Functionality:**
*   **Activation:** The assistant can be triggered by a dedicated button on the canvas UI. It will analyze the current room dimensions (e.g., the high aspect ratio of a 2.65m x 6m room) and any existing furniture.
*   **Suggestion Engine:** The AI will use a rules-based engine combined with a trained model. The rules will enforce best practices: minimum 75cm walkway clearance, placement of desks near light sources, and logical zoning (e.g., not placing a wardrobe where it blocks a door). 
*   **Output:** The assistant will present 2-3 alternative layout suggestions in a modal or side panel. Each suggestion will be a visual thumbnail. Clicking a suggestion will temporarily apply it to the main canvas for the user to preview in 2D/3D. The user can then choose to accept the layout or revert to their previous state.
*   **Feedback:** The AI can also provide passive feedback, highlighting potential issues in the user's current layout, such as red overlays on areas with insufficient clearance.

### 4. Project & Iteration Management `(id: project-management)`

**Priority:** P0

**Description:** Allows users to save and manage their work effectively. All projects are stored in the `rooms` collection.

**Functionality:**
*   **Dashboard:** The **My Projects Dashboard** page will display all user-created projects as cards, showing a thumbnail, project name, and last modified date.
*   **CRUD Operations:** From the dashboard, users can create a new project, edit an existing one (which loads the **Room Designer Canvas**), or delete a project (with confirmation).
*   **Iteration:** A 'Duplicate' or 'Save As' feature will be available within the designer. This allows users to create a copy of their current design to experiment with a new layout (e.g., 'Bedroom v2') without losing their original work. This is a core part of the creative process.

### 5. Secure User Authentication `(id: authentication)`

**Priority:** P0

**Description:** Standard, secure user account management powered by Firebase Authentication.

**Functionality:**
*   **Signup/Login:** The **Authentication Page** will provide forms for email/password signup and login. It will also integrate social providers, starting with Google Sign-in.
*   **Session Management:** Securely manages user sessions, ensuring that users remain logged in across visits and that their projects are accessible only to them.
*   **Password Recovery:** A standard 'Forgot Password' flow that sends a reset link to the user's registered email.

### 6. Profile & App Settings `(id: settings)`

**Priority:** P0

**Description:** A central location for users to manage their account and application preferences.

**Functionality:**
*   **Account Management:** On the **Settings Page**, users can update their display name and change their password.
*   **Preferences:** Users can set their preferred unit of measurement (meters vs. feet), which will be reflected across the entire application, especially on the **Room Designer Canvas**. They can also manage notification preferences.

### 7. Interactive Onboarding Tour `(id: onboarding)`

**Priority:** P0

**Description:** A guided experience for new users to ensure they understand the core functionality and can get started quickly.

**Functionality:**
*   **Trigger:** This flow begins immediately after a new user completes signup.
*   **Guided Steps:** The **Onboarding Flow** will walk the user through:
    1.  A welcome message.
    2.  Prompting to input their first room's dimensions (pre-filling with 2.65 x 6m as an example).
    3.  A short, interactive tutorial on the **Room Designer Canvas** that highlights the 2D/3D toggle, the furniture library, and how to drag an item.
    4.  A prompt to name and save their first project.