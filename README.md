# review
<span class="shopify-product-reviews-badge" data-id="{{ product.id }}"></span>

<div id="shopify-product-reviews" data-id="{{product.id}}">{{ product.metafields.spr.reviews }}</div>

//background image section
style="background-image:url('{{ section.settings.image | img_url : 'original' }}')"

{
      "type": "image_picker",
      "id": "image",
      "label": "Background Image"
    },

Swatches color code: 
 {% if product.options[forloop.index0] == 'Color' %}

label: style="background-color: {{ value | split: ' ' | last | handle }} !important; background-image: url({{ value | handle | append: '.png' | asset_url }})"

	  {% else %}
    	  {% else %}
