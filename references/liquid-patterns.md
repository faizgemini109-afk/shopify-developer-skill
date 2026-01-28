# Liquid Patterns & Performance

## Table of Contents
- [Performance Optimization](#performance-optimization)
- [Metafield Patterns](#metafield-patterns)
- [Metaobject Patterns](#metaobject-patterns)
- [Loop Patterns](#loop-patterns)
- [Conditional Patterns](#conditional-patterns)
- [Image Optimization](#image-optimization)

## Performance Optimization

### Minimize Liquid Loops
```liquid
{% comment %} Bad: Nested loops {% endcomment %}
{% for product in collection.products %}
  {% for variant in product.variants %}
    ...
  {% endfor %}
{% endfor %}

{% comment %} Better: Use limits and where filter {% endcomment %}
{% assign available = product.variants | where: 'available', true %}
{% for variant in available limit: 5 %}
  {{ variant.price | money }}
{% endfor %}
```

### Use Native Filters Over JavaScript
```liquid
{% comment %} Prefer Liquid filters {% endcomment %}
{{ product.price | money_with_currency }}
{{ product.title | truncate: 50 }}
{{ collection.products | size }}

{% comment %} Avoid: Custom JS for simple formatting {% endcomment %}
```

### Cache Expensive Lookups
```liquid
{% comment %} Assign once, use multiple times {% endcomment %}
{% assign featured_image = product.featured_image %}
{% if featured_image %}
  {{ featured_image | image_url: width: 300 | image_tag }}
  {{ featured_image | image_url: width: 600 | image_tag }}
{% endif %}
```

## Metafield Patterns

### Basic Access
```liquid
{{ product.metafields.custom.field_key.value }}
{{ collection.metafields.namespace.key.value }}
{{ page.metafields.custom.content.value }}
```

### Safe Access with Defaults
```liquid
{% assign subtitle = product.metafields.custom.subtitle.value | default: '' %}
{% if subtitle != blank %}
  <p class="subtitle">{{ subtitle }}</p>
{% endif %}
```

### List Metafields (2024+)
```liquid
{% if product.metafields.features.highlights %}
  <ul class="product-highlights">
    {% for highlight in product.metafields.features.highlights.value %}
      <li>{{ highlight }}</li>
    {% endfor %}
  </ul>
{% endif %}
```

### JSON Metafields
```liquid
{% assign specs = product.metafields.custom.specifications.value %}
{% if specs %}
  <dl class="specs">
    {% for spec in specs %}
      <dt>{{ spec.label }}</dt>
      <dd>{{ spec.value }}</dd>
    {% endfor %}
  </dl>
{% endif %}
```

### File Reference Metafields
```liquid
{% assign pdf = product.metafields.custom.manual %}
{% if pdf %}
  <a href="{{ pdf.value }}" download>Download Manual (PDF)</a>
{% endif %}
```

## Metaobject Patterns

### Basic Iteration
```liquid
{% for item in shop.metaobjects.testimonials.values %}
  <div class="testimonial">
    <h3>{{ item.title }}</h3>
    <p>{{ item.content }}</p>
    {% if item.image %}
      {{ item.image | image_url: width: 300 | image_tag }}
    {% endif %}
  </div>
{% endfor %}
```

### Product-Linked Metaobject
```liquid
{% assign author = product.metafields.custom.author.value %}
{% if author %}
  <div class="author-bio">
    <h4>{{ author.name }}</h4>
    <p>{{ author.bio }}</p>
    {% if author.photo %}
      {{ author.photo | image_url: width: 100 | image_tag: class: 'author-photo' }}
    {% endif %}
  </div>
{% endif %}
```

## Loop Patterns

### Pagination
```liquid
{% paginate collection.products by 12 %}
  {% for product in collection.products %}
    {% render 'product-card', product: product %}
  {% endfor %}

  {% if paginate.pages > 1 %}
    {% render 'pagination', paginate: paginate %}
  {% endif %}
{% endpaginate %}
```

### Loop with Index
```liquid
{% for block in section.blocks %}
  <div class="block block-{{ forloop.index }}" {{ block.shopify_attributes }}>
    {% case block.type %}
      {% when 'text' %}
        {{ block.settings.text }}
      {% when 'image' %}
        {{ block.settings.image | image_url: width: 400 | image_tag }}
    {% endcase %}
  </div>
{% endfor %}
```

### Cycle for Alternating Styles
```liquid
{% for product in collection.products %}
  <div class="product {% cycle 'odd', 'even' %}">
    {{ product.title }}
  </div>
{% endfor %}
```

## Conditional Patterns

### Product Availability
```liquid
{% if product.available %}
  {% if product.compare_at_price > product.price %}
    <span class="sale-badge">Sale</span>
  {% endif %}
  <button type="submit">Add to Cart</button>
{% else %}
  <button disabled>Sold Out</button>
{% endif %}
```

### Customer-Specific Content
```liquid
{% if customer %}
  <p>Welcome back, {{ customer.first_name }}!</p>
  {% if customer.tags contains 'wholesale' %}
    {% render 'wholesale-pricing', product: product %}
  {% endif %}
{% else %}
  <a href="/account/login">Log in for wholesale pricing</a>
{% endif %}
```

### B2B Pricing
```liquid
{% if customer.b2b? %}
  <div class="wholesale-pricing">
    {% for tier in product.volume_pricing_tiers %}
      <div class="tier">
        <span>{{ tier.minimum_quantity }}+ units:</span>
        <span>{{ tier.price | money }}</span>
      </div>
    {% endfor %}
  </div>
{% endif %}
```

## Image Optimization

### Responsive Images
```liquid
{{ product.featured_image | image_url: width: 600 | image_tag:
  loading: 'lazy',
  sizes: '(min-width: 768px) 50vw, 100vw',
  widths: '300, 600, 900, 1200'
}}
```

### Aspect Ratio Container
```liquid
<div class="aspect-ratio aspect-ratio--square">
  {{ product.featured_image | image_url: width: 600 | image_tag:
    loading: 'lazy',
    class: 'aspect-ratio__image'
  }}
</div>
```

### Placeholder for Missing Images
```liquid
{% if product.featured_image %}
  {{ product.featured_image | image_url: width: 400 | image_tag }}
{% else %}
  {{ 'product-placeholder.svg' | asset_url | image_tag: class: 'placeholder' }}
{% endif %}
```
