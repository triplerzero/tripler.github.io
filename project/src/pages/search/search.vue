<template>
    <div>
      <div class="search">
        <div>
          <input type="text" placeholder="请输入您要搜索的菜谱" class="search_Inp" @change="handleInputChange"/>
          <span class="search_Btn" @click="handleSearch">搜索</span>
        </div>
      </div>
      <div class="search_con">
      <div class="con" v-for="(items,index) in data" :key="items" @click="Detail(items.id)">
        <div class="img"><img :src="items.albums" alt=""/></div>
        <div class="text">
          <h2 class="title">{{items.title}}</h2>
          <p>功效：{{items.tags}}</p>
          <p>材料：{{items.ingredients}}</p>
        </div>
      </div>
      </div>
    </div>
</template>

<script>
  export default {
    data(){
      return{
        data:[],
        value:""
      }
    },
    methods:{
      Detail(id){
        let url=`../detail/main?id=${id}`;
        wx.navigateTo({url});
      },
      handleInputChange(e){
        this.value=e.mp.detail.value;
      },
      handleSearch(){
        this.data=[];
        this.getData();
      },
      getData(){
        let that =this;
        let url=`https://apis.juhe.cn/cook/query?key=8222e0cd93648c607f844d1d7adc86d8&menu=${that.value}&rn=30&pn=3`;
        wx.request({
          url:url,
          success:function (res) {
            that.data=res.data.result.data;
            that.loadingHidden=false;
          }
        })
      }
    },
    created(){

    }
  }
</script>

<style scoped>
  .search{
    position: fixed;
    left: 0;
    top: 0;
    background: #ff8244;
    overflow: hidden;
    width: 100%;
    padding: 6rpx 0;
  }
  .search div{
    width: 80%;
    margin: 0 auto;
    overflow: hidden;
  }
  .search_Inp{
    border: 1rpx solid #ff8244;
    width: 80%;
    float: left;
    border-radius:20rpx;
    font-size: 24rpx;
    padding-left: 0.3rem;
    background: #fff;
  }
  .search_Btn{
    display: block;
    float: left;
    height: 0.56rem;
    font-size: 28rpx;
    color: #fff;
    line-height: 0.56rem;
    width: 14%;
    text-align: center;
  }
  .search_con{
    margin-top: 70rpx;
    border-top: 1px solid #eee;
  }
  .con{
    padding: 4% 0;
    border-bottom: 1px solid #eee;
    overflow: hidden;
  }
  .img{
    width: 40%;
    max-height: 200rpx;
    float:left;
  }
  .img img{
    width: 100%;
    max-height: 200rpx;
  }
  .text{
    width: 50%;
    margin-left: 4%;
    float: left;
  }
  .text h2{
    color: #ff8244;
    font-weight: 700;
    font-size: 32rpx;
    margin-bottom: 8%;
  }
  .text p{
    margin-bottom: 8%;
    font-weight: 600;
    font-size:26rpx ;
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
  }
</style>
