<template>
  <div class="form-group">
  <div class="form-check">
    <input
    v-uni-id="name"
    type="checkbox"
    class="form-check-input"
    :class="classList"
    :name="name"
    :disabled="disabled"
    :required='required'
    :checked="checked"
    :crop="crop"
    @change="updateValue">
    <label class="form-check-label" v-uni-for="name">
    {{label}}</label>
    <div v-if="(validator && validator.errorCount) || error" class="invalid-feedback">
      <div v-for="(error, index) in validator.errors.get(this.name)" :key="index">{{error}}</div>
      <div v-if="error">{{error}}</div>
    </div>
 
    <small v-if="helper" class="form-text text-muted">{{helper}}</small>
  </div>
  </div>
</template>

<script>
import ValidationMixin from './mixins/validation'

import { createUniqIdsMixin } from 'vue-uniq-ids'
// Create the mixin
const uniqIdsMixin = createUniqIdsMixin()
export default {
  mixins: [uniqIdsMixin, ValidationMixin],
  model: {
    prop: 'checked',
    event: 'change'
  },
  props: [
    'error',
    'checked',
    'options',
    'disabled',
    'required',
    'label',
    'crop',
    'name',
    'helper',
    'controlClass'
  ],
  computed:{
    classList(){
      let classList = {
        'is-invalid': (this.validator && this.validator.errorCount) || this.error, 
      }
      if(this.controlClass){
        classList[this.controlClass] = true
      }
      return classList
    }
  },
  data() {
    return {
      content: '',
    }
  },
  methods: {
    updateValue(e) {
      this.content = e.target.checked;
      this.$emit('change', this.content)
    }
  }
}
</script>

<style lang="scss" scoped>
</style>