<template>
    <div>
      <div v-for="(item,inex) in data" :key="item">
      <div class="img" >
        <img :src="item.albums" alt="">
      </div>
      <div class="title">
        {{item.title}}
      </div>
      <div class="material">
        <h2 class="stuff">用料</h2>
        <ul class="list">
          <li class="lists" v-for="(ingr,index) in ingredients" :key="ingr">{{ingr}}</li>
          <li class="lists" v-for="(burd,idx) in burden" :key="burd">{{burd}}</li>
        </ul>
      </div>
      <div class="step_con">
        <h3 class="step_title">步骤</h3>
        <ul class="steps">
          <li class="step" v-for="(step,idex) in item.steps" :key="step">
            <h2>{{step.step}}</h2>
            <img :src="step.img" alt="">
          </li>
        </ul>
      </div>
      </div>
    </div>
</template>

<script>
    export default {
        data(){
          return{
            data:[],
            burden:[],
            ingredients:[]
          }
        },
      onReady(){
        let that=this;
        let pages = getCurrentPages() ;
        let currentPage = pages[pages.length-1];
        let url=`https://apis.juhe.cn/cook/queryid?key=8222e0cd93648c607f844d1d7adc86d8&id=${currentPage.options.id}`;
        wx.request({
          url:url,
          success:function (res) {
            if (res.data.error_code==0){
              that.data = res.data.result.data;
              let ingredients=that.data[0].ingredients.split(';');
              let material=that.data[0].burden.split(';');
              for (let i=0;i<=material.length-1;i++) {
                that.burden=material[i].split(',').concat(that.burden);
              }
              for (let j=0;j<=ingredients.length-1;j++){
                that.ingredients=ingredients[j].split(',').concat(that.ingredients);
              }
              that.loadingHidden=false
            }else {
              return
            }
          }
        });
      },
      created(){

      }
    }
</script>

<style scoped>
  .img{
    width: 100%;
  }
  .img img{
    width: 100%;
  }
  .title{
    text-align: center;
    color: #ff8244;
    font-weight: 600;
    font-size: 34rpx;
  }
  .material{
    overflow: hidden;
  }
  .stuff{
    margin-left: 4%;
    height: 60rpx;
    line-height: 60rpx;
    padding:20rpx 0;
    border-bottom: 1px solid #eee;
    font-size: 34rpx;
    color: #ff8244;
  }
  .list{
    margin-left: 4%;
  }
  .lists{
    width: 46%;
    float: left;
    height: 60rpx;
    line-height: 60rpx;
    font-size: 30rpx;
    padding:10rpx 0;
    border-bottom: 1px solid #e7e7e7;
  }
  .step_con{
    clear: both;
    margin: 4% 4% 0 4%;
  }
  .step_title{
    font-size: 34rpx;
    margin-bottom: 4%;
    color: #ff8244;
  }
  .step{
    padding:0 0 4%  0;
  }
  .step  h2{
    font-size: 30rpx;
    padding-bottom:2%;
  }

  .step  img{
    width: 100%;
  }

</style>
