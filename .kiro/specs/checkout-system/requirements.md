# Checkout System Requirements

## Introduction

The Checkout System enables customers to complete their purchase of Lunar Essence candles through a secure, multi-step process. This system handles order processing, payment collection (demo mode), shipping information, and order confirmation while maintaining the luxury brand experience.

## Glossary

- **Checkout System**: The multi-step process for completing a purchase on the Lunar Essence website
- **Cart**: Collection of products selected by the user for purchase
- **Order Summary**: Display of items, quantities, prices, and totals for the current purchase
- **Shipping Information**: Customer's delivery address and contact details
- **Billing Information**: Customer's payment address (may differ from shipping)
- **Payment Information**: Credit card details for processing payment (demo mode with auto-fill)
- **Order Confirmation**: Final page displaying successful order completion and details
- **GCT**: General Consumption Tax (16% for Jamaica)
- **Demo Mode**: Simulated payment processing without real transactions

## Requirements

### Requirement 1

**User Story:** As a customer, I want to review my cart items before checkout, so that I can verify my selections and quantities are correct.

#### Acceptance Criteria

1. WHEN a customer accesses the checkout page, THE Checkout System SHALL display all cart items with product images, names, selected sizes, quantities, and individual prices
2. WHEN displaying cart items, THE Checkout System SHALL show the subtotal for each line item (price × quantity)
3. WHEN cart is empty, THE Checkout System SHALL redirect the customer to the products page with a message indicating the cart is empty
4. THE Checkout System SHALL provide a link to return to the cart for modifications
5. THE Checkout System SHALL calculate and display the order subtotal as the sum of all line item subtotals

### Requirement 2

**User Story:** As a customer, I want to provide my shipping information, so that my order can be delivered to the correct address.

#### Acceptance Criteria

1. THE Checkout System SHALL require the customer to enter full name, email address, phone number, street address, city, parish/state, postal code, and country
2. WHEN a customer is logged in, THE Checkout System SHALL pre-fill the email address from their account information
3. THE Checkout System SHALL validate that all shipping information fields are completed before allowing progression
4. THE Checkout System SHALL validate email format using proper email regex pattern
5. THE Checkout System SHALL validate phone number format for Jamaican phone numbers
6. THE Checkout System SHALL default the country selection to Jamaica with a dropdown for other countries

### Requirement 3

**User Story:** As a customer, I want to specify billing information, so that payment can be processed correctly.

#### Acceptance Criteria

1. THE Checkout System SHALL provide a checkbox option "Billing address same as shipping"
2. WHEN the billing checkbox is checked, THE Checkout System SHALL use shipping information for billing and hide billing form fields
3. WHEN the billing checkbox is unchecked, THE Checkout System SHALL display separate billing information fields with the same validation as shipping
4. THE Checkout System SHALL validate that all billing information fields are completed when billing differs from shipping
5. THE Checkout System SHALL maintain separate billing and shipping addresses in the order data

### Requirement 4

**User Story:** As a customer, I want to enter payment information, so that I can complete my purchase.

#### Acceptance Criteria

1. THE Checkout System SHALL display payment form fields for card holder name, card number, expiry date, and CVV
2. THE Checkout System SHALL auto-fill demo payment information (Card: 4532 1234 5678 9010, Expiry: 12/26, CVV: 123)
3. THE Checkout System SHALL display a prominent notice "Demo Mode - No real payment processed"
4. THE Checkout System SHALL validate that all payment fields are completed
5. THE Checkout System SHALL format card number input with spaces for readability (XXXX XXXX XXXX XXXX)

### Requirement 5

**User Story:** As a customer, I want to add special instructions for my order, so that I can customize my delivery or include gift messages.

#### Acceptance Criteria

1. THE Checkout System SHALL provide a checkbox option for "This is a gift"
2. WHEN the gift checkbox is checked, THE Checkout System SHALL display a textarea for gift message with 200 character limit
3. THE Checkout System SHALL provide an optional textarea for delivery instructions
4. THE Checkout System SHALL include gift message and delivery instructions in the final order data
5. THE Checkout System SHALL display character count for gift message field

### Requirement 6

**User Story:** As a customer, I want to see order totals and tax calculations, so that I understand the final cost before completing my purchase.

#### Acceptance Criteria

1. THE Checkout System SHALL calculate and display order subtotal as the sum of all cart items
2. THE Checkout System SHALL calculate GCT (General Consumption Tax) at 16% of the subtotal
3. THE Checkout System SHALL provide shipping options: Standard Shipping ($10, 5-7 business days) and Express Shipping ($25, 2-3 business days)
4. THE Checkout System SHALL calculate and display the grand total as subtotal + tax + selected shipping cost
5. THE Checkout System SHALL update totals in real-time when shipping method changes

### Requirement 7

**User Story:** As a customer, I want to review all order details before final submission, so that I can confirm everything is correct.

#### Acceptance Criteria

1. THE Checkout System SHALL display a final review step showing all order information including items, quantities, addresses, payment method, and totals
2. THE Checkout System SHALL require acceptance of Terms & Conditions via checkbox before order submission
3. THE Checkout System SHALL provide "Place Order" button that is disabled until Terms & Conditions are accepted
4. THE Checkout System SHALL provide "Cancel" button that returns to cart without processing order
5. THE Checkout System SHALL validate all previous steps before allowing order submission

### Requirement 8

**User Story:** As a customer, I want to receive order confirmation, so that I have proof of my purchase and order details.

#### Acceptance Criteria

1. WHEN order is successfully submitted, THE Checkout System SHALL generate a unique order number in format "LE-YYYYMMDD-XXXX"
2. THE Checkout System SHALL display order confirmation page with order number, order summary, shipping address, and estimated delivery date
3. THE Checkout System SHALL clear the shopping cart from localStorage after successful order completion
4. THE Checkout System SHALL provide "Continue Shopping" button that returns to products page
5. THE Checkout System SHALL store order details in localStorage for customer reference

### Requirement 9

**User Story:** As a customer, I want the checkout process to be secure and trustworthy, so that I feel confident providing my information.

#### Acceptance Criteria

1. THE Checkout System SHALL display security indicators and trust badges throughout the checkout process
2. THE Checkout System SHALL use HTTPS protocol indicators (simulated for demo)
3. THE Checkout System SHALL validate all form inputs with real-time feedback and error messages
4. THE Checkout System SHALL prevent form submission with invalid or incomplete data
5. THE Checkout System SHALL display clear progress indicators showing current step and remaining steps

### Requirement 10

**User Story:** As a customer, I want to navigate between checkout steps, so that I can review and modify information if needed.

#### Acceptance Criteria

1. THE Checkout System SHALL provide "Back" and "Continue" buttons for navigation between steps
2. THE Checkout System SHALL save form data when navigating between steps
3. THE Checkout System SHALL validate current step before allowing progression to next step
4. THE Checkout System SHALL display progress indicator showing completed, current, and remaining steps
5. THE Checkout System SHALL allow return to previous steps without losing entered data