<script type="text/babel">
  import Vue from '../node_modules/vue/dist/vue.js'
  import limit from './index.vue'
  import VueGitHubCorners from 'vue-gh-corners'

  Vue.use(VueGitHubCorners)

  new Vue({
    components: {
      'vue-limit': limit
    },
    data: {
      fields: [
        'page', 'time', 'rows', 'firstLabel', 'previousLabel', 'label', 'nextLabel', 'lastLabel'
      ],
      model: {
        page: 1,
        time: 0,
        rows: 3,
        label: 'Page (${page} / ${pages})',
        firstLabel: 'First',
        previousLabel: 'Previous',
        nextLabel: 'Next',
        lastLabel: 'Last',
        buttonClass: 'button is-dark',
        buttonStyle: 'margin:0 10px 0 0',
        selectClass: 'input',
        selectStyle: 'margin:0 10px 0 0',
        class: 'field is-horizontal'
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
