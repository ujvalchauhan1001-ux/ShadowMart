# ShadowMart# ShadowMart

A premium modern e-commerce platform built with:

- Next.js
- React
- Tailwind CSS
- Node.js
- Express.js
- MongoDB
- JWT Authentication
- Razorpay
- Cloudinary

## Features

- User Authentication
- Product Catalog
- Cart & Wishlist
- Admin Dashboard
- Order Management
- Coupon System
- Payment Gateway
- Responsive Design

Author: Ujvalnode_modules
.next
.env
.env.local
dist
build
coverage
.vscode
.DS_Store{
  "name": "shadowmart",
  "private": true,
  "workspaces": [
    "client",
    "server"
  ],
  "scripts": {
    "client": "npm run dev --workspace client",
    "server": "npm run dev --workspace server",
    "dev": "concurrently \"npm run client\" \"npm run server\""
  },
  "devDependencies": {
    "concurrently": "^9.0.1"
  }
}
