<template>
    <div class="left-nav">
      <ul>
        <li>
          <i class="icon iconfont icon-pay"></i>
          <div>收营</div>
        </li>
        <li>
          <i class="icon iconfont icon-shop"></i>
          <div>店铺</div>
        </li>
        <li>
          <i class="icon iconfont icon-goods"></i>
          <div>商品</div>
        </li>
        <li>
          <i class="icon iconfont icon-vip"></i>
          <div>会员</div>
        </li>
        <li>
          <i class="icon iconfont icon-statistics"></i>
          <div>统计</div>
        </li>
        <li>
          <i class="icon iconfont icon-settings"></i>
          <div>设置</div>
        </li>
      </ul>
    </div>
</template>

<script>
  export default {
    name: 'leftNav'
  }
</script>

<style>
  .left-nav {
    width: 5%;
    height: 100%;
    background-color: #1d8ce0;
    float: left;
    font-size: 14px;
    color: #ffffff;
  }
  .left-nav ul {
    margin: 0;
    padding: 0;
  }
  .left-nav ul li {
    list-style: none;
    text-align: center;
    padding: 5px 0;
    border-bottom: 1px solid #20a0ff;
  }
  .iconfont {
    font-size: 18px;
  }
</style>
