<template>
  <div>
    <div class="top">

      <el-button type="primary" size="mini" @click="add()">新增</el-button>

    </div>
    <el-container>
      <el-header height="40px">
        <el-form size="mini" :model="search" inline>
          <el-form-item label="回访编号：">
            <el-input v-model="search.vistId"></el-input>
          </el-form-item>
          <el-form-item>
            <el-button type="primary" size="mini" @click="initDate(search.vistId)">确定</el-button>
          </el-form-item>
        </el-form>
      </el-header>
      <el-main>
        <el-table :data="tableDate.slice((currentPage-1)*pagesize,currentPage*pagesize)" style="width: 100%">
          <el-table-column label="操作" width="200px">
            <template #default="scope">
              <router-link :to="{path: '/UpdateRecords',query:{key:scope.row.recordId,value:JSON.stringify(scope.row)}}">
                <el-button type="text" size="mini" >修改</el-button>
              </router-link>
            </template>
          </el-table-column>
          <el-table-column prop="vistId" label="编号" width="120"/>
          <!--      <el-table-column prop="ggBrand.brandName" label="编号" width="80"/>-->
          <el-table-column prop="clAssess.ggBrand.brandName" label="汽车品牌"/>
          <el-table-column prop="clAssess.ggDesign.designName" label="汽车款式" width="200px"/>
          <el-table-column prop="clAssess.ggColor.colorName" label="车身颜色"/>
          <el-table-column prop="clAssess.assessLeaveTime" label="出厂时间" width="120px"/>
          <el-table-column prop="clAssess.clOrders.clRecord.recordPrice" label="预售价"/>
          <el-table-column prop="clAssess.assessBegain" label="初登时间" width="120px"/>
          <el-table-column prop="clAssess.assessCard" label="车牌号"/>
          <el-table-column prop="clAssess.assessMileage" label="行驶里程"/>
          <el-table-column prop="clAssess.clOrders.clRecord.recordName" label="车主姓名"/>
          <el-table-column prop="clAssess.ggColor.colorName" label="车身颜色"/>
          <el-table-column prop="clAssess.assessType" label="燃油种类"/>
          <el-table-column prop="clAssess.assessDrive" label="是否四轮驱动" width="100px"/>
          <el-table-column prop="clAssess.assessEngine" label="发动机"/>
          <el-table-column prop="clAssess.assessTap" label="档位形式"/>
          <el-table-column prop="clAssess.assessTransferTimes" label="过户次数"/>
          <el-table-column prop="clAssess.clOrders.clRecord.contactName" label="联系人"/>
          <el-table-column prop="clAssess.clOrders.clRecord.contactPhone" label="手机号码" width="120px"/>
          <el-table-column prop="clAssess.clOrders.clRecord.contactEmail" label="邮箱" width="170px"/>
          <el-table-column prop="clAssess.clOrders.clRecord.recordTime" label="咨询时间" width="120px"/>
          <el-table-column prop="clAssess.clOrders.clRecord.recordWay" label="咨询方式" />
          <el-table-column prop="clAssess.clOrders.ordersTime" label="评估日期" width="120"/>
          <el-table-column prop="clAssess.clOrders.ordersMethdo" label="评估方式"/>
          <el-table-column prop="clAssess.assessAppraise" label="总体评价"/>
          <el-table-column prop="clAssess.assessNewcarPrice" label="新车市场价"/>
          <el-table-column prop="clAssess.assessOffice" label="评估人报价"/>
          <el-table-column prop="clAssess.assessPrice" label="定价中心"/>
          <el-table-column prop="vistPrice" label="最终定价"/>
          <el-table-column prop="clAssess.assessUser" label="主评人"/>
<!--          <el-table-column prop="clAssess.assessReview" label="副评"/>-->
          <el-table-column prop="vistState" label="车辆状态">
            <template #default="scope">
              <template v-if="scope.row.vistState=='0'">
                签约中
              </template>
              <template v-if="scope.row.vistState=='1'">
                已签约
              </template>
              <template v-if="scope.row.vistState=='2'">
                签约失败
              </template>
            </template>
          </el-table-column>
          <el-table-column prop="vistTime" label="回访时间" width="120px"/>
          <el-table-column prop="acId" label="归档类型"/>
        </el-table>

        <!--分页-->
        <div class="fy_div">
          <el-pagination
              @size-change="handleSizeChange"
              @current-change="handleCurrentChange"
              :current-page="currentPage"
              :page-sizes="[5, 10, 20, 40]"
              :page-size="pagesize"
              layout="total, sizes, prev, pager, next, jumper"
              :total="tableDate.length">
          </el-pagination>
        </div>
      </el-main>
    </el-container>
    <el-dialog v-model="dialogVisible" title="详情">
      <el-descriptions
          class="margin-top"
          :column="2"
          :size="mini"
          border >
        <el-descriptions-item label="姓名：">
          张三
        </el-descriptions-item>
        <el-descriptions-item label="金额">
          18100000000
        </el-descriptions-item>
        <el-descriptions-item label="年龄">
          Suzhou
        </el-descriptions-item>
        <el-descriptions-item label="就读学校">
          <el-tag size="small">School</el-tag>
        </el-descriptions-item>
        <el-descriptions-item label="家庭住址">
          株洲市茶陵县MM村MM组EE户
        </el-descriptions-item>
      </el-descriptions>
    </el-dialog>

  </div>
</template>

<script>
export default {
  data() {
    return {
      //表格数据
      tableDate: [],
      dialogVisible:false,//详情弹出
      grid:{//详情实体类
      },

        search:{
        vistId: ''
        },
      currentPage:1, //初始页
      pagesize:10,    //    每页的数据


    }
  },
  methods:{
    //查询详情

    initDate(vistId){
      this.axios.get("http://localhost:8088/find-clReturnVist2",{params:{
        vistId:vistId
        }})
          .then((v) => {
            this.tableDate = v.data;
          })
    },


    //跳转新增
    add(){
      this.$router.push({
        path: '/AddReturnVist'
      })
    },

    // 初始页currentPage、初始每页数据数pagesize和数据data
    handleSizeChange: function (size) {
      this.pagesize = size;
      console.log(this.pagesize)  //每页下拉显示数据
    },
    handleCurrentChange: function(currentPage){
      this.currentPage = currentPage;
      console.log(this.currentPage)  //点击第几页
    },
  },
  created() {
    this.initDate();
  }


}
</script>

<style scoped>
/deep/.el-descriptions__label.is-bordered-label {
  font-weight: 700;
  color: #909399;
  background: #ffffff;
}
.el-pagination {
  white-space: nowrap;
  padding: 2px 5px;
  margin-top: 10px;
  color: #303133;
  font-weight: 700;
  text-align: center;
}

.top {
  background-color: #fff;
  line-height: 35px;
  height: 35px;
  padding: 0 20px;
}

.el.header {
  margin: 5px 0px;

}

.el-main {
  background-color: #fff;
  padding: 10px;
  min-height: 400px;
}

.el-form-item--mini.el-form-item {
  margin-top: 5px;
}


</style>
