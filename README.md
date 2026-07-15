# Talabaty (طلباتي)

**Your Local Market, One Tap Away**

A regional hyper-local marketplace platform connecting customers with local businesses across the Bekaa Valley. Built on the MERN stack with TypeScript across the entire codebase.

---

## Table of Contents

- [Overview](#overview)
- [The Problem](#the-problem)
- [Tech Stack](#tech-stack)
- [Core User Roles](#core-user-roles)
- [Store Categories](#store-categories)
- [Data Model](#data-model)
- [Key Technical Decisions](#key-technical-decisions)
- [Authentication & Access Control](#authentication--access-control)
- [What We Learned](#what-we-learned)
- [Future Enhancements](#future-enhancements)
- [Team](#team)

---

## Overview

Talabaty digitizes local commerce in the Bekaa Valley by replacing scattered phone calls and messaging-app orders with a single, centralized, multi-vendor marketplace. It brings customers, local businesses, and platform administrators together in one system.

## The Problem

Restaurants, supermarkets, pharmacies, clothing stores, bakeries, and other neighborhood shops still rely on phone calls and messaging apps to receive orders. This creates several recurring issues:

1. **Manual, Error-Prone Ordering** — Phone and chat orders lead to communication errors, incomplete details, and delayed responses.
2. **Limited Product Visibility** — Customers can't easily compare products and services across nearby businesses before ordering.
3. **Fragmented Store Management** — Owners juggle stores, catalogs, inventory, and incoming orders with no unified system.

Talabaty replaces this fragmented process with one centralized, multi-vendor marketplace.

## Tech Stack

- **MongoDB** — Database
- **Express.js** — Backend framework
- **React.js** — Frontend framework
- **Node.js** — Runtime environment
- **TypeScript** — Used across both frontend and backend for type safety
- **Bootstrap 5 + Bootstrap Icons** — Consistent, responsive UI components

## Core User Roles

| Role | Responsibilities |
|------|-------------------|
| **Customers** | Browse partner stores and products, add items to cart, and check out online. |
| **Partners** | Manage their own product catalog, inventory, and incoming orders through a dedicated dashboard. |
| **Admins** | Oversee accounts, approve partners and categories, and monitor the overall platform. |

## Store Categories

Talabaty organizes partner businesses into the following categories:

- 🍲 Restaurants
- 🛒 Supermarkets
- 💊 Pharmacies
- 👗 Fashion
- 🥖 Bakeries
- ☕ Cafés

## Data Model

The database design (ERD) follows this structure:

- **Users** branch into `partner_profiles` (1:0..1) and drive both `carts` and `orders`.
- **Products** belong to a `partner` and a `category`.
- **Carts** and **Orders** each break into line-item collections (`cart_items`, `order_items`).

### Admin & Partner Permissions

**Admin**
- Freeze or delete any user account
- Approve, freeze, or delete partner profiles
- Monitor all products (view-only)
- Create and approve new categories

**Partner**
- Request new categories for admin approval

## Key Technical Decisions

- **TypeScript Everywhere** — Both frontend and backend are written in TypeScript for type safety across the stack.
- **Mixed MongoDB IDs** — Some seed documents use custom string/number `_id`s instead of `ObjectId`, handled with `Schema.Types.Mixed` and manual joins where needed.
- **Bootstrap for UI** — Consistent, responsive components styled with Bootstrap 5 and Bootstrap Icons.
- **JWT + Role Middleware** — `protect()` verifies the token; `authorize(role)` gates access per route.
- **Bcrypt Password Hashing** — Passwords are never stored in plain text; they are hashed before persistence.
- **Role-Based Access Control** — Customer, Partner, and Admin routes are gated by role middleware.

## Authentication & Access Control

Talabaty implements a clean, role-based authentication flow:

- JSON Web Tokens (JWT) are used to authenticate requests.
- A `protect()` middleware verifies the token on protected routes.
- An `authorize(role)` middleware restricts access based on the user's role (Customer, Partner, or Admin).
- Passwords are hashed with Bcrypt before being stored.

## What We Learned

**New skills we gained confidence in:**

- **JWT & Role-Based Access** — Designing an authentication flow that cleanly separates Customer, Partner, and Admin permissions across the stack.
- **TypeScript Across the Stack** — Writing typed models, controllers, and React components, and using types to catch mismatches early.

**Where we're still growing:**

- **State Management & Data Joins Across Collections** — Working with MongoDB's flexible schema meant manually joining data across collections (e.g., linking a partner's user status to their store profile) instead of relying on relational joins — an area of the MERN stack the team is still building deeper comfort with.

## Future Enhancements

1. **Online Payment Gateway** — Automatic payment verification beyond Cash on Delivery / Whish Money.
2. **Mobile Applications** — Native apps for customers and partners on the go.
3. **Push Notifications** — Real-time order status updates for customers and partners.
4. **Loyalty & Rewards** — Points, coupons, and promotional discounts for returning customers.
5. **Reviews & Ratings** — Product reviews and store ratings to build trust.
6. **Live Chat** — Direct chat between customers and businesses.

## Team

- Hala Al-Ali
- Hidaya Abo Al Oyoun Assud
- Yasser Kayed

---

*Talabaty — Your Local Market, One Tap Away.*
