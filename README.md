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
	  
 {% for media in product.media %}
  <div class="item custom_slider_images">
        <img class="slider_images" src="{{ media | img_url: '100x100'}}" alt="{{ media.alt }}">
       </div>
          {% endfor %}
	  
 {% for image in product.images %}
           {% for variant in image.variants %}
               <li><img src="{{variant.image.src | img_url: '100*100'}}"
                        alt="{{variant.title}}"/></li>
           {% endfor %}
       {% endfor %}
       
       
       //feature
       
       {{ 'section-main-product.css' | asset_url | stylesheet_tag }}
{{ 'section-featured-product.css' | asset_url | stylesheet_tag }}
{{ 'component-accordion.css' | asset_url | stylesheet_tag }}
{{ 'component-price.css' | asset_url | stylesheet_tag }}
{{ 'component-rte.css' | asset_url | stylesheet_tag }}
{{ 'component-loading-overlay.css' | asset_url | stylesheet_tag }}

{%- style -%}
  .section-{{ section.id }}-padding {
    padding-top: {{ section.settings.padding_top | times: 0.75 | round: 0 }}px;
    padding-bottom: {{ section.settings.padding_bottom | times: 0.75 | round: 0 }}px;
  }

  @media screen and (min-width: 750px) {
    .section-{{ section.id }}-padding {
      padding-top: {{ section.settings.padding_top }}px;
      padding-bottom: {{ section.settings.padding_bottom }}px;
    }
  }
{%- endstyle -%}

<script src="{{ 'product-info.js' | asset_url }}" defer="defer"></script>

<link rel="stylesheet" href="{{ 'component-deferred-media.css' | asset_url }}" media="print" onload="this.media='all'">

{%- liquid
  assign product = section.settings.product

  if section.settings.media_size == 'large'
    assign media_width = 0.65
  elsif section.settings.media_size == 'medium'
    assign media_width = 0.55
  elsif section.settings.media_size == 'small'
    assign media_width = 0.45
  endif
-%}

{% comment %} TODO: assign `product.selected_or_first_available_variant` to variable and replace usage to reduce verbosity {% endcomment %}

{%- assign first_3d_model = product.media | where: 'media_type', 'model' | first -%}
{%- if first_3d_model -%}
  {{ 'component-product-model.css' | asset_url | stylesheet_tag }}
  <link
    id="ModelViewerStyle"
    rel="stylesheet"
    href="https://cdn.shopify.com/shopifycloud/model-viewer-ui/assets/v1.0/model-viewer-ui.css"
    media="print"
    onload="this.media='all'"
  >
  <link
    id="ModelViewerOverride"
    rel="stylesheet"
    href="{{ 'component-model-viewer-ui.css' | asset_url }}"
    media="print"
    onload="this.media='all'"
  >
{%- endif -%}

