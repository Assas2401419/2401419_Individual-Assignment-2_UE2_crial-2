# Checkout System Design

## Overview

The Checkout System implements a multi-step, progressive disclosure approach that guides customers through order completion while maintaining the Lunar Essence luxury brand experience. The system uses client-side validation, localStorage for data persistence, and provides a seamless flow from cart review to order confirmation.

## Architecture

### System Flow
```
Cart → Checkout Entry → Shipping Info → Billing Info → Payment → Review → Confirmation
```

### Data Flow
1. **Input**: Cart data from localStorage (`lunarEssence_cart`)
2. **Processing**: Form validation, calculations, step management
3. **Output**: Order confirmation and localStorage updates
4. **Storage**: Form data persistence between steps, final order storage

### Technology Stack
- **Frontend**: Vanilla HTML5, CSS3, JavaScript (ES6+)
- **Storage**: Browser localStorage for cart, user data, and order persistence
- **Validation**: Client-side form validation with real-time feedback
- **Calculations**: JavaScript arithmetic for totals, tax, shipping

## Components and Interfaces

### 1. Checkout Controller (`checkout.js`)
**Purpose**: Main orchestrator for checkout process

**Key Methods**:
```javascript
class CheckoutController {
  constructor()
  initializeCheckout()
  loadCartData()
  validateStep(stepNumber)
  navigateToStep(stepNumber)
  calculateTotals()
  processOrder()
  generateOrderNumber()
  showConfirmation(orderData)
}
```

**Responsibilities**:
- Step navigation and validation
- Form data persistence
- Order calculations
- Order processing and confirmation

### 2. Form Validator
**Purpose**: Handles all form validation logic

**Key Methods**:
```javascript
class FormValidator {
  validateEmail(email)
  validatePhone(phone)
  validateRequired(fieldValue)
  validateCardNumber(cardNumber)
  validateExpiryDate(expiry)
  showFieldError(fieldId, message)
  clearFieldError(fieldId)
}
```

### 3. Order Calculator
**Purpose**: Handles all monetary calculations

**Key Methods**:
```javascript
class OrderCalculator {
  calculateSubtotal(cartItems)
  calculateTax(subtotal, taxRate = 0.16)
  calculateShipping(shippingMethod)
  calculateTotal(subtotal, tax, shipping)
  formatCurrency(amount)
}
```

### 4. Progress Indicator Component
**Purpose**: Visual step progress tracking

**Structure**:
```html
<div class="checkout-progress">
  <div class="progress-step completed">1. Shipping</div>
  <div class="progress-step active">2. Billing</div>
  <div class="progress-step">3. Payment</div>
  <div class="progress-step">4. Review</div>
</div>
```

## Data Models

### Checkout Session Object
```javascript
const checkoutSession = {
  currentStep: 1,
  cartItems: [], // From localStorage
  shippingInfo: {
    fullName: '',
    email: '',
    phone: '',
    street: '',
    city: '',
    parish: '',
    postalCode: '',
    country: 'Jamaica'
  },
  billingInfo: {
    sameAsShipping: true,
    // Same fields as shipping if different
  },
  paymentInfo: {
    cardHolder: '',
    cardNumber: '4532 1234 5678 9010', // Demo auto-fill
    expiryDate: '12/26', // Demo auto-fill
    cvv: '123' // Demo auto-fill
  },
  specialOptions: {
    isGift: false,
    giftMessage: '',
    deliveryInstructions: ''
  },
  orderTotals: {
    subtotal: 0,
    tax: 0,
    shipping: 10, // Default standard shipping
    total: 0
  },
  shippingMethod: 'standard' // 'standard' or 'express'
}
```

### Order Object (Final)
```javascript
const order = {
  orderNumber: 'LE-20251105-1234',
  orderDate: '2025-11-05T15:30:00Z',
  customerId: null, // If logged in
  items: [
    {
      productId: 'prod-001',
      productName: 'New Moon Essence',
      collection: 'New Moon',
      selectedSize: 'Medium Jar',
      quantity: 2,
      unitPrice: 32.00,
      subtotal: 64.00,
      image: 'assets/images/products/new-moon-small.jpg'
    }
  ],
  shippingAddress: { /* shipping info */ },
  billingAddress: { /* billing info */ },
  paymentMethod: 'Credit Card (Demo)',
  giftMessage: '',
  deliveryInstructions: '',
  orderTotals: {
    subtotal: 64.00,
    tax: 10.24,
    shipping: 10.00,
    total: 84.24
  },
  shippingMethod: 'standard',
  status: 'confirmed'
}
```

## Error Handling

### Form Validation Errors
- **Real-time validation**: Show errors as user types or leaves fields
- **Error styling**: Red borders, error icons, descriptive messages
- **Error prevention**: Disable "Continue" buttons until validation passes

### System Errors
- **Empty cart**: Redirect to products page with message
- **Storage errors**: Graceful fallback with user notification
- **Calculation errors**: Default to safe values, log errors

### Error Message Examples
```javascript
const errorMessages = {
  required: 'This field is required',
  email: 'Please enter a valid email address',
  phone: 'Please enter a valid phone number (876-XXX-XXXX)',
  cardNumber: 'Please enter a valid card number',
  expiry: 'Please enter a valid expiry date (MM/YY)',
  cvv: 'Please enter a valid 3-digit CVV',
  terms: 'You must accept the Terms & Conditions to continue'
}
```

## Testing Strategy

