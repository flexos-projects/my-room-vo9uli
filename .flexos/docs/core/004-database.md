---
id: 004-database
title: Database Schema
description: The specification for the Firestore database collections and data models.
type: doc
subtype: core
status: draft
sequence: 4
tags:
  - database
  - firestore
  - architecture
createdAt: "2023-10-27T10:00:00Z"
updatedAt: "2023-10-27T10:00:00Z"
---

# Database Schema (Firebase Firestore)

This document defines the data structures for the My Room application using Firebase Firestore. The schema is designed to be scalable and efficient for the application's core features.

## Collections

### 1. `users`

This collection stores public profile information and application-specific settings for each user. The primary user record and authentication details are managed by Firebase Authentication; this collection supplements that with our app's data.

*   **Path:** `users/{userId}`
*   **Description:** The `userId` for each document will match the UID provided by Firebase Authentication.

**Fields:**

| Field Name          | Type                | Required | Description                                                                                             | Example                                                              |
| ------------------- | ------------------- | -------- | ------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------- |
| `email`             | String              | Yes      | User's email address. Stored for reference and potential communication.                                 | `"user@example.com"`                                                 |
| `displayName`       | String              | No       | User's chosen display name.                                                                             | `"Alex Doe"`                                                         |
| `createdAt`         | Timestamp           | Yes      | Server timestamp when the user document is created.                                                     | `October 27, 2023 at 10:00:00 AM UTC+0`                                |
| `preferences`       | Map (Object)        | No       | An object containing user-specific settings.                                                            | `{ "units": "metric", "notificationsEnabled": true }`              |
| `preferences.units` | String (`enum`)     | No       | The preferred unit of measurement for the user. Can be `"metric"` or `"imperial"`. Defaults to `"metric"`. | `"metric"`                                                           |

### 2. `rooms`

This collection contains all the room design projects created by users. It is the most critical collection for the core application experience.

*   **Path:** `rooms/{roomId}`
*   **Description:** Each document represents a single saved room design.

**Fields:**

| Field Name      | Type           | Required | Description                                                                                                                                                                                            | Example                                                                                                                               |
| --------------- | -------------- | -------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------- |
| `userId`        | String         | Yes      | The UID of the user who owns this project. This is crucial for security rules.                                                                                                                         | `"abc123xyz"`                                                                                                                         |
| `name`          | String         | Yes      | The user-defined name for the project.                                                                                                                                                                 | `"My Bedroom - Minimalist Idea"`                                                                                                      |
| `widthM`        | Number         | Yes      | The width of the room in meters. Storing in a base unit (meters) is essential for consistency.                                                                                                        | `2.65`                                                                                                                                |
| `lengthM`       | Number         | Yes      | The length of the room in meters.                                                                                                                                                                      | `6`                                                                                                                                   |
| `heightM`       | Number         | Yes      | The height of the room in meters.                                                                                                                                                                      | `2.8`                                                                                                                                 |
| `layoutData`    | Map (Object)   | Yes      | A JSON object containing all positional data for the room's contents.                                                                                                                                  | `{ "furniture": [ { "id": "bed-01", "x": 1.5, "y": 3.2, "rot": 90, "width": 1.6, "length": 2.0 } ], "doors": [ ... ] }` |
| `createdAt`     | Timestamp      | Yes      | Server timestamp for when the project was first created.                                                                                                                                               | `October 27, 2023 at 10:05:00 AM UTC+0`                                                                                                 |
| `updatedAt`     | Timestamp      | Yes      | Server timestamp for the last time the project was saved. Used for sorting on the dashboard.                                                                                                           | `October 27, 2023 at 11:30:00 AM UTC+0`                                                                                                 |
| `thumbnailUrl`  | String (URL)   | No       | A URL pointing to an auto-generated thumbnail of the design, stored in Firebase Storage. This is for the `ProjectCard` on the dashboard.                                                                 | `"https://firebasestorage.googleapis.com/..."`                                                                                        |

**Firestore Rules:**
*   A user can only read/write documents where `room.userId == request.auth.uid`.

### 3. `furniture_catalog`

This is a global, read-only (for users) collection that serves as the master library of all available furniture and decor items.

*   **Path:** `furniture_catalog/{furnitureId}`
*   **Description:** Each document represents a single item that can be placed in a room.

**Fields:**

| Field Name             | Type                | Required | Description                                                                                                   | Example                                                                                                  |
| ---------------------- | ------------------- | -------- | ------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------- |
| `name`                 | String              | Yes      | The display name of the item.                                                                                 | `"Minimalist Double Bed"`                                                                                |
| `category`             | String (`enum`)     | Yes      | The primary category for filtering. E.g., `Bed`, `Storage`, `Desk`, `Chair`, `Decor`.                           | `"Bed"`                                                                                                  |
| `defaultDimensionsM`   | Map (Object)        | Yes      | The default dimensions of the item in meters.                                                                 | `{ "width": 1.6, "length": 2.0, "height": 0.9 }`                                                     |
| `resizable`            | Boolean             | Yes      | A flag indicating if the user is allowed to modify the dimensions of this item.                               | `true`                                                                                                   |
| `modelUrl2d`           | String (URL)        | Yes      | URL to the 2D vector graphic (SVG) for the top-down canvas view.                                              | `"https://firebasestorage.googleapis.com/.../bed_2d.svg"`                                                |
| `modelUrl3d`           | String (URL)        | No       | URL to the 3D model (glTF) for the 3D visualization.                                                          | `"https://firebasestorage.googleapis.com/.../bed_3d.gltf"`                                               |
| `tags`                 | Array of Strings    | No       | An array of keywords for search and filtering.                                                                | `["modern", "minimalist", "wood", "narrow-space"]`                                                 |

### 4. `design_inspirations`

This collection holds the curated content for the **Inspiration Feed** page.

*   **Path:** `design_inspirations/{inspirationId}`
*   **Description:** Each document is a piece of inspirational content, like a beautifully designed narrow room.

**Fields:**

| Field Name       | Type             | Required | Description                                                                         | Example                                                                        |
| ---------------- | ---------------- | -------- | ----------------------------------------------------------------------------------- | ------------------------------------------------------------------------------ |
| `title`          | String           | Yes      | The title of the inspiration piece.                                                 | `"Cozy & Functional 2.5m Wide Bedroom"`                                        |
| `description`    | String           | Yes      | A brief description or set of tips related to the design.                           | `"This layout uses a floating desk and a low-profile bed to create..."`        |
| `imageUrl`       | String (URL)     | Yes      | The primary image URL for the inspiration card and modal.                           | `"https://firebasestorage.googleapis.com/.../inspiration_01.jpg"`              |
| `tags`           | Array of Strings | No       | Tags for filtering the feed.                                                        | `["narrow-room", "small-space", "scandinavian"]`                           |
| `roomDimensions` | Map (Object)     | No       | The dimensions of the room featured in the inspiration, for user context.           | `{ "width": 2.5, "length": 5.5 }`                                            |
| `createdAt`      | Timestamp        | Yes      | Timestamp for when the inspiration was added, used for sorting the feed by recency. | `October 26, 2023 at 09:00:00 AM UTC+0`                                        |