# Codestyle

The project is written based on the code style provided by Vue.js official documentation.  

Source: https://v2.vuejs.org/v2/style-guide/?redirect=true

For convenient, I just show essential rules here. Other not essential rules please go to the url above.

## Priority A Rules: Essential (Error Prevention)
### Multi-word component names

Component names should always be multi-word, except for root App components, and built-in components provided by Vue, such as <transition> or <component>.  

This prevents conflicts with existing and future HTML elements, since all HTML elements are a single word.   

bad:

    Vue.component('todo', {
      // ...
    })
    export default {
      name: 'Todo',
      // ...
    }

good:

    Vue.component('todo-item', {
      // ...
    })
    export default {
      name: 'TodoItem',
      // ...
    }
    
### Component data

Component data must be a function.  

When using the data property on a component (i.e. anywhere except on new Vue), the value must be a function that returns an object.

bad:

    Vue.component('some-comp', {
      data: {
        foo: 'bar'
      }
    })
    export default {
      data: {
        foo: 'bar'
      }
    }

good:

    Vue.component('some-comp', {
      data: function () {
        return {
          foo: 'bar'
        }
      }
    })
    // In a .vue file
    export default {
      data () {
        return {
          foo: 'bar'
        }
      }
    }

### Prop definitions

Prop definitions should be as detailed as possible.  

In committed code, prop definitions should always be as detailed as possible, specifying at least type(s).

bad:

    // This is only OK when prototyping
    props: ['status']

good:

    props: {
      status: String
    }
    // Even better!
    props: {
      status: {
        type: String,
        required: true,
        validator: function (value) {
          return [
            'syncing',
            'synced',
            'version-conflict',
            'error'
          ].indexOf(value) !== -1
        }
      }
    }

    
### Avoid v-if with v-for

Never use v-if on the same element as v-for.  

There are two common cases where this can be tempting:

- To filter items in a list (e.g. v-for="user in users" v-if="user.isActive"). In these cases, replace users with a new computed property that returns your filtered list (e.g. activeUsers).

- To avoid rendering a list if it should be hidden (e.g. v-for="user in users" v-if="shouldShowUsers"). In these cases, move the v-if to a container element (e.g. ul, ol).

bad:

    <ul>
      <li
        v-for="user in users"
        v-if="user.isActive"
        :key="user.id"
      >
        {{ user.name }}
      </li>
    </ul>

    
    <ul>
      <li
        v-for="user in users"
        v-if="shouldShowUsers"
        :key="user.id"
      >
        {{ user.name }}
      </li>
    </ul>

good:

    <ul>
      <li
        v-for="user in activeUsers"
        :key="user.id"
      >
        {{ user.name }}
      </li>
    </ul>

    
    <ul v-if="shouldShowUsers">
      <li
        v-for="user in users"
        :key="user.id"
      >
        {{ user.name }}
      </li>
    </ul>
    
### Component style scoping

**For applications, styles in a top-level App component and in layout components may be global, but all other components should always be scoped.**

This is only relevant for single-file components. It does not require that the scoped attribute be used. Scoping could be through CSS modules, a class-based strategy such as BEM, or another library/convention.  

**Component libraries, however, should prefer a class-based strategy instead of using the scoped attribute.**

This makes overriding internal styles easier, with human-readable class names that don’t have too high specificity, but are still very unlikely to result in a conflict.

bad:

    <template>
      <button class="btn btn-close">X</button>
    </template>

    <style>
    .btn-close {
      background-color: red;
    }
    </style>

good:

    <template>
      <button class="button button-close">X</button>
    </template>
    
    <!-- Using the `scoped` attribute -->
    <style scoped>
    .button {
      border: none;
      border-radius: 2px;
    }
    
    .button-close {
      background-color: red;
    }
    </style>

### Private property names

**Use module scoping to keep private functions inaccessible from the outside. If that’s not possible, always use the '$_' prefix for custom private properties in a plugin, mixin, etc that should not be considered public API. Then to avoid conflicts with code by other authors, also include a named scope (e.g. '$_yourPluginName_').**

bad:

    var myGreatMixin = {
      // ...
      methods: {
        update: function () {
          // ...
        }
      }
    }

good:
    
    var myGreatMixin = {
      // ...
      methods: {
        publicMethod() {
          // ...
          myPrivateFunction()
        }
      }
    }
    
    function myPrivateFunction() {
      // ...
    }

    export default myGreatMixin
