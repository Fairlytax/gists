# [Gist link](https://gist.github.com/babbitt/b939ced2b72a18eadfe67661b8e3672a)
The full gist is below as of 1/25/21. The most up to date version of this gist as well as comments are available by visiting the gist directly (link above)

‎

‎

‎
# Tailwind hidden submenu with reveal on hover/focus
```
                      //////                      
                   ////////////        /////      Created @ Fairly
                //////////////////,    /////      Dev: @babbitt
             /////////.    ./////////, /////      Released: 1/25/2021
          /////////.          ./////////////      License: MIT
       /////////,                .//////////      
      ///////,                      ,///////      
      //////                          //////      
      //////         .........        //////      
      //////         .........        //////      
      //////                          //////      
      //////                          //////      
      //////                          //////      
      //////////////////////////////////////      
      //////////////////////////////////////         
```

## Demo
![css-only-nav](https://user-images.githubusercontent.com/10392896/105752237-05220680-5f15-11eb-801d-91a482584f5c.gif)

## Code
This solution requires two variant plugins. The main effect achieved here is making sure that the menu doesn't close when you're selecting an item

```javascript
// In tailwind.config.js
const plugin = require("tailwindcss/plugin");

const hoveredParentPlugin = plugin(function ({ addVariant, e }) {
  addVariant("hovered-parent", ({ container }) => {
    container.walkRules((rule) => {
      rule.selector = `:hover > .hovered-parent\\:${rule.selector.slice(1)}`;
    });
  });
});

const focusedWithinParentPlugin = plugin(function ({ addVariant, e }) {
    addVariant("focused-within-parent", ({ container }) => {
      container.walkRules((rule) => {
        rule.selector = `:focus-within > .focused-within-parent\\:${rule.selector.slice(1)}`;
      });
    });
});
```

And remember to add these to your variants:
```
...
variants: {
    ...
    plugins: [ 
        hoveredParentPlugin,
        focusedWithinParentPlugin,
    ],
}
...
```

Finally in our html, we can insert the following to use this effect
```html
...
<div>
    <a href="#">Header</a>
    <div class="focused-within-parent:h-24 hovered-parent:h-24 focused-within-parent:mt-2 hovered-parent:mt-2 transition-height duration-100 h-0 overflow-hidden flex flex-col">
        <a href="#">Menu Item</a>
        <a href="#">Menu Item</a>
        <a href="#">Menu Item</a>
    </div>
</div>
```
## Notes / Caveats
The height of the menu item div is pre-determined (ex `h-24`). In my experience, each menu item appears to be aprox. 8 units in height, but that will differ based on your text size, margins, and padding. This solution breaks very quickly if the inner div doesn't have a fixed height.  


## Sources
(Sorry for the edit), almost forgot about this SO question I found which *heavily* inspired the plugins I "wrote"
* [SO: Tailwind CSS : Is there a way to target next sibling?](https://stackoverflow.com/a/65321069)