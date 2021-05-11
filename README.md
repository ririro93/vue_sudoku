# sudoku

## Learned

### Reactivity
> [Reactivity in Depth](http://man.hubwiz.com/docset/VueJS.docset/Contents/Resources/Documents/vuejs.org/guide/reactivity.html)
- Objects and Arrays require extra care for automatic change detection
- Objects: **property addtion or deletion not detected**
    - should initialize with all the required properties at `data`
- Arrays: **mutation methods and replacements are detected**
    - `test = [1, 2, 3]` in data and `this.test[0] = 4` in a method will not be detected
    - `this.test = this.test.slice()` then `this.test[0] = 4` will be detected
    - lodash.cloneDeep() can be used for 2D Arrays

