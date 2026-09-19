# ECommerce — Angular 19 Storefront

A server-side rendered (SSR) e-commerce single-page application built with **Angular 19**, styled with **Tailwind CSS**, and backed by the [Route E-Commerce API](https://ecommerce.routemisr.com).

## Features

- **Authentication** — Register, login, and forgot-password flows backed by JWT (`jwt-decode`), with route guards that redirect logged-in users away from auth pages and unauthenticated users away from protected pages
- **Catalog browsing** — Home, Products, Categories, Brands, and Product Details pages with a search pipe and an owl-carousel for featured items
- **Cart & Wishlist** — Add/remove/update items, persisted through dedicated services
- **Checkout** — Order placement flow (`checkout/:id`) plus an "All Orders" history page
- **Cross-cutting concerns** — HTTP interceptors for request headers, centralized error handling, and a global loading spinner (`ngx-spinner`)
- **UX niceties** — Toast notifications (`ngx-toastr`), Angular Router view transitions, and Flowbite UI components on top of Tailwind
- **SSR & Hydration** — Server-rendered with `@angular/ssr`, non-destructive client hydration and event replay for fast first paint

## Tech Stack

| Layer | Technology |
|---|---|
| Framework | Angular 19 (standalone components, `loadComponent` lazy routes) |
| Styling | Tailwind CSS, Flowbite, SCSS |
| HTTP | `HttpClient` with `withFetch()` + interceptors |
| State/Utilities | RxJS, `jwt-decode` |
| UI Extras | `ngx-toastr`, `ngx-spinner`, `ngx-owl-carousel-o` |
| Rendering | Angular Universal (SSR) with client hydration |
| Backend API | [Route E-Commerce API](https://ecommerce.routemisr.com) |

## Project Structure

```
src/
├── app/
│   ├── core/
│   │   ├── environments/    # environment.ts (API base URL)
│   │   ├── guards/          # authGuard, logedGuard
│   │   ├── interceptors/    # headers, errors, loading
│   │   └── services/        # auth, products, categories, brands, cart, wishlist, orders, flowbite
│   ├── layout/               # auth-layout, blank-layout, navbar, footer
│   ├── pages/                 # home, login, register, forgot-password, products, product-details,
│   │                          # categories, brands, cart, wishlist, checkout, allorders, notfound
│   ├── shared/
│   │   ├── components/       # business & ui components
│   │   ├── directives/
│   │   ├── interfaces/       # IProduct, ICategory, IBrand, ICart, IOrder
│   │   └── pipes/            # search, trimtext
│   ├── app.component.ts
│   ├── app.config.ts         # client providers
│   ├── app.config.server.ts  # SSR providers
│   ├── app.routes.ts         # client routes
│   └── app.routes.server.ts  # SSR route rendering strategy
├── index.html
├── main.ts
├── main.server.ts
├── server.ts                 # Express-based SSR server entry
└── styles.scss
```

## Routing Overview

| Path | Page | Guard |
|---|---|---|
| `/register` | Register | `logedGuard` (redirects if already logged in) |
| `/login` | Login | `logedGuard` |
| `/forgotpassword` | Reset Password | `logedGuard` |
| `/home` | Home | `authGuard` (redirects to login if not authenticated) |
| `/products` | Products | `authGuard` |
| `/productdetails/:id` | Product Details | `authGuard` |
| `/categories` | Categories | `authGuard` |
| `/brands` | Brands | `authGuard` |
| `/cart` | Cart | `authGuard` |
| `/wishlist` | Wishlist | `authGuard` |
| `/checkout/:id` | Checkout | `authGuard` |
| `/allorders` | All Orders | `authGuard` |
| `**` | Not Found | — |

Authentication state is derived from a `userToken` stored in `localStorage` and decoded on the client with `jwt-decode`.

## Getting Started

### Prerequisites

- [Node.js](https://nodejs.org/) (LTS recommended)
- [Angular CLI](https://angular.dev/tools/cli) v19: `npm install -g @angular/cli`

### Installation

```bash
git clone <repository-url>
cd <project-folder>
npm install
```

### Development server

```bash
ng serve
```

Navigate to `http://localhost:4200/`. The app reloads automatically on source changes.

### Build

```bash
ng build
```

Build artifacts are output to `dist/`.

### Server-Side Rendering

This project is configured for SSR. To build and run the SSR server locally:

```bash
ng build
npm run serve:ssr
```

> Check `package.json` for the exact SSR script name/port if it differs.

## Configuration

The API base URL is set in `src/app/core/environments/environment.ts`:

```ts
export const environment = {
  baseUrl: 'https://ecommerce.routemisr.com',
};
```

Update this file (and add a `environment.prod.ts` if needed) to point to a different backend.
