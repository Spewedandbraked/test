<script>
export default{
  data(){
    return{
        width: -1,
        height: -1,
        top: 200,
        left:0,
        color:'#'+(Math.random()*0xFFFFFF<<0).toString(16)
    }
  },
  methods:{
    track(){
        if (event.target.id == 'move') {
            addEventListener('mousemove', this.move);
            console.log([event.target.id, 'as']);
        } else {
            addEventListener('mousemove', this.scale);
            console.log([event.target.id, 'sss']);
        }
    },
    move(){
        this.top = event.pageY -12.5 ;
        this.left = event.clientX -12.5
    },
    scale(){
            this.width = event.pageX - this.left +12.5;
            this.height = event.clientY - this.top +12.5;
    },
    destroy(){
        removeEventListener('mousemove', this.move);
        removeEventListener('mousemove', this.scale);
    }
  },
  mounted: function () {
    addEventListener('mouseup', this.destroy);
  }
}
</script>

<template>
    <div id="border" style="position: absolute; z-index: -1;">
        <div class="ctrl" id="move" @mousedown="track()" @mouseup="destroy()"></div>
        <div class="ctrl" id="scale" @mousedown="track()" @mouseup="destroy()"></div>
        <div style="overflow: scroll; width: 100%; height: 100%;"><slot></slot></div>     
    </div>
</template>

<style>
  #border {
    padding: 25px;
    padding-right: 5px;
    padding-bottom: 5px;
    background-color: rgba(20, 20, 20, 5%);
    box-shadow:inset 0px 0px 0px 5px  v-bind(color);
    height: v-bind(height + 'px');
    width: v-bind(width + 'px');
    top: v-bind(top + 'px');
    left: v-bind(left + 'px');
  }
  .ctrl{
    z-index: 2;
    border: thick dashed v-bind(color);
    position: absolute;
    width: 25px;
    height: 25px;
  }
  #move{
    top:0;
    left: 0;
  }
  #scale{
    right: 0;
    bottom: 0;
  }
</style>