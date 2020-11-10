<template>
<div>
<network class="wrapper" ref="network"
      :nodes="nodes"
      :edges="edges"
      :options="options"
      >
    </network>
<br/><button @click="deleteNode">Delete selected node.</button><br/><br/>
<button @click="editNode" class="editSelectedNode">Edit selected node.</button>
<form>
<p><textarea id="edit" rows=20 cols=150 ref="edit"></textarea></p>
</form>
<button @click="confirmChngs" id="cnfChg" ref="cnfChg">Confirm changes.</button>
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
          color: '#9CEE90'
        }
      },
      CurrentNode: 99,
      selected: false
  }),
  methods: {
    deleteNode(){
         this.nodes.splice(this.CurrentNode, 1);
        },
    editNode(){
    if (this.selected==true){
        this.$refs.cnfChg.style.display="block";
        this.$refs.edit.style.display="block";
        this.$refs.edit.value=JSON.stringify(this.nodes[this.CurrentNode]);
        }
    },
    confirmChngs(){
        this.$refs.cnfChg.style.display="none";
        this.$refs.edit.style.display="none";
        this.nodes[this.CurrentNode]=JSON.parse(this.$refs.edit.value);
    }
  },
  mounted(){
  var vm = this;
  document.onclick = function(e){
      if ( e.target.className == 'editSelectedNode') {
          vm.selected=false;
      }
  }
  this.$refs.network.network.on('click', function (properties){
    for (let i=0; i<vm.nodes.length; i++){
        vm.nodes[i].id=i;
    }
    vm.selected=true;
    var clickedNodes = properties.nodes[0];
    Vue.set(vm, 'CurrentNode', clickedNodes);
    console.log(clickedNodes);
    });
   }
  }
</script>

<style>
.wrapper{
  min-height: 75vh;
  min-width: 120vh;
  border: 1px solid black;
  background-color: #565656;
  padding: 10px;
  height: 75vh;
  width: auto;
}
#edit{
display: none;
margin-left: auto;
margin-right: auto;
background-color: #424242;
color: white;
}
#cnfChg{
display: none;
margin-left: auto;
margin-right: auto;
}
</style>
