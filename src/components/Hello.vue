<template>
  <div class="fluid container">

    <div  class="col-md-3">
      <draggable class="list-group" element="ul" v-model="list" :options="dragOptions" :move="onMove" @start="isDragging=true" @end="isDragging=false">
        <transition-group style="display: flex;flex-direction: row;justify-content: flex-start;flex-wrap: wrap" type="transition" :name="'flip-list'">
          <div style="background-color: red;width: 100px;height: 120px;margin-left: 30px;margin-top: 15px"  class="list-group-item" v-for="(element , index) in list" :key="element.order">

            <img  v-if="index % 2 ==0" style="width: 100px;height: 120px" src="../assets/01.png">

            <img  v-else style="width: 100px;height: 120px" src="../assets/02.png">

          </div>
        </transition-group>
      </draggable>
    </div>

    <div  class="list-group col-md-3">
      <pre>{{listString}}</pre>
    </div>
  </div>
</template>

<script>
  import draggable from 'vuedraggable'

  export default {
    name: 'hello',
    components: {
      draggable,
    },
    data () {
      return {
        list:[],
        editable:true,
        isDragging: false,
        delayedDragging:false
      }
    },
    created () {

      this.getData();

    },
    methods:{

      getData:function () {

        this.list = [{name:1,order:1,fixed: false},
          {name:2,order:2,fixed: false},
          {name:3,order:3,fixed: false},
          {name:4,order:4,fixed: false},
          {name:5,order:5,fixed: false},
          {name:6,order:6,fixed: false},
          {name:7,order:7,fixed: false},
          {name:8,order:8,fixed: false},
          {name:9,order:9,fixed: false},
          {name:10,order:10,fixed: false},
          {name:11,order:11,fixed: true},
        ]


      },
      orderList () {
        this.list = this.list.sort((one,two) =>{return one.order-two.order; })
      },
      onMove ({relatedContext, draggedContext}) {
        const relatedElement = relatedContext.element;
        const draggedElement = draggedContext.element;
        return (!relatedElement || !relatedElement.fixed) && !draggedElement.fixed
      }
    },
    computed: {
      dragOptions () {
        return  {
          animation: 0,
          group: 'description',
          disabled: !this.editable,
          ghostClass: 'ghost'
        };
      },
      listString(){
        return JSON.stringify(this.list, null, 2);
      },
      
    },
    watch: {
      isDragging (newValue) {
        if (newValue){
          this.delayedDragging= true
          return
        }
        this.$nextTick( () =>{
          this.delayedDragging =false
        })
      }
    }
  }
</script>

<style>
  .flip-list-move {
    transition: transform 0.5s;
  }

  .no-move {
    transition: transform 0s;
  }

  .ghost {
    opacity: .5;
    background: #C8EBFB;
  }

  .list-group {
    min-height: 20px;
  }

  .list-group-item {
    cursor: move;
  }

  .list-group-item i{
    cursor: pointer;
  }
</style>
