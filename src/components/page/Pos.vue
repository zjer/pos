<template>
  <div class="pos">
    <el-row>
      <el-col :span="8" class="pos-order" id="orderList">
        <el-tabs>
          <el-tab-pane label="点餐">
            <el-table :data="tableData" border style="width: 100%;">
              <el-table-column prop="goodsName" label="商品名称"></el-table-column>
              <el-table-column prop="count" label="商品数量"></el-table-column>
              <el-table-column prop="price" label="商品单价"></el-table-column>
              <el-table-column label="操作" fixed="right">
                <template slot-scope="scope">
                  <el-button type="text" size="small" @click="delSingleGoods(scope.row)">删除</el-button>
                  <el-button type="text" size="small" @click="addOrderList(scope.row)">增加</el-button>
                </template>
              </el-table-column>
            </el-table>
            <div class="total">
              <el-row>
                <el-col :span="12">数量：<span>{{ totalCount }}</span></el-col>
                <el-col :span="12">金额：<span>{{ totalMoney }}</span>元</el-col>
              </el-row>
            </div>
            <div class="btns">
              <el-button type="warning">挂单</el-button>
              <el-button type="danger" @click="delAllGoods()">删除</el-button>
              <el-button type="primary" @click="checkout()">结账</el-button>
            </div>
          </el-tab-pane>
          <el-tab-pane label="挂单">挂单</el-tab-pane>
          <el-tab-pane label="外卖">外卖</el-tab-pane>
        </el-tabs>
      </el-col>
      <el-col :span="16">
        <div class="offenGoods">
          <div class="offenTitle">常用商品</div>
          <div class="offenGoodsList">
            <ul>
              <li v-for="goods in offenGoods" @click="addOrderList(goods)">
                <span>{{ goods.goodsName }}</span>
                <span class="oPrice">￥{{ goods.price }}</span>
              </li>
            </ul>
          </div>
        </div>
        <div class="goodsType">
          <el-tabs>
            <el-tab-pane label="汉堡">
              <div>
                <ul class="cookList">
                  <li v-for="goods in type0Goods" @click="addOrderList(goods)">
                    <span class="foodImg"><img :src="goods.goodsImg" alt="汉堡"></span>
                    <span class="foodName">{{ goods.goodsName }}</span>
                    <span class="foodPrice">￥{{ goods.price }}</span>
                  </li>
                </ul>
              </div>
            </el-tab-pane>
            <el-tab-pane label="小食">
              <div>
                <ul class="cookList">
                  <li v-for="goods in type1Goods" @click="addOrderList(goods)">
                    <span class="foodImg"><img :src="goods.goodsImg" alt="小食"></span>
                    <span class="foodName">{{ goods.goodsName }}</span>
                    <span class="foodPrice">￥{{ goods.price }}</span>
                  </li>
                </ul>
              </div>
            </el-tab-pane>
            <el-tab-pane label="饮料">
              <div>
                <ul class="cookList">
                  <li v-for="goods in type2Goods" @click="addOrderList(goods)">
                    <span class="foodImg"><img :src="goods.goodsImg" alt="饮料"></span>
                    <span class="foodName">{{ goods.goodsName }}</span>
                    <span class="foodPrice">￥{{ goods.price }}</span>
                  </li>
                </ul>
              </div>
            </el-tab-pane>
            <el-tab-pane label="套餐">
              <div>
                <ul class="cookList">
                  <li v-for="goods in type3Goods" @click="addOrderList(goods)">
                    <span class="foodImg"><img :src="goods.goodsImg" alt="套餐"></span>
                    <span class="foodName">{{ goods.goodsName }}</span>
                    <span class="foodPrice">￥{{ goods.price }}</span>
                  </li>
                </ul>
              </div>
            </el-tab-pane>
          </el-tabs>
        </div>
      </el-col>
    </el-row>
  </div>
</template>

