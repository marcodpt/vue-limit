<script type="text/babel">
  import T from 'libt'

  module.exports = {
    props: {
      model: {
        type: Object,
        required: true
      },
      id: {
        type: String,
        default: 'page'
      },
      rows: {
        type: Number,
        default: 10
      },
      input: {
        type: Array,
        default: () => []
      },
      output: {
        type: Array,
        default: () => []
      },
      label: {
        type: String,
        default: 'Page (${page} / ${pages})'
      },
      time: {
        type: Number,
        default: 0
      },
      firstLabel: {
        type: String,
        default: 'First'
      },
      previousLabel: {
        type: String,
        default: 'Previous'
      },
      nextLabel: {
        type: String,
        default: 'Next'
      },
      lastLabel: {
        type: String,
        default: 'Last'
      },
      buttonClass: {
        type: [String, Array, Object],
        default: ''
      },
      buttonStyle: {
        type: [String, Array, Object],
        default: ''
      },
      selectClass: {
        type: [String, Array, Object],
        default: ''
      },
      selectStyle: {
        type: [String, Array, Object],
        default: ''
      }
    },
    data: function () {
      return {
        pages: T.pager(this.rows)(this.input),
        oldPage: 0,
        interval: null
      }
    },
    mounted: function () {
      this.setPage ()
    },
    methods: {
      setPage: function (action) {
        if (this.time && !this.$data.interval) {
          this.$data.interval = setInterval(() => {
            this.setPage('rotate')
          }, 1000 * this.time)
        } else if (!this.time && this.$data.interval) {
          clearInterval (this.$data.interval)
          this.$data.interval = null
        }

        this.$data.pages = T.pager(this.rows)(this.input)
        this.model[this.id] = parseInt(this.model[this.id]) || 1
        this.$data.oldPage = this.model[this.id]

        if (action === 'begin') {
          this.$set(this.model, this.id, 1)
        }
        if (action === 'back') {
          this.$set(this.model, this.id, this.model[this.id] - 1)
        }
        if (action === 'next' || action === 'rotate') {
          this.$set(this.model, this.id, this.model[this.id] + 1)
        }
        if (action === 'end') {
          this.$set(this.model, this.id, this.$data.pages)
        }

        if (this.model[this.id] > this.$data.pages) {
          if (action === 'rotate') {
            this.$set(this.model, this.id, 1)
          } else {
            this.$set(this.model, this.id, this.$data.pages)
          }
        }
        if (!this.model[this.id] || this.model[this.id] < 1) {
          this.$set(this.model, this.id, 1)
        }

        if (action === undefined || this.$data.oldPage !== this.model[this.id]) {
          T.sync(this.output, T.pager(this.rows)(this.model[this.id])(this.input))
        }
      },
      begin: function () {
        this.setPage('begin')
      },
      back: function () {
        this.setPage('back')
      },
      next: function () {
        this.setPage('next')
      },
      end: function () {
        this.setPage('end')
      },
      getLabel: function (i) {
        return T.compose(
          T.replaceAll('${pages}', this.pages),
          T.replaceAll('${page}', i)
        )(this.label)
      },
      getOptions: function () {
        var options = []
        for (var i = 1; i <= this.pages; i += 1) {
          options.push({
            id: i,
            label: this.getLabel(i)
          })
        }

        return options
      }
    },
    watch: {
      model: {
        deep: true,
        handler: function () {
          if (this.$data.oldPage !== this.model[this.id]) {
            this.setPage ()
          }
        }
      },
      rows: function () {
        this.setPage ()
      },
      input: function () {
        this.setPage ()
      },
      time: function () {
        this.setPage ()
      }
    }
  }
</script>

<template>
  <div>
    <button :class="buttonClass" :style="buttonStyle" :disabled="model[id] <= 1" @click="begin">
      <slot name="first">{{firstLabel}}</slot>
    </button>
    <button :class="buttonClass" :style="buttonStyle" :disabled="model[id] <= 1" @click="back">
      <slot name="previous">{{previousLabel}}</slot>
    </button>
    <slot name="select" :id="id" :model="model" :options="getOptions()" :label="getLabel(model[id])">
      <select
        v-model="model[id]"
        :class="selectClass"
        :style="selectStyle"
      >
        <option v-for="opt in getOptions()" :value="opt.id">{{opt.label}}</option>
      </select>
    </slot>
    <button :class="buttonClass" :style="buttonStyle" :disabled="model[id] >= pages" @click="next">
      <slot name="next">{{nextLabel}}</slot>
    </button>
    <button :class="buttonClass" :style="buttonStyle" :disabled="model[id] >= pages" @click="end">
      <slot name="last">{{lastLabel}}</slot>
    </button>
  </div>
</template>
