<template>
  <div class="pos">
    <el-row>
      <el-col :span="7" class="pos-order" id ='order-list'>
        <el-tabs type="border-card" style="height: 100%">
          <el-tab-pane label="点餐">
            <el-table :data="tableData" border  style="width: 100%" >

              <el-table-column prop="goodsName" label="商品"  ></el-table-column>
              <el-table-column prop="count" label="量" width="50"></el-table-column>
              <el-table-column prop="price" label="金额" width="70"></el-table-column>
              <el-table-column  label="操作" width="100" fixed="right">
                <template slot-scope="scope">
                  <el-button type="text" size="small" @click="deleteOrderList(scope.row)">删除</el-button>
                  <el-button type="text" size="small" @click="addOrderList(scope.row)">增加</el-button>

                </template>
              </el-table-column>
            </el-table>
              <div class="total-div">
                <small>数量：</small>{{orderCount}}  <small>金额：</small>{{orderTotalPrice}}元
              </div>

            <div class="div-btn">
              <el-button type="warning" >挂单</el-button>
              <el-button type="danger" @click="deleteAllList()">删除</el-button>
              <el-button type="success" @click="checkAllList()">结账</el-button>
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
      <el-col :span="17">
        <div class="often-goods">
          <div class="title">常用商品</div>
          <div class="often-goods-list">

            <ul>
              <li v-for="goods in oftenGoods" @click="addOrderList(goods)">
                <span>{{goods.goodsName}}</span>
                <span class="o-price">￥{{goods.price}}元</span>
              </li>

            </ul>
          </div>
        </div>

        <div class="goods-type">

          <el-tabs >
            <el-tab-pane label="汉堡" >
              <ul class='cookList'>
                <li v-for="goods in type0Goods" @click="addOrderList(goods)">
                  <span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
                  <span class="foodName">{{goods.goodsName}}</span>
                  <span class="foodPrice">￥{{goods.price}}元</span>
                </li>
              </ul>
            </el-tab-pane>
            <el-tab-pane label="小食">
              <ul class='cookList'>
                <li v-for="goods in type1Goods" @click="addOrderList(goods)">
                  <span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
                  <span class="foodName">{{goods.goodsName}}</span>
                  <span class="foodPrice">￥{{goods.price}}元</span>
                </li>
              </ul>
            </el-tab-pane>
            <el-tab-pane label="饮料">
              <ul class='cookList'>
                <li v-for="goods in type2Goods" @click="addOrderList(goods)">
                  <span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
                  <span class="foodName">{{goods.goodsName}}</span>
                  <span class="foodPrice">￥{{goods.price}}元</span>
                </li>
              </ul>
            </el-tab-pane>
            <el-tab-pane label="套餐">
              <ul class='cookList'>
                <li v-for="goods in type3Goods" @click="addOrderList(goods)">
                  <span class="foodImg"><img :src="goods.goodsImg" width="100%"></span>
                  <span class="foodName">{{goods.goodsName}}</span>
                  <span class="foodPrice">￥{{goods.price}}元</span>
                </li>
              </ul>
            </el-tab-pane>

          </el-tabs>
        </div>

      </el-col>

    </el-row>

  </div>
</template>

<script>
  import axios from 'axios'
  export default {
    name: 'pos',
    data () {
      return {
        tableData: [],
        oftenGoods:[],
        type0Goods:[],
        type1Goods:[],
        type2Goods:[],
        type3Goods:[],
        orderCount:0,
        orderTotalPrice:0,
      }
    },
    created(){
      axios.get('http://jspang.com/DemoApi/oftenGoods.php')
        .then(data=>{
          this.oftenGoods=data.data;
        })
        .catch(error=>{
          alert('网络错误，不能访问');
        }),
        axios.get('http://jspang.com/DemoApi/typeGoods.php').then(response=>{
        this.type0Goods = response.data[0];
        this.type1Goods = response.data[1];
        this.type2Goods = response.data[2];
        this.type3Goods = response.data[3];
      }).catch(error=>{
        alert('网络错误');
      })
    },
    mounted: function () {
      var orderHeight = document.body.clientHeight
      document.getElementById('order-list').style.height = orderHeight + 'px'
    },
    methods:{
      addOrderList(goods){
        this.orderCount =0;
        this.orderTotalPrice = 0;
        let havegood = this.tableData.filter((item)=>item.goodsId == goods.goodsId);
        if(havegood!==null && havegood!==undefined && havegood.length>0){
            havegood[0].count++;
        }else{
          let newGoods={goodsId:goods.goodsId,goodsName:goods.goodsName,price:goods.price,count:1};
          this.tableData.push(newGoods);
        }
        this.tableData.forEach((item)=>{
          this.orderCount+=item.count;
          this.orderTotalPrice= this.orderTotalPrice +item.count*item.price;
        });
      },
      deleteOrderList(goods){
        this.orderCount =0;
        this.orderTotalPrice = 0
        let havegood = this.tableData.filter((item)=>item.goodsId == goods.goodsId);
        if(havegood!==null && havegood!==undefined && havegood.length>0){
          if(havegood[0].count>1){
            havegood[0].count--;
          }else{
            Array.prototype.removeByValue = function(val) {
              for(let i=0; i<this.length; i++) {
                if(this[i] == val) {
                  this.splice(i, 1);
                  break;
                }
              }
            }
            this.tableData.removeByValue(havegood[0]);

          }
        }else{
          alert('点击错误!!');
        }
        this.tableData.forEach((item)=>{
          this.orderCount+=item.count;
          this.orderTotalPrice= this.orderTotalPrice +item.count*item.price;
        });
      },
      deleteAllList(){
        this.orderCount =0;
        this.orderTotalPrice = 0
        this.tableData = [];
      },
      checkAllList(){
        if(this.orderCount!==0){
          this.orderCount =0;
          this.orderTotalPrice = 0
          this.tableData = [];
          this.$message.success('结账成功！');
        }else{
          this.$message.error('别特么点了！');
        }
      }

    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .pos-order{
    background-color: #F9FAFC;
    border-right: 1px solid #C0CCDA;
    height:100%;
  }
  .div-btn{
    margin-top: 10px;
    float: right;
    margin-right: 10px;
  }
  .goods-type {
    clear: both;
  }
  .title{
    height: 20px;
    border-bottom:1px solid #D3DCE6;
    background-color: #F9FAFC;
    padding:10px;
  }
  .often-goods-list ul li{
    list-style: none;
    float:left;
    border:1px solid #E5E9F2;
    padding:10px;
    margin:5px;
    background-color:#fff;
  }
  .o-price{
    color:#58B7FF;
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
  .total-div{
    height: auto;
    overflow: hidden;
    text-align: right;
    font-size: 16px;
    background-color: #fff;
    border-bottom: 1px solid #E5E9F2;
    padding: 10px;
  }

</style>
