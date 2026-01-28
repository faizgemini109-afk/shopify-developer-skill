# Section Settings Reference

Complete reference for all Shopify section setting types.

## Basic Input Types

### Text
```json
{
  "type": "text",
  "id": "title",
  "label": "Title",
  "default": "Welcome",
  "placeholder": "Enter title..."
}
```

### Textarea
```json
{
  "type": "textarea",
  "id": "description",
  "label": "Description",
  "default": "Lorem ipsum..."
}
```

### Richtext
```json
{
  "type": "richtext",
  "id": "content",
  "label": "Content",
  "default": "<p>Rich text content</p>"
}
```

### Number
```json
{
  "type": "number",
  "id": "columns",
  "label": "Columns",
  "default": 3,
  "min": 1,
  "max": 6
}
```

### Range
```json
{
  "type": "range",
  "id": "opacity",
  "label": "Opacity",
  "min": 0,
  "max": 100,
  "step": 5,
  "default": 100,
  "unit": "%"
}
```

### Checkbox
```json
{
  "type": "checkbox",
  "id": "show_title",
  "label": "Show title",
  "default": true
}
```

### Select
```json
{
  "type": "select",
  "id": "alignment",
  "label": "Alignment",
  "default": "center",
  "options": [
    { "value": "left", "label": "Left" },
    { "value": "center", "label": "Center" },
    { "value": "right", "label": "Right" }
  ]
}
```

### Radio
```json
{
  "type": "radio",
  "id": "layout",
  "label": "Layout",
  "default": "grid",
  "options": [
    { "value": "grid", "label": "Grid" },
    { "value": "list", "label": "List" }
  ]
}
```

## Visual Types

### Color
```json
{
  "type": "color",
  "id": "background_color",
  "label": "Background color",
  "default": "#ffffff"
}
```

### Color Background
```json
{
  "type": "color_background",
  "id": "gradient",
  "label": "Background gradient"
}
```

### Color Scheme
```json
{
  "type": "color_scheme",
  "id": "color_scheme",
  "label": "Color scheme",
  "default": "scheme-1"
}
```

### Image Picker
```json
{
  "type": "image_picker",
  "id": "image",
  "label": "Image"
}
```

### Video
```json
{
  "type": "video",
  "id": "video",
  "label": "Video"
}
```

### Video URL
```json
{
  "type": "video_url",
  "id": "video_url",
  "label": "Video URL",
  "accept": ["youtube", "vimeo"],
  "placeholder": "https://www.youtube.com/watch?v=..."
}
```

## Resource Types

### URL
```json
{
  "type": "url",
  "id": "link",
  "label": "Link",
  "default": "/"
}
```

### Product
```json
{
  "type": "product",
  "id": "featured_product",
  "label": "Featured product"
}
```

### Product List
```json
{
  "type": "product_list",
  "id": "products",
  "label": "Products",
  "limit": 12
}
```

### Collection
```json
{
  "type": "collection",
  "id": "collection",
  "label": "Collection"
}
```

### Collection List
```json
{
  "type": "collection_list",
  "id": "collections",
  "label": "Collections",
  "limit": 8
}
```

### Blog
```json
{
  "type": "blog",
  "id": "blog",
  "label": "Blog"
}
```

### Article
```json
{
  "type": "article",
  "id": "article",
  "label": "Article"
}
```

### Page
```json
{
  "type": "page",
  "id": "page",
  "label": "Page"
}
```

### Link List (Menu)
```json
{
  "type": "link_list",
  "id": "menu",
  "label": "Menu",
  "default": "main-menu"
}
```

## Advanced Types

### HTML
```json
{
  "type": "html",
  "id": "custom_html",
  "label": "Custom HTML"
}
```

### Liquid
```json
{
  "type": "liquid",
  "id": "custom_liquid",
  "label": "Custom Liquid"
}
```

### Font Picker
```json
{
  "type": "font_picker",
  "id": "heading_font",
  "label": "Heading font",
  "default": "helvetica_n4"
}
```

### Metaobject
```json
{
  "type": "metaobject",
  "id": "testimonial",
  "label": "Testimonial",
  "metaobject_type": "testimonials"
}
```

### Metaobject List
```json
{
  "type": "metaobject_list",
  "id": "testimonials",
  "label": "Testimonials",
  "metaobject_type": "testimonials",
  "limit": 10
}
```

## Layout Settings

### Header
```json
{
  "type": "header",
  "content": "Layout settings"
}
```

### Paragraph
```json
{
  "type": "paragraph",
  "content": "Configure the section layout below."
}
```

## Accessing Settings in Liquid

```liquid
{% comment %} Basic settings {% endcomment %}
{{ section.settings.title }}
{{ section.settings.show_title }}

{% comment %} Image picker {% endcomment %}
{% if section.settings.image %}
  {{ section.settings.image | image_url: width: 800 | image_tag }}
{% endif %}

{% comment %} Color {% endcomment %}
<div style="background-color: {{ section.settings.background_color }};">

{% comment %} Product {% endcomment %}
{% if section.settings.featured_product %}
  {% assign product = section.settings.featured_product %}
  {{ product.title }}
{% endif %}

{% comment %} Collection {% endcomment %}
{% for product in section.settings.collection.products limit: 4 %}
  {{ product.title }}
{% endfor %}

{% comment %} Video URL {% endcomment %}
{% if section.settings.video_url %}
  {{ section.settings.video_url | external_video_tag }}
{% endif %}

{% comment %} Metaobject {% endcomment %}
{% if section.settings.testimonial %}
  {{ section.settings.testimonial.quote }}
  {{ section.settings.testimonial.author }}
{% endif %}
```

## Block Settings

Block settings use the same types. Access via:

```liquid
{% for block in section.blocks %}
  <div {{ block.shopify_attributes }}>
    {{ block.settings.heading }}
  </div>
{% endfor %}
```

## Common Patterns

### Responsive Padding
```json
{
  "type": "range",
  "id": "padding_top",
  "label": "Top padding",
  "min": 0,
  "max": 100,
  "step": 4,
  "default": 40,
  "unit": "px"
},
{
  "type": "range",
  "id": "padding_bottom",
  "label": "Bottom padding",
  "min": 0,
  "max": 100,
  "step": 4,
  "default": 40,
  "unit": "px"
}
```

### Width Control
```json
{
  "type": "select",
  "id": "content_width",
  "label": "Content width",
  "default": "medium",
  "options": [
    { "value": "small", "label": "Small" },
    { "value": "medium", "label": "Medium" },
    { "value": "large", "label": "Large" },
    { "value": "full", "label": "Full width" }
  ]
}
```
