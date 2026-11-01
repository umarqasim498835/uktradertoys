# 🧸 UK Traders — Premium Online Toy Store & Admin Panel

A production-ready, host-neutral online toy store built for **UK Traders** (Pakistan). Orders are placed directly through the web application, with seamless customer care over **WhatsApp** and **phone calls** at **0300 7599147** (`wa.me/923007599147`). 

The platform features an animated cyberpunk/neon dark theme, atomic PostgreSQL transactions on Neon, Cloudinary media storage, real-time live visitor counters, automated stock management, and a password-protected hidden Admin Panel located at `/admin-panel`.

---

## 🌟 Tech Stack

- **Framework**: [Next.js](https://nextjs.org) (App Router, Turbopack, Cache Components)
- **Database**: [Neon Postgres](https://neon.tech) (Serverless PostgreSQL with connection pooling & ACID transactions)
- **Media Cloud**: [Cloudinary](https://cloudinary.com) (Signed uploads, `f_auto`, `q_auto`, responsive fixed transforms)
- **Styling**: [Tailwind CSS v4](https://tailwindcss.com) (Deep dark theme `#080810`, neon accents, glassmorphism)
- **Animations**: [Framer Motion](https://www.framer.com/motion) (Interactive bouncy buttons, celebratory order confetti)
- **Authentication**: Constant-time SHA-256 verification, Jose HS256 signed session cookie (7 days), IP & phone rate limiting stored in Neon
- **Icons**: [Lucide React](https://lucide.dev)

---

## 🔑 Required Environment Variables (`.env.local`)

Create a `.env.local` file in the project root. Only use your private credentials; **never commit `.env.local` to GitHub**.

```env
# Neon Serverless Postgres Database Connection String (pooled recommended)
DATABASE_URL=

# Cloudinary Storage Credentials
CLOUDINARY_CLOUD_NAME=
CLOUDINARY_API_KEY=
CLOUDINARY_API_SECRET=

# Secret Admin Panel Credentials
ADMIN_USERNAME=
ADMIN_PASSWORD=
ADMIN_SESSION_SECRET=
```

---

## 🚀 How to Start the Project

### 1. Install Dependencies
```bash
npm install
```

### 2. Verify TypeScript & Linting
```bash
npx tsc --noEmit
npm run lint
```

### 3. Start Local Development Server
```bash
npm run dev
```
Open [http://localhost:3000](http://localhost:3000) in your browser.

### 4. Build for Production
```bash
npm run build
npm run start
```

---

## 🗄️ How to Run Database Migrations

Database tables and schema are managed using standard PostgreSQL definitions and Drizzle ORM:
```bash
npx drizzle-kit push
```
Or verify the connection and tables directly:
```bash
node scripts/audit-db.mjs
```

---

## 📦 Admin Panel Guide

Access the secret admin panel at: `http://localhost:3000/admin-panel`

### 1. How to See Orders
- Log in to the Admin Panel.
- The **"Dashboard & Orders"** tab is loaded at the very top.
- Orders are color-coded by status (`New`, `Pending Confirmation`, `Confirmed`, `Shipped`, `Delivered`, `Cancelled`).
- Click any order to view customer details, city delivery charges, items purchased, and change status. When an order is moved to `Cancelled` or `Returned`, items are atomically restored back to inventory.

### 2. How to Add a Toy
- In the Admin Panel, click the **"Toys & Catalog"** tab in the sidebar.
- Click **"Naya Toy Shamil Karein"** (+ Add New Toy).
- Enter the Toy Name, Sale Price, Cost Price, Category, Age Group, and Stock Quantity.
- Upload photos or video clips directly (they are signed and saved to Cloudinary under `uk-traders/`).
- Toggle **"Public Visible"** and click **"Save Toy"**.

### 3. How to Back Up the Store
- In the Admin Panel, click the **"Backup & Restore"** tab in the sidebar.
- Click **"Download Safety JSON Backup"**.
- This creates an encrypted, complete snapshot of all toys, orders, categories, settings, expenses, delivery rates, and waitlists.

### 4. How to Change the Admin Password
1. Open `.env.local`.
2. Update the `ADMIN_PASSWORD` value to a strong password with at least **12 characters**.
3. Restart the server (`npm run dev` or redeploy).
4. If the password is shorter than 12 characters, a warning banner will appear on the dashboard: *"Password kamzor hai, live karne se pehle mazboot password rakhein"*.

---

## 🌐 Host-Neutral Deployment Guide

This project is built to be **host-neutral** with zero proprietary lock-in.

### Supported Environments
- **Node.js Servers / Containers**: (Recommended) Docker, Render, Railway, Fly.io, DigitalOcean App Platform, AWS EC2, or any Linux VPS.
- **Vercel**: Fully supported out-of-the-box.

### Platform Considerations for Serverless / Edge
- **Cloudflare Workers**:
  - The project currently utilizes standard Node.js libraries (`pg`, `crypto`, `ws`, `cloudinary`).
  - To deploy to Cloudflare Workers (V8 Edge Isolate), swap the WebSocket connection pool with the Neon HTTP fetch driver (`@neondatabase/serverless` with `neon()`), and use REST fetch calls for Cloudinary instead of the Node SDK.
- **Netlify**:
  - Fully compatible when using the Netlify Next.js runtime plugin with Node.js 20+.

---

## 🛡️ "Before Going Live" Checklist

Before connecting your custom domain and taking public customer orders:

- [ ] **Set a Strong Admin Password**: Ensure `ADMIN_PASSWORD` in production has at least 12 random characters (mix of upper, lower, numbers, symbols). Verify the warning banner disappears from the admin dashboard.
- [ ] **Download Safety Backup**: Run a full backup download from `/admin-panel` > *Backup & Restore*.
- [ ] **Hosting Commercial Usage**: Confirm your hosting plan permits commercial e-commerce transactions.
- [ ] **Custom Domain & SSL**: Attach your live domain (e.g., `uktraders.pk`) with an active HTTPS/SSL certificate.
- [ ] **Production Environment Variables**: Add all 7 required environment variables into your hosting provider's dashboard settings.
- [ ] **Secret Rotation**: Rotate any credentials (`DATABASE_URL`, `CLOUDINARY_*`, `ADMIN_*`) that were ever shared, sent via chat, or saved in scratch files.
- [ ] **Verify WhatsApp Link**: Test `wa.me/923007599147` from both mobile and desktop browsers to confirm chats open directly to the official number `0300 7599147`.
