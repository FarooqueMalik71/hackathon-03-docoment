# Hackathon-03-Docoment
# General E-commerce website
Bandage Online Shopping Platform
Author: Muhammad Farooque
Role: Rising Star, Bandage Online Shopping

Overview
This document outlines the technical foundation and enhanced workflow for the Bandage Online Shopping Platform, specifically designed for wholesalers dealing in stitched kurtis. The platform aims to streamline inventory management and facilitate seamless bulk purchasing experiences for wholesalers.

System Architecture
Tech Stack
Frontend: Next.js

Backend: Sanity CMS

API Integration: External APIs for data import and shipment tracking

## Document Structure
1. [Business Goals](#business-goals)
2. [Data Schema Breakdown](#data-schema-breakdown)
3. [Workflow Diagram](#workflow-diagram)
4. [Code Snippets](#code-snippets)

## Business Goals

### 1. Solve Inventory Management
Offer a platform for wholesalers to manage and display their stitched kurtis efficiently.

### 2. Target Audience
- Wholesalers
- Retailers
- Small shop owners

### 3. Key Differentiators
- Real-time stock updates for accurate availability.
- Customization options for wholesalers to curate their collections.
- A cost-effective platform to manage and display products.

## Data Schema Breakdown

### Products
- **ID:** Unique identifier for each product.
- **Name:** Name of the product.
- **Category:** Category of the product (e.g., Kurtis).
- **Details:** Detailed description of the product.
- **Price:** Price of the product.
- **Stock:** Availability of the product.
- **Image:** URL of the product image.
- **Description:** Short description of the product.

### Orders
- **Order ID:** Unique identifier for each order.
- **Customer Info:** Information about the customer (e.g., name, email, phone number).
- **Product Details:** Details of the products ordered.
- **Status:** Order status (e.g., pending, shipped, delivered).
- **Timestamp:** Date and time of the order.

### Customers
- **ID:** Unique identifier for each customer.
- **Name:** Name of the customer.
- **Contact Info:** Contact information (e.g., email, phone number).
- **Address:** Shipping address.
- **Order History:** List of past orders.

### Delivery Zones
- **Zone Name:** Name of the delivery zone.
- **Coverage Area:** Area covered by the zone.
- **Assigned Drivers:** List of drivers assigned to the zone.

### Shipments
- **Shipment ID:** Unique identifier for each shipment.
- **Order ID:** ID of the associated order.
- **Status:** Shipment status (e.g., pending, in transit, delivered).
- **Delivery Date:** Expected delivery date.

## Workflow Diagram
Here’s a simple workflow diagram for the user journey:

User -> Select Product -> Add to Cart -> Proceed to Checkout -> Generate Order ID -> Submit Basic Info -> Order Confirmation -> Payment -> Shipping API -> Order Confirmation Page

## Code Snippets

### Sanity Schema Example
```javascript
// schemas/product.js
export default {
  name: 'product',
  title: 'Product',
  type: 'document',
  fields: [
    {
      name: 'name',
      title: 'Product Name',
      type: 'string',
    },
    {
      name: 'category',
      title: 'Category',
      type: 'string',
    },
    {
      name: 'price',
      title: 'Price',
      type: 'number',
    },
    {
      name: 'stock',
      title: 'Stock',
      type: 'number',
    },
    {
      name: 'image',
      title: 'Product Image',
      type: 'image',
    },
    {
      name: 'description',
      title: 'Description',
      type: 'text',
    },
  ],
};
```

### Next.js Page: Add to Cart
```javascript
// pages/product/[id].js
import { useState, useEffect } from 'react';
import { useRouter } from 'next/router';
import { client } from '../../lib/sanityClient';

export default function ProductDetail() {
  const router = useRouter();
  const { id } = router.query;
  const [product, setProduct] = useState(null);

  useEffect(() => {
    if (id) {
      client
        .fetch(`*[_id == "${id}"]`)
        .then((data) => setProduct(data[0]))
        .catch((err) => console.error(err));
    }
  }, [id]);

  if (!product) return <div>Loading...</div>;

  const addToCart = () => {
    // Add to cart logic here
    console.log('Added to cart:', product);
  };

  return (
    <div>
      <h1>{product.name}</h1>
      <img src={product.image.asset.url} alt={product.name} />
      <p>{product.description}</p>
      <button onClick={addToCart}>Add to Cart</button>
    </div>
  );
}
```

### Next.js Page: Checkout
```javascript
// pages/checkout.js
import { useState } from 'react';

export default function Checkout() {
  const [orderID, setOrderID] = useState('');
  const [customerInfo, setCustomerInfo] = useState({
    name: '',
    email: '',
    phone: '',
    address: '',
  });

  const handleInputChange = (e) => {
    setCustomerInfo({ ...customerInfo, [e.target.name]: e.target.value });
  };

  const submitOrder = () => {
    // Submit order logic here
    console.log('Order ID:', orderID);
    console.log('Customer Info:', customerInfo);
    // Call API to process order
  };

  return (
    <div>
      <h1>Checkout</h1>
      <input
        type="text"
        name="name"
        placeholder="Name"
        value={customerInfo.name}
        onChange={handleInputChange}
      />
      <input
        type="email"
        name="email"
        placeholder="Email"
        value={customerInfo.email}
        onChange={handleInputChange}
      />
      <input
        type="text"
        name="phone"
        placeholder="Phone"
        value={customerInfo.phone}
        onChange={handleInputChange}
      />
      <input
        type="text"
        name="address"
        placeholder="Address"
        value={customerInfo.address}
        onChange={handleInputChange}
      />
      <button onClick={submitOrder}>Submit Order</button>
    </div>
  );
}
