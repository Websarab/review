# review
<span class="shopify-product-reviews-badge" data-id="{{ product.id }}"></span>

<div id="shopify-product-reviews" data-id="{{product.id}}">{{ product.metafields.spr.reviews }}</div>


Swatches color code: 
 {% if product.options[forloop.index0] == 'Color' %}

label: style="background-color: {{ value | split: ' ' | last | handle }} !important; background-image: url({{ value | handle | append: '.png' | asset_url }})"

	  {% else %}
    	  {% else %}
