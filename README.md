# TownTeen — Premium Fashion E-Commerce

## Overview

**TownTeen** is a front-end e-commerce web application for a premium fashion brand. It presents a polished, responsive online storefront where users can browse products by category, filter and sort products, view detailed product pages, add items to a cart, and maintain a personal wishlist.

The project is built with **vanilla HTML, CSS, and JavaScript** (no framework), styled primarily with **Bootstrap 5** plus a custom stylesheet, and powered by a remote **MockAPI** backend for product data. All user state (cart, wishlist, quantities) is persisted in the browser's `localStorage`.

---

## Tech Stack

| Layer      | Technology                                                        |
|------------|-------------------------------------------------------------------|
| Markup     | HTML5 (semantic structure)                                        |
| Styling    | Bootstrap 5.3.0-alpha1 + Bootstrap Icons 1.11.3 + custom `style.css` |
| Fonts      | Google Fonts **Inter**                                            |
| JavaScript | Vanilla JS (DOM manipulation, `fetch`, `localStorage`)            |
| Backend    | [MockAPI](https://mockapi.io) — hosted RESTful API for products   |
| Versioning | Git (branch: `master`)                                            |

---

## Project Structure

```
TownTeen/
├── index.html            # Home page (hero video, categories, featured products)
├── products_index.html   # Products listing page (filters + grid)
├── product_detail.html   # Single product page (size/qty + add to cart)
├── cart.html             # Shopping cart + order summary
├── wishlist.html         # Saved/wishlist items page
├── timer.html            # Standalone stopwatch utility page
├── style.css             # Custom styles, color variables, hover effects
├── css/                  # Local Bootstrap distribution files
│   ├── bootstrap.css / bootstrap-grid.css / bootstrap-reboot.css
│   ├── bootstrap-utilities.css / bootstrap.rtl.css (+ .min / .map variants)
├── js/                   # Local Bootstrap JS bundles (+ .min / .map)
├── resources/            # Static assets
│   ├── logo.png          # Brand logo
│   ├── main-hero-video.mp4  # Home page hero background video
│   ├── grid.png          # 1-column layout control icon
│   ├── column.png        # 2-column layout control icon
│   └── check.png         # "Added to Cart" confirmation icon
└── timer/
    ├── play.png          # Timer play button
    └── pause.png         # Timer pause button
```

---

## API / Data Source

All product data is loaded asynchronously from a MockAPI endpoint:

```
Base:  https://69bea7ae17c3d7d977929e89.mockapi.io/TownTeen/products
GET   .../products                 → fetch all products (array)
GET   .../products/{id}            → fetch a single product by id
```

Product object shape (as used by the app):
```json
{
  "id": "1",
  "name": "...",
  "category": "T-shirts | Shoes | Shirts | Hoodies",
  "price": 25.0,
  "image": "https://..."
}
```

> Note: Category filter values on the products page are `T-shirts`, `Shoes`, `Shirts`, and `Hoodies`. The home page also references these categories.

---

## Pages & Features

### 1. `index.html` — Home Page
- **Hero section** with an autoplay, muted, looping background video (`resources/main-hero-video.mp4`), a semi-transparent green overlay, headline, subtitle, and a **Shop Collection** CTA.
- **Shop by Category** — a responsive grid of 4 category cards (T-Shirts, Shorts, Shirts, Hoodies) with inline remote images.
- **Featured Products** — dynamically fetches and renders up to **4 products** (`maxRange = 4`) into `#products_grid`.
- **Footer** — brand info, Company/Support link columns, social icons.
- Product cards link to the detail page via `product_detail.html?id=${product.id}` and include a heart wishlist toggle.

### 2. `products_index.html` — Products Listing
- Product grid rendered from the API.
- **Filter sidebar** (collapsible on mobile via the funnel `#filtersBtn`):
  - **Category** — multi-select checkboxes (`T-shirts`, `Shoes`, `Shirts`, `Hoodies`).
  - **Price** — radio sort: Low to High / High to Low.
  - **Size** — pill-style checkboxes (XS, S, M, L, XL, XXL).
- **Active filter chips** shown under the header (`#filter_bar_items`).
- **Clear All** button resets all filters and restores the full product list.
- **Layout control** (visible on screens < 768px): toggles between 1-column and 2-column product grid using `resources/grid.png` and `resources/column.png`.
- "Showing products out of..." counter (`#showingProductsOutOf`).

### 3. `product_detail.html` — Product Detail
- Reads the product id from the query string (`?id=`) and fetches that product.
- **Size selection** (radio buttons XS–XXL, `M` default selected).
- **Quantity selector** with increment/decrement buttons.
- **Add to Cart** button — shows a loading spinner, stores quantity in `localStorage`, updates the cart badge, and displays an animated **"Added to Cart"** confirmation toast (fade + translate).
- Product info panel: category, name, price, and a fixed description.
- Static details sections: **Materials**, **Care Instructions**, **Shipping**.
- **You May Also Like** — fetches up to 4 related products from the same category (excluding the current product).

### 4. `cart.html` — Shopping Cart
- Lists all products currently stored in `localStorage`.
- Each item shows a thumbnail, name, category, unit price, per-item **Total** (price × qty), quantity controls (`+`/`–`), and a **trash** button to remove the item entirely.
- Quantity changes update `localStorage` and the cart badge.
- **Order Summary** sidebar:
  - Subtotal (item count + sum)
  - Shipping — `$15` if subtotal < `$75`, otherwise **Free**
  - Tax — calculated at 10% of subtotal
  - Total — subtotal × 1.1
- **Proceed to Checkout** button (non-functional placeholder).
- "Continue Shopping" link back to the products page.

### 5. `wishlist.html` — Wishlist
- Loads products whose `wishlist-{id}` key exists in `localStorage`.
- Renders each wishlisted product with a trash button to remove it (and clear the localStorage flag).
- Shows an empty-state message ("Your wishlist is empty") with a **Continue Shopping** button when no items exist.

### 6. `timer.html` — Stopwatch (Standalone)
- A simple stopwatch (`HH:MM:SS`) with a play/pause button (`timer/play.png` / `timer/pause.png`).
- Toggles via button click **or** the **Spacebar** key.
- Persists the current time in `localStorage` under the key `time`, so it survives page reloads.

---

## State Management (`localStorage`)

| Key                    | Purpose                                                      | Format       |
|------------------------|--------------------------------------------------------------|--------------|
| `{productId}`          | Quantity of a product in the cart                            | number/string |
| `totalCartQuantity`    | Total number of items across the whole cart                  | number/string |
| `wishlist-{productId}` | Marks a product as wishlisted (`true` value)                 | any          |
| `time`                 | Current stopwatch time (timer.html)                          | `HH:MM:SS`   |

---

## Design System

### Brand Colors (defined as CSS variables in `style.css`)
| Variable            | Value      | Usage                          |
|---------------------|------------|--------------------------------|
| `--brand-green`     | `#2C5F2D`  | Primary green (buttons, accents) |
| `--brand-dark`      | `#1a1a1a`  | Headings / primary text        |
| `--brand-gray`      | `#4a4a4a`  | Secondary text / icons         |
| `--brand-light-gray`| `#707070`  | Muted text / small labels      |
| `--bg-warm`         | `#faf8f5`  | Navbar background              |
| `--card-bg`         | `#ffffff`  | Cards                          |
| `--footer-bg`       | `#F7F4EF`  | Footer / warm sections         |
| `--border-light`    | `#e8e3dc`  | Borders / dividers             |

Additional accents: `#e83e8c` (pink for active wishlist hearts), `#308932` (button hover).

### Typography
- **Inter** font family (Google Fonts), weights 300–700.
- Fallback stack: `-apple-system`, `'Segoe UI'`, `Roboto`, `Helvetica`, `Arial`, `sans-serif`.

### Key Custom Classes (`style.css`)
- `.hover-zoom` — subtle 1.02 scale on hover.
- `.hover-color` — green button with a lighter hover state.
- `.btn-brand-green` / `.bg-brand-green` / `.text-brand-green` — green utility helpers.
- `.heart-icon` — circular white frosted-glass wishlist heart button.
- `.navbar-custom` / `.navbar-brand-custom` — custom warm navbar styling.
- `.video-container` — full-width hero video wrapper (`object-fit: cover`).
- `.add-to-cart-cotainer` + `.fade` / `.fade.show` — animated "Added to Cart" toast.
- `.section-pad` — responsive section padding across breakpoints.

---

## Responsive Behavior
- Bootstrap grid system throughout (`col-sm`, `col-md`, `col-lg`, `col-xl`).
- Navbar collapses below the `lg` breakpoint with a hamburger toggler.
- Product grids adapt from 4 columns on desktop down to 1–2 columns on mobile.
- Mobile-specific **layout control** (grid vs. column) appears only under 768px.
- Filters collapse into a toggleable panel on small screens.

---

## Getting Started

This is a static site — no build step or dependencies to install.

1. Clone / open the repository.
2. Serve the folder locally (e.g., `npx serve`, VS Code Live Server, or Python `http.server`) for best results.
3. Open `index.html` in a browser.

> A backend/API is not required locally; product data is fetched live from MockAPI over the network.

---

## Deployment

The TownTeen website is deployed as a static site using **GitHub Pages**. No build process or server-side hosting is required because the project consists of HTML, CSS, and vanilla JavaScript files, while product data is retrieved from the remote MockAPI service.

### Live Website

You can access the deployed application here:

**https://ahmedmamdouh32.github.io/TownTeen/index.html**

### Deployment Platform

- **Platform:** GitHub Pages
- **Repository:** `TownTeen`
- **Deployment type:** Static website
- **Entry point:** `index.html`
- **Backend/API:** MockAPI remains the remote data source for products

### Notes

Because the application is deployed on GitHub Pages, all assets and internal page links are served as static files. The application does not require a backend server to run, but an internet connection is required to load product data from MockAPI.

## Known Notes & Placeholders
- **Checkout** buttons are visual placeholders (no payment flow).
- Product detail **description** is a static string, not sourced from the API.
- Size filter pills and category labels are wired to markup, but the size filter is not fully integrated into the filtering logic on the products page.
- The `#showingProductsOutOf` counter and some filter chip logic are partially implemented.
