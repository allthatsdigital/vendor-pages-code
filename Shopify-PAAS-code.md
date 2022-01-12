# **PAAS Shopify Code**


### Step 1: -   Login to shopify store admin panel.
1. Store login >> Themes inside Online store
2. Click on **Themes**
3. Make sure the selected theme or current theme is the **debut theme.**
4.  If the selected theme is different you will have to scroll down the and navigate to free themes and click >> select **Explore free themes.**
5. Select **debut theme**  >> select **default option** >>   select **Add to theme library.**

Theme will be added in the theme library. Now go to  - 

 1. Inside debut theme section click **Actions** >> select **publish**
 2. Now, Current theme will debut.
 3. Select **Actions** button >> select **Edit code** option.
 4. Click on Sections and open **product-template.liquid** file and replace the below code

```
<!--   <div class="loading"> -->
<!--   <img id="loader1" src={{ 'ajax-loader.gif' | asset_img_url: '300x' }} width="36" height="36" alt="loading gif"/> -->

<div class="product-template__container page-width"
  id="ProductSection-{{ section.id }}"
  data-section-id="{{ section.id }}"
  data-section-type="product"
  data-enable-history-state="true"
  data-ajax-enabled="{{ settings.enable_ajax }}"
>

  {% comment %}
    Get first variant, or deep linked one
  {% endcomment %}
  {%- assign current_variant = product.selected_or_first_available_variant -%}
  {%- assign product_image_zoom_size = '1024x1024' -%}
  {%- assign product_image_scale = '2' -%}
  {%- assign enable_image_zoom = section.settings.enable_image_zoom -%}
  {%- assign compare_at_price = current_variant.compare_at_price -%}
  {%- assign price = current_variant.price -%}

  {% case section.settings.media_size %}
    {% when 'small' %}
      {%- assign product_media_width = 'medium-up--one-third' -%}
      {%- assign product_description_width = 'medium-up--two-thirds' -%}
      {%- assign height = 345 -%}
    {% when 'medium' %}
      {%- assign product_media_width = 'medium-up--one-half' -%}
      {%- assign product_description_width = 'medium-up--one-half' -%}
      {%- assign height = 530 -%}
    {% when 'large' %}
      {%- assign product_media_width = 'medium-up--two-thirds' -%}
      {%- assign product_description_width = 'medium-up--one-third' -%}
      {%- assign height = 720 -%}
    {% when 'full' %}
      {%- assign product_media_width = '' -%}
      {%- assign product_description_width = '' -%}
      {%- assign height = 1090 -%}
      {%- assign enable_image_zoom = false -%}
  {% endcase %}

  <div class="grid product-single{% if section.settings.enable_payment_button %} product-single--{{ section.settings.media_size }}-media{% endif %}">
    <div class="grid__item product-single__media-group {{ product_media_width }}{% if section.settings.media_size == 'full' %} product-single__media-group--full{% endif %}" data-product-single-media-group>
      {%- assign featured_media = product.selected_or_first_available_variant.featured_media | default: product.featured_media -%}

      {%- for media in product.media -%}
        {% include 'media', media: media, featured_media: featured_media, height: height, enable_image_zoom: enable_image_zoom, image_zoom_size: product_image_zoom_size, image_scale: product_image_scale %}
      {%- endfor -%}

      <noscript>
        {% capture product_image_size %}{{ height }}x{% endcapture %}
        <img src="{{ featured_media | img_url: product_image_size, scale: product_image_scale }}" alt="{{ featured_media.alt }}" id="FeaturedMedia-{{ section.id }}" class="product-featured-media" style="max-width: {{ height }}px;">
      </noscript>

      {% assign first_3d_model = product.media | where: "media_type", "model" | first %}

      {%- if first_3d_model -%}
        <button
          aria-label="{{ 'products.product.view_in_space_label' | t }}"
          class="product-single__view-in-space"
          data-shopify-xr
          data-shopify-model3d-id="{{ first_3d_model.id }}"
          data-shopify-title="{{ product.title | escape }}"
          data-shopify-xr-hidden
        >
          {% include 'icon-3d-badge-full-color' %}<span class='product-single__view-in-space-text'>{{ 'products.product.view_in_space' | t }}</span>
        </button>
      {%- endif -%}


      {% if product.media.size > 1 %}
        {% if product.media.size > 4 %}
          {%- assign enable_thumbnail_slides = true -%}
        {% endif %}

        <div class="thumbnails-wrapper{% if enable_thumbnail_slides == true %} thumbnails-slider--active{% endif %}">
          {% if enable_thumbnail_slides == true %}
            <button type="button" class="btn btn--link medium-up--hide thumbnails-slider__btn thumbnails-slider__prev thumbnails-slider__prev--{{ section.id }}">
              {% include 'icon-chevron-left' %}
              <span class="icon__fallback-text">{{ 'sections.slideshow.previous_slide' | t }}</span>
            </button>
          {% endif %}
          <ul class="product-single__thumbnails product-single__thumbnails-{{ section.id }}">
            {% for media in product.media %}
              <li class="product-single__thumbnails-item product-single__thumbnails-item--{{ section.settings.media_size }} js">
                <a href="{{ media.preview_image | img_url: product_image_zoom_size, scale: product_image_scale }}"
                   class="text-link product-single__thumbnail product-single__thumbnail--{{ section.id }}"
                   data-thumbnail-id="{{ section.id }}-{{ media.id }}"
                   {% if enable_image_zoom %}data-zoom="{{ media.preview_image | img_url: product_image_zoom_size, scale: product_image_scale }}"{% endif %}>

                    {%- capture thumbnailAlt -%}
                      {%- if media.media_type == 'video' or media.media_type == 'external_video' -%}
                        {{ 'sections.featured_product.video_thumbnail_alt' | t: imageAlt: media.alt | escape }}
                      {%- elsif media.media_type == 'model' -%}
                        {{ 'sections.featured_product.model_thumbnail_alt' | t: imageAlt: media.alt | escape }}
                      {%- else -%}
                        {{ 'sections.featured_product.gallery_thumbnail_alt' | t: imageAlt: media.alt | escape }}
                      {%- endif -%}
                    {%- endcapture -%}

                    <img class="product-single__thumbnail-image" src="{{ media.preview_image | img_url: '110x110', scale: 2 }}" alt="{{ thumbnailAlt }}">
                    {%- if media.media_type == 'video' or media.media_type =='external_video' -%}
                      <div class="product-single__thumbnail-badge">
                        {% include 'icon-video-badge-full-color' %}
                      </div>
                    {%- endif -%}
                    {%- if media.media_type == 'model' -%}
                      <div class="product-single__thumbnail-badge">
                        {% include 'icon-3d-badge-full-color' %}
                      </div>
                    {%- endif -%}
                </a>
              </li>
            {% endfor %}
          </ul>
          {% if enable_thumbnail_slides == true %}
            <button type="button" class="btn btn--link medium-up--hide thumbnails-slider__btn thumbnails-slider__next thumbnails-slider__next--{{ section.id }}">
              {% include 'icon-chevron-right' %}
              <span class="icon__fallback-text">{{ 'sections.slideshow.next_slide' | t }}</span>
            </button>
          {% endif %}
        </div>
      {% endif %}
    </div>

    <div class="grid__item {{ product_description_width }}">
      <div class="product-single__meta">

        <h1 class="product-single__title">{{ product.title }}</h1>

          <div class="product__price">
            {% include 'product-price', variant: current_variant, show_vendor: section.settings.show_vendor %}
          </div>
		  <div class="selectedSlot"></div>
          {%- if shop.taxes_included or shop.shipping_policy.body != blank -%}
            <div class="product__policies rte" data-product-policies>
              {%- if shop.taxes_included -%}
                {{ 'products.product.include_taxes' | t }}
              {%- endif -%}
              {%- if shop.shipping_policy.body != blank -%}
                {{ 'products.product.shipping_policy_html' | t: link: shop.shipping_policy.url }}
              {%- endif -%}
            </div>
          {%- endif -%}

          {% capture "form_classes" -%}
            product-form product-form-{{ section.id }}
            {%- unless section.settings.show_variant_labels %} product-form--hide-variant-labels {% endunless %}
            {%- if section.settings.enable_payment_button and product.has_only_default_variant %} product-form--payment-button-no-variants {%- endif -%}
            {%- if current_variant.available == false %} product-form--variant-sold-out {%- endif -%}
          {%- endcapture %}

          {% form 'product', product, class:form_classes, novalidate: 'novalidate', data-product-form: '' %}
            {% unless product.has_only_default_variant %}
              <div class="product-form__controls-group">
                {% for option in product.options_with_values %}
                  <div class="selector-wrapper js product-form__item">
                    <label {% if option.name == 'default' %}class="label--hidden" {% endif %}for="SingleOptionSelector-{{ forloop.index0 }}">
                      {{ option.name }}
                    </label>
                    <select class="single-option-selector single-option-selector-{{ section.id }} product-form__input"
                      id="SingleOptionSelector-{{ forloop.index0 }}"
                      data-index="option{{ forloop.index }}"
                    >
                      {% for value in option.values %}
                        <option value="{{ value | escape }}"{% if option.selected_value == value %} selected="selected"{% endif %}>{{ value }}</option>
                      {% endfor %}
                    </select>
                  </div>
                {% endfor %}
              </div>
            {% endunless %}

            <select name="id" id="ProductSelect-{{ section.id }}" class="product-form__variants no-js">
              {% for variant in product.variants %}
                <option value="{{ variant.id }}"
                  {%- if variant == current_variant %} selected="selected" {%- endif -%}
                >
                  {{ variant.title }}  {%- if variant.available == false %} - {{ 'products.product.sold_out' | t }}{% endif %}
                </option>
              {% endfor %}
            </select>

            {% if section.settings.show_quantity_selector %}
              <div class="product-form__controls-group">
                <div class="product-form__item">
                  <label for="Quantity-{{ section.id }}">{{ 'products.product.quantity' | t }}</label>
                  <input type="number" id="Quantity-{{ section.id }}"
                    name="quantity" value="1" min="1" pattern="[0-9]*"
                    class="product-form__input product-form__input--quantity" data-quantity-input
                  >
                </div>
              </div>
            {% endif %}

            <div class="product-form__error-message-wrapper product-form__error-message-wrapper--hidden{% if section.settings.enable_payment_button %} product-form__error-message-wrapper--has-payment-button{% endif %}"
              data-error-message-wrapper
              role="alert"
            >
              <span class="visually-hidden">{{ 'general.accessibility.error' | t }} </span>
              {% include 'icon-error' %}
              <span class="product-form__error-message" data-error-message>{{ 'products.product.quantity_minimum_message' | t }}</span>
            </div>

            <div class="product-form__controls-group product-form__controls-group--submit">
              <div class="product-form__item product-form__item--submit
                {%- if section.settings.enable_payment_button %} product-form__item--payment-button {%- endif -%}
                {%- if product.has_only_default_variant %} product-form__item--no-variants {%- endif -%}"
              >
                 
                <!-- quantity box -->
                <div class="inputQuantity">
                	<label for="quantity">Qty: </label>
			<input type="number" id="quantity" name="quantity" value=1 />
                </div>
                <!-- End quantity box -->
                <!-- Line item values -->
                <p class="line-item-property__field">

                  <input type="hidden" class="Date" id="Date" name="properties[Date]" value=""/>
                  <input type="hidden" class="Time" id="Time" name="properties[Time]" value=""/>
                  <input type="hidden" class="service-id" id="service-id" name="properties[service-id]" value=""/>
<!--                   <input type="hidden" class="quantity" id="quantity" name="properties[quantity]" value=""/> -->
                </p>
                <!-- ----->
                <button type="submit" name="add"
                  {% unless current_variant.available %} aria-disabled="true"{% endunless %}
                  aria-label="{% unless current_variant.available %}{{ 'products.product.sold_out' | t }}{% else %}{{ 'products.product.add_to_cart' | t }}{% endunless %}"
                  class="paymentButton btn product-form__cart-submit{% if section.settings.enable_payment_button %} btn--secondary-accent{% endif %}"
                  data-add-to-cart>
                  <span data-add-to-cart-text>
                    {% unless current_variant.available %}
                      {{ 'products.product.sold_out' | t }}
                    {% else %}
                      {{ 'products.product.add_to_cart' | t }}
                    {% endunless %}
                  </span>
                  <span class="hide" data-loader>
                    {% include 'icon-spinner' %}
                  </span>
                </button>
                
                <!-- Product id -->
<!--                  <input type="hidden" id="productId" name="productId" value=""/> -->
<!--                 {{ product.metafields.service }} -->
                <!-- -->

                <!-- Expiration time -->
                
                <div class="expireTime">
                  <span id="demo" class="timeFrame"></span>
                </div>
                
                <!-- End Expiration time -->
                <div id="variant" data-variant="{{ product.selected_or_first_available_variant.id }}"></div>
                <div id="shopifyProductId" data-variant="{{ product.id }}"></div>
                <input type="hidden" id="serviceId" name="serviceId" value="serviceId">
                {%assign key = 'serviceId'%}
               	<input type="hidden" id="serviceTag" name="serviceTag" value={{ key }}>

                <!-- End Fetching Product Tag -->
                
                <!--- Frame --> 
                <a class="btn btn-primary trigger_popup_fricc">Check Availability</a>
                
<label>{{ product.options }}</label>

              
                <div class="hover_bkgr_fricc">
                  <span class="helper"></span>
                  <div>
                    <div class="popupCloseButton">&times;</div>
                     <iframe id="myframe" src=""></iframe>
                  </div>
                </div>
             <!--- End Frame -->
                 <div class="paymentButton"> 
               {% if section.settings.enable_payment_button %}
                  {{ form | payment_button }}
                {% endif %}
                </div>
              </div>
            </div>
          {% endform %}
        </div>

        {%- comment -%}
          Live region for announcing updated price and availability to screen readers
        {%- endcomment -%}
        <p class="visually-hidden" data-product-status
          aria-live="polite"
          role="status"
        ></p>

        {%- comment -%}
          Live region for announcing that the product form has been submitted and the
          product is in the process being added to the cart
        {%- endcomment -%}
        <p class="visually-hidden" data-loader-status
          aria-live="assertive"
          role="alert"
          aria-hidden="true"
        >{{ 'products.product.loader_label' | t }}</p>

        <div class="product-single__description rte">
          {{ product.description }}
        </div>

        {% if section.settings.show_share_buttons %}
          {% include 'social-sharing', share_title: product.title, share_permalink: product.url, share_image: product.featured_media %}
        {% endif %}
    </div>
  </div>
</div>
<!-- </div> -->
 
{% unless product == empty %}
  <script type="application/json" id="ProductJson-{{ section.id }}">
    {{ product | json }}
  </script>
  <script type="application/json" id="ModelJson-{{ section.id }}">
    {{ product.media | where: 'media_type', 'model' | json }}
  </script>
{% endunless %}



{% schema %}
{
  "name": {
    "da": "Produktsider",
    "de": "Produktseiten",
    "en": "Product pages",
    "es": "Páginas de productos",
    "fi": "Tuotesivut",
    "fr": "Pages de produits",
    "hi": "उत्पाद पेज",
    "it": "Pagine di prodotto",
    "ja": "商品ページ",
    "ko": "제품 페이지",
    "nb": "Produktsider",
    "nl": "Productpagina's",
    "pt-BR": "Páginas de produtos",
    "pt-PT": "Páginas de produtos",
    "sv": "Produktsidor",
    "th": "หน้าสินค้า",
    "zh-CN": "产品页面",
    "zh-TW": "產品頁面"
  },
  "settings": [
    {
      "type": "checkbox",
      "id": "show_quantity_selector",
      "label": {
        "da": "Vis antalsvælger",
        "de": "Quantitäts-Auswahl anzeigen",
        "en": "Show quantity selector",
        "es": "Mostrar selector de cantidad",
        "fi": "Näytä määrän valitsin",
        "fr": "Afficher le sélecteur de quantité",
        "hi": "मात्रा चयनकर्ता दिखाएं",
        "it": "Mostra selettore quantità",
        "ja": "数量セレクターを表示する",
        "ko": "수량 선택기 표시",
        "nb": "Vis mengdevelger",
        "nl": "Hoeveelheidskiezer weergeven",
        "pt-BR": "Exibir seletor de quantidade",
        "pt-PT": "Mostrar um seletor de quantidade",
        "sv": "Visa kvantitetsväljare",
        "th": "แสดงตัวเลือกจำนวน",
        "zh-CN": "显示数量选择器",
        "zh-TW": "顯示數量選擇器"
      },
      "default": false
    },
    {
      "type": "checkbox",
      "id": "show_variant_labels",
      "label": {
        "da": "Vis variantlabels",
        "de": "Varianten-Etiketten anzeigen",
        "en": "Show variant labels",
        "es": "Mostrar etiquetas de variantes",
        "fi": "Näytä vaihtoehtoiset tarrat",
        "fr": "Afficher le nom des variantes",
        "hi": "वेरिएंट लेबल दिखाएं",
        "it": "Mostra etichette varianti",
        "ja": "バリエーションのラベルを表示する",
        "ko": "이형 상품 레이블 표시",
        "nb": "Vis variantetiketter",
        "nl": "Variantlabels weergeven",
        "pt-BR": "Exibir etiquetas de variantes",
        "pt-PT": "Mostrar etiquetas de variantes",
        "sv": "Visa variantetiketter",
        "th": "แสดงป้ายกำกับตัวเลือกสินค้า",
        "zh-CN": "显示多属性标签",
        "zh-TW": "顯示子類選項標籤"
      },
      "default": true
    },
    {
      "type": "checkbox",
      "id": "show_vendor",
      "label": {
        "da": "Vis leverandør",
        "de": "Lieferanten anzeigen",
        "en": "Show vendor",
        "es": "Mostrar proveedor",
        "fi": "Näytä myyjä",
        "fr": "Afficher les vendeurs",
        "hi": "विक्रेता दिखाएं",
        "it": "Mostra fornitore",
        "ja": "販売元を表示する",
        "ko": "공급업체 표시",
        "nb": "Vis leverandør",
        "nl": "Leverancier weergeven",
        "pt-BR": "Exibir fornecedor",
        "pt-PT": "Mostrar fornecedor",
        "sv": "Visa säljare",
        "th": "แสดงผู้ขาย",
        "zh-CN": "显示厂商",
        "zh-TW": "顯示廠商"
      },
      "default": false
    },
    {
      "type": "checkbox",
      "id": "enable_payment_button",
      "label": {
        "da": "Vis dynamisk betalingsknap",
        "de": "Dynamischen Checkout Button anzeigen",
        "en": "Show dynamic checkout button",
        "es": "Mostrar botón de pago dinámico",
        "fi": "Näytä dynaaminen kassapainike",
        "fr": "Afficher le bouton de passage à la caisse dynamique",
        "hi": "डायनेमिक चेकआउट बटन दिखाएं",
        "it": "Mostra pulsante di check-out dinamico",
        "ja": "ダイナミックチェックアウトボタンを表示する",
        "ko": "동적 결제 버튼 표시",
        "nb": "Vis dynamisk knapp for å gå til kassen",
        "nl": "Dynamische checkout knop weergeven",
        "pt-BR": "Exibir botão dinâmico de finalização da compra",
        "pt-PT": "Mostrar o botão dinâmico de finalização da compra",
        "sv": "Visa dynamiska utcheckningsknappar",
        "th": "แสดงปุ่มชำระเงินแบบไดนามิก",
        "zh-CN": "显示动态结账按钮",
        "zh-TW": "顯示動態結帳按鈕"
      },
      "info": {
        "da": "Den enkelte kunde vil se sin foretrukne betalingsmetode blandt dem, der er tilgængelige i din butik, f.eks. PayPal eller Apple Pay. [Få mere at vide](https://help.shopify.com/manual/using-themes/change-the-layout/dynamic-checkout)",
        "de": "Jeder Kunde sieht seine bevorzugte Zahlungsmethode aus den in Ihrem Shop verfügbaren Zahlungsmethoden wie PayPal oder Apple Pay. [Mehr Informationen](https://help.shopify.com/manual/using-themes/change-the-layout/dynamic-checkout)",
        "en": "Each customer will see their preferred payment method from those available on your store, such as PayPal or Apple Pay. [Learn more](https://help.shopify.com/manual/using-themes/change-the-layout/dynamic-checkout)",
        "es": "Cada cliente verá su forma de pago preferida entre las disponibles en tu tienda, como PayPal o Apple Pay. [Más información](https://help.shopify.com/manual/using-themes/change-the-layout/dynamic-checkout)",
        "fi": "Kukin asiakas näkee ensisijaisen valintansa kauppasi tarjoamista maksutavoista, esim. PayPal tai Apple Pay. [Lisätietoja](https://help.shopify.com/manual/using-themes/change-the-layout/dynamic-checkout)",
        "fr": "Chaque client verra son moyen de paiement préféré parmi ceux qui sont proposés sur votre boutique, tels que PayPal ou Apple Pay. [En savoir plus](https://help.shopify.com/manual/using-themes/change-the-layout/dynamic-checkout)",
        "hi": "प्रत्येक ग्राहक आपके स्टोर पर उपलब्ध अपनी पसंदीदा भुगतान की विधि देखेंगे जैसे PayPal या Apple Pay. [अधिक जानें](https://help.shopify.com/manual/using-themes/change-the-layout/dynamic-checkout)",
        "it": "Ogni cliente vedrà il suo metodo di pagamento preferito tra quelli disponibili nel tuo negozio, come PayPal o Apple Pay. [Maggiori informazioni](https://help.shopify.com/manual/using-themes/change-the-layout/dynamic-checkout)",
        "ja": "PayPalやApple Payなど、ストアで利用可能な希望の決済方法がお客様に表示されます。[詳細情報](https://help.shopify.com/manual/using-themes/change-the-layout/dynamic-checkout)",
        "ko": "각 고객은 PayPal 또는 Apple Pay와 같이 스토어에서 사용 가능한 지불 방법을 확인할 수 있습니다. [자세히 알아보기](https://help.shopify.com/manual/using-themes/change-the-layout/dynamic-checkout)",
        "nb": "Hver enkelt kunde vil se sin foretrukne betalingsmåte blant de som er tilgjengelig i butikken din, som PayPal eller Apple Pay. [Finn ut mer](https://help.shopify.com/manual/using-themes/change-the-layout/dynamic-checkout)",
        "nl": "Elke klant ziet zijn of haar beschikbare voorkeursmethode om af te rekenen, zoals PayPal of Apple Pay. [Meer informatie](https://help.shopify.com/manual/using-themes/change-the-layout/dynamic-checkout)",
        "pt-BR": "Cada cliente verá sua forma de pagamento preferida dentre as disponíveis na loja, como PayPal ou Apple Pay. [Saiba mais](https://help.shopify.com/manual/using-themes/change-the-layout/dynamic-checkout)",
        "pt-PT": "Cada cliente irá ver o seu método de pagamento preferido entre os disponíveis na loja, como o PayPal ou Apple Pay. [Saiba mais](https://help.shopify.com/manual/using-themes/change-the-layout/dynamic-checkout)",
        "sv": "Varje kund kommer att se den föredragna betalningsmetoden från de som finns tillgängliga i din butik, till exempel PayPal eller Apple Pay. [Läs mer](https://help.shopify.com/manual/using-themes/change-the-layout/dynamic-checkout)",
        "th": "ลูกค้าแต่ละรายจะเห็นวิธีการชำระเงินที่ต้องการจากวิธีที่ใช้ได้ในร้านค้าของคุณ เช่น PayPal หรือ Apple Pay [เรียนรู้เพิ่มเติม](https://help.shopify.com/manual/using-themes/change-the-layout/dynamic-checkout)",
        "zh-CN": "每位客户都可在您商店提供的付款方式中看到他们的首选付款方式，例如 PayPal 或 Apple Pay。[了解详细信息](https://help.shopify.com/manual/using-themes/change-the-layout/dynamic-checkout)",
        "zh-TW": "每位顧客都可以在您商店內開放使用的付款方式中看見他們偏好使用的方式，如 PayPal、Apple Pay 等。[深入瞭解](https://help.shopify.com/manual/using-themes/change-the-layout/dynamic-checkout)"
      },
      "default": true
    },
    {
      "type": "checkbox",
      "id": "show_share_buttons",
      "label": {
        "da": "Vis knapper til deling på sociale medier",
        "de": "Buttons für Social Media anzeigen",
        "en": "Show social sharing buttons",
        "es": "Mostrar botones para compartir en redes sociales",
        "fi": "Näytä sosiaalisen median jakamispainikkeet",
        "fr": "Affichez les boutons de partage sur les médias sociaux",
        "hi": "सोशल शेयरिंग बटन दिखाएं",
        "it": "Mostra i pulsanti per la condivisione sui social",
        "ja": "ソーシャル共有ボタンを表示する",
        "ko": "소셜 공유 버튼 표시",
        "nb": "Vis knapper for deling på sosiale medier",
        "nl": "Knoppen voor sociaal delen weergeven",
        "pt-BR": "Exibir botões de compartilhamento em redes sociais",
        "pt-PT": "Mostrar botões de partilha nas redes sociais",
        "sv": "Visa knappar för delning i sociala medier",
        "th": "แสดงปุ่มสำหรับแชร์ลงโซเชียล",
        "zh-CN": "显示社交分享按钮",
        "zh-TW": "顯示社群分享按鈕"
      },
      "default": true
    },
    {
      "type": "header",
      "content": {
        "da": "Medie",
        "de": "Medien",
        "en": "Media",
        "es": "Medios de comunicación",
        "fi": "Media",
        "fr": "Support multimédia",
        "hi": "मीडिया",
        "it": "Media",
        "ja": "メディア",
        "ko": "미디어",
        "nb": "Medier",
        "nl": "Media",
        "pt-BR": "Mídia",
        "pt-PT": "Multimédia",
        "sv": "media",
        "th": "สื่อ",
        "zh-CN": "媒体",
        "zh-TW": "媒體"
      },
      "info": {
        "da": "Få mere at vide om [media types](https://help.shopify.com/manual/products/product-media)",
        "de": "Mehr Informationen über [Medientypen ](https://help.shopify.com/manual/products/product-media)",
        "en": "Learn more about [media types](https://help.shopify.com/manual/products/product-media)",
        "es": "Más información sobre [tipos de archivos multimedia](https://help.shopify.com/manual/products/product-media)",
        "fi": "Lue lisää [mediatyypeistä](https://help.shopify.com/manual/products/product-media)",
        "fr": "En savoir plus sur les [types de supports multimédia](https://help.shopify.com/manual/products/product-media)",
        "hi": "[मीडिया प्रकार](https://help.shopify.com/manual/products/product-media) के बारे में और जानें",
        "it": "Scopri di più sulle [tipologie di file multimediali](https://help.shopify.com/manual/products/product-media)",
        "ja": "[メディアのタイプ](https://help.shopify.com/manual/products/product-media) について詳しく知る",
        "ko": "[미디어 유형](https://help.shopify.com/manual/products/product-media)에 대해 자세히 알아보기",
        "nb": "Lær mer om [medietyper](https://help.shopify.com/manual/products/product-media)",
        "nl": "Meer informatie over [mediatypen](https://help.shopify.com/manual/products/product-media)",
        "pt-BR": "Saiba mais sobre [tipos de mídia](https://help.shopify.com/manual/products/product-media)",
        "pt-PT": "Saiba mais sobre [media types](https://help.shopify.com/manual/products/product-media)",
        "sv": "Läs mer om [mediatyper](https://help.shopify.com/manual/products/product-media)",
        "th": "ดูข้อมูลเพิ่มเติมเกี่ยวกับ [ประเภทของสื่อ](https://help.shopify.com/manual/products/product-media)",
        "zh-CN": "详细了解 [媒体类型](https://help.shopify.com/manual/products/product-media)",
        "zh-TW": "深入瞭解 [媒體類型](https://help.shopify.com/manual/products/product-media)"
      }
    },
    {
      "type": "select",
      "id": "media_size",
      "label": {
        "da": "Størrelse",
        "de": "Größe",
        "en": "Size",
        "es": "Tamaño",
        "fi": "Koko",
        "fr": "Taille",
        "hi": "आकार",
        "it": "Dimensione",
        "ja": "サイズ",
        "ko": "사이즈",
        "nb": "Størrelse",
        "nl": "Grootte",
        "pt-BR": "Tamanho",
        "pt-PT": "Tamanho",
        "sv": "Storlek",
        "th": "ขนาด",
        "zh-CN": "大小",
        "zh-TW": "尺寸"
      },
      "options": [
        {
          "value": "small",
          "label": {
            "da": "Lille",
            "de": "Klein",
            "en": "Small",
            "es": "Pequeño",
            "fi": "Pieni",
            "fr": "Petit",
            "hi": "छोटा",
            "it": "Piccolo",
            "ja": "スモール",
            "ko": "스몰",
            "nb": "Liten",
            "nl": "Klein",
            "pt-BR": "Pequeno",
            "pt-PT": "Pequeno",
            "sv": "Liten",
            "th": "เล็ก",
            "zh-CN": "小",
            "zh-TW": "小型"
          }
        },
        {
          "value": "medium",
          "label": {
            "da": "Medium",
            "de": "Mittel",
            "en": "Medium",
            "es": "Mediano",
            "fi": "Keskisuuri",
            "fr": "Moyenne",
            "hi": "मध्यम",
            "it": "Medio",
            "ja": "中",
            "ko": "보통",
            "nb": "Middels",
            "nl": "Gemiddeld",
            "pt-BR": "Médio",
            "pt-PT": "Médio",
            "sv": "Medium",
            "th": "ปานกลาง",
            "zh-CN": "中等",
            "zh-TW": "中等"
          }
        },
        {
          "value": "large",
          "label": {
            "da": "Stor",
            "de": "Groß",
            "en": "Large",
            "es": "Grande",
            "fi": "Suuri",
            "fr": "Grande",
            "hi": "बड़ा",
            "it": "Grande",
            "ja": "大",
            "ko": "라지",
            "nb": "Stor",
            "nl": "Groot",
            "pt-BR": "Grande",
            "pt-PT": "Grande",
            "sv": "Stor",
            "th": "ใหญ่",
            "zh-CN": "大",
            "zh-TW": "大型"
          }
        },
        {
          "value": "full",
          "label": {
            "da": "Fuld bredde",
            "de": "Volle Breite",
            "en": "Full-width",
            "es": "Ancho completo",
            "fi": "Täysi leveys",
            "fr": "Pleine largeur",
            "hi": "पूर्ण चौड़ाई",
            "it": "Intera larghezza",
            "ja": "全幅",
            "ko": "전체 폭",
            "nb": "Full bredde",
            "nl": "Volledige breedte",
            "pt-BR": "Largura completa",
            "pt-PT": "Largura completa",
            "sv": "Full bredd",
            "th": "เต็มความกว้าง",
            "zh-CN": "全宽",
            "zh-TW": "完整寬度"
          }
        }
      ],
      "default": "medium"
    },
    {
      "type": "checkbox",
      "id": "enable_image_zoom",
      "label": {
        "da": "Aktivér billedzoom",
        "de": "Foto-Zoom zulassen",
        "en": "Enable image zoom",
        "es": "Habilitar zoom de imagen",
        "fi": "Ota kuvan zoomaus käyttöön",
        "fr": "Activer le zoom d'image",
        "hi": "इमेज ज़ूम सक्षम करें",
        "it": "Abilita lo zoom dell'immagine",
        "ja": "画像ズームを有効にする",
        "ko": "이미지 확대 사용",
        "nb": "Aktiver bildezoom",
        "nl": "Inzoomen op afbeelding inschakelen",
        "pt-BR": "Habilitar o zoom da imagem",
        "pt-PT": "Ativar o zoom da imagem",
        "sv": "Aktivera bildzoom",
        "th": "เปิดใช้การซูมภาพ",
        "zh-CN": "启用图片缩放",
        "zh-TW": "啟用圖片縮放"
      },
      "default": true
    },
    {
      "type": "checkbox",
      "id": "enable_video_looping",
      "label": {
        "da": "Aktivér looping af videoer",
        "de": "Videschleife aktivieren",
        "en": "Enable video looping",
        "es": "Habilitar la reproducción de video en bucle",
        "fi": "Ota käyttöön videosilmukka",
        "fr": "Activer le bouclage de la vidéo",
        "hi": "वीडियो लूपिंग सक्षम करें",
        "it": "Abilita la riproduzione in loop dei video",
        "ja": "ビデオのループを有効にする",
        "ko": "동영상 루프",
        "nb": "Aktiver løkkeavspilling av video",
        "nl": "Video-looping inschakelen",
        "pt-BR": "Habilitar loop de vídeo",
        "pt-PT": "Ativar ciclo de vídeo",
        "sv": "Aktivera video-loopning",
        "th": "เปิดใช้งานการวนซ้ำวิดีโอ",
        "zh-CN": "启用视频循环",
        "zh-TW": "啟用影片循環功能"
      },
      "default": false
    }
  ]
}
{% endschema %}
```
       
