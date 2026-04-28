# Security Specification for GameMarket

## Data Invariants
- An account cannot be created without a valid sellerId matching the authenticated user.
- An account's status can only be changed to 'sold' if it is currently 'available'.
- Only the seller can update an account's details (except for status changes during an order).
- Users can only read their own private data (profile) and everyone's public listings.
- Orders must link a buyer and seller, and the accountId must exist and be 'available'.

## The "Dirty Dozen" Payloads (Targeting Rejection)

1. **Identity Injection**: Try to create an account with a `sellerId` that doesn't match the auth UID.
2. **Status Escalation**: Try to update an account status to 'sold' directly without an order.
3. **Ghost Field**: Adding `isVerified: true` to a user profile.
4. **Price Poisoning**: Setting a negative price for an account.
5. **Unauthorized Edit**: Trying to edit another user's account listing.
6. **Self-Rating**: Trying to update own rating in user profile.
7. **Order Spoofing**: Creating an order where `buyerId` is someone else.
8. **Resource Exhaustion**: Sending a 2MB string as an account description.
9. **Invalid Game**: Sending a game name that is a script tag.
10. **Admin Spoof**: Adding `isAdmin: true` to a user profile update.
11. **Status Bypass**: Changing an order status from 'cancelled' back to 'completed'.
12. **Public PII**: Reading another user's email if it's considered private.

## Test Runner Plan
- Implement `firestore.rules.test.ts` using `@firebase/rules-unit-testing`. (Note: I might not be able to run vitest/jest if not configured, but I will write the rules following the patterns).
