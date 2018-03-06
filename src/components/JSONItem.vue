<template>
  <div>
    <li>
      <!--  +/- name  -->
      <div
        :class="{bold: hasChild}"
        @click="toggle">
        <span v-if="hasLeaves">[{{ open ? '-' : '+'}}]</span>
        {{name}}
      </div>
      <!-- key: value -->
      <ul v-show="open" v-for="(value, key) in model" :key="key">
        <span v-if="isLeave(value)"> {{ key }} : {{ value }} </span>
        <JSONItem v-else :name="key" :model="value" :expand="expand"/>
      </ul>
    </li>
  </div>
</template>
<script>
function isArray (a) {
  return a !== null && Array.isArray(a)
}
function isObject (o) {
  return (o !== null) && (typeof o === 'object')
}
function isChild (c) {
  return isArray(c) || isObject(c)
}
export default {
  name: 'JSONItem',
  props: {
    model: { type: Object },
    name: { type: String, required: true },
    expand: { type: Boolean, required: false, default: false }
  },
  data () {
    return {
      open: false
    }
  },
  watch: {
    expand (newVal, oldVal) {
      if (newVal !== oldVal && this.hasLeaves) {
        this.open = newVal
      }
    }
  },
  computed: {
    isArray () {
      return isArray(this.model)
    },
    isObject () {
      return isObject(this.model)
    },
    hasLeaves () {
      return Object.keys(this.model).length > 0
    },
    hasChild () {
      let result = false
      Object.keys(this.model).forEach(key => {
        const value = this.model[key]
        if (isChild(value)) {
          result = true
        }
      })
      return result
    }
  },
  methods: {
    isNumber (n) {
      return (n !== null) && (typeof n === 'number')
    },
    isString (n) {
      return (n !== null) && (typeof n === 'string')
    },
    toggle () {
      if (this.hasLeaves) {
        this.open = !this.open
      }
    },
    isLeave (val) {
      // console.log(`isLeave(${JSON.stringify(val)}) => ${!isChild(val)}`)
      return !isChild(val)
    },
    isSubTree (val) {
      return isChild(val)
    }
  }
}
</script>
<style>
ul {
  padding-left: 1em;
  line-height: 1.5em;
  list-style-type: dot;
}
body {
  font-family: Menlo, Consolas, monospace;
  color: #444;
}
.item {
  cursor: pointer;
}
.bold {
  font-weight: bold;
}
</style>
