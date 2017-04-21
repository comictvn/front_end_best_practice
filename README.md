# **Frontend Readme**


_Incomplete — Currently in Progress_
_Last update: 20 Apr 2017_


## **1. CSS**

[CSS Wizardry](https://www.csswizardry.com) is quite a good resource! For starters:
- Use `Block__Element--Modifier`: [BEM Syntax](https://csswizardry.com/2013/01/mindbemding-getting-your-head-round-bem-syntax/)
- Include namespace prefixes to improve identification: [About Namespaces](https://csswizardry.com/2015/03/more-transparent-ui-code-with-namespaces/)
- Eventually the structure should be somewhat like this: [ITCSS](https://www.xfive.co/blog/itcss-scalable-maintainable-css-architecture/)

Important to know and avoid:
- [About CSS Specificity](https://csswizardry.com/2014/10/the-specificity-graph/)
- [Bad CSS](https://csswizardry.com/2012/11/code-smells-in-css/)

Up for discussion:
- Limiting BEM to a single element level i.e. disallowing `block__element__element`
- Use of `--modifier` state suffixes vs. `is-`/`has-` conditional prefixes
  - I think `--modifier` types should be used when adding classes to elements in the codebase, while `is-`/`has-` types should only be applied via JS appendation
- Usage of CSS3 wildcards & attribute selectors, especially for important UI features



### **1.1 Namespacing**

Broadly speaking, there are 2 categories of namespaces.



- **_Preset_** classes have pre-determined functions and should not have any additional properties attributed and/or bound to them.
  - Globally-applicable *by default*:
      - `o-`: Objects. Used for globally applicable elements and complex blocks. Should be purely structural with no aesthetic style properties (normalising properties are exception) Examples include `o-gridbox__list` and `o-swipe__list`
      - `ic-`: Icons. Used to apply HipVan symbols (as a pseudo-element). All HipVan symbols should be applied from the predefined list of `ic-` prefixed classes @.
      - `u-`: Utility. Single-purpose class that applies important but isolated propert(ies) to an element
      - `hk-`: Hack. Write only into `../override_hipvan/shame.scss`. Used for single-purpose & isolated tricks, cheats, magic numbers, etc. Should be returned to and removed once a proper solution is found

  - Component-specific *by default*:
      - `js-`: Javscript-binding. Used only by scripts. Should be made globally-applicable if requested for CMS content usage
      - `is-`, `has-`: JS-appended conditions. Used to modify the style of an element / element + siblings / elements > children
      - `s-`: Scope. Used only for sections where all children are naked elements with no selector names, e.g. CMS FAQ block, blog post content

- **_Styling_** classes are created as UI components are being designed. They contain all kinds of properties that lead to the final look of the element / complex block.
  - `c-`: Components. Used to define a specific piece of UI. Should be named clearly so its function and reach is understood. Use additional modifier classes to style variants whenever reasonable to do so (i.e. when structural contents don't change, e.g. USP bar)





### **1.2 Additional**

- **NEVER** use `id`'s and `!important`'s, ever. We will remove all existing `id`'s and `!important`'s out over time

- Always use `border: 0;` to define no-border.

- By default, use soft commenting for CSS, i.e.
  ```
  // This is a soft comment that will not appear in CSS output
  
  //------------------------------------------------------------
  // This is a multi-line soft comment that keeps within a max
  // line-length of 60 characters so that the comment message
  // can be read easily without excess horizontal scrolling
  //------------------------------------------------------------
  ```
  Only use `/* COMMENT */` if it is necessary to show comemnts in the output

- In almost all cases, CSS should be set in this format:
  ```
  .icon {
    display: inline-block;
    padding: 10px;
    background-color: #fafafa;
  }
  ```
  In cases where values are to be matched or compared, it is ok to format lines like this:
  ```
    width:  16px;
    height: 16px;
  ```
  Or even like this:
  ```
  .icon--home       { background-position:  0    0  ; }
  .icon--nav        { background-position: -5px  0  ; }
  .icon--collection { background-position:  0   -5px; }
  .icon--product    { background-position: -5px -5px; }
  ```
- Please use variables and mixins wherever possible, especially for specified-colour variables and vendor-prefix assisting mixins





## **2. Symbols**

- A continuously updated symbols font is being maintained in LLQ at: `../assets/fonts`
- All currently available symbols for use can be looked up at: [/symbols-reference](https://www.hipvan.com/symbols-reference)

### **2.1 Usage**

- Symbols are applied as CSS pseudo-element content
- `.ic-bef` _(applied to :before)_ or `.ic-aft` _(applied to :after)_ declares the `HipVanSymbols` font-family property
- Symbol styling can be applied quickly through the `font_icon` series of mixins available from `../abstracts/hv-mixins.scss`
- Where applicable, symbol variants can be shown by adding modifier classes:

  | Weight Modifier: |                                                                                      |
  |------------------|--------------------------------------------------------------------------------------|
  | Bold Icon        | .ic-bold                                                                             |
  | Solid Icon       | .ic-solid                                                                            |

  | Style Modifier:  |                                                                                      |
  |------------------|--------------------------------------------------------------------------------------|
  | Arrows           | .ic-dir-up, .ic-dir-down, .ic-dir-left, .ic-dir-left2, .ic-dir-right, .ic-dir-right2 |
  | Shipping         | .ic-ship-free, .ic-ship-fast, .ic-ship-fsfr                                          |
  | Magnify, Stretch | .ic-zoom-out, .ic-zoom-in                                                            |
  | Star             | .ic-star-half, .ic-star-full                                                         |

### **2.2 Example**

- The CSS
  ```
  <style>
    .yourWrapper {
      font-size: 0; // removes excess spacing from inline-blocks
    }
    span[class^="yourClass"] {
      display: inline-block;
      font-size: 24px;
      vertical-align: middle;
    }
    span[class^="yourClass"]:before,
    span[class^="yourClass"]:after {
      display: inline-block;
      height: 44px;
      width: 44px;
      font-size: 44px;
      line-height: 1;
      margin-right: 5px;
      vertical-align: middle;
    }
  </style>
  ```

- The HTML
  ```
  <div class="yourWrapper">
    <span class="yourClass1 ic-bef ic-misc-ship ic-bold">HELLO</span>
    <br>
    <span class="yourClass2 ic-bef ic-misc-ship ic-ship-fsfr">WORLD</span>
    <br>
    <span class="yourClass3 ic-aft ic-site-arrow ic-dir-right2">LOREM</span>
  </div>
  ```
- The Result:

  ![Example Result](https://hiptruck-files.imgix.net/cms-files/717bfe58-2ac0-4047-95a7-e549e1a5dcd3/Result.png)





## **3. Standard Elements**

### **3.1. Mixins & Variables**
- Gradually switch the @import of `mixins` & `variables` → `hv-mixins` & `hv-variables` in all SCSS partials. Only copy across **required** variables & mixins to the new files and sort them accordingly
- Following from above, **only** `hv-mixins` & `hv-variables` will receive updates; the 2 olders ones are deprecated assets


### **3.2. Font Sizes**
- For most cases moving forward, `font-size` can be simply defined via the mixin `text_size`. Font-sizes are graded according to the modular scale described by [Bourbon](http://bourbon.io/docs/#modular-scale). We make use of 2 scales:
  - For h1 — h6: `$minor-third`; example for &lt;h1&gt;: `@include text_size(4, $minor-third);`
  - For regular body content: `$minor-second`; example for &lt;p&gt;: `@include text_size(0);` (no need to specify scale used because `$minor-second` is the default value)
- `text_size` generates both mobile & non-mobile font-sizes. To declare custom font-sizes for individual media breakpoints, use the original `modular-scale` function instead:
  - Default body font-size on mobile: `modular-scale(0, $sizeFont_em);`
  - Default body font-size on non-mobile: `modular-scale(0, $sizeFont);`


### **3.3. Form boxes & Buttons**
- Form boxes and buttons are defined in 5 sizes:  
`xs` / `s` / `m` / `l` / `xl`

- There are 3 preset colours and 1 outlining modifier style:  
`primary` / `secondary` / `grey` / `ghost`
  


### **3.4. z-index**
- Box-shadows should be set via the mixin `z-shadow`, which produces acceptable output between the values 1 - 9




## **4. Others**

### **4.1 Serving images**
- Marketing & Merchandising will upload `.png` files wherever possible
- The current set of Imgix parameters to be applied by default for LLQ are:
    - `auto=format`: Lets Imgix serve WebP or otherwise fall back to other supported optimised formats
    - `lossless=0`: Allows lossy images whenever they are optimised for
    - `cs=srgb`: Sets colourspace as Marketing & Merchandising intended
- Usage of `srcset`  and the Imgix `crop` attributes `crop=entropy`/`crop=edges`


### **4.2 OpenType Features**
- Useful introductory article [here](https://css-tricks.com/almanac/properties/f/font-feature-settings/)
- Currently, case-sensitive forms are switched on by default via `font-feature-settings: "case" 1;` so that brackets and hyhens/dashes are rendered correctly
- We can consider applying `"tnum" 1` for cart / order page tabulated figures
