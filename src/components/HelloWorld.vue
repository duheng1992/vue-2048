<template>
  <div class="hello-root">
    <div class="hello-title">
      <h1>{{ msg }}</h1>
    </div>
    <div class="hello-container">
      <div class="hello-container-nodes" v-for="(num, index) of nums" :key="index">
        <Node :number="num[0]"></Node>
        <Node :number="num[1]"></Node>
        <Node :number="num[2]"></Node>
        <Node :number="num[3]"></Node>
      </div>
    </div>
    <div class="hello-footer">
      得分：<div class="score" v-text="score"></div>
      最高得分：<div class="higtest-score" v-text="score"></div>
    </div>
  </div>
</template>

<script>
import Node from './Node';

export default {
  name: 'HelloWorld',
  data () {
    return {
      msg: '2048 games',
      nums: [
        [0, 0, 0, 0],
        [0, 0, 0, 0],
        [0, 0, 0, 0],
        [0, 0, 0, 0]
      ],
      score: 0,
      isScoreChanged: false,
    }
  },

  components: {
    Node
  },

  methods: {
    init() {
      this.generateRandomCell(this.nums);
      // 右39 左37 上38 下40
      document.onkeydown = (e) => {
        switch(e.keyCode) {
          case 37:
            if(this.canMoveRight(this.hangqufan(this.nums))){
              this.nums = this.moveLeft(this.nums);
              if(!this.isScoreChanged){
                this.generateRandomCell(this.nums);
              }
            }
            break;
          case 38:
            if(this.canMoveRight(this.liequfan(this.nums))){
              this.nums = this.zhuanzhi(this.moveLeft(JSON.parse(JSON.stringify(this.zhuanzhi(this.nums)))));
              if(!this.isScoreChanged){
                this.generateRandomCell(this.nums);
              }
            }
            break;
          case 39:
            if(this.canMoveRight(this.nums)){
              this.nums = this.moveRight(JSON.parse(JSON.stringify(this.nums)));
              if(!this.isScoreChanged){
                this.generateRandomCell(this.nums);
              }
            }
            break;
          case 40:
            if(this.canMoveRight(this.zhuanzhi(this.nums))){
              this.nums = this.zhuanzhi(this.moveRight(JSON.parse(JSON.stringify(this.zhuanzhi(this.nums)))));
              if(!this.isScoreChanged){
                this.generateRandomCell(this.nums);
              }
            }
            break;
        }
      }
    },

    generateRandomCell(data) {
      let _data = JSON.parse(JSON.stringify(data));
      let zeros = [];  //存入目前是0的单元格的坐标
      for(let i = 0; i < _data.length; i++){
        for(let j = 0; j < _data[i].length; j++){
          if(_data[i][j] === 0){
            zeros.push([i, j]);
          }
        }
      }

      //随机取两个
      if(zeros.length >= 2){
        let zero1 = zeros.splice(Math.floor(Math.random()*(zeros.length-1)), 1);
        let zero2 = zeros.splice(Math.floor(Math.random()*(zeros.length-1)), 1);
  
        _data[zero1[0][0]][zero1[0][1]] = 2;
        _data[zero2[0][0]][zero2[0][1]] = 4;
      } else {
        let zero1 = zeros.splice(Math.floor(Math.random()*(zeros.length-1)), 1);
        _data[zero1[0][0]][zero1[0][1]] = 4;
      }

      //这里值改变了，dom却没有刷新
      this.nums = _data;
      // console.log(this.nums)
    },

    canMoveRight(_data) {
      for(let i = 3; i >= 0; i--){
        for(let j = 2; j >= 0; j--){
          if(_data[i][j] != 0){ //0表示空位
            if(_data[i][j+1] === 0 || _data[i][j+1] === _data[i][j]){
              return true;
            }
          }
        }
      }
      return false;
    },

    moveRight(_data) {
      let _score = this.score;
      //遍历行
      for(let i = 3; i >= 0; i--){
        //遍历具体行上的每一列
        for(let j = 3; j >= 0; j--){
          if(_data[i][j] != 0){
            for(let k = 3; k > j; k--){
              //1. k在该行j列的右边，且k处为0
              if(_data[i][k] === 0 && this.noBlockHorizontal(i, j, k, _data)){
                //TODO:添加动画
                _data[i][k] = _data[i][j];
                _data[i][j] = 0;
                continue;
              } else if(_data[i][k] === _data[i][j] && this.noBlockHorizontal(i, j, k, _data)){
                //2. j和k相等了，合并
                 _data[i][k] += _data[i][k];
                 this.addScore();
                _data[i][j] = 0;
                continue;
              }
            }
          }
        }
      }

      if(_score === this.score){
        //说明移动了，但没有合并
        this.isScoreChanged = false;
      } else {
        this.isScoreChanged = true;
      }

      return _data;
    },

    moveLeft(_data) {
      return this.hangqufan(this.moveRight(JSON.parse(JSON.stringify(this.hangqufan(_data)))));
    },

    //每一行上两格之间是否有0
    noBlockHorizontal(row, col1, col2, data) {
      for(let i = col1 + 1; i < col2; i++){
        if(data[row][i] != 0){
          return false;
        }
      }
      return true;
    },

    //转置
    zhuanzhi(arr) {
      let arr_new = [];
      for (let i = 0; i < arr[0].length; i++){
	      arr_new[i]=[];
      }

      for (let i = 0; i < arr.length; i++){   
	      for (let j = 0; j < arr[i].length; j++){
		      arr_new[j][i] = arr[i][j];
	      }
      }
      
      return arr_new;
    },

    //行取反
    hangqufan(arr) {
      let _arr = JSON.parse(JSON.stringify(arr));
      let newArr = [];
      for(let i in _arr){
        newArr.push(_arr[i].reverse());
      }
      return newArr;
    },

    //列取反
    liequfan(arr) {
      return this.hangqufan(this.zhuanzhi(arr));
    },

    addScore() {
      this.score++;
    },
  },


  mounted() {
    this.init();
  },

}
</script>

<style scoped>
  .hello-root {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
  }

  .hello-container {
    height: 500px;
    width: 500px;
    display: flex;
    justify-content: space-around;
    flex-direction: column;
    background: #999;
    border-radius: 8px;
    padding: 10px;
  }

  .hello-container-nodes {
    flex: 1 1 0%;
    display: flex;
    justify-content: space-around;
    flex-direction: row;
  }

  .hello-footer {
    width: 500px;
    display: flex;
    justify-content: space-around;
    margin-top: 5px;
    font-family: "microsoft yahei";
    font-size: 20px;
    font-weight: bold;
  }
</style>
