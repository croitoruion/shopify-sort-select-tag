shopify-sort-select-tag
=======================

NOTE: An official version of this is here - https://gist.github.com/carolineschnapp/11352987

Have noticed after posting this version that Shopify have shared this just a few days ago, possibly worth sharing this anyway.

HTML and JavaScript code for implementing a select tag with all possible sorting options for the page without needing to duplicate each collection.

Shopify has Liquid method which can get the sort_by query in the URI string:

``` javascript
{% if collection.sort_by %}
We are on a collection page that is sorted by the use of 'sort_by=?SORT-ORDER' in the URL.
The sort order is:
{{ collection.sort_by }}
{% endif %}
```
The details are available in the Shopify docs:
```
http://docs.shopify.com/themes/liquid-variables/collection
```

Not wanting to pay for an app to do this (which it does by duplicating each collection by the number of sorting options- ouch!), or to do the same thing manually - this snippet of code does the job by forcing a page reload.

```html
      <script>
        // jquery is already running on the site, so we use it to check the document is ready
        $(document).ready(function() {
            // Identify a Select Option from the value in the URL query text
            // Where this exists as an Option Value, add the 'selected' attribute to the Option
            // This ensures that when the page reloads the current option will be showing
            $("#selectSort option[value='{{collection.sort_by}}']").attr('selected', true);
        });
      </script>
    
      <div>
        <!-- 
        The Select Tag watches for a change, then pushes a new window.location using the value
        of the Option selected by the user
        -->
        <select id="selectSort" onchange='window.location="{{ collection.url }}?sort_by=" + this.value'>
          <option value="price-ascending">Price (lowest first)</option>
          <option value="price-descending">Price (Highest first)</option>
          <option value="title-ascending">A-Z</option>
          <option value="title-descending">Z-A</option>
          <option value="created-descending">Latest Addition</option>
          <option value="best-selling">Most Popular</option>
        </select>
      </div>
```
