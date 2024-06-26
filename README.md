# CSS-Assignment

## :has(:not()) vs :not(:has())
### Description
- :has(:not()): Selects an element that has children which do not match a specified selector.
In this case, all .task boxes are marked with a red border if the task has not yet been completed.
In other words, all boxes whose checkbox is not :checked have a red border.
- :not(:has()): Selects an element that does not have any children matching a specified selector.
All ".task" boxes are display: flex so that the image appears next to the text.
However, if a ".task" has no image, nothing should be displayed next to each other.
Therefore, all .task boxes that do not have an image are displayed with display: block flow-root and are given a padding.

### Code Examples 
CSS File: Line 182 - 188

### Compatibility
Here's a revised version of the text:

The `:has()` pseudo-class has been confirmed as compatible with all major browsers and devices, including Chrome, Edge, Firefox, and Safari. Interop's assessment last year rated `:has()` as 97,3% compatible across these browsers. Additionally, experimental testing on Chrome Dev, Edge Dev, Firefox Nightly, and Safari Technology Preview also indicated full compatibility.

- [Can I use](https://caniuse.com/?search=:has())
- [MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/:has)
- [Interop](https://wpt.fyi/interop-2024?stable)

In contrast, `:not()` has not undergone baseline verification. However, it has demonstrated compatibility with the four main browsers for several years. Since Interop has not yet evaluated `:not()`, no definitive statements can be made regarding its status. While `:has()` is suitable for use in production environments without any issues, caution is advised when using `:not()`.

- [Can I use](https://caniuse.com/?search=:not())
- [MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/:not?retiredLocale=de)
- [Interop](https://wpt.fyi/interop-2024?stable)

#### Possible Fallbacks
- JavaScript: First you could check via JavaScript if the Browser supports :has and :not and if not, you could do the following: 
  - Looping through all .task Elements and if their checkboxes are not checked, you add the class 'incomplete' to the Element. 
  ```js
  document.querySelectorAll('.task').forEach(task => {
      if (!task.querySelector('input[type="checkbox"]:checked')) {
          task.classList.add('incomplete');
      }
  });
    ```    
    ```css
      /* Fallback for unsupported browsers */
        .task.incomplete {
            border: 3px solid red;
        }
      /* Apply class via JavaScript if no `:has()` support */
    ```
- Progressive Enhancement: Use progressive enhancement to provide a basic experience that works across all browsers, enhancing the experience for browsers that support :has():
    ```css
        .task {
            border: 3px solid grey; 
        }
    
        /* Enhanced style for browsers supporting `:has()` */
        @supports(selector(:has())) and ( selector(:not())) {
            .task:has(input[type="checkbox"]:not(:checked)) {
                border: 3px solid red;
            }
        }
    ```
- Use a polyfill: If you want to use :has() or :not() in older browsers, you can use a polyfill library.

Source: The possible Fallbacks were suggested by ChatGPT and I have evaluated and optimized them.

---

## Container Query + Grid

### Description
I have a website where you can check off to-dos as completed. The layout is divided into header, sidebar, main and footer. Furthermore, the main area is a two-column grid. There are a total of three container queries:
- header: As soon as the width of the header becomes smaller than 1200px, the menu is hidden and a menu button appears.
- container: As soon as the main container becomes narrower than 800px, it is re-layouted and the aside appears above the main area
- .task-box: As soon as the box becomes smaller than 500px, the text is no longer next to the image but below it.

### Compatibility
CSS Container Queries (Size) are Baseline since 2023 and are newly available across major browsers according to CanIUse.
But according to MDN, container queries are not yet baseline. So it could be that individual features are not yet baseline. I'm not sure about the current status here. I would still be cautious about using it in Production. I think you can use it for smaller projects with users of modern browsers. For older projects, perhaps enjoy it with caution. In any case, extensive cross-browser testing is necessary.
Interop's assessment last year rated Container Queries as 92% compatible across these browsers.

- [Can I use](https://caniuse.com/?search=CSS%20Container%20Queries%20(Size))
- [MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_containment/Container_queries)
- [Interop](https://wpt.fyi/interop-2024?stable)


Grid is according to MDN Baseline and widely supported across all major browsers.
According to the Interop Dashboard, Grid works 83.8% the same across all browsers. So I think Grid can be used well in Production, with extensive cross-browser testing 
- [MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/grid)
- [Interop](https://wpt.fyi/interop-2023?stable)

#### Possible Fallbacks
- Traditional Media Queries: Check if the Browser supports Container Queries and if not, use viewport-based media queries to adjust the layout and styles based on the overall screen size. While this doesn't provide the same granularity as container queries, it allows you to create a responsive design that works across different screen sizes.
- JavaScript Solution: Check if the Browser supports Container Queries and if not use the Resize Observer: The Resize Observer API allows you to detect changes in the size of an element and adjust styles or classes accordingly. This approach can simulate the effect of container queries but requires JavaScript.
```js
  const container = document.querySelector('.container');
  const observer = new ResizeObserver(entries => {
    for (let entry of entries) {
      if (entry.contentRect.width > 600) {
        container.classList.add('large');
        container.classList.remove('small');
      } else {
        container.classList.add('small');
        container.classList.remove('large');
      }
    }
  });
  observer.observe(container);
```
- Feature Detection or Progressive Enhancement: Use feature detection to check if the browser supports container queries and provide a fallback for browsers that don't. You can use @supports to apply styles based on whether the browser supports container queries.
```css
@supports (container-type: inline-size) {
  .container {
    /* Styles for browsers with container query support */
  }
}
@supports not (container-type: inline-size) {
  .container {
    /* Fallback styles */
  }
}
```
Source: The possible Fallbacks were suggested by ChatGPT and I have evaluated and optimized them.

