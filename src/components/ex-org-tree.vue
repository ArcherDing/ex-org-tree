<template>
  <div class="ex-org-tree-container">
    <div class="ex-org-tree" :class="{horizontal, collapsible}">
      <ex-org-tree-node
        :data="data"
        :props="props"
        :horizontal="horizontal"
        :label-width="labelWidth"
        :collapsible="collapsible"
        :label-class-name="labelClassName"
        @expand-click="(status, data) => {$emit('expand-click', status, data)}"
        @node-click="(data) => {$emit('node-click', data)}"
      >
        <template slot-scope="scope">
          <slot :data="scope.data">
            {{scope.data[props.label]}}
          </slot>
        </template>
      </ex-org-tree-node>
    </div>
  </div>
</template>
<script>
  import ExOrgTreeNode from './ex-org-tree-node';

  export default {
    name: 'ExOrgTree',
    components: {ExOrgTreeNode},
    props: {
      data: {
        type: Object,
        required: true
      },
      props: {
        type: Object,
        default: () => ({
          label: 'label',
          expand: 'expand',
          children: 'children'
        })
      },
      horizontal: Boolean,
      collapsible: Boolean,
      renderContent: Function,
      labelWidth: [String, Number],
      labelClassName: [Function, String]
    }
  };
</script>
<style lang="less" scoped>
  .ex-org-tree-container {
    display: inline-block;
    padding: 15px;
    background-color: #fff;
    .ex-org-tree {
      display: table;
      text-align: center;

      &:before, &:after {
        content: '';
        display: table;
      }

      &:after {
        clear: both;
      }
    }
  }

  .ex-org-tree > .ex-org-tree-node {
    padding-top: 0;

    &:after {
      border-left: 0;
    }
  }
</style>
