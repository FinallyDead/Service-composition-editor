<template>
<div>
<network class="wrapper" ref="network"
      :nodes="nodes"
      :edges="edges"
      :options="options"
      >
    </network>
<button @click="deleteNode">Delete selected node</button>
</div>

</template>

<script>
import Vue from "vue";
export default{
 data: () => ({
      nodes: [
        {id:0, type: '', service: '', callback: '', inputs: [{name: '', value: '', type: ''}], outputs: [{name: '', value: '', type: ''}],  label:'database',shape: 'database'},
        {id:1, type: '', service: '', callback: '', inputs: [{name: '', value: '', type: ''}], outputs: [{name: '', value: '', type: ''}],  label:'data1',shape: 'database'},
        {id:2, type: '', service: '', callback: '', inputs: [{name: '', value: '', type: ''}], outputs: [{name: '', value: '', type: ''}],  label:'data2',shape: 'database'},
        {id:3, type: '', service: '', callback: '', inputs: [{name: '', value: '', type: ''}], outputs: [{name: '', value: '', type: ''}],  label:'data3',shape: 'database'}
      ],
      edges: [
        {from: 0, to: 1},
        {from: 2, to: 1},
        {from: 2, to: 0},
        {from: 3, to: 1},
      ],
      options: {
         nodes: {
          borderWidth: 4
         },
         edges: {
          color: 'green'
        }
      },
      CurrentNode: ''
  }),
  methods: {
    deleteNode(){
           this.nodes.splice(this.CurrentNode, 1);
        }
  },
  mounted(){
  var vm = this;
    this.$refs.network.network.on('click', function (properties){
    var clickedNodes = properties.nodes[0];
    Vue.set(vm, 'CurrentNode', clickedNodes)
    console.log(clickedNodes);
    });
   }
  }




</script>

<style>
.wrapper{
  min-height: 100vh;
  border: 1px solid black;
  background-color: #ccc;
  padding: 10px;
  height: 100vh;
}
</style>
