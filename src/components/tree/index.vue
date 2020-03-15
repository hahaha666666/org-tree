<template>
  <div class="tree-box">
    <vue2-org-tree
      :data="data"
      :horizontal="horizontal"
      :collapsable="collapsable"
      :label-class-name="labelClassName"
      :render-content="renderContent"
      @on-expand="onExpand"
      @on-node-click="onNodeClick"
    />
    <div class="tipBg" v-if="tipShow">
      <div class="tipBox">
        <div class="tip-title">添加下级</div>
        <input type="text" placeholder="请输入级别" v-model="inputValue">
        <div class="div-button" @click="cancel">取消</div>
        <div class="div-button" @click="addNode">增加</div>
      </div>
    </div>
  </div>
</template>
<script>
  import Vue2OrgTree from './org-tree/index.js'
  export default{
    name:'architecture',
    components: {
      Vue2OrgTree
    },
    data: () => ({
      data: {
        id: 0,
        label:'总节点',
        children:[]
      },
      horizontal: false,
      collapsable: true,
      expandAll: false,
      labelClassName: 'bg-white',
      tipShow:false,
      inputValue:null,
      nodeData:null
    }),
    methods: {
      renderContent: function(h, data) {
        return data.label
      },
      onExpand: function(e,data) {
        if ('expand' in data) {
          data.expand = !data.expand
          if (!data.expand && data.children) {
            this.collapse(data.children)
          }
        } else {
          this.$set(data, 'expand', true)
        }
      },
      onNodeClick: function(e, data) {
          this.tipShow=true;
          this.nodeData=data;
      },
      collapse: function(list) {
        var _this = this
        list.forEach(function(child) {
          if (child.expand) {
            child.expand = false
          }
          child.children && _this.collapse(child.children)
        })
      },
      expandChange: function() {
        this.toggleExpand(this.data, this.expandAll)
      },
      toggleExpand: function(data, val) {
        var _this = this
        if (Array.isArray(data)) {
          data.forEach(function(item){
            _this.$set(item, 'expand', val);
            if (item.children) {
              _this.toggleExpand(item.children, val)
            }
          });
        }else {
          this.$set(data, 'expand', val)
          if (data.children) {
            _this.toggleExpand(data.children, val)
          }
        }
      },
      cancel(){
          this.tipShow=false;
          this.inputValue=null
      },
      addNode(){
        let childrenList;
        if(this.nodeData.children){
          childrenList= this.nodeData.children.concat({label:this.inputValue});
        }else{
          childrenList= [{label:this.inputValue}];
        }
        this.$set(this.nodeData, 'children', childrenList);
        this.$set(this.nodeData, 'expand', true)
        this.toggleExpand(this.nodeData,true)
        this.cancel()
      }
    }
  }
</script>
<style scoped>
  .tree-box{width: 100%;height: 100%;position: absolute;top:0;bottom:0;left:0;right:0;
    background: white}
  .tipBg{position: absolute;top:0;bottom:0;left: 0;right:0;background-color: rgba(0,0,0,0.5);z-index: 9999}
  .tipBox{width:350px;height: 200px;border-radius: 10px 10px;background-color: white;position: absolute;top:0;bottom:0;left: 0;right:0;overflow: hidden;
  margin:auto}
  .tip-title{width: 100%;height: 40px;background-color: red;color:white;text-align: center;line-height: 40px;letter-spacing: 2px;}
  .tipBox input{width: 80%;height:40px;margin:30px 10%;border: solid 1px black;border-radius: 5px 5px;padding-left: 20px;box-sizing: border-box}
  .tipBox .div-button{width: 80px;height: 30px;text-align: center;line-height: 30px;background-color: red;color:white;cursor: pointer;
  border-radius: 5px;display: inline-block;margin-left: 65px}
</style>
