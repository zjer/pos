<template>
  <div class="pos">
    <el-row>
      <el-col :span="8" class="pos-order" id="orderList">
        <el-tabs>
          <el-tab-pane label="点餐">
            <el-table :data="tableData" border style="width: 100%;">
              <el-table-column prop="goodsName" label="商品名称"></el-table-column>
              <el-table-column prop="goodsCount" label="商品数量"></el-table-column>
              <el-table-column prop="goodsPrice" label="商品单价"></el-table-column>
              <el-table-column label="操作" fixed="right">
                <template slot-scope="scope">
                  <el-button type="text" size="small">删除</el-button>
                  <el-button type="text" size="small">增加</el-button>
                </template>
              </el-table-column>
            </el-table>
            <div class="btns">
              <el-button type="warning">挂单</el-button>
              <el-button type="danger">删除</el-button>
              <el-button type="primary">结账</el-button>
            </div>
          </el-tab-pane>
          <el-tab-pane label="挂单">挂单</el-tab-pane>
          <el-tab-pane label="外卖">外卖</el-tab-pane>
        </el-tabs>
      </el-col>
      <el-col :span="16">
        我是产品栏
      </el-col>
    </el-row>
  </div>
</template>

<script>
  import ElTabPane from "element-ui/packages/tabs/src/tab-pane";
  import ElButton from "element-ui/packages/button/src/button";

  export default {
    components: {
      ElButton,
      ElTabPane
    },
    name: 'Pos',
    data() {
      return {
        tableData: [{
          goodsName: '可口可乐',
          goodsPrice: 8,
          goodsCount:1
        }, {
          goodsName: '香辣鸡腿堡',
          goodsPrice: 15,
          goodsCount:1
        }, {
          goodsName: '爱心薯条',
          goodsPrice: 8,
          goodsCount:1
        }, {
          goodsName: '甜筒',
          goodsPrice: 8,
          goodsCount:1
        }]
      }
    },
    mounted: function () {
     var orderHeight = document.body.clientHeight;
     console.log(orderHeight);
     document.getElementById('orderList').style.height = orderHeight + 'px';
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
</style>
