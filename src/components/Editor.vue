<template>
<div>
<network class="wrapper" ref="network"
      :nodes="nodes"
      :edges="edges"
      :options="options"
      >
    </network>
<br/><button @click="addNewTask">Add new Tasks.</button><br/>
<br/><button @click="deleteNode">Delete selected node.</button><br/><br/>
<button @click="editNode" class="editSelectedNode">Edit selected node.</button>
<form>
<p><textarea id="edit" rows=20 cols=150 ref="edit"></textarea></p>
</form>
<button @click="confirmChngs" id="cnfChg" ref="cnfChg">Confirm changes.</button>
</div>

</template>
<script>
export default{
 data: () => ({
      nodes: [

      ],
      edges: [

      ],
      options: {
         nodes: {
          borderWidth: 4
         },
         edges: {
          color: '#9CEE90'
        }
      },
      CurrentIndex: 0,
      selected: false
  }),
  methods: {
    deleteNode(){
         this.nodes.splice(this.currentIndex, 1);
        },
    editNode(){
    if (this.selected){
        this.$refs.cnfChg.style.display="block";
        this.$refs.edit.style.display="block";
        this.$refs.edit.value=JSON.stringify(this.nodes[this.currentIndex]);
        }
    },
    confirmChngs(){
        this.$refs.cnfChg.style.display="none";
        this.$refs.edit.style.display="none";
        this.nodes[this.currentIndex]=JSON.parse(this.$refs.edit.value);
        this.edges.splice(0,this.edges.length);
        for (let i=0;i<this.nodes.length;i++){
                    var strEdgBond = " ";
                    if (this.nodes[i].inputs[0].value.indexOf("ref::")==0){
                            strEdgBond = this.nodes[i].inputs[0].value.slice(5);
                            var pos = 0;
                            pos = strEdgBond.indexOf("::");
                            strEdgBond = strEdgBond.slice(0,pos);
                            this.edges.push({from: this.nodes[i].id, to:strEdgBond});
                    }
               }
    },
    addNewTask(){
       this.nodes.splice(0,this.nodes.length);
       this.edges.splice(0,this.edges.length);
       var Tasks = [
                {id:"0",  type: '', service: '', callback: '', inputs: [{name: '', value: 'ref::1::out', type: ''}], outputs: [{name: '', value: '', type: ''}]},
                {id:"1",  type: '', service: '', callback: '', inputs: [{name: '', value: '', type: ''}], outputs: [{name: '', value: '', type: ''}]},
                {id:"2",  type: '', service: '', callback: '', inputs: [{name: '', value: 'ref::1::out', type: ''}], outputs: [{name: '', value: '', type: ''}]},
                {id:"3",  type: '', service: '', callback: '', inputs: [{name: '', value: 'ref::1::out', type: ''}], outputs: [{name: '', value: '', type: ''}]}
              ];
       for (let i=0;i<Tasks.length;i++){
            this.nodes.push(Tasks[i]);
            this.nodes[i].label="Task "+(i+1);
       }
       for (let i=0;i<this.nodes.length;i++){
            var strEdgBond = " ";
            if (this.nodes[i].inputs[0].value.indexOf("ref::")==0){
                    strEdgBond = this.nodes[i].inputs[0].value.slice(5);
                    var pos = 0;
                    pos = strEdgBond.indexOf("::");
                    strEdgBond = strEdgBond.slice(0,pos);
                    this.edges.push({from: this.nodes[i].id, to:strEdgBond});
            }
       }
    }
  },
  mounted(){
  var vm = this;
  function onloadCallback(callback){
  callback();
  }
  onloadCallback(function(){
  var Tasks = [
                    {id:"0",  type: '', service: '', callback: '', inputs: [{name: '', value: 'ref::1::out', type: ''}], outputs: [{name: '', value: '', type: ''}]},
                    {id:"1",  type: '', service: '', callback: '', inputs: [{name: '', value: '', type: ''}], outputs: [{name: '', value: '', type: ''}]},
                    {id:"2",  type: '', service: '', callback: '', inputs: [{name: '', value: 'ref::1::out', type: ''}], outputs: [{name: '', value: '', type: ''}]},
                    {id:"3",  type: '', service: '', callback: '', inputs: [{name: '', value: 'ref::1::out', type: ''}], outputs: [{name: '', value: '', type: ''}]}
                  ];
           for (let i=0;i<Tasks.length;i++){
                vm.nodes.push(Tasks[i]);
                vm.nodes[i].label="Task "+(i+1);
           }
           for (let i=0;i<vm.nodes.length;i++){
                var strEdgBond = " ";
                if (vm.nodes[i].inputs[0].value.indexOf("ref::")==0){
                        strEdgBond = vm.nodes[i].inputs[0].value.slice(5);
                        var pos = 0;
                        pos = strEdgBond.indexOf("::");
                        strEdgBond = strEdgBond.slice(0,pos);
                        vm.edges.push({from: vm.nodes[i].id, to:strEdgBond});
                }
           }
  });
  document.onclick = function(e){
      if ( e.target.className == 'editSelectedNode') {
          vm.selected=false;
      }
  }
  this.$refs.network.network.on('click', function (properties){
    vm.selected=true;
    for (let i=0; i<vm.nodes.length; i++){
            if (properties.nodes[0]==vm.nodes[i].id){
                vm.currentIndex=i;
            }
        }
    console.log(properties);
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