<template>
  <div :class="classes">
    <div :class="[cssPrefix + 'toast-inner',cssPrefix + 'toast-' + this.align]">
      <div :class="[cssPrefix + 'toast-content']">
        <template v-if="type">
          <icon v-if="iconCode[type]" v-html="iconCode[type]"></icon>
          <spinner v-if="type==='loading'" color="#999" primary-color="#fff" :class="[cssPrefix + 'toast-spinner']"/>
          <br/>
        </template>
        <slot></slot>
      </div>
    </div>
  </div>
</template>

<script>
import { cssPrefix } from 'utils/variable.js'
import Popup from '../popup'
import Spinner from '../spinner'
import Icon from '../icon'

const iconCode = {
  success: '&#xe654;',
  warn: '&#xe653;',
  fail: '&#xe605;'
}

export default {
  components: {
    Popup,
    Spinner,
    Icon
  },
  props: {
    open: {
      type: Boolean,
      default: false
    },
    align: {
      type: String,
      default: 'top'
    },
    duration: {
      type: Number,
      default: 2000
    },
    type: {
      type: String
    },
    destroy: {
      type: Boolean,
      default: false
    }
  },
  computed: {
    classes () {
      return [cssPrefix + 'toast']
    }
  },
  mounted () {
    this.openChange(this.open)
  },
  methods: {
    openChange (value) {
      if (value) {
        requestAnimationFrame(() => {
          this.$el.style.display = 'table'
        })
        this.duration && setTimeout(() => {
          requestAnimationFrame(() => {
            this.$el.style.display = 'none'
            this.$emit('on-close')
            if (this.destroy) {
              this.$destroy()
            }
          })
        }, this.duration)
      }
    }
  },
  destroyed () {
    requestAnimationFrame(() => {
      this.$el.remove()
    })
  },
  watch: {
    open (value) {
      this.openChange(value)
    }
  },
  data () {
    return {
      cssPrefix: cssPrefix,
      iconCode: iconCode
    }
  }
}
</script>
