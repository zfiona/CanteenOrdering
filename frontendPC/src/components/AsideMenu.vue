<template>
  <el-container>
    <!-- 侧边栏菜单区域 -->
    <el-menu background-color="#333744" text-color="#fff" unique-opened router :default-active="activePath">
      <!-- 订单处理 -->
      <el-submenu index="/CanteenPCmanageOrders">
        <template slot="title">
          <i class="el-icon-tickets"></i>
          <span>订单处理</span>
        </template>
        <!-- 堂食 -->
        <el-menu-item index="/CanteenPC/manageOrders" @click="saveNavState('/CanteenPC/manageOrders')">
          <i class="el-icon-tableware"></i>
          <span slot="title">堂食处理</span>
        </el-menu-item>
        <!-- 外卖 -->
        <el-menu-item index="/CanteenPC/takeOut" @click="saveNavState('/CanteenPC/takeOut')">
          <i class="el-icon-shopping-cart-2"></i>
          <span slot="title">外卖处理</span>
        </el-menu-item>
      </el-submenu>

      <!-- 挂起订单
      <el-menu-item index="/CanteenPC/hangUp" @click="saveNavState('/CanteenPC/hangUp'), drawerVisible=false">
        <i class="el-icon-refresh-left"></i>
        <span slot="title" >
          <span type="primary">挂起订单</span>
        </span>
      </el-menu-item>
      -->

      <!-- 菜系管理 -->
      <el-menu-item index="/CanteenPC/editFood" @click="saveNavState('/CanteenPC/editFood')">
        <i class="el-icon-dish-1"></i>
        <span slot="title">菜系管理</span>
      </el-menu-item>

      <!-- 订单记录 -->
      <el-menu-item index="/CanteenPC/ordersRecord" @click="saveNavState('/CanteenPC/ordersRecord')">
        <i class="el-icon-receiving"></i>
        <span slot="title">订单记录</span>
      </el-menu-item>

      <!-- 意见处理 -->
      <el-menu-item index="/CanteenPC/opinion" @click="saveNavState('/CanteenPC/opinion')">
        <i class="el-icon-edit"></i>
        <span slot="title">意见处理</span>
      </el-menu-item>

      <!-- 数据统计 -->
      <el-submenu index="/CanteenPCdataAnalysis">
        <template slot="title">
          <i class="el-icon-pie-chart"></i>
          <span>数据统计</span>
        </template>
        <el-menu-item-group>
          <!-- 销量分析 -->
          <el-menu-item index="/CanteenPC/salesAnalysis" @click="saveNavState('/CanteenPC/salesAnalysis')">
            <i class="el-icon-data-line"></i>
            <span>销量分析</span>
          </el-menu-item>
          <!-- 菜品分析 -->
          <el-menu-item index="/CanteenPC/foodAnalysis" @click="saveNavState('/CanteenPC/foodAnalysis')">
            <i class="el-icon-data-analysis"></i>
            <span>菜品分析</span>
          </el-menu-item>
        </el-menu-item-group>
      </el-submenu>
    </el-menu>

    <!-- 抽屉 -->
    <el-drawer :open="getHangUpOrders" :visible.sync="drawerVisible" size="50%" direction="rtl">
      <el-table :data="hangUpOrders" style="width: 100%">
        <!-- 日期 -->
        <el-table-column label="日期">
          <template slot-scope="scope">
            <span>{{scope.row.time}}</span>
          </template>
        </el-table-column>
        <!-- 订单号 -->
        <el-table-column label="订单号">
          <template slot-scope="scope">
            <div slot="reference" class="name-wrapper">
              <span size="medium">{{scope.row.orderId}}</span>
            </div>
          </template>
        </el-table-column>
        <!-- 菜系 -->
        <el-table-column label="菜系">
          <template slot-scope="scope">
            <div slot="reference" class="name-wrapper">
              <span size="medium">{{scope.row.foodName1}}</span>
            </div>
          </template>
        </el-table-column>
        <!-- 类型 -->
        <el-table-column label="类型">
          <template slot-scope="scope">
            <el-button plain type="info">{{scope.row.type}}</el-button>
          </template>
        </el-table-column>
        <!-- 操作 -->
        <el-table-column label="操作">
          <template slot-scope="scope">
            <el-button type="primary" @click="handle(scope.$index, scope.row)">完成</el-button>
          </template>
        </el-table-column>
      </el-table>
    </el-drawer>
  </el-container>
</template>

<script>
  export default {
    data () {
      return {
        // 激活的路由
        activePath: '',
        // 控制抽屉的激活状态
        drawerVisible: false,
        // 已挂起的订单
        hangUpOrders: []
      }
    },
    created () {
      // 从缓存中获取已保存的链接地址
      this.activePath = window.sessionStorage.getItem('activePath')
      this.getHangUpOrders()
    },
    methods: {
      // 保存链接的激活状态
      saveNavState (activePath) {
        console.log(activePath)
        window.sessionStorage.setItem('activePath', activePath)
        this.activePath = activePath
      },
      getHangUpOrders () {
        this.$http.get("/hangUp").then(res => { this.hangUpOrders = res.data })
      }
    }
  }
</script>

<style scoped>
  .el-container {
    background-color: #333744;
  }

  .el-menu {
    border-right: none;
  }

  .el-menu-item {
    width: 200px;
  }
</style>
