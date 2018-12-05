<template>
    <div>
      <div class="con" v-for="(items,index) in data" :key="index" @click="Detail(items.id)">
        <div class="img"><img :src="items.albums" alt=""/></div>
        <div class="text">
          <h2 class="title">{{items.title}}</h2>
          <p>功效：{{items.tags}}</p>
          <p>材料：{{items.ingredients}}</p>
        </div>
      </div>
    </div>
</template>

<script>
    export default {
        data(){
          return{
            data:[]
          }
        },
      onLoad(){
        let that=this;
        let pages = getCurrentPages() ;
        let currentPage = pages[pages.length-1];
        wx.setNavigationBarTitle({
          title:'食谱列表'
        });
        let url=`https://apis.juhe.cn/cook/index?key=8222e0cd93648c607f844d1d7adc86d8&rn=30&cid=${currentPage.options.id}`;
        wx.request({
          url:url,
          success:function (res) {
            that.data=res.data.result.data;
            that.loadingHidden=false;
          }
        })
      },
      methods:{
        Detail(id){
          let url=`../detail/main?id=${id}`;
          wx.navigateTo({url});
        }
      },
      created(){

      }
    }
</script>

<style scoped>
.con{
  padding: 4% 4% 4% 4%;
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