### Step 2: Add Two new asset files.
1. Client on **Asset** >> click **Add a new asset**
2. **Create a blank file** >> Add file **custom.js** and copy below code -     

/* code for custom.js*/

    var interval;
	var cartItem;
	var slotServiceId = $('#serviceId').val();
    var token = "token";	
    var serviceUrl = "https://beta-service.marketcube.io/api"; // for BETA environment
			 // for TEST environment change to 
		         //"https://test-service.marketcube.io/api";  
			 // for UAT environment change to
			 //"https://uat-service.marketcube.io/api";
    var clientUrl = "https://mc-beta-paas-ui.onrender.com"; // for BETA environment
			 // for TEST environment change to
		    	 //"https://mc-test-product-as-a-service-ui.onrender.com";
		    	 // for UAT environment change to
		    	 //"https://mc-uat-paas-ui.onrender.com";
    var timeObj = {};
    var tempTimeObj = {};
    var timerObj = {};
    var tempTimerObj = {};

    async function userIp() {
        let result = await $.getJSON("https://api.ipify.org?format=json",
            function (data) {
                return data.ip;
            })
        return result.ip;
        }
    
    async function createToken() {
        let ip = await userIp();
        let dt = new Date();
        let time = dt.getHours() + ":" + dt.getMinutes() + ":" + dt.getSeconds();
        let date = dt.getDate() + "-" + (dt.getMonth() + 1) + "-" + dt.getFullYear();
        // Encrypt
        const encryptdata = `${ip}${date}${time}`;
        let ciphertext = CryptoJS.AES.encrypt(encryptdata, 'custom').toString();
        ciphertext = ciphertext.replace(/[^a-zA-Z ]/g, "");
        console.log('ciphertext', ciphertext);
        // Decrypt
        //   let bytes = CryptoJS.AES.decrypt(ciphertext.toString(), 'custom');
        //   let plaintext = bytes.toString(CryptoJS.enc.Utf8);
        return ciphertext;
    }
    
    function setLocalStorage(key, value) {
        return localStorage.setItem(key, value);
    }
    
    function getLocalStorage(key) {
        return localStorage.getItem(key);
    }
    
    function removeLocalStorage(key) {
        return localStorage.removeItem(key);
    }
    
    async function checkLocalStorage() {
        let user = getLocalStorage(token);
        if (user != null) {
            return user;
        }
        else {
            const res = await createToken();
            setLocalStorage(token, res);
            let user = getLocalStorage(token);
            return user;
        }
    }

    popUp = () => {
        console.log('popup called');
        $('.timerPopUp').show();
        $('.timerCloseButton').click(function () {
            $('.timerPopUp').hide();
            $('.paymentButton').hide();
            $('.selectedSlot').hide();
            $('.expireTime').hide();
            $('#Date').val('');
            $('#Time').val('');
            $('#service-id').val('');
            location.reload();
        });
    
    }
    
    serviceStatusRemove = (productId) => {
        console.log('serviceStatusRemove productId', productId);
        $.ajax({
            url: `${serviceUrl}/service/booking/${productId}`,
            type: 'PUT',
            data: {
                status: 'removed',
            },
            error: async function () {
                return true;
            },
            success: async function (result) {
                console.log('result', result);
                return result;
            }
        });
    }

    serviceStatusProvisional = (productId) => {
        console.log('serviceStatusProvisional productId', productId);
        $.ajax({
            url: `${serviceUrl}/service/booking/${productId}`,
            type: 'PUT',
            data: {
                status: 'provisional',
            },
            error: function () {
                return true;
            },
            success: function (result) {
                $('.paymentButton').hide();
                $('.selectedSlot').hide();
                $('.expireTime').hide();
                $('#Date').val('');
                $('#Time').val('');
                $('#service-id').val('');
            }
        });
    }
    
    function isEmptyObject(value) {
        return Object.keys(value).length === 0 && value.constructor === Object;
    }
    function startTimer(duration, display, productId, slotServiceId) {
        console.log('slotServiceId', slotServiceId);
        console.log('productId', productId);
        let minutes;
        let seconds;
        let localStorageTimer = JSON.parse(getLocalStorage('Timer'));
	    let timer = localStorageTimer && localStorageTimer[productId] ? localStorageTimer[productId] : 900;
  	
        console.log('timer', timeObj);
        clearInterval(interval);
        interval = setInterval(function () {
        minutes = parseInt(timer / 60, 10);
        seconds = parseInt(timer % 60, 10);
        minutes = minutes < 10 ? "0" + minutes : minutes;
        seconds = seconds < 10 ? "0" + seconds : seconds;

        display.text(`Provisionally booked for ${minutes} : ${seconds}`);

        if (--timer < 0) {
            removeLocalStorage('Timer');
            serviceStatusRemove(productId, slotServiceId);
            popUp();
            display.text(` Your Order has been expired `);
            clearInterval(interval);
            return false;
        }
        removeLocalStorage('Timer');
        if (tempTimeObj.hasOwnProperty(productId)) {
            tempTimeObj = { [productId]: timer };
        }
        else {
            tempTimeObj = { [productId]: timer };
        }
        setLocalStorage('Timer', JSON.stringify(tempTimeObj));
        tempTimerObj = JSON.parse(getLocalStorage('Timer'));
        timer = tempTimerObj[productId]
    }, 1000);
    }
    
    getKeyByValue = (object, value) => {
        return Object.keys(object).filter(key => object[key] == value);
    }
    
    getKey = (object, value) => {
        return Object.keys(object).find(key => key == value).toString()
    }
    
    getMaxValue = (object) => {
        let arr = Object.keys(object).map(key => object[key]);
        return Math.max(...arr);
    }
    
    domFind = (deleteKey) => {

    $.getJSON('/cart.js', (cart) => {
        console.log('cart item>>>>', cart);
        cartItem = cart;
    });
    console.log('cart domFind>>>>>', cartItem)
    removeProduct(deleteKey, cartItem)

    }
    
    function cartTimer(productId, duration) {
        console.log('duration', duration);
        console.log('productId', productId);
        localStorageObj = JSON.parse(getLocalStorage('cartTimer'));
        //     localStorageObjStringify = JSON.stringify(getLocalStorage('cartTimer'));
        console.log('localStorageObj', localStorageObj);
        //   	console.log('localStorageObjStringify>>>>>>>>>>>>>',localStorageObjStringify);
        let timer = 0;
        let minutes;
        let seconds;
        timer = duration;
        //     clearInterval(interval);
        let interval = setInterval(function () {
            minutes = parseInt(timer / 60, 10);
            seconds = parseInt(timer % 60, 10);
            minutes = minutes < 10 ? "0" + minutes : minutes;
            seconds = seconds < 10 ? "0" + seconds : seconds;
            let checkValue = getKeyByValue(localStorageObj, "0");
            //       	console.log('timer>>>>>>>>>',timer);
            //       	console.log('checkValue in cartTimer>>>>',checkValue)
            if (checkValue.length > 0) {
                localStorageObj = JSON.parse(getLocalStorage('cartTimer'));
                checkValue.map(item => {
                    if (localStorageObj.hasOwnProperty(checkValue)) {
                        //                 console.log('caleed');
                        let deleteKey = getKey(localStorageObj, checkValue);
                        //                 console.log('deleteKey in cartTimer >>>>',deleteKey);
                        //                 console.log('deleteKey in cartTimer >>>>',JSON.stringify(deleteKey));
                        //                 popUp();
                        domFind(deleteKey)
                        delete localStorageObj[deleteKey];
                        serviceStatusRemove(deleteKey, slotServiceId)
                        //                 console.log('localStorageObj in cartTimer',localStorageObj)
                        setLocalStorage('cartTimer', JSON.stringify(localStorageObj));
                    }
                    //               $('.form.cart').
                });
            }
            if (--timer < 0) {
                console.log('cleed')
                //           	popUp();
                //           	removeLocalStorage('cartTimer');
                //             $('.cart__remove a').trigger('click')
                clearInterval(interval);
                return false;
            }
            if (localStorageObj.hasOwnProperty(productId)) {
                //       		console.log('orp',productId);
                //           	console.log('timer',timer);
                let addObj = $.extend(true, localStorageObj, {
                    [String(productId)]: timer,
                    //               localStorageObj[productId]: timer
                });
                //           	console.log('addObj',addObj)
                removeLocalStorage('cartTimer');
                localStrageObj = { ...addObj }
                //             console.log('localStorageObj',localStorageObj);
                setLocalStorage('cartTimer', JSON.stringify(localStrageObj));
                timerObj = JSON.parse(getLocalStorage('cartTimer'));
                //           	console.log('timerObj',timerObj)
                timer = timerObj[productId]
                //           	console.log('timer',timer)
            }
            //         removeLocalStorage('Timer');
            //      setLocalStorage('cartTimer', timer);
            //      timer = getLocalStorage('Timer');
        }, 1000);
    }
    
    getMapValue = (obj, key) => {
        //   	console.log('obj',obj);
        //   	console.log('key',key);
        if (obj.hasOwnProperty(key))
            return obj[key];
        throw new Error("Invalid map key.");
    }
    
    cartTimerIntialization = (productId) => {
        console.log('cartTimerIntialization', productId)
        if (productId) {
            //       console.log('callled')
            //       console.log('getLocalStorage',getLocalStorage('cartTimer'));
            localStorageObj = JSON.parse(getLocalStorage('cartTimer'));
            console.log('localStorageObj', localStorageObj)
            timer = getMapValue(localStorageObj, productId);
            //       console.log('timer',timer);
            //       if(timer == 0){
            //       	console.log('ddddd');
            //       	$('.cart__remove').trigger('click')
            //       }
            //       else
            //       {
            //       	cartTimer(productId,timer)
            //       }
            console.log('getMaxValue', getMaxValue(localStorageObj));
            cartTimer(productId, timer)
        }
    
    }
    
    cartContents = () => {
        console.log('called cartcontents')
        $.getJSON('/cart.js', (cart) => {
            console.log('cart item>>>>', cart);
            cartItem = cart;
            cart.items.map(item => {
                //       	console.log('item',item);
                //       	tempId = item.properties['service-id'];
                let productId = getMapValue(item.properties, 'service-id');
                cartTimerIntialization(productId);
            })
            //      removeProduct(tempId,cartItem);
        });
    }
    
    removeProduct = (tempId, cartItem) => {
        let index = cartItem.items.findIndex(p => {
            return p.properties['service-id'] == tempId
        });
        var data = {
            'quantity': 0,
            'line': index + 1,
        }
    $.ajax({
        type: 'post',
        url: '/cart/change.js',
        data: data,
        success: function (res) {
            console.log("removeProduct response>>>>>>>", res);
            popUp();
    //           	$('.template-cart').load(location.href + '#PageContainer');
            },
            dataType: 'json'
        });
    }
    
    serviceIdCheck = () => {
      console.log('calledd>>>>')
      let variantId = window.location.search.split('variant=')[1] || $('#variant').attr('data-variant');
        let shopifyProductId = $('#shopifyProductId').attr('data-variant');
         $('#variant').attr('data-variant',variantId);
    	  	console.log('variantId',variantId);
        	console.log('shopifyProductId',shopifyProductId);
        	$.ajax({
              type:'get',
              url:`https://${window.location.hostname}/admin/api/2020-04/products/${shopifyProductId}/variants/${variantId}/metafields.json`,
              //`https://product-as-a-service.myshopify.com/admin/api/2020-04/products/${shopifyProductId}/variants/${variantId}/metafields.json`,
              success: function (result) {
                console.log('>>>>>>>>>>>>>>',result)
                if(result.metafields.length > 0){
    //             $('#serviceTag').val(result.metafields[0].key)
                console.log('result',result.metafields);
                $('#serviceId').val(result.metafields[0].value);
                slotServiceId = $('#serviceId').val();
                  if(result.metafields[0].value){
                  	$('.trigger_popup_fricc').show();
                    $('.paymentButton').hide();
                    console.log('slotServiceId',slotServiceId)
                  }
                }
                else{
                  $('#serviceId').val('');
    //               $('#service-id').val('');
                  $('.trigger_popup_fricc').hide();
                  $('.paymentButton').show();
                }
          	
          }
        })
    }
    $(document).ready(function () {
      //variant
    //   $('.selector-wrapper.js.product-form__item').hide();
      //end
      	console.log('location>>>>')
        $.ajax({
            type: 'get',
            url: '/admin/api/2020-10/locations.json',
            success: function (res) {
                console.log("location response>>>>>>>", res);
            },
        });
      	$.ajax({
            type: 'get',
            url: '/admin/api/2020-10/storefront_access_tokens.json',
            success: function (res) {
                console.log("storefront access tokens>>>>>>>", res);
            },
        });
      	$.ajax({
            type: 'get',
            url: '/admin/api/2020-10/shop.json',
            success: function (res) {
                console.log("shop response>>>>>>>", res);
            },
        });
    //   	$('.trigger_popup_fricc').hide();
        let productId;
        let display = $('#demo');
        cartContents();
      	if( window.location.search.split('variant=')[1] || $('#variant').attr('data-variant')){ 
          	console.log('>>>>>>>lkdldksslk')
      		serviceIdCheck();
      	 }
     	 $('select#SingleOptionSelector-0').click(function(){
       		serviceIdCheck();
        });
        let serviceTag = $('#serviceTag').val();
     	console.log('serviceTag',serviceTag)
        if (serviceTag === 'serviceId') {
            $('.paymentButton').hide();
    		console.log('inside side')
            $(".trigger_popup_fricc").click(async function () {
                const token = await checkLocalStorage();
              console.log('clientUrl', clientUrl);
              let shopifyProductId = $('#shopifyProductId').attr('data-variant');
              var shopifyToken = "2e0182580669522fa232501231d70721";
              console.log('shopifyProductId', shopifyProductId);
                $('.hover_bkgr_fricc').show();
                   $('#myframe').attr('src', `${clientUrl}/slotbooking/${slotServiceId}&token=${token}`);
    //            $('#myframe').attr('src', `${clientUrl}/slotbooking/${slotServiceId}&token=${token}&productId=${shopifyProductId}&shopifyToken=${shopifyToken}`);
            });         

  


        // Iframe hide making get request
        $('.hover_bkgr_fricc').click(async function () {
            $('#myframe').attr('src', ``);
            //removeLocalStorage();
            $('.hover_bkgr_fricc').hide();

            let token;
            token = await checkLocalStorage();
            //removeLocalStorage();
            $.ajax({
                url: `${serviceUrl}/service/bookings/?token=${token}`,
                type: 'GET',
                data: {
                    status: 'inProgress',
                    serviceId: slotServiceId
                },
                error: function () {
                    return true;
                },
                success: function (result) {
                    if (result.data.length) {
                        $('.selectedSlot').show();
                        $('.expireTime').show();
                        productId = result.data[0]._id
                        let quantity = result.data[0].quantity;
                      	// for variant
    //                       for(let i= 0 ; i < result.data[0].metafields.length ; i++){
    //                         console.log('element', result.data[0].metafields[i]);
    //                          const varaintPositionID = `#SingleOptionSelector-${result.data[0].metafields[i].position}`;
    //                       	 const variantLabel = result.data[0].metafields[i].namespace;
    //                       	 const variantName = result.data[0].metafields[i].variantName;
    //                          console.log('varaintPositionID', varaintPositionID);
    //                       $(varaintPositionID).val(variantName).change();
    //                         $('.selector-wrapper.js.product-form__item').show();
                          
    //                       };
                             	
                     	// end
                        console.log('result',result)
                        $('#quantity').val(`${quantity}`);
                        let slotStartTime = new Date(result.data[0].startTime);
                        let startDateSlot = `${slotStartTime.getDate()}-${slotStartTime.getMonth() + 1}-${slotStartTime.getFullYear()}`;
                        //           console.log('date === ', startDateSlot);
                        let startTimeSlot = `${slotStartTime.getHours()}:${(slotStartTime.getMinutes()) < 10 ? '0' : ''}${slotStartTime.getMinutes()}`;
                      	console.log('slotStartTime',slotStartTime)
                        console.log('startTimeSlot',startTimeSlot)
                        console.log('slotStartTime',slotStartTime.getMinutes())


                        const slotEndTime = new Date(result.data[0].endTime);
                        let endDateSlot = `${slotEndTime.getDate()}-${slotEndTime.getMonth() + 1}-${slotEndTime.getFullYear()}`;
                        let endTimeSlot = `${slotEndTime.getHours()}:${slotEndTime.getMinutes()}${(slotEndTime.getMinutes()) < 10 ? '0' : ''}`;

                        if (startDateSlot !== endDateSlot) {
                            //             console.log('adads');
                            $('.selectedSlot').html(`
			<div><b>Date:</b> ${startDateSlot} - ${endDateSlot}</div>
          	<div><b>Slot:</b> ${startTimeSlot} - ${endTimeSlot}</div>
          `);
                            $('#Date').val(`${startDateSlot} - ${endDateSlot}`);
                            $('#Time').val(`${startTimeSlot} - ${endTimeSlot}`);
                            $('#service-id').val(`${productId}`);
                        }
                        else {
                            //             console.log('eeee');
                            $('.selectedSlot').html(`
          <div><b>Date:</b> ${startDateSlot}</div>
          <div><b>Time:</b> ${startTimeSlot} - ${endTimeSlot}</div>
          `);
                            $('#Date').val(`${startDateSlot}`);
                            $('#Time').val(`${startTimeSlot} - ${endTimeSlot}`);
                            $('#service-id').val(`${productId}`);
                        }
                        //Old date conversion
                        let Minutes = 60 * result.data[0].expiresIn;
                        //                         setLocalStorage('Timer', Minutes);
                        startTimer(Minutes, display, productId, slotServiceId);
                        $('.hover_bkgr_fricc').hide();
                        if (result.status === 'ok') {
                            $('.paymentButton').show();
                          $('.paymentButton .shopify-payment-button').hide();
                          
                        }
                    }
                  else{
                    $('.selectedSlot').hide();
                    $('.expireTime').hide();
                    $('#Date').val('');
                    $('#Time').val('');
                    $('#service-id').val('');
                  }
                }
            });

        });



        // Add to cart button for making status provisional
     if(slotServiceId){
       	console.log('slotServiceId?>>>>>>>>>>>>>>',slotServiceId)
        $('.paymentButton').click(function () {
            let productId = $('#service-id').val();
            if (JSON.parse(getLocalStorage('Timer'))) {
                localStorageObj = JSON.parse(getLocalStorage('Timer'));
                if (localStorageObj.hasOwnProperty(productId)) {
                    timer = localStorageObj[productId];
                    timeObj = JSON.parse(getLocalStorage('cartTimer'));
                    if (timeObj) {
                        console.log('timeObj if block', timeObj)
                        timeObj = { ...timeObj, ...{ [String(productId)]: timer } }
                    }
                    else {
                        timeObj = { [String(productId)]: timer }
                    }
                    setLocalStorage('cartTimer', JSON.stringify(timeObj));
                }
            }
            else {
                console.log('dddd>>>>else ', timeObj)
                timeObj = {};
            }
            cartTimerIntialization(productId)
            cartContents()
            console.log('productId',productId)
            console.log('slotServiceId',slotServiceId);
      		if(productId){
            serviceStatusProvisional(productId);
            }
        });
    }
    
      window.onbeforeunload = function (event) {
    //             $('.paymentButton').hide();
                $('#Date').val('');
                $('#Time').val('');
                $('#service-id').val('');
            }
        }
        // cart page js for making status removed
        $('.cart__remove').click(async function () {
          if($(this).parent().find('ul.product-details li:nth-child(4)').text()){
              let originalId = $(this).parent().find('ul.product-details li:last-child').text();
              let productIdSplit = originalId.split(':');
              let productId = productIdSplit[1].trim();
              localStorageObj = JSON.parse(getLocalStorage('cartTimer'));
              delete localStorageObj[productId];
              removeLocalStorage('cartTimer');
              setLocalStorage('cartTimer', JSON.stringify(localStorageObj));
              await serviceStatusRemove(productId, slotServiceId);
            }
        });
    
    });



