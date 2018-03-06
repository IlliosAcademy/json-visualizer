<template>
  <div>
    <b-tabs>
      <b-tab title="Tree">
        <Item name="JSON" :model="jsonObject" :expand="expand" />
        <br/>
        <b-button @click="toggleExpand"> {{ openOrCloseAll }} </b-button>
      </b-tab>
      <b-tab title="JSON" active>
        <Paster @onTextChange="t => handleTextChange(t)"/>
      </b-tab>
    </b-tabs>
  </div>
</template>

<script>
import Paster from './JSONPaster'
import Item from './JSONItem'

export default {
  name: 'HelloWorld',
  components: {
    'Paster': Paster,
    'Item': Item
  },
  data () {
    return {
      jsonObject: {},
      expand: false
    }
  },
  computed: {
    openOrCloseAll () {
      return this.expand ? 'Close All' : 'Expand All'
    }
  },
  methods: {
    handleTextChange (t) {
      try {
        this.jsonObject = JSON.parse(t)
      } catch (e) {
        console.log('Error in json happened = ' + e.toString())
      }
    },
    toggleExpand () {
      this.expand = !this.expand
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
</style>
