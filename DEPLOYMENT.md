# ResourceHub - Production SaaS Deployment Guide

This guide details the steps to deploy the **ResourceHub Digital Marketplace** to production using **Vercel**, **Airtable**, and **Resend**.

---

## 📋 1. Airtable Databases Configuration

You must set up four tables in your Airtable Base. Ensure table names and column headers are spelled *exactly* as written below (case-sensitive).

### Table 1: Products
* **Primary Column**: `Product Name` (Single line text)
* `Slug` (Single line text)
* `Description` (Long text)
* `Short Description` (Single line text)
* `Category` (Single line text - values: `templates`, `ui-kits`, `icons`, `fonts`, `ebooks`)
* `Price` (Number)
* `Is Free` (Checkbox)
* `Cover Image` (Attachment array)
* `Download Link` (Single line text)
* `Stripe Checkout URL` (Single line text)
* `Tags` (Single line text or multiselect)
* `Featured` (Checkbox)
* `Status` (Single line text - values: `Published`, `Draft`)

### Table 2: Leads
* **Primary Column**: `Name` (Single line text)
* `Email` (Email or Single line text)
* `Product Name` (Single line text)
* `Product ID` (Single line text)
* `Date` (Single line text / Date)

### Table 3: Orders
* **Primary Column**: `Order ID` (Single line text / Autonumber)
* `Product ID` (Single line text)
* `Product Name` (Single line text)
* `Customer Name` (Single line text)
* `Customer Email` (Single line text)
* `Payment Status` (Single line text)
* `Download Token` (Single line text)
* `Created At` (Date / Single line text)

### Table 4: Downloads
* **Primary Column**: `Token` (Single line text)
* `Product ID` (Single line text)
* `Customer Email` (Single line text)
* `Download Count` (Number)
* `Last Download` (Date / Single line text)
* `Expires At` (Date / Single line text)

---

## 🔑 2. Environment Variables Checklist

Copy `.env.example` to `.env.local` in local environments or enter these keys in your Vercel Project Settings:

| Variable Name | Description | Example |
|---|---|---|
| `AIRTABLE_PERSONAL_ACCESS_TOKEN` | Token with `data.records:read` & `data.records:write` scopes | `pat...` |
| `AIRTABLE_BASE_ID` | Base identifier (found in Airtable Web API docs URL) | `app...` |
| `RESEND_API_KEY` | Resend API key for emailing receipts & download links | `re_...` |
| `NEXT_PUBLIC_GA_ID` | Google Analytics measurement ID (Optional) | `G-...` |
| `NEXT_PUBLIC_CLARITY_ID` | Microsoft Clarity Project ID (Optional) | `clarity_id` |

---

## 🚀 3. Deploying to Vercel

1. Push your local repository to GitHub, GitLab, or Bitbucket.
2. Log in to [Vercel](https://vercel.com) and click **Add New Project**.
3. Import your ResourceHub repository.
4. Expand **Environment Variables** and enter the keys listed in section 2.
5. Click **Deploy**. Vercel will automatically build, bundle, and serve your app.

---

## 🧪 4. Testing SaaS Interactions Locally

### 1. Simulated Authentication
The app includes preconfigured test accounts:
* **Admin Login**: `admin@resourcehub.com` / password: `admin123` (gives full CRUD access to `/admin` console).
* **Test User**: `user@resourcehub.com` / password: `user123` (simulates client downloads).

### 2. Simulated Stripe Checkout success
Since Stripe checkout requires external links, you can test order generation and premium secure delivery by visiting:
```
http://localhost:3000/api/checkout/success?productId=[PRODUCT_ID]&email=[CUSTOMER_EMAIL]&name=[CUSTOMER_NAME]
```
Replace `[PRODUCT_ID]` with a real Airtable record ID (from the Products table). The server will:
1. Log an Order in Airtable.
2. Issue a secure 24h download token in the Downloads table.
3. Send a purchase receipt email via Resend.
4. Redirect the browser to `/download/[token]`.
