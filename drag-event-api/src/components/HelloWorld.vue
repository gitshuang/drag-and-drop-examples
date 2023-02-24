<template>
  <div>
    <div class="header" >
      <a v-on:click="$event=>handleClick('file:///Users/shuanger/Desktop/drag-drop/drag-mouse-style.html')">鼠标样式</a>
      <a v-on:click="$event=>handleClick('file:///Users/shuanger/Desktop/drag-drop/drag-set-drag-image.html')">拖拽浮层自定义</a>
      <a v-on:click="$event=>handleClick('file:///Users/shuanger/Desktop/drag-drop/drag-file.html')">外部文件</a>
      <a v-on:click="$event=>handleClick('file:///Users/shuanger/Desktop/drag-drop/drag-use-mouse-event.html')">鼠标事件拖拽</a>
    </div>
    
  <transition-group class="list">
    <div
      @dragstart="dragstart(index)"
      @dragenter="dragenter($event, index)"
      @dragover="dragover($event, index)"
      draggable= "true"
      v-for="(item, index) in list"
      :key="item.label"
      class="list-item"
    >
      {{item.label}}
  </div>
  </transition-group>
  </div>
  
</template>

<script>
export default {
data() {
  return {
     list: [
      { label: '列表1' },
      { label: '列表2' },
      { label: '列表3' },
      { label: '列表4' },
      { label: '列表5' },
      { label: '列表6' },
      { label: '列表7' },
      { label: '列表8' }
    ],
    // 源对象的下标
    dragIndex: '',
    // 目标对象的下标
    enterIndex: '',
    timeout: null,
  }
},
destroyed() {
 // 每次离开当前界面时，清除定时器
  clearInterval(this.timeout)
  this.timeout = null
},
methods: {
  handleClick(url){
    console.log(11111)
    window.open(url)
  },
  dragstart(index) {
    console.log('start index ===>>> ',index)
    this.dragIndex = index
  },

  // dragenter 和 dragover 事件的默认行为是拒绝接受任何被拖放的元素。 
  // 因此，我们要在这两个拖放事件中使用`preventDefault`来阻止浏览器的默认行为
  dragenter(e,index) {  
    e.preventDefault();
    this.enterIndex = index
    if( this.timeout !== null) {
      clearTimeout(this.timeout)
    }
    // 拖拽事件的防抖
    this.timeout = setTimeout(() => {
       if( this.dragIndex !== index){
          const source = this.list[this.dragIndex]
          this.list.splice(this.dragIndex,1)
          this.list.splice(index, 0 , source )
          // 排序变化后目标对象的索引变成源对象的索引
          this.dragIndex = index;
        }
    }, 100);
    console.log('dragenter:', index);
  },
  dragover(e, index) {
    console.log("dragover:",index)
    e.preventDefault();
  }
},
}
</script>

<style lang='scss' scoped>
.header{
  margin-bottom: 40px;
  a{
    margin-right: 30px;
  }
}
.list {
list-style: none;
display: block;
.list-item {
  // 设置 动画效果
  transition: transform .3s;
  cursor: move;
  width: 200px;
  background: #EA6E59;
  border-radius: 4px;
  color: #FFF;
  margin-bottom: 6px;
  height: 200px;
  line-height: 50px;
  text-align: center;
  float: left
}
}
</style>