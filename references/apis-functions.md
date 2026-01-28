# Shopify APIs & Functions

## Table of Contents
- [Storefront API](#storefront-api)
- [Admin API](#admin-api)
- [Shopify Functions](#shopify-functions)
- [Checkout Extensions](#checkout-extensions)
- [Hydrogen Components](#hydrogen-components)

## Storefront API

### GraphQL Product Query
```graphql
query ProductByHandle($handle: String!) {
  productByHandle(handle: $handle) {
    id
    title
    description
    priceRange {
      minVariantPrice {
        amount
        currencyCode
      }
    }
    variants(first: 10) {
      edges {
        node {
          id
          title
          availableForSale
          price {
            amount
            currencyCode
          }
        }
      }
    }
    metafields(identifiers: [
      {namespace: "custom", key: "subtitle"},
      {namespace: "features", key: "highlights"}
    ]) {
      key
      value
      type
    }
  }
}
```

### Collection with Products
```graphql
query CollectionProducts($handle: String!, $first: Int!) {
  collectionByHandle(handle: $handle) {
    title
    products(first: $first) {
      edges {
        node {
          id
          title
          handle
          featuredImage {
            url
            altText
          }
          priceRange {
            minVariantPrice {
              amount
              currencyCode
            }
          }
        }
      }
      pageInfo {
        hasNextPage
        endCursor
      }
    }
  }
}
```

### Cart Operations (AJAX)
```javascript
// Add to cart
fetch('/cart/add.js', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({
    items: [{ id: variantId, quantity: 1 }]
  })
});

// Update cart
fetch('/cart/update.js', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({ updates: { [variantId]: newQty } })
});

// Get cart
fetch('/cart.js').then(r => r.json());
```

## Admin API

### Metafield Management (GraphQL)
```graphql
mutation MetafieldsSet($metafields: [MetafieldsSetInput!]!) {
  metafieldsSet(metafields: $metafields) {
    metafields {
      key
      value
    }
    userErrors {
      field
      message
    }
  }
}

# Variables
{
  "metafields": [{
    "ownerId": "gid://shopify/Product/123456",
    "namespace": "custom",
    "key": "subtitle",
    "type": "single_line_text_field",
    "value": "Best seller"
  }]
}
```

### Product Update
```graphql
mutation ProductUpdate($input: ProductInput!) {
  productUpdate(input: $input) {
    product {
      id
      title
    }
    userErrors {
      field
      message
    }
  }
}
```

## Shopify Functions

### Discount Function
```javascript
// functions/discount.js
// Must execute in <5ms
export default function(input) {
  const config = JSON.parse(input.functionConfiguration);
  const discount = config.discountPercentage;

  return {
    discounts: [{
      value: {
        percentage: { value: discount }
      },
      targets: [{
        productVariant: {
          id: input.cart.lines[0].merchandise.id
        }
      }]
    }]
  };
}
```

### Delivery Customization
```javascript
export default function(input) {
  const config = JSON.parse(input.functionConfiguration);

  return {
    operations: input.cart.deliveryGroups.flatMap(group =>
      group.deliveryOptions.map(option => ({
        rename: {
          deliveryOptionHandle: option.handle,
          title: option.title.includes('Express')
            ? `🚀 ${option.title}`
            : option.title
        }
      }))
    )
  };
}
```

### Cart Transform (Bundles)
```javascript
export default function(input) {
  const bundleLines = input.cart.lines.filter(
    line => line.merchandise.__typename === 'ProductVariant' &&
            line.merchandise.product.hasTag('bundle-parent')
  );

  return {
    operations: bundleLines.map(line => ({
      expand: {
        cartLineId: line.id,
        expandedCartItems: getBundleItems(line)
      }
    }))
  };
}
```

## Checkout Extensions

### Checkout UI Extension
```jsx
import {
  reactExtension,
  BlockStack,
  Text,
  Button,
  useApplyDiscountCodeChange,
} from '@shopify/ui-extensions-react/checkout';

export default reactExtension(
  'purchase.checkout.block.render',
  () => <PromoBlock />
);

function PromoBlock() {
  const applyDiscount = useApplyDiscountCodeChange();

  return (
    <BlockStack>
      <Text>Special offer available!</Text>
      <Button
        onPress={() => applyDiscount({
          type: 'addDiscountCode',
          code: 'SPECIAL10'
        })}
      >
        Apply 10% Discount
      </Button>
    </BlockStack>
  );
}
```

### Order Status Extension
```jsx
import {
  reactExtension,
  Banner,
  useOrder,
} from '@shopify/ui-extensions-react/customer-account';

export default reactExtension(
  'customer-account.order-status.block.render',
  () => <ShippingInfo />
);

function ShippingInfo() {
  const order = useOrder();

  return (
    <Banner title="Shipping Update">
      Your order is being prepared for shipment.
    </Banner>
  );
}
```

## Hydrogen Components

### Product Card
```jsx
import { Money, Image } from '@shopify/hydrogen';
import { Link } from '@remix-run/react';

export function ProductCard({ product }) {
  return (
    <Link to={`/products/${product.handle}`}>
      <Image
        data={product.featuredImage}
        aspectRatio="1/1"
        sizes="(min-width: 45em) 20vw, 50vw"
      />
      <h3>{product.title}</h3>
      <Money data={product.priceRange.minVariantPrice} />
    </Link>
  );
}
```

### Cart with Optimistic UI
```jsx
import { useFetcher } from '@remix-run/react';
import { CartForm } from '@shopify/hydrogen';

export function AddToCartButton({ variantId }) {
  return (
    <CartForm
      route="/cart"
      action={CartForm.ACTIONS.LinesAdd}
      inputs={{ lines: [{ merchandiseId: variantId, quantity: 1 }] }}
    >
      <button type="submit">Add to Cart</button>
    </CartForm>
  );
}
```

## Official Documentation Links

Always verify against current docs:
- **Liquid**: https://shopify.dev/docs/api/liquid
- **Storefront API**: https://shopify.dev/docs/api/storefront
- **Admin API**: https://shopify.dev/docs/api/admin-graphql
- **Functions**: https://shopify.dev/docs/api/functions
- **Checkout Extensions**: https://shopify.dev/docs/api/checkout-ui-extensions
- **Hydrogen**: https://shopify.dev/docs/custom-storefronts/hydrogen
- **Changelog**: https://changelog.shopify.com
