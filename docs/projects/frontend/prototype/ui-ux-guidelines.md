# UI/UX Guidelines for Workspace Flow

## Overview

These guidelines define the Workspace Flow experience for project memory, contextual work, explicit source consent, and explainable diagnosis. The interface follows the root mockups and uses Ant Design as its component foundation.

---

## Table of Contents

1. [Design Principles](#design-principles)
2. [Information Architecture](#information-architecture)
3. [Components & Patterns](#components--patterns)
4. [Memory and AI Review](#memory-and-ai-review)
5. [Diagnosis and Reporting](#diagnosis-and-reporting)
6. [Responsive Design](#responsive-design)
7. [Accessibility](#accessibility)
8. [Mockup Handoff](#mockup-handoff)

---

## Design Principles

- **Context first**: Show why a task or finding exists.
- **User control**: Make AI suggestions reviewable and source consent explicit.
- **Explainability**: Pair scores with dimensions, evidence, and recommendations.
- **Calm density**: Support scanning across multiple projects without dashboard noise.
- **Progressive disclosure**: Keep advanced diagnosis details available without overwhelming the workspace.
- **Accessible by default**: Keyboard, semantic structure, contrast, and mobile behavior are part of every state.

## Information Architecture

```text
Workspace Flow
├── Portfolio Dashboard
├── Projects
│   ├── Project List
│   └── Project Workspace
│       ├── Memory
│       ├── Contextual Work
│       ├── Diagnosis
│       └── Activity
├── Account
└── Consent and Source Settings
```

## Components & Patterns

- Use Ant Design `Layout`, `Menu`, `Card`, `List`, `Table`, `Tabs`, `Drawer`, `Modal`, `Form`, `Alert`, `Progress`, and `Skeleton` consistently.
- Provide clear empty, loading, error, success, confirmation, and retry states.
- Use a task card only when it represents contextual work; link back to the source note when available.
- Use badges and color sparingly for severity, status, and consent state.
- Keep destructive actions behind confirmation and explain their scope.

## Memory and AI Review

- Free-form note capture is the shortest path into a project.
- Display raw note content before any generated structure.
- Present AI suggestions as proposals with distinct **Accept**, **Edit**, and **Discard** actions.
- Show whether a note became a task or an architecture decision and preserve its history.

## Diagnosis and Reporting

- Explain source scope and read-only behavior before analysis begins.
- Show analysis progress, completion, failure, and retry states.
- Display overall score beside dimension scores and the previous-run trend.
- Present each finding with severity, evidence, location, and recommendation.
- Mark findings that intersect with an intentional architecture decision.

## Responsive Design

- **Mobile**: 320px and above; use stacked workspace sections and touch-friendly controls.
- **Tablet**: 768px and above; use a two-column project workspace where space permits.
- **Desktop**: 1024px and above; use portfolio comparison and persistent project navigation.
- Preserve the same task, memory, consent, and diagnosis actions at every breakpoint.

## Accessibility

- Use semantic headings, landmarks, labels, and live regions for asynchronous analysis.
- Provide keyboard access to menus, drawers, modals, filters, and task status changes.
- Never use color as the only indicator for severity, score, or consent.
- Announce analysis status and validation errors to assistive technology.
- Test the complete MVP journey at desktop and mobile viewport sizes.

## Mockup Handoff

The base visual references are [`1.png`](../../../ui-ux/prototypes/images/1.png), [`2.png`](../../../ui-ux/prototypes/images/2.png), [`3.png`](../../../ui-ux/prototypes/images/3.png), [`4.png`](../../../ui-ux/prototypes/images/4.png), [`home.png`](../../../ui-ux/prototypes/images/home.png), and [`screen.png`](../../../ui-ux/prototypes/images/screen.png). Each implemented screen must document its states, responsive behavior, data dependencies, and acceptance criteria.

---