3. Change the URLs according to the environment in the above custom.js file (change the following urls ) - 

>       var serviceUrl = "https://beta-service.marketcube.io/api";
>        var clientUrl = "https://mc-beta-paas-ui.onrender.com";

4. Save the file
5. click on **Add a new asset** >> click on **Create a blank file** and add  **custom.css.liquid** and copy following code - 

/* custom.css.liquid */
	

    /* Popup box BEGIN */
        .hover_bkgr_fricc{
            background:rgba(0,0,0,.4);
            cursor:pointer;
            display:none;
            height:100%;
            position:fixed;
          	left:0;
            text-align:center;
            top:0;
            width:100%;
            z-index:10000;
        }

    .hover_bkgr_fricc .helper{
        display:inline-block;
        height:100%;
        vertical-align:middle;
    }
    .hover_bkgr_fricc > div {
      	background-color: #fff;
        box-shadow: 10px 10px 60px #555;
        display: inline-block;
        height: auto;
        max-width: 1000px;
        min-height: 720px;
        vertical-align: middle;
        width: 100%;
        position: relative;
        border-radius: 8px;
    }
    .popupCloseButton {
        background-color: #fff;
    /*     border: 3px solid #999; */
        border-radius: 50px;
        cursor: pointer;
        display: inline-block;
        font-family: arial;
        font-weight: bold;
        position: absolute;
        top: -20px;
        right: -20px;
        font-size: 25px;
        line-height: 30px;
        width: 30px;
        height: 30px;
        text-align: center;
    }

    .popupCloseButton:hover {
        background-color: #ccc;
    }
    .trigger_popup_fricc {
        cursor: pointer;
        display: block;
        width: 100%;
        line-height: 1.4;
        padding-left: 5px;
        padding-right: 5px;
        white-space: normal;
        margin-top: 0;
        margin-bottom: 10px;
        min-height: 44px;
    }
    iframe#myframe {
        width: 100%;
        border: none;
        display: block;
        height: 720px;
    }
    /* Popup box BEGIN */

    /* time frame */
    .expireTime {
        padding: 5px 0px;
    }
    
    span#demo {
        font-weight: bold;
    }
    
    /* loader  */
    
    #loader1 {
      	position: absolute;
        top: 50%;
        left: 50%;
        z-index: 9999;
    }

    /* Absolute Center Spinner */
    .loading {
      position: relative;
      overflow: show;
      margin: auto;
      top: 0;
      left: 0;
      bottom: 0;
      right: 0;
    }
    
    /* Transparent Overlay */
    .loading:before {
      content: '';
      display: block;
      position: fixed;
      top: 0;
      left: 0;
      z-index: 999;
      width: 100%;
      height: 100%;
        background: radial-gradient(rgba(20, 20, 20,.8), rgba(0, 0, 0, .8));
    
      background: -webkit-radial-gradient(rgba(20, 20, 20,.8), rgba(0, 0, 0,.8));
    }

    /* :not(:required) hides these rules from IE9 and below */
    
    .loading:not(:required):after {
      content: '';
      display: block;
      font-size: 10px;
      width: 1em;
      height: 1em;
      margin-top: -0.5em;
      -webkit-animation: spinner 150ms infinite linear;
      -moz-animation: spinner 150ms infinite linear;
      -ms-animation: spinner 150ms infinite linear;
      -o-animation: spinner 150ms infinite linear;
      animation: spinner 150ms infinite linear;
      border-radius: 0.5em;
      -webkit-box-shadow: rgba(255,255,255, 0.75) 1.5em 0 0 0, rgba(255,255,255, 0.75) 1.1em 1.1em 0 0, rgba(255,255,255, 0.75) 0 1.5em 0 0, rgba(255,255,255, 0.75) -1.1em 1.1em 0 0, rgba(255,255,255, 0.75) -1.5em 0 0 0, rgba(255,255,255, 0.75) -1.1em -1.1em 0 0, rgba(255,255,255, 0.75) 0 -1.5em 0 0, rgba(255,255,255, 0.75) 1.1em -1.1em 0 0;
    box-shadow: rgba(255,255,255, 0.75) 1.5em 0 0 0, rgba(255,255,255, 0.75) 1.1em 1.1em 0 0, rgba(255,255,255, 0.75) 0 1.5em 0 0, rgba(255,255,255, 0.75) -1.1em 1.1em 0 0, rgba(255,255,255, 0.75) -1.5em 0 0 0, rgba(255,255,255, 0.75) -1.1em -1.1em 0 0, rgba(255,255,255, 0.75) 0 -1.5em 0 0, rgba(255,255,255, 0.75) 1.1em -1.1em 0 0;
    }

    .cart__qty {
        pointer-events: none;
    }
    
    .cart__qty-input {
  	-webkit-user-select: none;
        -moz-user-select: none;
        -ms-user-select: none;
        user-select: none;          
     }
    
    /* ul.product-details li:nth-child(4), .inputQuantity {
        display: none;
    } */
    
    .inputQuantity {
        display: none;
    }
    
    /* Popup box BEGIN */
    .timerPopUp{
        background:rgba(0,0,0,.4);
        cursor:pointer;
        display:none;
        height:100%;
        position:fixed;
        text-align:center;
        top:0;
      	left:0;
        width:100%;
        z-index:10000;
    }
    .timerPopUp .helper{
        display:inline-block;
        height:100%;
        vertical-align:middle;
    }
    .timerPopUp > div {
        background-color: #fff;
        box-shadow: 10px 10px 60px #555;
        display: inline-block;
        height: auto;
        max-width: 551px;
        min-height: 100px;
        vertical-align: middle;
        width: 60%;
        position: relative;
        border-radius: 8px;
        padding: 15px 5%;
    }
    .timerCloseButton:hover {
        background-color: #ccc;
    }
    /* Popup box BEGIN */

6.  click on **Save**
7.  Open Theme.lequid and add following code just above `<script>` tag and after `<style>` tag

      > `{{ 'custom.css' | asset_url | stylesheet_tag }}`

8. Click save
9. Now link  **custom.js and other required libraries** by adding following code in **theme.liquid** (same file as mentioned in point 6) before `<head>` tag.

>     <script src="{{ 'custom.js' | asset_url }}" defer="defer"></script>  
>       
>     <script src= "https://ajax.googleapis.com/ajax/libs/jquery/3.2.1/jquery.min.js"></script>
> 
>     <script src="https://cdnjs.cloudflare.com/ajax/libs/crypto-js/3.1.2/rollups/aes.js"></script>
> 
>     <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.4.1/jquery.min.js"></script>

10.  Add following code just after `<body>` tag start in the above **theme.lequid** file - 

	  
		    <div class="timerPopUp">
	    	      <span class="helper"></span>
	    	      <div>
			    		  <p>Your service has been expired<br />Add your service again!</p>
			    		  <button type="button" class="btn btn-default timerCloseButton">Ok</button>
	    	      </div>
    	    </div>

11.  Click Save.


