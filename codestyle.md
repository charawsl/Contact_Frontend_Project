# Codestyle

The project is written based on the code style provided by Vue.js official documentation.  

Source: https://v2.vuejs.org/v2/style-guide/?redirect=true

## Priority A Rules: Essential (Error Prevention)
### Multi-word component names

Component names should always be multi-word, except for root App components, and built-in components provided by Vue, such as <transition> or <component>.  

This prevents conflicts with existing and future HTML elements, since all HTML elements are a single word.

### Component data

Component data must be a function.  

When using the data property on a component (i.e. anywhere except on new Vue), the value must be a function that returns an object.

### Prop definitions

Prop definitions should be as detailed as possible.  

In committed code, prop definitions should always be as detailed as possible, specifying at least type(s).