<script>
  import axios from 'axios';
  import ElTabPane from "element-ui/packages/tabs/src/tab-pane";
  import ElButton from "element-ui/packages/button/src/button";
  import ElTabs from "element-ui/packages/tabs/src/tabs";

  export default {
    components: {
      ElTabs,
      ElButton,
      ElTabPane
    },
    name: 'Pos',
    data() {
      return {
        tableData: [],
        offenGoods: [],
        type0Goods: [],
        type1Goods: [],
        type2Goods: [],
        type3Goods: [],
        totalMoney: 0,
        totalCount: 0
      }
    },
    created: function () {
      axios.get('http://jspang.com/DemoApi/oftenGoods.php')
      .then(
        response => {
          console.log(response);
          this.offenGoods = response.data;
        }
      )
      .catch(
        error => {
          console.log(error);
          alert('网络错误，不能访问');
        }
      ),
      axios.get('http://jspang.com/DemoApi/typeGoods.php')
      .then(
        response => {
          console.log(response);
          this.type0Goods = response.data[0];
          this.type1Goods = response.data[1];
          this.type2Goods = response.data[2];
          this.type3Goods = response.data[3];
        }
      )
      .catch(
        error => {
          console.log(error);
          alert('网络错误，不能访问');
        }
      )
    },
    mounted: function () {
     var orderHeight = document.body.clientHeight;
     console.log(orderHeight);
     document.getElementById('orderList').style.height = orderHeight + 'px';
    },
    methods: {
      //添加订单列表的方法
      addOrderList(goods) {
        let isHave = false;
        //判断是否这个商品已经存在于订单列表
        for (let i = 0; i < this.tableData.length; i ++) {
          if (this.tableData[i].goodsId == goods.goodsId) {
            isHave = true;//存在
          }
        }
        //根据isHave的值判断订单列表中是否已经有此商品
        if (isHave) {
          let arr = this.tableData.filter(o => o.goodsId == goods.goodsId);
          arr[0].count ++;
        } else {
          //不存在就推入数组
          let newGoods = {goodsId: goods.goodsId, goodsName: goods.goodsName, price: goods.price, count: 1};
          this.tableData.push(newGoods);
        }
        //计算数量和金额
        this.getAll();
      },
      //删除单个商品
      delSingleGoods(goods) {
        this.tableData = this.tableData.filter(o => o.goodsId != goods.goodsId);
        //计算数量和金额
        this.getAll();
      },
      //删除所有商品
      delAllGoods() {
        this.tableData = [];
        this.totalMoney = 0;
        this.totalCount = 0;
      },
      //结账
      checkout() {
        if (this.totalCount != 0) {
          this.tableData = [];
          this.totalMoney = 0;
          this.totalCount = 0;
          this.$message({
            message: '很棒，完成一单交易！',
            type: 'success'
          })
        } else {
          this.$message({
            message: '没有商品，请勿点击结账！',
            type: 'warning'
          })
        }
      },
      //汇总数量和金额
      getAll() {
        this.totalMoney = 0;
        this.totalCount = 0;
        if (this.tableData) {
          this.tableData.forEach((element) => {
            this.totalCount += element.count;
            this.totalMoney = this.totalMoney + (element.price * element.count);
          })
        }
      }
    }
  }
</script>

<style scoped>
  .pos-order {
    background-color: #f9fafc;
    border-right: 1px solid #c0ccda;
  }
  .btns {
    margin-top: 15px;
  }
  .offenTitle {
    height: 20px;
    padding: 10px;
    background-color: #f9fafc;
    border-bottom: 1px solid #d3dce6;
    text-align: left;
  }
  .offenGoodsList ul li {
    list-style: none;
    padding: 10px;
    margin: 10px;
    background-color: #ffffff;
    border: 1px solid #e5e9f2;
    float: left;
    cursor: pointer;
  }
  .oPrice {
    color: #58B7FF;
  }
  .goodsType {
    clear: both;
  }
  .cookList li {
    list-style: none;
    width: 25%;
    height: auto;
    padding: 3px;
    margin: 5px;
    border: 1px solid #e5e9f2;
    background-color: #ffffff;
    overflow: hidden;
    float: left;
    cursor: pointer;
  }
  .cookList li span {
    display: block;
    float: left;
  }
  .foodImg {
    width: 40%;
  }
  .foodImg img {
    width: 100%;
    display: block;
  }
  .foodName {
    font-size: 16px;
    padding-left: 10px;
    color: brown;
  }
  .foodPrice {
    font-size: 16px;
    padding: 10px 0 0 10px;
  }
  .total {
    padding: 10px;
    background-color: #ffffff;
    border-bottom: 1px solid #d3dce6;
  }
</style>
