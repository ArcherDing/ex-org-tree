<template>
  <div :class="cls">
    <div class="ex-org-tree-node-label" @click="onNodeClick">
      <div :class="labelCls" :style="{width: innerLabelWidth}">
        <slot :data="data">
          {{data[props.label]}}
        </slot>
        <span v-if="collapsible && !isLeaf" :class="expandCls" @click="onExpandClick"></span>
      </div>
    </div>
    <div v-if="!isLeaf && (!collapsible || showChildren)" class="ex-org-tree-node-children">
      <ex-org-tree-node
        v-for="child in data[props.children]"
        :key="child.id"
        :data="child"
        :props="props"
        :horizontal="horizontal"
        :label-width="labelWidth"
        :collapsible="collapsible"
        :render-content="renderContent"
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
  export default {
    name: 'ExOrgTreeNode',
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
    },
    data() {
      return {
        cls: ['ex-org-tree-node'],
        labelCls: ['ex-org-tree-node-label-inner'],
        expandCls: ['ex-org-tree-node-btn'],
        innerLabelWidth: ''
      };
    },
    computed: {
      isLeaf() {
        const children = this.data[this.props.children];
        return !(Array.isArray(children) && children.length > 0);
      },
      isExpand() {
        return this.data[this.props.expand];
      },
      showChildren() {
        return this.expandCls.indexOf('expanded') !== -1;
      }
    },
    methods: {
      initView() {
        if (this.isLeaf) {
          this.cls.push('is-leaf');
        } else if (this.collapsible && !this.isExpand) {
          this.cls.push('collapsed');
        }
        if (typeof this.labelWidth === 'number') {
          this.innerLabelWidth = this.labelWidth + 'px';
        }
        if (typeof this.labelClassName === 'string') {
          this.labelCls.push(this.labelClassName);
        }
        if (typeof this.labelClassName === 'function') {
          this.labelCls.push(this.labelClassName(this.data));
        }
        if (this.isExpand) {
          this.expandCls.push('expanded');
        }
      },
      onNodeClick(e) {
        e.stopPropagation();
        this.$emit('node-click', this.data);
      },
      onExpandClick(e) {
        e.stopPropagation();
        const index = this.expandCls.indexOf('expanded');
        if (index !== -1) {
          this.expandCls.splice(index, 1);
        } else {
          this.expandCls.push('expanded');
        }
        this.$emit('expand-click', this.showChildren ? 'expanded' : 'collapsed', this.data);
      }
    },
    created() {
      this.initView();
    }
  };
</script>
<style lang="less" scoped>
  .ex-org-tree-node,
  .ex-org-tree-node-children {
    position: relative;
    margin: 0;
    padding: 0;
    list-style-type: none;

    &:before, &:after {
      transition: all .35s;
    }
  }

  .ex-org-tree-node-label {
    position: relative;
    display: inline-block;

    .ex-org-tree-node-label-inner {
      padding: 10px 15px;
      text-align: center;
      border-radius: 3px;
      box-shadow: 0 1px 5px rgba(0, 0, 0, .15);
    }
  }

  .ex-org-tree-node-btn {
    position: absolute;
    top: 100%;
    left: 50%;
    width: 20px;
    height: 20px;
    z-index: 10;
    margin-left: -11px;
    margin-top: 9px;
    background-color: #fff;
    border: 1px solid #ccc;
    border-radius: 50%;
    box-shadow: 0 0 2px rgba(0, 0, 0, .15);
    cursor: pointer;
    transition: all .35s ease;

    &:hover {
      background-color: #e7e8e9;
      transform: scale(1.15);
    }

    &:before, &:after {
      content: '';
      position: absolute;
    }

    &:before {
      top: 50%;
      left: 4px;
      right: 4px;
      height: 0;
      border-top: 1px solid #ccc;
    }

    &:after {
      top: 4px;
      left: 50%;
      bottom: 4px;
      width: 0;
      border-left: 1px solid #ccc;
    }

    &.expanded:after {
      border: none;
    }
  }

  .ex-org-tree-node {
    padding-top: 20px;

    &:not(:only-child) {
      float: left;
    }

    &.is-leaf, &.collapsed {
      padding-left: 10px;
      padding-right: 10px;
    }

    &:before, &:after {
      content: '';
      position: absolute;
      top: 0;
      left: 0;
      width: 50%;
      height: 19px;
    }

    &:after {
      left: 50%;
      border-left: 1px solid #ddd;
    }

    &:not(:first-child):before,
    &:not(:last-child):after {
      border-top: 1px solid #ddd;
    }

  }

  .collapsible .ex-org-tree-node.collapsed {
    padding-bottom: 30px;

    >.ex-org-tree-node-label:after {
      content: '';
      position: absolute;
      top: 100%;
      left: 0;
      width: 50%;
      height: 20px;
      border-right: 1px solid #ddd;
    }

    >.ex-org-tree-node-children:before {
      border: none;
    }
  }

  .ex-org-tree-node-children {
    padding-top: 20px;

    &:before {
      content: '';
      position: absolute;
      top: 0;
      left: 50%;
      width: 0;
      height: 20px;
      border-left: 1px solid #ddd;
    }

    &:after {
      content: '';
      display: table;
      clear: both;
    }
  }

  .horizontal {
    .ex-org-tree-node {
      // display: flex;
      // flex-direction: row;
      // justify-content: flex-start;
      // align-items: center;
      display: table-cell;
      float: none;
      padding-top: 0;
      padding-left: 20px;

      &.is-leaf, &.collapsed {
        padding-top: 10px;
        padding-bottom: 10px;
      }

      &:before, &:after {
        width: 19px;
        height: 50%;
      }

      &:after {
        top: 50%;
        left: 0;
        border-left: 0;
      }

      &:only-child:before {
        top: 1px;
        border-bottom: 1px solid #ddd;
      }

      &:not(:first-child):before,
      &:not(:last-child):after {
        border-top: 0;
        border-left: 1px solid #ddd;
      }

      &:not(:only-child):after {
        border-top: 1px solid #ddd;
      }

      .org-tree-node-inner {
        display: table;
      }

    }

    .ex-org-tree-node-label {
      display: table-cell;
      vertical-align: middle;
    }

    &.collapsible .ex-org-tree-node.collapsed {
      padding-right: 30px;

      >.ex-org-tree-node-label:after {
        top: 0;
        left: 100%;
        width: 20px;
        height: 50%;
        border-right: 0;
        border-bottom: 1px solid #ddd;
      }

      >.ex-org-tree-node-children:before {
        border: none;
      }
    }

    .ex-org-tree-node-btn {
      top: 50%;
      left: 100%;
      margin-top: -11px;
      margin-left: 9px;
    }

    & > .ex-org-tree-node:only-child:before {
      border-bottom: 0;
    }

    .ex-org-tree-node-children {
      // display: flex;
      // flex-direction: column;
      // justify-content: center;
      // align-items: flex-start;
      display: table-cell;
      padding-top: 0;
      padding-left: 20px;

      &:before {
        top: 50%;
        left: 0;
        width: 20px;
        height: 0;
        border-left: 0;
        border-top: 1px solid #ddd;
      }

      &:after {
        display: none;
      }

      & > .ex-org-tree-node {
        display: block;
      }
    }
  }
</style>
