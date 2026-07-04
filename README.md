# ResourceHub - SaaS Digital Resource Marketplace

**ResourceHub** is a production-grade, highly optimized, and responsive digital asset marketplace built using Next.js (App Router), TypeScript, and Tailwind CSS. It connects dynamically to Airtable for product, lead, order, and download token storage, and incorporates Resend for automated transactional email delivery.

---

## 🛠️ Tech Stack & Architecture

* **Framework**: [Next.js 16 (App Router)](https://nextjs.org/)
* **Styling**: [Tailwind CSS v4](https://tailwindcss.com/)
* **Database**: [Airtable API](https://airtable.com/) (using official Airtable SDK with request-caching mechanisms)
* **Email Client**: [Resend SDK](https://resend.com/) (onboarding emails, download token deliveries, and purchase invoices)
* **Icons**: [Lucide React](https://lucide.dev/)
* **Type System**: [TypeScript](https://www.typescriptlang.org/) (strictly typed, zero compile warnings)
* **Analytics**: Google Analytics & Microsoft Clarity integrations

---

## ✨ Features

- 👤 **Local Account Authentication**: Pluggable client-side authentication (`AuthContext`) handling registrations, logins, profile updates, password editing, and account deletion gates.
- 📦 **Instant Filterable Products Catalog**: Dynamic search (by title, description, category, and tags) and category/price sorting that updates search parameters on the URL path.
- 🔐 **Secure Digital Product Delivery**: Generates temporary, single-use download tokens (UUIDs) valid for 24 hours. Audits download click rates to prevent unauthorized link sharing.
- 💳 **Stripe Checkout Simulation**: Simulates successful Stripe checkouts by generating order receipts and secure delivery download pages.
- ⚙️ **Admin CMS Console (`/admin`)**: Restricted dashboard displaying KPI totals, revenue performance trends (SVG area graph), and a complete product CRUD manager (creation modal, details editor, deletion triggers, and draft-to-published toggles).
- 📧 **Automated transactional emails**: Delivers welcome notifications, temporary download links, and order payment invoices.
- 🌐 **Sitemap & SEO Optimization**: Outputs valid, search-engine indexable dynamic sitemaps (`sitemap.xml`), crawler guidelines (`robots.txt`), and schema.org JSON-LD structured product tags on dynamic details routes.
- 🌓 **Dark Mode Support**: Context-driven dark mode syncing with system preferences and persisted in local storage.
- 🔌 **Resilient Offline Fallbacks**: Renders informative setup guides and warning drawers if database keys are unconfigured.

---

## 📂 Project Folder Structure

```
resource-hub/
├── DEPLOYMENT.md                    # Detailed deployment guidelines
├── README.md                        # Documentation overview
├── .env.example                     # Reference file for credentials
├── package.json                     # Configuration, scripts, and dependencies
├── src/
│   ├── types/
│   │   └── index.ts                 # TypeScript type definitions (Product, User, Category)
│   ├── data/
│   │   └── mockData.ts              # Static site categories & details configs
│   ├── context/
│   │   ├── ThemeContext.tsx         # Dark mode provider state
│   │   └── AuthContext.tsx          # Local sign-ups, log-ins, and histories database
│   ├── components/
│   │   ├── Navbar.tsx               # Responsive layout header with user states
│   │   ├── Footer.tsx               # Core footer mapping legal pages
│   │   ├── Analytics.tsx            # GA & Microsoft Clarity script injector
│   │   ├── ProductCard.tsx          # Card layout mapping tags, prices, and badges
│   │   ├── ProductCardSkeleton.tsx  # Dynamic loading state placeholder skeleton
│   │   └── CheckoutCard.tsx         # Modal capture and Stripe redirect drawer
│   ├── lib/
│   │   ├── airtable.ts              # Database client, caching, and mapping service
│   │   └── email.ts                 # Resend onboarding and receipt templates
│   └── app/
│       ├── page.tsx                 # Home landing page with featured products
│       ├── about/                   # Static site biography layout
│       ├── contact/                 # Message submission form
│       ├── products/                # Search catalog and category grids
│       ├── sitemap.ts               # sitemap.xml generator
│       ├── robots.ts                # robots.txt rules
│       ├── dashboard/               # User downloads & order records history
│       ├── settings/                # Password edit and profile deletion gates
│       ├── admin/                   # Administrator CRUD CMS console
│       ├── download/[token]/        # Token verifier & secure file download route
│       ├── privacy/                 # legal policy pages
│       ├── terms/
│       │   ├── refund/
│       │   └── cookies/
│       └── api/
│           ├── products/            # Returns published products catalog
│           ├── leads/               # Submits leads and generates free tokens
│           ├── download/            # Validates tokens and performs secure redirections
│           ├── checkout/success/    # Registers paid orders and receipt tokens
│           └── admin/products/      # REST API POST, PUT, DELETE product modifiers
```

---

## 🚀 Installation & Local Setup

### 1. Clone & Install Dependencies
```bash
git clone <repository-url>
cd resource-hub
npm install
```

### 2. Configure Environment Variables
Create a `.env.local` file at the root by copying `.env.example`:
```bash
cp .env.example .env.local
```
Fill in the database, Resend, and analytics values:
```env
# Airtable credentials
AIRTABLE_PERSONAL_ACCESS_TOKEN=your_pat_token
AIRTABLE_BASE_ID=your_base_id
AIRTABLE_TABLE_NAME=Products

# Email credentials
RESEND_API_KEY=re_your_api_key

# Analytics keys
NEXT_PUBLIC_GA_ID=G-your_ga_id
NEXT_PUBLIC_CLARITY_ID=your_clarity_id
```

### 3. Run Development Server
```bash
npm run dev
```
Open [http://localhost:3000](http://localhost:3000) to view the application locally.

### 4. Code Quality & Compilation Verification
Verify zero linter issues and successful production builds:
```bash
npm run lint    # Audits code cleanliness (returns zero errors)
npm run build   # Compiles optimized production Next.js bundle
```

---

## 💾 Airtable Table Schemas

Define four tables in your base using these exact case-sensitive schemas:

### 1. Products
- **Product Name** (`Single line text`)
- **Slug** (`Single line text`)
- **Short Description** (`Single line text`)
- **Description** (`Long text`)
- **Category** (`Single line text` / `select`)
- **Price** (`Number`)
- **Is Free** (`Checkbox`)
- **Cover Image** (`Attachment`)
- **Download Link** (`Single line text`)
- **Stripe Checkout URL** (`Single line text`)
- **Tags** (`Single line text` / `multiselect`)
- **Featured** (`Checkbox`)
- **Status** (`Single line text` - values: `Published`, `Draft`)

### 2. Leads
- **Name** (`Single line text`)
- **Email** (`Single line text`)
- **Product Name** (`Single line text`)
- **Product ID** (`Single line text`)
- **Date** (`Single line text`)

### 3. Orders
- **Product ID** (`Single line text`)
- **Product Name** (`Single line text`)
- **Customer Name** (`Single line text`)
- **Customer Email** (`Single line text`)
- **Payment Status** (`Single line text`)
- **Download Token** (`Single line text`)
- **Created At** (`Single line text`)

### 4. Downloads
- **Token** (`Single line text`)
- **Product ID** (`Single line text`)
- **Customer Email** (`Single line text`)
- **Download Count** (`Number`)
- **Last Download** (`Single line text`)
- **Expires At** (`Single line text`)

---

## 🧪 Testing Account Credentials

Log in with preconfigured credentials to test all app scopes:

### Admin CMS Account
* **Email**: `admin@resourcehub.com`
* **Password**: `admin123`
* *Grants complete CRUD access to the `/admin` CMS console.*

### Standard Tester Account
* **Email**: `user@resourcehub.com`
* **Password**: `user123`
* *Simulates purchases, reviews lead log access, and verifies download histories.*

---

## 🌐 Production Deployment

For step-by-step instructions on deploying the frontend build to **Vercel** and linking your domain names, reference the [`DEPLOYMENT.md`](./DEPLOYMENT.md) guide.

---

## 🔮 Future Improvements

1. **Stripe Webhooks**: Integrate live Stripe API checkout webhook listeners to create Airtable orders automatically on successful payments.
2. **Dynamic File Uploads**: Integrate AWS S3 or Google Cloud Storage in the Admin CRUD CMS modal to upload product file packages directly from the interface.
3. **Database Migration**: Migrate backend tables from Airtable to PostgreSQL or Supabase database engines as inventory scales beyond 10,000+ items.
