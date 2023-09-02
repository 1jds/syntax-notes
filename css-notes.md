# A Collection of Notes for CSS

## Selectors
- For helpful `nth-child` 'recipes', see [this article](https://css-tricks.com/useful-nth-child-recipies/) by Chris Coyier. One thing not mentioned in that exact page is how to select children in the middle of a range, as explained [here](https://css-tricks.com/nth-child-between-two-fixed-indexes/): 
  ```css
  div:nth-child(n + 2):nth-child(-n + 6) {
    background: green;
  }
  ```
