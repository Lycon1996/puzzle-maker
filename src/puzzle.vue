<template lang="">
  <div class="puzzle">
    <div class="puzzle-content">
    <div class="item" :class="'item-' + index" :id="index === 8 ? 'white' : ''" v-for="(item, index) in 9" :key="index" :style="'background-image:url(' + require('./img/bg.png') + ')'" @click="handle"></div>
    </div>
  </div>
</template>
<script>
export default {
  props: {
    bg: {
      type: String,
      default: './img/bg.png'
    },
    white: {
      type: Number,
      default: 8
    }
  },
  data () {
    return {
      pos: [
        {left: '0px', top: '0px'},
        {left: '200px', top: '0px'},
        {left: '400px', top: '0px'},
        {left: '0px', top: '200px'},
        {left: '200px', top: '200px'},
        {left: '400px', top: '200px'},
        {left: '0px', top: '400px'},
        {left: '200px', top: '400px'},
        {left: '400px', top: '400px'}
      ],
      sor: []
    }
  },
  mounted() {
    this.sor = this.pos.concat()
    this.sor.sort(() => {
      return Math.random() - 0.5
    })
    //获取所有拼图标签
    let game = document.querySelectorAll('.item')
    console.log(game)
    for (let i = 0; i < game.length; i++) {
      game[i].style.left = this.sor[i].left
      game[i].style.top = this.sor[i].top
    }
  },
  methods: {
    handle (e) {
      let game = document.querySelectorAll('.item')
      let lucency = document.querySelector('#white')
      //拿到空白位置的left值
      let pleft = lucency.offsetLeft
      //拿到空白位置的top值
      let ptop = lucency.offsetTop
      //获取点击方块的left
      let gleft = e.target.offsetLeft
      //获取点击方块的top
      let gtop = e.target.offsetTop
      if(gleft - pleft <= 200 && gleft - pleft > 0 && gtop === ptop || pleft - gleft <= 200 && pleft - gleft >0 && gtop === ptop) {
        e.target.style.left = pleft + 'px'
        lucency.style.left = gleft + 'px'
      }else if(gtop - ptop <= 200 && gtop - ptop >0 && gleft === pleft || ptop - gtop <= 200 && ptop-gtop > 0 && gleft === pleft) {
        e.target.style.top = ptop + 'px'
        lucency.style.top = gtop + 'px'
      }
      let a = []
      for (let j = 0; j < this.pos.length; j++) {
        if(this.pos[j].left === game[j].style.left && this.pos[j].top === game[j].style.top){
        a.push(j)
        if(a.length == this.pos.length){
          console.log('完成')
          this.$emit('finish')
        }
      }
      }
    }
  }
}
</script>
<style scoped lang="scss">
.puzzle{
  position: fixed;
  top: 40px;
  right: 0;
  bottom: 0;
  left: 0;
  margin: auto;
  width: 600px;
  height: 600px;
  border: 2px solid skyblue;
  padding: 1px;
}
.puzzle .puzzle-content{
  height: 100%;
  width: 100%;
  position: relative;
}
.item{
  position: absolute;
  width: 200px;
  height: 200px;
  /* background-image: url("./img/bg.png"); */
  background-repeat: no-repeat;
  padding: 1px;
  box-sizing: border-box;
  background-clip: content-box;
  transition: all 0.3s;
}
.item-0{
  left: 0;
  top: 0;
  background-position: 0px 0px;
}
.item-1{
  left: 200px;
  top: 0;
  background-position: -200px 0px;
}
.item-2{
  left: 400px;
  top: 0;
  background-position: -400px 0px;
}
.item-3{
  left: 0;
  top: 200px;
  background-position: 0 -200px;
}
.item-4{
  left: 200px;
  top: 200px;
  background-position: -200px -200px;
}
.item-5{
  left: 400px;
  top: 200px;
  background-position: -400px -200px;
}
.item-6{
  left: 0;
  top: 400px;
  background-position: 0 -400px;
}
.item-7{
  left: 200px;
  top: 400px;
  background-position: -200px -400px;
}
.item-8{
  left: 400px;
  top: 400px;
  opacity: 0;
  background-position: -400px -400px;
}
</style>
