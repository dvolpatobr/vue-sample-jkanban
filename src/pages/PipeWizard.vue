<template>
  <div class="content">
    <h1>PIPE WIZARD</h1>
    <div class="container-fluid" style="width: 98%;overflow-x: auto;white-space: nowrap;border: solid 1px black">
      <jkanban></jkanban>
    </div>
  </div>
</template>
<script>
  import jkanban from 'src/components/kanban/JKanbanC.vue'

  export default {
    components: {
      jkanban
    },
    data () {
      return {
        type: ['', 'info', 'success', 'warning', 'danger'],
        notifications: {
          topCenter: false
        }
      }
    },
    methods: {
      notifyVue (verticalAlign, horizontalAlign) {
        const color = Math.floor((Math.random() * 4) + 1)
        this.$notifications.notify(
          {
            message: `<span>Welcome to <b>Light Bootstrap Dashboard</b> - a beautiful freebie for every web developer.</span>`,
            icon: 'nc-icon nc-app',
            horizontalAlign: horizontalAlign,
            verticalAlign: verticalAlign,
            type: this.type[color]
          })
      }
    }
  }

</script>
<style lang="scss">

</style>
