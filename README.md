# StoreBoard

A live e-commerce dashboard powered by [smart-mock.com](https://smart-mock.com). No backend, no database, no setup.

**[Live demo](https://marek-lyre.github.io/storeboard)**

---

## What it is

StoreBoard is a single HTML file that turns your smart-mock schema into a working dashboard. It shows orders, products, customers, revenue charts, and a live event feed - all pulled from your smart-mock API endpoint.

It works in two modes:

| Mode | What happens |
|------|-------------|
| Mock | Runs immediately with generated data. No account needed. |
| Live | Enter your smart-mock `user_id` and token, fetch your real tables. |

---

## Quickstart

**1. Clone and open**

```bash
git clone https://github.com/marek-lyre/storeboard.git
cd storeboard
open index.html
```

No npm. No build step. One file.

**2. Create your schema on smart-mock.com**

Go to [smart-mock.com](https://smart-mock.com) and create three tables:

| Table | Key columns |
|-------|-------------|
| `customers` | `id`, `first_name`, `last_name`, `email`, `city`, `total_spent` |
| `products` | `id`, `name`, `category`, `price`, `stock` |
| `orders` | `id`, `customer_id`, `product`, `amount`, `status`, `date` |

Link `orders.customer_id` to `customers.id` via a cable to get relational data.

Full setup guide: [schema/setup.md](schema/setup.md)

**3. Connect to live data**

Paste your `user_id` and `smt_` token into the config bar and click **fetch live data**.

---

## How it works

```
smart-mock.com
  customers table --+
  products table  --+--> StoreBoard (index.html)
  orders table    --+         |
  (linked via cable)          +--> KPIs, Orders, Products, Customers, Revenue
```

StoreBoard calls the smart-mock REST API directly from the browser:

```
GET https://smart-mock.com/users/{user_id}/tables/orders?token={smt_token}
GET https://smart-mock.com/users/{user_id}/tables/products?token={smt_token}
GET https://smart-mock.com/users/{user_id}/tables/customers?token={smt_token}
```

No proxy. No server. Just fetch.

---

## Screenshot

![StoreBoard dashboard](assets/screenshot.png)

---

## Customize it

StoreBoard is one HTML file with no dependencies. The data layer is about 50 lines of vanilla JS.

Want different tables? Change the fetch URLs and column names in the `renderAll()` function.

Want a different use case? Swap `products/orders/customers` for `users/sessions/events` and you have a SaaS metrics dashboard.

---

## Built with

- [smart-mock.com](https://smart-mock.com) - live REST API for realistic fake data
- Vanilla HTML/CSS/JS
- Share Tech Mono + DM Sans (Google Fonts)

---

## License

MIT
