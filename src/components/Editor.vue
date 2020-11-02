<template>
<network class="wrapper" ref="network"
      :nodes="nodes"
      :edges="edges"
      :options="options"
      >
    </network>
<button @click="deleteNode">Delete selected node</button>


</template>

<script>
window.CurrentNode = 0;
export default{
 data() {
    return {
      nodes: [
        {id:1, type: '', service: '', callback: '', inputs: [{name: '', value: '', type: ''}], outputs: [{name: '', value: '', type: ''}],  label:'database',shape: 'database'},
        {id:2, type: '', service: '', callback: '', inputs: [{name: '', value: '', type: ''}], outputs: [{name: '', value: '', type: ''}],  label:'data1',shape: 'database'},
        {id:3, type: '', service: '', callback: '', inputs: [{name: '', value: '', type: ''}], outputs: [{name: '', value: '', type: ''}],  label:'data2',shape: 'database'},
        {id:4, type: '', service: '', callback: '', inputs: [{name: '', value: '', type: ''}], outputs: [{name: '', value: '', type: ''}],  label:'data3',shape: 'database'}
      ],
      edges: [
        {from: 1, to: 2},
        {from: 3, to: 2},
        {from: 3, to: 1},
        {from: 4, to: 2},
      ],
      options: {
         nodes: {
          borderWidth: 4
         },
         edges: {
          color: 'green'
        }
      },
      idOfNode: 5
    }
  },
  mounted(){
    this.$refs.network.network.on('click', function (properties){
    var clickedNodes = properties.nodes[0];
    console.log(clickedNodes);
    CurrentNode = clickedNodes;
    }),
    deleteNode(){
        this.nodes.splice(CurrentNode, 1);
    }
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
