---
id: 005-flows
title: User Flows
description: Key user journeys and interaction sequences within the My Room application.
type: doc
subtype: core
status: draft
sequence: 5
tags:
  - user-flow
  - ux
  - interaction
createdAt: "2023-10-27T10:00:00Z"
updatedAt: "2023-10-27T10:00:00Z"
---

# Key User Flows

This document details the step-by-step user journeys for the most critical interactions in the My Room application. These flows are essential for understanding the user experience and ensuring a cohesive product.

## 1. New User Onboarding & First Room Creation

**Goal:** To seamlessly guide a new user from signup to having their first project saved, ensuring they understand the app's core value immediately.

*   **Trigger:** A new user successfully creates an account on the **Authentication Page** (`/auth`) and is authenticated for the first time.

| Step | Actor  | Action & Description                                                                                                                                                                   | Associated Page/Feature                                  |
| :--- | :----- | :----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :------------------------------------------------------- |
| 1    | System | Redirects the user to the onboarding sequence.                                                                                                                                             | **Onboarding Flow** (`/onboarding`)                        |
| 2    | System | Displays the first step: a welcoming modal with the app's tagline. A "Let's Go!" button proceeds to the next step.                                                                         | **Onboarding Flow**                                      |
| 3    | User   | Is presented with the 'Set Your Room Dimensions' step. The user enters the length and width of their room (e.g., 2.65, 6) and selects units (meters/feet). The form is pre-filled with an example. | **Onboarding Flow**                                      |
| 4    | System | The system validates the input and proceeds to the next step, which loads a simplified version of the main designer interface.                                                               | **Onboarding Flow**                                      |
| 5    | System | An interactive tutorial begins. Tooltips appear sequentially, pointing to: 1) The 2D/3D view toggle, 2) The Furniture Library panel, and 3) The Save button.                                 | **Onboarding Flow**, **Room Designer Canvas**            |
| 6    | User   | The tutorial prompts the user to drag a bed from the library onto the canvas. The system provides positive feedback upon completion.                                                          | **Furniture Library**, **Interactive Room Canvas**       |
| 7    | System | The final onboarding step prompts the user to name their new project (e.g., "My Bedroom").                                                                                                   | **Onboarding Flow**                                      |
| 8    | User   | Enters a name and clicks "Finish & Save".                                                                                                                                                  | **Project Management**                                   |
| 9    | System | The user's first room is saved to the `rooms` collection. The onboarding modals disappear, and the user is now in the full **Room Designer Canvas** (`/designer/:roomId`) for their new project. | **Room Designer Canvas**                                 |

## 2. Designing & Optimizing a Bedroom with AI

**Goal:** To illustrate the core design loop where a user furnishes their room and leverages the AI assistant to find an optimal layout.

*   **Trigger:** User clicks on an existing project from the **My Projects Dashboard** (`/projects`) or has just completed the onboarding flow.

| Step | Actor  | Action & Description                                                                                                                                                                                            | Associated Page/Feature                               |
| :--- | :----- | :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :---------------------------------------------------- |
| 1    | System | Loads the selected project's data from the `rooms` collection and renders it on the **Room Designer Canvas**.                                                                                                   | **Room Designer Canvas**                              |
| 2    | User   | Browses the **Furniture Library**, using the search and category filters to find items like a wardrobe and a desk.                                                                                              | **Furniture Library**                                 |
| 3    | User   | Drags the selected items onto the 2D canvas. The user moves them around, rotates them, and uses the inspector panel to resize the wardrobe to fit a specific nook.                                                  | **Interactive Room Canvas**                           |
| 4    | User   | Toggles to the 3D view to get a better sense of the space and scale. Realizes the layout feels cramped and the path to the window is awkward.                                                                      | **Interactive Room Canvas**                           |
| 5    | User   | Clicks the 'Get AI Suggestions' floating action button.                                                                                                                                                        | **AI-Powered Layout Assistant**                       |
| 6    | System | The AI analyzes the room's narrow dimensions (2.65m x 6m) and the selected furniture. It generates 3 layout suggestions and displays them as thumbnails in a modal, each with a brief title like "Work Zone by Window". | **AI-Powered Layout Assistant**                       |
| 7    | User   | Clicks on the first suggestion thumbnail. The system temporarily applies this layout to the main canvas for a live preview. The user explores it in both 2D and 3D.                                            | **AI-Powered Layout Assistant**, **Room Designer Canvas** |
| 8    | User   | Likes the second suggestion better. Clicks on it to preview, then clicks the "Apply Layout" button in the modal.                                                                                                 | **AI-Powered Layout Assistant**                       |
| 9    | System | The modal closes, and the new AI-suggested layout is now the active state of the canvas.                                                                                                                        | **Room Designer Canvas**                              |
| 10   | User   | Makes a few final minor adjustments to the decor items.                                                                                                                                                         | **Interactive Room Canvas**                           |
| 11   | User   | Clicks the "Save" button in the top bar.                                                                                                                                                                      | **Project Management**                                |
| 12   | System | The `layoutData` and `updatedAt` fields for the project document in the `rooms` collection are updated. A success toast notification is displayed.                                                              | **Project Management**                                |

## 3. Creating a New Version of a Design

**Goal:** To allow a user to experiment with a different design direction without losing their original work.

*   **Trigger:** User wants to try a completely different style for an existing project.

| Step | Actor  | Action & Description                                                                                                                                                           | Associated Page/Feature                |
| :--- | :----- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :------------------------------------- |
| 1    | User   | Navigates to the **My Projects Dashboard** (`/projects`).                                                                                                                        | **My Projects Dashboard**              |
| 2    | User   | Hovers over the project card for "My Bedroom v1" and clicks the kebab menu icon.                                                                                               | **My Projects Dashboard**              |
| 3    | User   | Selects the "Duplicate" option from the menu.                                                                                                                                  | **Project Management**                 |
| 4    | System | Creates a new document in the `rooms` collection, copying all data from the original project. The new project is automatically named "My Bedroom v1 (Copy)".              | **Project Management**                 |
| 5    | System | The dashboard refreshes to show the new project card. The user is then automatically navigated to the designer for this newly created, duplicated project.                   | **Room Designer Canvas**               |
| 6    | User   | Clicks on the project title in the top bar and renames it to "My Bedroom v2 - Boho Style".                                                                                     | **Room Designer Canvas**               |
| 7    | User   | Proceeds to make significant changes, such as replacing the furniture with different items from the library, knowing their original "v1" design is safe and accessible. | **Furniture Library**, **Room Designer Canvas** |
| 8    | User   | Clicks "Save" to persist the changes to the "v2" project.                                                                                                                    | **Project Management**                 |