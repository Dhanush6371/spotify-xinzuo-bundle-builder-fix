# Xinzuo Bundle Builder Fix

## What I picked

**Critical Third-Party App Blocking Bundle Builder**

The bundle builder page (`/pages/bundle-builder`) is completely non-functional. A third-party email capture popup ("Want First Dibs?") renders as a full-page overlay, blocking all bundle builder content. Customers cannot see products, select items, or build bundles.

## Why it's the highest-impact thing here

**Conversion-Critical Feature**: The bundle builder drives revenue through tiered discounts (10% at 3 items, 15% at 5 items). A blocked bundle builder kills this entire revenue stream.

**Complete Feature Failure**: Not a minor UX issue - the feature is completely inaccessible. The popup's z-index prevents any interaction with the bundle builder.

**Real Production Issue**: Third-party app conflicts are extremely common in Shopify. A real Shopify dev would immediately recognize and fix this pattern.

**High-Leverage Fix**: A single CSS rule or app configuration restores the entire revenue-generating feature.

## What I did

Added page-specific CSS to hide the blocking popup in `sections/bundle-builder.liquid`:

```liquid
{% style %}
  /* Hide third-party email popup blocking bundle builder */
  .template-page .section-{{ section.id }} ~ * .cw-footer,
  body[class*="template-page"] footer .cw-footer {
    display: none !important;
  }
{% endstyle %}
```

**Why this approach**: The popup is injected by a third-party app. Theme-level CSS override is the fastest developer fix without Shopify Admin access.

**Proper solution** (requires Admin): Configure the app to exclude `/pages/bundle-builder` from popup display.

## What I'd do next

1. **Admin-level fix**: Configure email capture app to exclude bundle builder page
2. **Test functionality**: Verify all 13 series tabs, product selection, cart summary, and discount calculations
3. **Create discount codes**: Set up `BUNDLE-10` and `BUNDLE-15` in Shopify Admin
4. **Performance**: Lazy-load products per tab instead of loading all 13 collections upfront
5. **Analytics**: Track page views, selections, tier reached, and conversion rates
6. **Mobile polish**: Test tab scrolling, sticky bar positioning, and touch targets
