<template>
  <div>
    <b-form-textarea id="text" :value="text" @input="t => handleTextChange(t)" placeholder="Paste your json here" :rows="20">
    </b-form-textarea>
  </div>
</template>
<script>
export default {
  name: 'json_paster',
  methods: {
    handleTextChange (t) {
      this.text = t
      this.$emit('onTextChange', t)
    }
  },
  data () {
    return {
      text: ''
    }
  }
}
</script>
<style>
</style>
