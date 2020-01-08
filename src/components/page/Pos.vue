<template>
  <div class="pos">
    <el-row>
        <el-col :span='7' class="pos-order" id="order-list">
          <el-tabs>
            <el-tab-pane label="点餐">
              <el-table :data="tableData">
                <el-table-column prop="goodsName" label="商品名称"></el-table-column>
                <el-table-column prop="count" label="数量" width="50"></el-table-column>
                <el-table-column prop="price" label="金额" width="70"></el-table-column>
                <el-table-column label="操作" width="100" fixed="right">
                  <template slot-scope="scope">
                    <el-button type="text" size="small" @click="delSingleGoods(scope.row)">删除</el-button>
                    <el-button type="text" size="small" @click="addOrderList(scope.row)">增加</el-button>
                  </template>
                </el-table-column>
              </el-table>
              <div class="totalDiv">
                <small>数量:</small>{{totalCount}}个 &nbsp;&nbsp;&nbsp;&nbsp;<small>金额：</small>{{totalMoney}}元
              </div>
              <div class="div-btn">
                <el-button type="warning">挂单</el-button>
                <el-button type="danger" @click="delAllGoods()">删除</el-button>
                <el-button type="success" @click="checkout()">结账</el-button>
              </div>
            </el-tab-pane>
            <el-tab-pane label="挂单">
              挂单
            </el-tab-pane>
            <el-tab-pane label="外卖">
              外卖
            </el-tab-pane>
          </el-tabs>
        </el-col>
        <el-col :span='17'>
          <div class="often-goods">
            <div class="title">商用商品</div>
            <div class="often-goods-list">
              <ul>
                <li v-for='goods in oftenGoods' v-bind:key="goods.id" @click="addOrderList(goods)">
                  <span>{{goods.goodsName}}</span>
                   <span class="o-price">￥{{goods.price}}元</span>
                </li>
              </ul>
            </div>
          </div>
          <div class="goods-type">
            <el-tabs>
                <el-tab-pane label="汉堡">
                  <div>
                    <ul class='cookList'>
                      <li v-for="goods in type0Goods" :key="goods.index" @click="addOrderList(goods)">
                          <span class="foodImg"><img src="goods.goodsImg" width="100%"></span>
                          <span class="foodName">{{goods.goodsName}}</span>
                          <span class="foodPrice">￥{{goods.price}}元</span>
                      </li>
                  </ul>
                  </div>


                </el-tab-pane>
                <el-tab-pane label="小食">
                  小食
                </el-tab-pane>
                <el-tab-pane label="饮料">
                  饮料
                </el-tab-pane>
                <el-tab-pane label="套餐">
                  套餐
                </el-tab-pane>
            </el-tabs>

          </div>
        </el-col>
    </el-row>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: 'pos',
  data:function(){
    return {
      tableData:[],
      oftenGoods:[
          {
              goodsId:1,
              goodsName:'香辣鸡腿堡',
              price:18
          }, {
              goodsId:2,
              goodsName:'田园鸡腿堡',
              price:15
          }, {
              goodsId:3,
              goodsName:'和风汉堡',
              price:15
          }, {
              goodsId:4,
              goodsName:'快乐全家桶',
              price:80
          }, {
              goodsId:5,
              goodsName:'脆皮炸鸡腿',
              price:10
          }, {
              goodsId:6,
              goodsName:'魔法鸡块',
              price:20
          }, {
              goodsId:7,
              goodsName:'可乐大杯',
              price:10
          }, {
              goodsId:8,
              goodsName:'雪顶咖啡',
              price:18
          }, {
              goodsId:9,
              goodsName:'大块鸡米花',
              price:15
          }, {
              goodsId:20,
              goodsName:'香脆鸡柳',
              price:17
          }, {
              goodsId:20,
              goodsName:'香脆鸡柳',
              price:17
          }

      ],
      type0Goods:[
          {
              goodsId:1,
              goodsImg:"http://7xjyw1.com1.z0.glb.clouddn.com/pos001.jpg",
              goodsName:'香辣鸡腿堡',
              price:18
          }, {
              goodsId:2,
              goodsImg:"http://7xjyw1.com1.z0.glb.clouddn.com/pos002.jpg",
              goodsName:'田园鸡腿堡',
              price:15
          }, {
              goodsId:3,
              goodsImg:"http://7xjyw1.com1.z0.glb.clouddn.com/pos004.jpg",
              goodsName:'和风汉堡',
              price:15
          }, {
              goodsId:4,
               goodsImg:"http://7xjyw1.com1.z0.glb.clouddn.com/pos003.jpg",
              goodsName:'快乐全家桶',
              price:80
          }, {
              goodsId:5,
               goodsImg:"http://7xjyw1.com1.z0.glb.clouddn.com/pos003.jpg",
              goodsName:'脆皮炸鸡腿',
              price:10
          }, {
              goodsId:6,
               goodsImg:"http://7xjyw1.com1.z0.glb.clouddn.com/pos004.jpg",
              goodsName:'魔法鸡块',
              price:20
          }, {
              goodsId:7,
               goodsImg:"http://7xjyw1.com1.z0.glb.clouddn.com/pos001.jpg",
              goodsName:'可乐大杯',
              price:10
          }, {
              goodsId:8,
               goodsImg:"http://7xjyw1.com1.z0.glb.clouddn.com/pos003.jpg",
              goodsName:'雪顶咖啡',
              price:18
          }, {
              goodsId:9,
               goodsImg:"http://7xjyw1.com1.z0.glb.clouddn.com/pos002.jpg",
              goodsName:'大块鸡米花',
              price:15
          }, {
              goodsId:20,
               goodsImg:"http://7xjyw1.com1.z0.glb.clouddn.com/pos002.jpg",
              goodsName:'香脆鸡柳',
              price:17
          }

      ],
      totalMoney:0,
      totalCount:0
    }
  },
  //   created:function(){
  //   axios.get('https://www.easy-mock.com/mock/5b8b30dbf032f03c5e71de7f/kuaican/oftenGoods')
  //   .then(reponse=>{
  //     console.log(111);
  //     console.log(reponse);
      
  //   })
  //   .catch(error=>{
  //     console.log('error');
  //   })
  // },
  mounted:function(){
    var orderHeight=document.body.clientHeight;
    document.getElementById('order-list').style.height=orderHeight+'px';
  },
  methods:{
    addOrderList(goods){
      this.totalMoney = 0;
      this.totalCount = 0;
      let isHave = false;
      for (let i = 0 ; i < this.tableData.length; i++){
        if (this.tableData[i].goodsId == goods.goodsId){
          isHave = true;
        }
      }

      if(isHave){
        let arr = this.tableData.filter(a=>a.goodsId == goods.goodsId);
        arr[0].count ++ ;
      }else {
        let newGoods={goodsId:goods.goodsId,goodsName:goods.goodsName,price:goods.price,count:1};
        this.tableData.push(newGoods);
      }

      this.tableData.forEach(a=>{
        this.totalCount += a.count;
        this.totalMoney =this.totalMoney + (a.price*a.count);
      })
    },
    delSingleGoods(goods){
        this.tableData = this.tableData.filter(o=>o.goodsId != goods.goodsId);
        this.getAllMoney();

    },
    delAllGoods(){
      this.tableData = [];
      this.totalCount = 0 ;
      this.totalMoney = 0 ;
    },
    checkout(){
      if(this.totalCount != 0){
        this.tableData = [];
        this.totalMoney = 0 ;
        this.totalMoney = 0 ;
        this.$message({
          message:'结账成功',
          type:'success'
        })
      }else {
        this.$message.error("没有商品");
        
      }
    },
    getAllMoney(){
      this.totalCount =0 ;
      this.totalMoney = 0 ;
      if (this.tableData){
         this.tableData.forEach(a=>{
        this.totalCount += a.count;
        this.totalMoney =this.totalMoney + (a.price*a.count);
      })
      }
    }

  }
  
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.pos-order{
  /* padding-left: 10px; */
  background-color: #f9fafc;
  border-right: 1px solid #C0CCDA;


}
.div-btn{
  margin-top: 10px;
}
.title{
  height: 20px;
  border-bottom: 1px solid #D3dce6;
  background-color: #f9fafc;
  padding: 10px;
  text-align: left;
}
.often-goods-list ul li{
  list-style: none;
  float: left;
  border: 1px solid #e5e9f2;
  padding: 10px;
  margin: 10px;
  background-color: #ffffff
}
.o-price{
  color:aqua;
}
.goods-type{
  clear: both;
}
.cookList li{
       list-style: none;
       width:23%;
       border:1px solid #E5E9F2;
       height: auot;
       overflow: hidden;
       background-color:#fff;
       padding: 2px;
       float:left;
       margin: 2px;

   }
   .cookList li span{

        display: block;
        float:left;
   }
   .foodImg{
       width: 40%;
   }
   .foodName{
       font-size: 18px;
       padding-left: 10px;
       color:brown;

   }
   .foodPrice{
       font-size: 16px;
       padding-left: 10px;
       padding-top:10px;
   }
   .totalDiv{
     background-color: #fff;
     padding: 10xp;
     border-bottom: 1px solid #D3dce6;
   }
</style>
