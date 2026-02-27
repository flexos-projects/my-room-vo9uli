---
id: 006-design
title: Design System & Vibe
description: The visual identity, color palette, typography, and overall user experience principles for My Room.
type: doc
subtype: core
status: draft
sequence: 6
tags:
  - design
  - ui
  - ux
createdAt: "2023-10-27T10:00:00Z"
updatedAt: "2023-10-27T10:00:00Z"
---

# Design System & Vibe

This document outlines the design principles, visual identity, and user experience goals for the My Room application. The design should be clean, modern, and inspiring, making the process of room design feel accessible and fun.

## 1. Vibe & Principles

Our design should embody the following principles:

*   **Creative & Empowering:** The interface should feel like a creative tool, not a technical one. We empower users by giving them powerful features (like the AI assistant) through a simple interface. The user should feel like a designer, confident in the choices they are making.
*   **Personalized & Playful:** The experience is about designing a personal space. The UI should be friendly and engaging, with playful micro-interactions and a vibrant color palette. The aesthetic should be clean but not sterile.
*   **Intuitive & Uncluttered:** The core design canvas is the hero. All surrounding UI elements—toolbars, sidebars, modals—should be secondary and unobtrusive. We will prioritize clarity and avoid visual clutter, ensuring the user can focus on their creation.

## 2. Color Palette

The color palette is designed to be modern, energetic, and clean.

*   **Primary (`#ec4899`):** A vibrant pink. This color will be used for primary calls-to-action (CTAs) like "Start Designing", "Save", and other key interactive elements. It's bold and creative, reflecting the app's personality.
*   **Accent (`#66ccb2`):** A soft teal. This color will be used for secondary actions, highlights, selection states (e.g., highlighting a selected piece of furniture), and instructional overlays. It provides a cool, calming contrast to the primary pink.
*   **Neutral Dark (`#1a202c`):** A deep charcoal. This will be used for all primary text, headings, and dark UI elements to ensure high readability and a modern feel.
*   **Neutral Light (`#f8fafc`):** A very light, almost white gray. This will be the primary background color for the application, providing a clean, bright canvas for the user's designs.
*   **Supporting Neutrals:**
    *   `#e2e8f0` (Light Gray): For borders, dividers, and disabled states.
    *   `#cbd5e1` (Mid Gray): For secondary text and icons.

## 3. Typography

We will use a single font family to maintain consistency and a modern aesthetic.

*   **Font Family: Poppins**
    *   **Why Poppins?** It's a geometric sans-serif typeface that is clean, friendly, and highly legible at various sizes. Its rounded forms give it a playful yet professional quality that aligns perfectly with our brand vibe.
*   **Hierarchy:**
    *   **Headings (h1, h2, h3):** Poppins Bold (e.g., 32px, 24px, 20px). Used for page titles and major section headers.
    *   **Body Text:** Poppins Regular (16px). Used for all standard paragraphs, descriptions, and labels.
    *   **UI Elements (Buttons, Labels):** Poppins Medium (14px). Used for button text and important UI labels to give them slightly more prominence than body text.

## 4. Iconography

Icons will be a key part of the intuitive UI. We will use a consistent, clean, line-art style. The icon set should feel modern and lightweight. Examples include:

*   **2D/3D Toggle:** An icon showing a cube and a square.
*   **AI Assistant:** A sparkling wand or a lightbulb icon.
*   **Furniture Library:** An armchair icon.
*   **Save:** A classic floppy disk or a cloud-with-arrow-up icon.

## 5. Layout & Navigation

*   **Navigation Pattern:** The primary navigation for authenticated users will be a persistent **sidebar** on the left. This provides quick access to 'My Projects', 'Inspiration', and 'Settings'. This pattern is scalable and keeps the top of the screen clean, especially on the critical **Room Designer Canvas** page.
*   **Spacing:** We will use a consistent spacing system based on a 4px or 8px grid. This ensures visual harmony and rhythm throughout the application. For example, padding within cards might be 16px (4 units), and the gap between cards might be 24px (6 units).
*   **Visuals:** The app will make heavy use of visuals. The **My Projects Dashboard** will feature auto-generated thumbnails. The **Inspiration Feed** will be an image-first grid. The furniture library will use clear 2D icons for easy identification.

## 6. Component Style

*   **Buttons:** Primary buttons will have a solid background of `#ec4899` with white text. Secondary buttons will be outlined with `#66ccb2`. All buttons will have slightly rounded corners (e.g., 8px border-radius) to maintain a friendly feel.
*   **Cards:** Cards (used for projects, inspiration items) will have a white background (`#ffffff`), a subtle box-shadow to lift them off the `#f8fafc` page background, and rounded corners.
*   **Forms:** Input fields will be clean and simple, with a light gray border that turns to the accent color (`#66ccb2`) on focus. Labels will be placed above the input fields for clarity.