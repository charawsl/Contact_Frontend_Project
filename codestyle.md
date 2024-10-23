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

### Avoid v-if with v-for

Never use v-if on the same element as v-for.  

There are two common cases where this can be tempting:

- To filter items in a list (e.g. v-for="user in users" v-if="user.isActive"). In these cases, replace users with a new computed property that returns your filtered list (e.g. activeUsers).

- To avoid rendering a list if it should be hidden (e.g. v-for="user in users" v-if="shouldShowUsers"). In these cases, move the v-if to a container element (e.g. ul, ol).
