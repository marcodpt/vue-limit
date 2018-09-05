<script type="text/babel">
  import Vue from '../node_modules/vue/dist/vue.js'
  import limit from './index.vue'

  new Vue({
    components: {
      'vue-limit': limit
    },
    data: {
      model: {
        page: 1,
        time: 0,
        rows: 3,
        label: 'Page ({{page}} / {{pages}})',
        firstLabel: 'First',
        previousLabel: 'Previous',
        nextLabel: 'Next',
        lastLabel: 'Last'
      },
      input: [
        'item 1',
        'item 2',
        'item 3',
        'item 4',
        'item 5',
        'item 6',
        'item 7',
        'item 8',
        'item 9',
        'item 10'
      ],
      output: []
    }
  }).$mount('#app')
</script>