### Unit Testing Focus Areas
1. **Form Validation Functions**
   - Email format validation
   - Phone number validation
   - Required field validation
   - Card number formatting

2. **Calculation Functions**
   - Subtotal calculations
   - Tax calculations (16% GCT)
   - Shipping cost application
   - Total calculations

3. **Data Persistence**
   - localStorage save/load operations
   - Form data persistence between steps
   - Cart data integrity

### Integration Testing
1. **Step Navigation Flow**
   - Forward navigation with validation
   - Backward navigation with data retention
   - Progress indicator updates

2. **Order Processing**
   - Complete checkout flow
   - Order confirmation generation
   - Cart clearing after completion

### User Experience Testing
1. **Form Usability**
   - Tab navigation through forms
   - Error message clarity
   - Auto-fill functionality

2. **Responsive Design**
   - Mobile checkout experience
   - Touch-friendly interactions
   - Readable text and buttons

## User Interface Design

### Layout Structure

#### Desktop Layout (2-Column)
```
┌─────────────────────────────────────────────────────────┐
│                    Progress Bar                          │
├─────────────────────────────┬───────────────────────────┤
│                             │     Order Summary         │
│        Checkout Form        │   ┌─────────────────────┐ │
│                             │   │ Items: 2            │ │
│   ┌─────────────────────┐   │   │ Subtotal: $64.00    │ │
│   │ Shipping Info       │   │   │ Tax: $10.24         │ │
│   │ [Full Name]         │   │   │ Shipping: $10.00    │ │
│   │ [Email]             │   │   │ Total: $84.24       │ │
│   │ [Phone]             │   │   └─────────────────────┘ │
│   │ [Address Fields]    │   │                           │
│   └─────────────────────┘   │   Trust Indicators        │
│                             │   🔒 Secure Checkout      │
│   [Back] [Continue]         │   📞 Need Help?           │
└─────────────────────────────┴───────────────────────────┘
```

#### Mobile Layout (Stacked)
```
┌─────────────────────────────┐
│       Progress Bar          │
├─────────────────────────────┤
│     Order Summary           │
│   Items: 2 | Total: $84.24 │
├─────────────────────────────┤
│                             │
│      Checkout Form          │
│                             │
│   ┌─────────────────────┐   │
│   │ Shipping Info       │   │
│   │ [Full Name]         │   │
│   │ [Email]             │   │
│   │ [Phone]             │   │
│   │ [Address Fields]    │   │
│   └─────────────────────┘   │
│                             │
│   [Back] [Continue]         │
└─────────────────────────────┘
```

### Step-by-Step Screens

#### Step 1: Order Review
- Display cart items with images, names, sizes, quantities
- Show line item subtotals
- "Edit Cart" link back to cart page
- Order subtotal calculation

#### Step 2: Shipping Information
- Form fields for delivery address
- Email pre-filled if logged in
- Country dropdown (default: Jamaica)
- Real-time validation feedback

#### Step 3: Billing Information
- "Same as shipping" checkbox (default checked)
- Conditional billing form display
- Same validation as shipping

#### Step 4: Payment Information
- Payment form with demo auto-fill
- Prominent "Demo Mode" notice
- Card number formatting (XXXX XXXX XXXX XXXX)
- Security indicators

#### Step 5: Special Options
- Gift checkbox and message textarea
- Delivery instructions textarea
- Character count for gift message (200 max)

#### Step 6: Review & Confirmation
- Complete order summary
- All addresses and payment method
- Terms & Conditions checkbox
- "Place Order" button (disabled until terms accepted)

#### Step 7: Order Confirmation
- Order number display
- Complete order details
- Estimated delivery information
- "Continue Shopping" button

### Visual Design Elements

#### Color Scheme (From Masterplan)
- **Primary Actions**: Soft Gold (#C9A961)
- **Secondary Actions**: Deep Teal (#4A6B6B)
- **Backgrounds**: Cream/Beige (#E8DCC4)
- **Text**: Deep Navy (#2C3E50)
- **Success**: Light Green
- **Error**: Soft Red
- **Warning**: Amber

#### Typography
- **Headings**: Playfair Display (serif)
- **Body Text**: Montserrat (sans-serif)
- **Form Labels**: Montserrat Medium
- **Button Text**: Montserrat SemiBold

#### Interactive Elements
- **Form Inputs**: 
  - Border: 1px solid #E0E0E0
  - Focus: Deep Teal border with subtle shadow
  - Error: Red border with error icon
  - Padding: 12px 16px
  - Border-radius: 6px

- **Buttons**:
  - Primary: Gold background, white text, 8px border-radius
  - Secondary: Transparent background, teal border and text
  - Hover: Slight scale (1.05) and color darkening
  - Disabled: 50% opacity, no hover effects

- **Progress Indicator**:
  - Completed steps: Gold background, white text
  - Current step: Teal background, white text
  - Future steps: Light gray background, dark text
  - Connecting lines between steps

#### Responsive Breakpoints
- **Mobile**: 0-599px (single column, stacked layout)
- **Tablet**: 600-949px (adjusted spacing, larger touch targets)
- **Desktop**: 950px+ (two-column layout with sidebar)

### Accessibility Considerations
- **Semantic HTML**: Proper form labels, fieldsets, legends
- **ARIA Labels**: Screen reader support for progress indicators
- **Keyboard Navigation**: Tab order through all interactive elements
- **Color Contrast**: WCAG AA compliance for all text
- **Focus Indicators**: Clear visual focus states
- **Error Announcements**: Screen reader accessible error messages