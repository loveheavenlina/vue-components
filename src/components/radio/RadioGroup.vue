<template>
  <div :class="classes" :disabled="disabled">
    <template v-for="item in options">
      <radio 
        :name="name"
        :disabled="item.disabled"
        :value="item.value"
        :checked="value===item.value"
        @on-change="changeHandler"
        >
          {{item.label}}
      </radio>
      <divider v-if="divider"></divider>
    </template>
  </div>
</template>

<script>
import { cssPrefix } from 'utils/variable.js'
import { input } from 'utils/mixins.js'
import Radio from './Radio'
import Divider from '../divider'

export default {
  mixins: [input],
  components: {
    Radio,
    Divider
  },
  props: {
    options: {
      type: Array,
      required: true
    },
    divider: {
      type: Boolean,
      default: true
    }
  },
  computed: {
    classes () {
      return [cssPrefix + 'radio-group']
    }
  },
  mounted () {
    this.value && this.updateLabel(this.value)
  },
  data () {
    return {
      cssPrefix: cssPrefix
    }
  },
  methods: {
    changeHandler (value) {
      this.$emit('on-change', value).$emit('input', value)
      this.updateLabel(value)
    },
    updateLabel (value) {
      this.options && this.options.forEach(item => {
        value === item.value && this.$emit('update:label', item.label)
      })
    }
  }
}
</script>
