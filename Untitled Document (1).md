### Step 1: Add a template for brand page.
1. Store login >> Themes >> Create Template
2. Click on "Add a new template"
3. Choose "Create a new template for page" called "Name of page"
4. Once page is created add a code 

<script type="text/javascript">
  // self executing function here
  document.addEventListener("DOMContentLoaded", function(event) {
    // the DOM will be available here
    const path = window.location.pathname.split("/");
    const vendorName = path.pop();
    const shopHost = "{{ shop.permanent_domain }}".split(".myshopify.com");
    const shop = shopHost && shopHost[0];
    const url = "https://brands.marketcube.io/brand/" + shop + "/" + vendorName;
    // const url = "https://brand-pages.marketcube.io/brand/" + shop + "/" + vendorName;
    document.getElementById("vendorPage").src=url;
  });
  // page-width
</script>
       <iframe id="vendorPage" src="" name="{{ shop.secure_url }}" scrolling="yes" height="1200px" width="100%" style="border: none"></iframe>
       
### Step 2: Add a page usning the created template. (/admin/pages)
1. Click on add page 
2. Title page as "brand"
3. Select "Template Suffix" which is created in step 1.

### Step 3: Enable show vendor on product under customize theme section
1. Select product page from dropdown
2. Enable show vendor check box from product settings
3. Save settings

### Step 4: Enable calling of vendor handle or vendor name to the product details
- Find the file from which the vendor is renderring on product page  (This is different in case of different themes or the customization done in the theme)
Code needs to be added is :

{% if product.metafields.marketcube.brandSlug %}
                  <a href="/pages/brand/{{ product.metafields.marketcube.brandSlug }}" 
                     title="{{ product.vendor }}" data-slug="{{ product.metafields.marketcube.brandSlug }}">{{ product.vendor }}</a>
                {%- else -%}
                  <a href="/pages/brand/{{ product.vendor }}" 
                     title="{{ product.vendor }}">{{ product.vendor }}</a>
                     {{ product.metafields.marketcube.brandSlug }}
