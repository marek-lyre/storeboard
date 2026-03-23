# Schema setup on smart-mock.com

This guide walks you through creating the three tables StoreBoard expects.
Takes about 3 minutes. No account required for mock mode — email account needed for live mode with persistent data.

---

## Step 1 — Create a free account

Go to [smart-mock.com/auth](https://smart-mock.com/auth) and register.
A username is enough to get started (no email required for basic use).

---

## Step 2 — Create the `customers` table

On your dashboard, add a new table named `customers` with these columns:

| Column | Type | Rule |
|--------|------|------|
| `id` | string | `composed: CUS-{seq:4}` |
| `first_name` | string | `rule:first_name` |
| `last_name` | string | `rule:last_name` |
| `email` | string | `rule:email` |
| `city` | string | `rule:city` |
| `country` | string | `rule:country` |
| `total_spent` | number | `rule:decimal, range(50-5000)` |

Generate **20 rows**.

---

## Step 3 — Create the `products` table

Add a new table named `products`:

| Column | Type | Rule |
|--------|------|------|
| `id` | string | `composed: PRD-{seq:3}` |
| `name` | string | `rule:enum(Wireless Headphones\|Running Shoes\|Coffee Maker\|Desk Lamp\|Yoga Mat\|Bluetooth Speaker\|Denim Jacket\|Novel Collection)` |
| `category` | string | `rule:enum(Electronics\|Sports\|Home & Garden\|Clothing\|Books)` |
| `price` | number | `rule:decimal, range(15-250)` |
| `stock` | number | `rule:integer, range(0-400)` |
| `sold` | number | `rule:integer, range(10-900)` |

Generate **8 rows**.

---

## Step 4 — Create the `orders` table

Add a new table named `orders`:

| Column | Type | Rule |
|--------|------|------|
| `id` | string | `composed: ORD-{seq:4}` |
| `customer_id` | string | *(linked via cable — see Step 5)* |
| `product` | string | `rule:enum(Wireless Headphones\|Running Shoes\|Coffee Maker\|Desk Lamp\|Yoga Mat\|Bluetooth Speaker)` |
| `amount` | number | `rule:decimal, range(15-500)` |
| `status` | string | `rule:enum(paid\|paid\|paid\|shipped\|pending\|processing\|failed)` |
| `date` | date | `rule:date, from(2025-01-01):until(2025-12-31)` |

Generate **30 rows**.

---

## Step 5 — Link orders to customers (cable)

On the dashboard canvas, draw a **cable** from `customers.id` → `orders.customer_id`.

This makes every order reference a real customer ID — not a random string.

After connecting, regenerate the `orders` table. The `customer_id` column will now contain actual IDs from your customers table.

---

## Step 6 — Get your token and user ID

1. Go to your **Profile** page on smart-mock.com
2. Copy your `smt_` token (data token with read access)
3. Your `user_id` is visible in the API panel on your dashboard — it looks like `user_xxxxxxxxxxxxxxxx`

---

## Step 7 — Connect StoreBoard

Open `index.html`, paste your `user_id` and token into the config bar, and click **↻ fetch live data**.

Your dashboard is now showing live data from smart-mock.com.

---

## Optional: use the live endpoint

For data that changes on every request, use the `/live` endpoint instead:

```
GET https://smart-mock.com/users/{user_id}/tables/orders/live?token={smt_token}
```

This generates a fresh set of rows each time — useful for demos and stress testing.
