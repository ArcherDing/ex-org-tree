<template>
  <div style="text-align: center;">
    <ex-org-tree
      :data="tree"
      :horizontal="horizontal"
      :collapsible="collapsible"
      :label-class-name="labelClassName"
      @expand-click="onExpandClick"
      @node-click="onNodeClick">
      <template slot-scope="scope">
        {{scope.data.name}}
      </template>
    </ex-org-tree>
  </div>
</template>

<script>
  import ExOrgTree from '../components/ex-org-tree';

  export default {
    components: {ExOrgTree},
    data() {
      return {
        tree: {
          id: 0,
          name: 'XXX科技有限公司',
          children: [{
            id: 2,
            name: '产品研发部',
            children: [{
              id: 5,
              name: '研发-前端'
            }, {
              id: 6,
              name: '研发-后端'
            }, {
              id: 9,
              name: 'UI设计'
            }, {
              id: 10,
              name: '产品经理'
            }]
          }, {
            id: 3,
            name: '销售部',
            children: [{
              id: 7,
              name: '销售一部'
            }, {
              id: 8,
              name: '销售二部'
            }]
          }, {
            id: 4,
            name: '财务部'
          }, {
            id: 9,
            name: 'HR人事'
          }]
        },
        horizontal: false,
        collapsible: true,
        labelClassName: 'bg-white'
      };
    },
    methods: {
      onExpandClick(status, data) {
        console.log('onExpandClick', status, data);
      },
      onNodeClick(data) {
        console.log('onNodeClick', data);
      }
    }
  };
</script>
<style>
  .bg-white {
  }
</style>