<section class="color-{{ section.settings.color_scheme }} {% if section.settings.secondary_background %}background-secondary{% else %}gradient{% endif %}">
  <div class="page-width section-{{ section.id }}-padding{% if section.settings.secondary_background %} isolate{% endif %}">
    <div class="featured-product product product--{{ section.settings.media_size }} grid grid--1-col gradient color-{{ section.settings.color_scheme }} product--{{ section.settings.media_position }}{% if section.settings.secondary_background == false %} isolate{% endif %} {% if product.media.size > 0 %}grid--2-col-tablet{% else %}product--no-media{% endif %}">
      <div class="grid__item product__media-wrapper{% if section.settings.media_position == 'right' %} medium-hide large-up-hide{% endif %}">
        <media-gallery
          id="MediaGallery-{{ section.id }}"
          role="region"
          aria-label="{{ 'products.product.media.gallery_viewer' | t }}"
          data-desktop-layout="stacked"
        >
          <div id="GalleryViewer-{{ section.id }}" class="product__media-list">
            {%- if product.selected_or_first_available_variant.featured_media != null -%}
              {%- assign media = product.selected_or_first_available_variant.featured_media -%}
              <div class="product__media-item" data-media-id="{{ section.id }}-{{ media.id }}">
                {% render 'product-thumbnail',
                  media: media,
                  position: 'featured',
                  loop: section.settings.enable_video_looping,
                  modal_id: section.id,
                  xr_button: false,
                  media_width: media_width,
                  media_fit: section.settings.media_fit,
                  constrain_to_viewport: section.settings.constrain_to_viewport
                %}
              </div>
            {%- endif -%}
            {%- liquid
              assign media_to_render = product.featured_media.id
              for variant in product.variants
                assign media_to_render = media_to_render | append: variant.featured_media.id | append: ' '
              endfor
            -%}
            {%- for media in product.media -%}
              {%- if media_to_render contains media.id
                and media.id != product.selected_or_first_available_variant.featured_media.id
              -%}
                <div class="product__media-item" data-media-id="{{ section.id }}-{{ media.id }}">
                  {% render 'product-thumbnail',
                    media: media,
                    position: forloop.index,
                    loop: section.settings.enable_video_looping,
                    modal_id: section.id,
                    xr_button: false,
                    media_width: media_width,
                    media_fit: section.settings.media_fit,
                    constrain_to_viewport: section.settings.constrain_to_viewport
                  %}
                </div>
              {%- endif -%}
            {%- endfor -%}
          </div>
          {%- if first_3d_model -%}
            <button
              class="button button--full-width product__xr-button"
              type="button"
              aria-label="{{ 'products.product.xr_button_label' | t }}"
              data-shopify-xr
              data-shopify-model3d-id="{{ first_3d_model.id }}"
              data-shopify-title="{{ product.title | escape }}"
              data-shopify-xr-hidden
            >
              {% render 'icon-3d-model' %}
              {{ 'products.product.xr_button' | t }}
            </button>
          {%- endif -%}
        </media-gallery>
      </div>
      <div class="product__info-wrapper grid__item">
        <product-info
          id="ProductInfo-{{ section.id }}"
          class="product__info-container"
          data-section="{{ section.id }}"
          data-url="{{ product.url }}"
        >
          {%- assign product_form_id = 'product-form-' | append: section.id -%}

          {%- for block in section.blocks -%}
            {%- case block.type -%}
              {%- when '@app' -%}
                {% render block %}
              {%- when 'text' -%}
                <p
                  class="product__text{% if block.settings.text_style == 'uppercase' %} caption-with-letter-spacing{% elsif block.settings.text_style == 'subtitle' %} subtitle{% endif %}"
                  {{ block.shopify_attributes }}
                >
                  {{- block.settings.text -}}
                </p>
              {%- when 'title' -%}
                <h2 class="product__title {{ block.settings.heading_size }}" {{ block.shopify_attributes }}>
                  {%- if product.title != blank -%}
                    {{ product.title | escape }}
                  {%- else -%}
                    {{ 'onboarding.product_title' | t }}
                  {%- endif -%}
                </h2>
              {%- when 'price' -%}
                <div class="no-js-hidden" id="price-{{ section.id }}" role="status" {{ block.shopify_attributes }}>
                  {%- render 'price',
                    product: product,
                    use_variant: true,
                    show_badges: true,
                    price_class: 'price--large'
                  -%}
                </div>
                {%- if shop.taxes_included or shop.shipping_policy.body != blank -%}
                  <div class="product__tax caption rte">
                    {%- if shop.taxes_included -%}
                      {{ 'products.product.include_taxes' | t }}
                    {%- endif -%}
                    {%- if shop.shipping_policy.body != blank -%}
                      {{ 'products.product.shipping_policy_html' | t: link: shop.shipping_policy.url }}
                    {%- endif -%}
                  </div>
                {%- endif -%}
                {%- if product != blank -%}
                  <div {{ block.shopify_attributes }}>
                    {%- form 'product', product -%}
                      <input type="hidden" name="id" value="{{ product.selected_or_first_available_variant.id }}">
                      {{ form | payment_terms }}
                    {%- endform -%}
                  </div>
                {%- endif -%}
              {%- when 'sku' -%}
                <p
                  class="product__sku no-js-hidden{% if block.settings.text_style == 'uppercase' %} caption-with-letter-spacing{% elsif block.settings.text_style == 'subtitle' %} subtitle{% endif %}{% if product.selected_or_first_available_variant.sku.size == 0 %} visibility-hidden{% endif %}"
                  id="Sku-{{ section.id }}"
                  role="status"
                  {{ block.shopify_attributes }}
                >
                  <span class="visually-hidden">{{ 'products.product.sku' | t }}:</span>
                  {{- product.selected_or_first_available_variant.sku -}}
                </p>
              {%- when 'quantity_selector' -%}
                <div
                  id="Quantity-Form-{{ section.id }}"
                  class="product-form__input product-form__quantity{% if settings.inputs_shadow_vertical_offset != 0 and settings.inputs_shadow_vertical_offset < 0 %} product-form__quantity-top{% endif %}"
                  {{ block.shopify_attributes }}
                >
                  {% comment %} TODO: enable theme-check once `item_count_for_variant` is accepted as valid filter {% endcomment %}
                  {% # theme-check-disable %}
                  {%- assign cart_qty = cart
                    | item_count_for_variant: product.selected_or_first_available_variant.id
                  -%}
                  {% # theme-check-enable %}
                  <label class="quantity__label form__label" for="Quantity-{{ section.id }}">
                    {{ 'products.product.quantity.label' | t }}
                    <span class="quantity__rules-cart no-js-hidden{% if cart_qty == 0 %} hidden{% endif %}">
                      <span class="loading-overlay hidden">
                        <span class="loading-overlay__spinner">
                          <svg
                            aria-hidden="true"
                            focusable="false"
                            class="spinner"
                            viewBox="0 0 66 66"
                            xmlns="http://www.w3.org/2000/svg"
                          >
                            <circle class="path" fill="none" stroke-width="6" cx="33" cy="33" r="30"></circle>
                          </svg>
                        </span>
                      </span>
                      <span
                        >(
                        {{- 'products.product.quantity.in_cart_html' | t: quantity: cart_qty -}}
                        )</span
                      >
                    </span>
                  </label>
                  <quantity-input class="quantity">
                    <button class="quantity__button no-js-hidden" name="minus" type="button">
                      <span class="visually-hidden">
                        {{- 'products.product.quantity.decrease' | t: product: product.title | escape -}}
                      </span>
                      {% render 'icon-minus' %}
                    </button>
                    <input
                      class="quantity__input"
                      type="number"
                      name="quantity"
                      id="Quantity-{{ section.id }}"
                      data-cart-quantity="{{ cart_qty }}"
                      data-min="{{ product.selected_or_first_available_variant.quantity_rule.min }}"
                      min="{{ product.selected_or_first_available_variant.quantity_rule.min }}"
                      {% if product.selected_or_first_available_variant.quantity_rule.max != null %}
                        data-max="{{ product.selected_or_first_available_variant.quantity_rule.max }}"
                        max="{{ product.selected_or_first_available_variant.quantity_rule.max }}"
                      {% endif %}
                      step="{{ product.selected_or_first_available_variant.quantity_rule.increment }}"
                      value="{{ product.selected_or_first_available_variant.quantity_rule.min }}"
                      form="{{ product_form_id }}"
                    >
                    <button class="quantity__button no-js-hidden" name="plus" type="button">
                      <span class="visually-hidden">
                        {{- 'products.product.quantity.increase' | t: product: product.title | escape -}}
                      </span>
                      {% render 'icon-plus' %}
                    </button>
                  </quantity-input>
                  <div class="quantity__rules caption no-js-hidden">
                    {%- if product.selected_or_first_available_variant.quantity_rule.increment > 1 -%}
                      <span class="divider">
                        {{-
                          'products.product.quantity.multiples_of'
                          | t: quantity: product.selected_or_first_available_variant.quantity_rule.increment
                        -}}
                      </span>
                    {%- endif -%}
                    {%- if product.selected_or_first_available_variant.quantity_rule.min > 1 -%}
                      <span class="divider">
                        {{-
                          'products.product.quantity.minimum_of'
                          | t: quantity: product.selected_or_first_available_variant.quantity_rule.min
                        -}}
                      </span>
                    {%- endif -%}
                    {%- if product.selected_or_first_available_variant.quantity_rule.max != null -%}
                      <span class="divider">
                        {{-
                          'products.product.quantity.maximum_of'
                          | t: quantity: product.selected_or_first_available_variant.quantity_rule.max
                        -}}
                      </span>
                    {%- endif -%}
                  </div>
                </div>
              {%- when 'share' -%}
                {% assign share_url = product.selected_variant.url | default: product.url | prepend: request.origin %}
                {% render 'share-button', block: block, share_link: share_url %}
              {%- when 'variant_picker' -%}
                {% render 'product-variant-picker', product: product, block: block, product_form_id: product_form_id, update_url: false %}
              {%- when 'buy_buttons' -%}
                {%- render 'buy-buttons', block: block, product: product, product_form_id: product_form_id, section_id: section.id -%}
              {%- when 'custom_liquid' -%}
                {{ block.settings.custom_liquid }}
              {%- when 'rating' -%}
                {%- if product.metafields.reviews.rating.value != blank -%}
                  {% liquid
                    assign rating_decimal = 0
                    assign decimal = product.metafields.reviews.rating.value.rating | modulo: 1
                    if decimal >= 0.3 and decimal <= 0.7
                      assign rating_decimal = 0.5
                    elsif decimal > 0.7
                      assign rating_decimal = 1
                    endif
                  %}
                  <div
                    class="rating"
                    role="img"
                    aria-label="{{ 'accessibility.star_reviews_info' | t: rating_value: product.metafields.reviews.rating.value, rating_max: product.metafields.reviews.rating.value.scale_max }}"
                  >
                    <span
                      aria-hidden="true"
                      class="rating-star color-icon-{{ settings.accent_icons }}"
                      style="--rating: {{ product.metafields.reviews.rating.value.rating | floor }}; --rating-max: {{ product.metafields.reviews.rating.value.scale_max }}; --rating-decimal: {{ rating_decimal }};"
                    ></span>
                  </div>
                  <p class="rating-text caption">
                    <span aria-hidden="true">
                      {{- product.metafields.reviews.rating.value }} /
                      {{ product.metafields.reviews.rating.value.scale_max -}}
                    </span>
                  </p>
                  <p class="rating-count caption">
                    <span aria-hidden="true">({{ product.metafields.reviews.rating_count }})</span>
                    <span class="visually-hidden">
                      {{- product.metafields.reviews.rating_count }}
                      {{ 'accessibility.total_reviews' | t -}}
                    </span>
                  </p>
                {%- endif -%}
              {%- when 'icon-with-text' -%}
                {% render 'icon-with-text', block: block %}
            {%- endcase -%}
          {%- endfor -%}
          <a
            {% if product == blank %}
              role="link" aria-disabled="true"
            {% else %}
              href="{{ product.url }}"
            {% endif %}
            class="link product__view-details animate-arrow"
          >
            {{ 'products.product.view_full_details' | t }}
            {% render 'icon-arrow' %}
          </a>
        </product-info>
      </div>
      {%- if section.settings.media_position == 'right' -%}
        <div class="grid__item product__media-wrapper small-hide">
          <media-gallery
            id="MediaGallery-{{ section.id }}-right"
            role="region"
            aria-label="{{ 'products.product.media.gallery_viewer' | t }}"
            data-desktop-layout="stacked"
          >
            <div id="GalleryViewer-{{ section.id }}-right" class="product__media-list">
              {%- if product.selected_or_first_available_variant.featured_media != null -%}
                {%- assign media = product.selected_or_first_available_variant.featured_media -%}
                <div class="product__media-item" data-media-id="{{ section.id }}-{{ media.id }}">
                  {% render 'product-thumbnail',
                    media: media,
                    position: 'featured',
                    loop: section.settings.enable_video_looping,
                    modal_id: section.id,
                    xr_button: false,
                    media_width: media_width,
                    media_fit: section.settings.media_fit,
                    constrain_to_viewport: section.settings.constrain_to_viewport
                  %}
                </div>
              {%- endif -%}
              {%- for media in product.media -%}
                {%- if media_to_render contains media.id
                  and media.id != product.selected_or_first_available_variant.featured_media.id
                -%}
                  <div class="product__media-item" data-media-id="{{ section.id }}-{{ media.id }}">
                    {% render 'product-thumbnail',
                      media: media,
                      position: forloop.index,
                      loop: section.settings.enable_video_looping,
                      modal_id: section.id,
                      xr_button: false,
                      media_width: media_width,
                      media_fit: section.settings.media_fit,
                      constrain_to_viewport: section.settings.constrain_to_viewport
                    %}
                  </div>
                {%- endif -%}
              {%- endfor -%}
            </div>
            {%- if first_3d_model -%}
              <button
                class="button button--full-width product__xr-button"
                type="button"
                aria-label="{{ 'products.product.xr_button_label' | t }}"
                data-shopify-xr
                data-shopify-model3d-id="{{ first_3d_model.id }}"
                data-shopify-title="{{ product.title | escape }}"
                data-shopify-xr-hidden
              >
                {% render 'icon-3d-model' %}
                {{ 'products.product.xr_button' | t }}
              </button>
            {%- endif -%}
          </media-gallery>
        </div>
      {%- endif -%}
    </div>
    {% render 'product-media-modal', product: product, variant_images: media_to_render %}
  </div>
</section>

<script src="{{ 'product-form.js' | asset_url }}" defer="defer"></script>
{%- if section.settings.image_zoom == 'hover' -%}
  <script id="EnableZoomOnHover-featured" src="{{ 'magnify.js' | asset_url }}" defer="defer"></script>
{%- endif %}
{%- if request.design_mode -%}
  <script src="{{ 'theme-editor.js' | asset_url }}" defer="defer"></script>
{%- endif -%}

{%- if first_3d_model -%}
  <script type="application/json" id="ProductJSON-{{ product.id }}">
    {{ product.media | where: 'media_type', 'model' | json }}
  </script>
  <script src="{{ 'product-model.js' | asset_url }}" defer></script>
{%- endif -%}

{%- liquid
  if product.selected_or_first_available_variant.featured_media
    assign seo_media = product.selected_or_first_available_variant.featured_media
  else
    assign seo_media = product.featured_media
  endif
-%}

<script type="application/ld+json">
  {
    "@context": "http://schema.org/",
    "@type": "Product",
    "name": {{ product.title | json }},
    "url": {{ request.origin | append: product.url | json }},
    {% if seo_media -%}
      "image": [
        {{ seo_media | image_url: width: 1920 | prepend: "https:" | json }}
      ],
    {%- endif %}
    "description": {{ product.description | strip_html | json }},
    {% if product.selected_or_first_available_variant.sku != blank -%}
      "sku": {{ product.selected_or_first_available_variant.sku | json }},
    {%- endif %}
    "brand": {
      "@type": "Brand",
      "name": {{ product.vendor | json }}
    },
    "offers": [
      {%- for variant in product.variants -%}
        {
          "@type" : "Offer",
          {%- if variant.sku != blank -%}
            "sku": {{ variant.sku | json }},
          {%- endif -%}
          {%- if variant.barcode.size == 12 -%}
              "gtin12": {{ variant.barcode }},
          {%- endif -%}
          {%- if variant.barcode.size == 13 -%}
            "gtin13": {{ variant.barcode }},
          {%- endif -%}
          {%- if variant.barcode.size == 14 -%}
            "gtin14": {{ variant.barcode }},
          {%- endif -%}
          "availability" : "http://schema.org/{% if variant.available %}InStock{% else %}OutOfStock{% endif %}",
          "price" : {{ variant.price | divided_by: 100.00 | json }},
          "priceCurrency" : {{ cart.currency.iso_code | json }},
          "url" : {{ request.origin | append: variant.url | json }}
        }{% unless forloop.last %},{% endunless %}
      {%- endfor -%}
    ]
  }
</script>

<script>
  document.addEventListener('DOMContentLoaded', function () {
    function isIE() {
      const ua = window.navigator.userAgent;
      const msie = ua.indexOf('MSIE ');
      const trident = ua.indexOf('Trident/');

      return msie > 0 || trident > 0;
    }

    if (!isIE()) return;
    const hiddenInput = document.querySelector('#{{ product_form_id }} input[name="id"]');
    const noScriptInputWrapper = document.createElement('div');
    const variantSwitcher =
      document.querySelector('variant-radios[data-section="{{ section.id }}"]') ||
      document.querySelector('variant-selects[data-section="{{ section.id }}"]');
    noScriptInputWrapper.innerHTML = document.querySelector(
      '.product-form__noscript-wrapper-{{ section.id }}'
    ).textContent;
    variantSwitcher.outerHTML = noScriptInputWrapper.outerHTML;

    document.querySelector('#Variants-{{ section.id }}').addEventListener('change', function (event) {
      hiddenInput.value = event.currentTarget.value;
    });
  });
</script>

{% if product.media.size > 0 %}
  <script src="{{ 'product-modal.js' | asset_url }}" defer="defer"></script>
  <script src="{{ 'media-gallery.js' | asset_url }}" defer="defer"></script>
{% endif %}

{% schema %}
{
  "name": "t:sections.featured-product.name",
  "tag": "section",
  "class": "section section-featured-product",
  "disabled_on": {
    "groups": ["header", "footer"]
  },
  "blocks": [
    {
      "type": "@app"
    },
    {
      "type": "text",
      "name": "t:sections.featured-product.blocks.text.name",
      "settings": [
        {
          "type": "text",
          "id": "text",
          "default": "Text block",
          "label": "t:sections.featured-product.blocks.text.settings.text.label"
        },
        {
          "type": "select",
          "id": "text_style",
          "options": [
            {
              "value": "body",
              "label": "t:sections.featured-product.blocks.text.settings.text_style.options__1.label"
            },
            {
              "value": "subtitle",
              "label": "t:sections.featured-product.blocks.text.settings.text_style.options__2.label"
            },
            {
              "value": "uppercase",
              "label": "t:sections.featured-product.blocks.text.settings.text_style.options__3.label"
            }
          ],
          "default": "body",
          "label": "t:sections.featured-product.blocks.text.settings.text_style.label"
        }
      ]
    },
    {
      "type": "title",
      "name": "t:sections.featured-product.blocks.title.name",
      "limit": 1,
      "settings": [
        {
          "type": "select",
          "id": "heading_size",
          "options": [
            {
              "value": "h2",
              "label": "t:sections.all.heading_size.options__1.label"
            },
            {
              "value": "h1",
              "label": "t:sections.all.heading_size.options__2.label"
            },
            {
              "value": "h0",
              "label": "t:sections.all.heading_size.options__3.label"
            }
          ],
          "default": "h1",
          "label": "t:sections.all.heading_size.label"
        }
      ]
    },
    {
      "type": "price",
      "name": "t:sections.featured-product.blocks.price.name",
      "limit": 1
    },
    {
      "type": "sku",
      "name": "t:sections.featured-product.blocks.sku.name",
      "limit": 1,
      "settings": [
        {
          "type": "select",
          "id": "text_style",
          "options": [
            {
              "value": "body",
              "label": "t:sections.featured-product.blocks.sku.settings.text_style.options__1.label"
            },
            {
              "value": "subtitle",
              "label": "t:sections.featured-product.blocks.sku.settings.text_style.options__2.label"
            },
            {
              "value": "uppercase",
              "label": "t:sections.featured-product.blocks.sku.settings.text_style.options__3.label"
            }
          ],
          "default": "body",
          "label": "t:sections.featured-product.blocks.sku.settings.text_style.label"
        }
      ]
    },
    {
      "type": "quantity_selector",
      "name": "t:sections.featured-product.blocks.quantity_selector.name",
      "limit": 1
    },
    {
      "type": "variant_picker",
      "name": "t:sections.featured-product.blocks.variant_picker.name",
      "limit": 1,
      "settings": [
        {
          "type": "select",
          "id": "picker_type",
          "options": [
            {
              "value": "dropdown",
              "label": "t:sections.featured-product.blocks.variant_picker.settings.picker_type.options__1.label"
            },
            {
              "value": "button",
              "label": "t:sections.featured-product.blocks.variant_picker.settings.picker_type.options__2.label"
            }
          ],
          "default": "button",
          "label": "t:sections.featured-product.blocks.variant_picker.settings.picker_type.label"
        }
      ]
    },
    {
      "type": "buy_buttons",
      "name": "t:sections.featured-product.blocks.buy_buttons.name",
      "limit": 1,
      "settings": [
        {
          "type": "checkbox",
          "id": "show_dynamic_checkout",
          "default": true,
          "label": "t:sections.featured-product.blocks.buy_buttons.settings.show_dynamic_checkout.label",
          "info": "t:sections.featured-product.blocks.buy_buttons.settings.show_dynamic_checkout.info"
        }
      ]
    },
    {
      "type": "share",
      "name": "t:sections.featured-product.blocks.share.name",
      "limit": 1,
      "settings": [
        {
          "type": "text",
          "id": "share_label",
          "label": "t:sections.featured-product.blocks.share.settings.text.label",
          "default": "Share"
        },
        {
          "type": "paragraph",
          "content": "t:sections.featured-product.blocks.share.settings.featured_image_info.content"
        },
        {
          "type": "paragraph",
          "content": "t:sections.featured-product.blocks.share.settings.title_info.content"
        }
      ]
    },
    {
      "type": "custom_liquid",
      "name": "t:sections.featured-product.blocks.custom_liquid.name",
      "settings": [
        {
          "type": "liquid",
          "id": "custom_liquid",
          "label": "t:sections.featured-product.blocks.custom_liquid.settings.custom_liquid.label"
        }
      ]
    },
    {
      "type": "rating",
      "name": "t:sections.featured-product.blocks.rating.name",
      "limit": 1,
      "settings": [
        {
          "type": "paragraph",
          "content": "t:sections.featured-product.blocks.rating.settings.paragraph.content"
        }
      ]
    },
    {
      "type": "icon-with-text",
      "name": "t:sections.main-product.blocks.icon_with_text.name",
      "settings": [
        {
          "type": "select",
          "id": "layout",
          "options": [
            {
              "value": "horizontal",
              "label": "t:sections.main-product.blocks.icon_with_text.settings.layout.options__1.label"
            },
            {
              "value": "vertical",
              "label": "t:sections.main-product.blocks.icon_with_text.settings.layout.options__2.label"
            }
          ],
          "default": "horizontal",
          "label": "t:sections.main-product.blocks.icon_with_text.settings.layout.label"
        },
        {
          "type": "header",
          "content": "t:sections.main-product.blocks.icon_with_text.settings.content.label",
          "info": "t:sections.main-product.blocks.icon_with_text.settings.content.info"
        },
        {
          "type": "select",
          "id": "icon_1",
          "options": [
            {
              "value": "none",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__1.label"
            },
            {
              "value": "apple",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__2.label"
            },
            {
              "value": "banana",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__3.label"
            },
            {
              "value": "bottle",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__4.label"
            },
            {
              "value": "box",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__5.label"
            },
            {
              "value": "carrot",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__6.label"
            },
            {
              "value": "chat_bubble",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__7.label"
            },
            {
              "value": "check_mark",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__8.label"
            },
            {
              "value": "clipboard",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__9.label"
            },
            {
              "value": "dairy",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__10.label"
            },
            {
              "value": "dairy_free",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__11.label"
            },
            {
              "value": "dryer",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__12.label"
            },
            {
              "value": "eye",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__13.label"
            },
            {
              "value": "fire",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__14.label"
            },
            {
              "value": "gluten_free",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__15.label"
            },
            {
              "value": "heart",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__16.label"
            },
            {
              "value": "iron",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__17.label"
            },
            {
              "value": "leaf",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__18.label"
            },
            {
              "value": "leather",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__19.label"
            },
            {
              "value": "lightning_bolt",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__20.label"
            },
            {
              "value": "lipstick",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__21.label"
            },
            {
              "value": "lock",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__22.label"
            },
            {
              "value": "map_pin",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__23.label"
            },
            {
              "value": "nut_free",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__24.label"
            },
            {
              "value": "pants",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__25.label"
            },
            {
              "value": "paw_print",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__26.label"
            },
            {
              "value": "pepper",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__27.label"
            },
            {
              "value": "perfume",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__28.label"
            },
            {
              "value": "plane",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__29.label"
            },
            {
              "value": "plant",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__30.label"
            },
            {
              "value": "price_tag",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__31.label"
            },
            {
              "value": "question_mark",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__32.label"
            },
            {
              "value": "recycle",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__33.label"
            },
            {
              "value": "return",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__34.label"
            },
            {
              "value": "ruler",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__35.label"
            },
            {
              "value": "serving_dish",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__36.label"
            },
            {
              "value": "shirt",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__37.label"
            },
            {
              "value": "shoe",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__38.label"
            },
            {
              "value": "silhouette",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__39.label"
            },
            {
              "value": "snowflake",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__40.label"
            },
            {
              "value": "star",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__41.label"
            },
            {
              "value": "stopwatch",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__42.label"
            },
            {
              "value": "truck",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__43.label"
            },
            {
              "value": "washing",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__44.label"
            }
          ],
          "default": "heart",
          "label": "t:sections.main-product.blocks.icon_with_text.settings.icon_1.label"
        },
        {
          "type": "image_picker",
          "id": "image_1",
          "label": "t:sections.main-product.blocks.icon_with_text.settings.image_1.label"
        },
        {
          "type": "text",
          "id": "heading_1",
          "default": "Heading",
          "label": "t:sections.main-product.blocks.icon_with_text.settings.heading_1.label",
          "info": "t:sections.main-product.blocks.icon_with_text.settings.heading.info"
        },
        {
          "type": "select",
          "id": "icon_2",
          "options": [
            {
              "value": "none",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__1.label"
            },
            {
              "value": "apple",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__2.label"
            },
            {
              "value": "banana",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__3.label"
            },
            {
              "value": "bottle",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__4.label"
            },
            {
              "value": "box",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__5.label"
            },
            {
              "value": "carrot",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__6.label"
            },
            {
              "value": "chat_bubble",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__7.label"
            },
            {
              "value": "check_mark",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__8.label"
            },
            {
              "value": "clipboard",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__9.label"
            },
            {
              "value": "dairy",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__10.label"
            },
            {
              "value": "dairy_free",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__11.label"
            },
            {
              "value": "dryer",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__12.label"
            },
            {
              "value": "eye",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__13.label"
            },
            {
              "value": "fire",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__14.label"
            },
            {
              "value": "gluten_free",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__15.label"
            },
            {
              "value": "heart",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__16.label"
            },
            {
              "value": "iron",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__17.label"
            },
            {
              "value": "leaf",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__18.label"
            },
            {
              "value": "leather",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__19.label"
            },
            {
              "value": "lightning_bolt",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__20.label"
            },
            {
              "value": "lipstick",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__21.label"
            },
            {
              "value": "lock",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__22.label"
            },
            {
              "value": "map_pin",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__23.label"
            },
            {
              "value": "nut_free",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__24.label"
            },
            {
              "value": "pants",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__25.label"
            },
            {
              "value": "paw_print",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__26.label"
            },
            {
              "value": "pepper",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__27.label"
            },
            {
              "value": "perfume",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__28.label"
            },
            {
              "value": "plane",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__29.label"
            },
            {
              "value": "plant",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__30.label"
            },
            {
              "value": "price_tag",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__31.label"
            },
            {
              "value": "question_mark",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__32.label"
            },
            {
              "value": "recycle",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__33.label"
            },
            {
              "value": "return",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__34.label"
            },
            {
              "value": "ruler",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__35.label"
            },
            {
              "value": "serving_dish",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__36.label"
            },
            {
              "value": "shirt",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__37.label"
            },
            {
              "value": "shoe",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__38.label"
            },
            {
              "value": "silhouette",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__39.label"
            },
            {
              "value": "snowflake",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__40.label"
            },
            {
              "value": "star",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__41.label"
            },
            {
              "value": "stopwatch",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__42.label"
            },
            {
              "value": "truck",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__43.label"
            },
            {
              "value": "washing",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__44.label"
            }
          ],
          "default": "return",
          "label": "t:sections.main-product.blocks.icon_with_text.settings.icon_2.label"
        },
        {
          "type": "image_picker",
          "id": "image_2",
          "label": "t:sections.main-product.blocks.icon_with_text.settings.image_2.label"
        },
        {
          "type": "text",
          "id": "heading_2",
          "default": "Heading",
          "label": "t:sections.main-product.blocks.icon_with_text.settings.heading_2.label",
          "info": "t:sections.main-product.blocks.icon_with_text.settings.heading.info"
        },
        {
          "type": "select",
          "id": "icon_3",
          "options": [
            {
              "value": "none",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__1.label"
            },
            {
              "value": "apple",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__2.label"
            },
            {
              "value": "banana",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__3.label"
            },
            {
              "value": "bottle",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__4.label"
            },
            {
              "value": "box",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__5.label"
            },
            {
              "value": "carrot",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__6.label"
            },
            {
              "value": "chat_bubble",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__7.label"
            },
            {
              "value": "check_mark",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__8.label"
            },
            {
              "value": "clipboard",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__9.label"
            },
            {
              "value": "dairy",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__10.label"
            },
            {
              "value": "dairy_free",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__11.label"
            },
            {
              "value": "dryer",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__12.label"
            },
            {
              "value": "eye",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__13.label"
            },
            {
              "value": "fire",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__14.label"
            },
            {
              "value": "gluten_free",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__15.label"
            },
            {
              "value": "heart",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__16.label"
            },
            {
              "value": "iron",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__17.label"
            },
            {
              "value": "leaf",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__18.label"
            },
            {
              "value": "leather",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__19.label"
            },
            {
              "value": "lightning_bolt",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__20.label"
            },
            {
              "value": "lipstick",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__21.label"
            },
            {
              "value": "lock",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__22.label"
            },
            {
              "value": "map_pin",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__23.label"
            },
            {
              "value": "nut_free",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__24.label"
            },
            {
              "value": "pants",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__25.label"
            },
            {
              "value": "paw_print",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__26.label"
            },
            {
              "value": "pepper",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__27.label"
            },
            {
              "value": "perfume",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__28.label"
            },
            {
              "value": "plane",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__29.label"
            },
            {
              "value": "plant",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__30.label"
            },
            {
              "value": "price_tag",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__31.label"
            },
            {
              "value": "question_mark",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__32.label"
            },
            {
              "value": "recycle",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__33.label"
            },
            {
              "value": "return",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__34.label"
            },
            {
              "value": "ruler",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__35.label"
            },
            {
              "value": "serving_dish",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__36.label"
            },
            {
              "value": "shirt",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__37.label"
            },
            {
              "value": "shoe",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__38.label"
            },
            {
              "value": "silhouette",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__39.label"
            },
            {
              "value": "snowflake",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__40.label"
            },
            {
              "value": "star",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__41.label"
            },
            {
              "value": "stopwatch",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__42.label"
            },
            {
              "value": "truck",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__43.label"
            },
            {
              "value": "washing",
              "label": "t:sections.main-product.blocks.collapsible_tab.settings.icon.options__44.label"
            }
          ],
          "default": "truck",
          "label": "t:sections.main-product.blocks.icon_with_text.settings.icon_3.label"
        },
        {
          "type": "image_picker",
          "id": "image_3",
          "label": "t:sections.main-product.blocks.icon_with_text.settings.image_3.label"
        },
        {
          "type": "text",
          "id": "heading_3",
          "default": "Heading",
          "label": "t:sections.main-product.blocks.icon_with_text.settings.heading_3.label",
          "info": "t:sections.main-product.blocks.icon_with_text.settings.heading.info"
        }
      ]
    }
  ],
  "settings": [
    {
      "type": "product",
      "id": "product",
      "label": "t:sections.featured-product.settings.product.label"
    },
    {
      "type": "select",
      "id": "color_scheme",
      "options": [
        {
          "value": "accent-1",
          "label": "t:sections.all.colors.accent_1.label"
        },
        {
          "value": "accent-2",
          "label": "t:sections.all.colors.accent_2.label"
        },
        {
          "value": "background-1",
          "label": "t:sections.all.colors.background_1.label"
        },
        {
          "value": "background-2",
          "label": "t:sections.all.colors.background_2.label"
        },
        {
          "value": "inverse",
          "label": "t:sections.all.colors.inverse.label"
        }
      ],
      "default": "background-1",
      "label": "t:sections.all.colors.label"
    },
    {
      "type": "checkbox",
      "id": "secondary_background",
      "default": false,
      "label": "t:sections.featured-product.settings.secondary_background.label"
    },
    {
      "type": "header",
      "content": "t:sections.featured-product.settings.header.content",
      "info": "t:sections.featured-product.settings.header.info"
    },
    {
      "type": "select",
      "id": "media_size",
      "options": [
        {
          "value": "small",
          "label": "t:sections.main-product.settings.media_size.options__1.label"
        },
        {
          "value": "medium",
          "label": "t:sections.main-product.settings.media_size.options__2.label"
        },
        {
          "value": "large",
          "label": "t:sections.main-product.settings.media_size.options__3.label"
        }
      ],
      "default": "medium",
      "label": "t:sections.main-product.settings.media_size.label",
      "info": "t:sections.main-product.settings.media_size.info"
    },
    {
      "type": "checkbox",
      "id": "constrain_to_viewport",
      "default": true,
      "label": "t:sections.main-product.settings.constrain_to_viewport.label"
    },
    {
      "type": "select",
      "id": "media_fit",
      "options": [
        {
          "value": "contain",
          "label": "t:sections.main-product.settings.media_fit.options__1.label"
        },
        {
          "value": "cover",
          "label": "t:sections.main-product.settings.media_fit.options__2.label"
        }
      ],
      "default": "contain",
      "label": "t:sections.main-product.settings.media_fit.label"
    },
    {
      "type": "select",
      "id": "media_position",
      "options": [
        {
          "value": "left",
          "label": "t:sections.featured-product.settings.media_position.options__1.label"
        },
        {
          "value": "right",
          "label": "t:sections.featured-product.settings.media_position.options__2.label"
        }
      ],
      "default": "left",
      "label": "t:sections.featured-product.settings.media_position.label",
      "info": "t:sections.featured-product.settings.media_position.info"
    },
    {
      "type": "select",
      "id": "image_zoom",
      "options": [
        {
          "value": "lightbox",
          "label": "t:sections.main-product.settings.image_zoom.options__1.label"
        },
        {
          "value": "none",
          "label": "t:sections.main-product.settings.image_zoom.options__3.label"
        }
      ],
      "default": "lightbox",
      "label": "t:sections.main-product.settings.image_zoom.label"
    },
    {
      "type": "checkbox",
      "id": "hide_variants",
      "default": false,
      "label": "t:sections.main-product.settings.hide_variants.label"
    },
    {
      "type": "checkbox",
      "id": "enable_video_looping",
      "default": false,
      "label": "t:sections.featured-product.settings.enable_video_looping.label"
    },
    {
      "type": "header",
      "content": "t:sections.all.padding.section_padding_heading"
    },
    {
      "type": "range",
      "id": "padding_top",
      "min": 0,
      "max": 100,
      "step": 4,
      "unit": "px",
      "label": "t:sections.all.padding.padding_top",
      "default": 36
    },
    {
      "type": "range",
      "id": "padding_bottom",
      "min": 0,
      "max": 100,
      "step": 4,
      "unit": "px",
      "label": "t:sections.all.padding.padding_bottom",
      "default": 36
    }
  ],
  "presets": [
    {
      "name": "t:sections.featured-product.presets.name",
      "blocks": [
        {
          "type": "text",
          "settings": {
            "text": "{{ section.settings.product.vendor }}",
            "text_style": "uppercase"
          }
        },
        {
          "type": "title"
        },
        {
          "type": "price"
        },
        {
          "type": "variant_picker"
        },
        {
          "type": "quantity_selector"
        },
        {
          "type": "buy_buttons"
        },
        {
          "type": "share"
        }
      ]
    }
  ]
}
{% endschema %}

	  
	  
	  
	  
	  
	  
	     <div class="custom-variont-img">
          {% if card_product.options contains 'Color' %}
    <ul class="variantimage-on">
       {% for image in card_product.images %}
           {% for variant in image.variants %}
             {% if card_product.options contains 'Color' %} 
             <li>
             
             <img class="custom-variont-img" custom_title="{{ variant.title }}" custom_id="{{ variant.id }}" src="{{variant.image.src | img_url: 'master'}}"
              alt="{{variant.title}}">
             </li>
             {% endif %}
           {% endfor %}
       {% endfor %}
    </ul>
{% else %}
  
{% endif %}     
        </div>
