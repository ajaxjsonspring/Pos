<template>
  <div class="left-menu">
    <!--<ul>-->
      <!--<li>-->
        <!--<i class="icon iconfont icon-31wode"></i>-->
      <!--</li>-->
      <!--<li>-->
        <!--<i class="icon iconfont icon-31huiyuanqia"></i>-->
      <!--</li>-->
      <!--<li>-->
        <!--<i class="icon iconfont icon-31wode"></i>-->
      <!--</li>-->
      <!--<li>-->
        <!--<i class="icon iconfont icon-gongnengjianyi"></i>-->
      <!--</li>-->
      <!--<li>-->
        <!--<i class="icon iconfont icon-hanbao"></i>-->
      <!--</li>-->
    <!--</ul>-->
    <ul>
    <li>
      <i class="icon iconfont icon-goumai"></i>
      <div>收银</div>
    </li>

    <li>
      <i class="icon iconfont icon-dianpu"></i>
      <div>店铺</div>
    </li>

    <li>
      <i class="icon iconfont icon-hanbao"></i>
      <div>商品</div>
    </li>

    <li>
      <i class="icon iconfont icon-huiyuanqia"></i>
      <div>会员</div>
    </li>


    <li>
      <i class="icon iconfont icon-gongnengjianyi"></i>
      <div>设置</div>
    </li>
    </ul>
  </div>
</template>

<script>
  export default {
    name: 'leftmenu'
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style >
  .left-menu{
    color:#fff;
    font-size:10px;
    height:100%;
    background-color: #1D8ce0;
    float:left;
    width:5%;
  }
  .iconfont{
    font-size:24px;
  }
  .left-menu ul{
    margin: 0px;
    padding:0px;
  }
  .left-menu ul li{
    list-style: none;
    text-align: center;
    border-bottom:1px solid #20a0ff;
    padding:10px;
  }
</style>
