<template>
  <div class="content">
    <div class="type" v-for="(types,index) in type" :key="index" :index="index">
      <div class="title">{{types.name}}</div>
      <div class="image" v-for="(items,idx) in types.list" :key="idx" @click="handleType(items.id)">
        <div>
        <img :src="img[items.id-1]" alt="">
        <p class="name">{{items.name}}</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  export default {
    data(){
      return{
        type:[],
        img:[]
      }
    },
    methods:{
      handleType(id){
        let url=`../list/main?id=${id}`;
        wx.navigateTo({url});
      }
     },
    onload(){

    },
    created(){
      let url='https://apis.juhe.cn/cook/category?key=8222e0cd93648c607f844d1d7adc86d8';
      let that=this;
      wx.request({
        url:url,
        success:function (res) {
          that.type=res.data.result;
          that.loadingHidden=false;
        }
      })
      wx.request({
        url:"https://apis.juhe.cn/cook/index?key=8222e0cd93648c607f844d1d7adc86d8&rn=30&cid=1",
        success:function (res) {
          for (let i=0;i<=res.data.result.data.length-1;i++){
            that.img=that.img.concat(res.data.result.data[i].albums);
          }
          for(let i=0;i<=3;i++){
            that.img=that.img.concat(that.img)
          }
        }
      })
    }
  }
</script>

<style scoped>
.type{
  margin-top: 2%;
  clear: both;
}
.title{
  color: #333;
  font-size: 30rpx;
  font-weight: 600;
  margin-bottom: 2%;
  margin-left: 4%;
  color:#ff8244;
  }
.image{
  width: 20%;
  position: relative;
  margin-left: 4%;
  float: left;
  margin-bottom: 2%;
  }
.image img{
  width: 100%;
  max-height: 150rpx;
  opacity: 0.8;
}
.name{
  width: 100%;
  text-align: center;
    position: absolute;
    bottom: 10%;
    font-size:26rpx ;
    color: #feffa8;
    font-weight: 600;
  }
</style>
