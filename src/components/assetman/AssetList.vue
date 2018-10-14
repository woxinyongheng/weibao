<template>
    <div class="assetlist">
        <div class="banner">
          <div class="bannerleft">
            <el-button type="primary" @click="addAssetlist">新增</el-button>
            <el-button type="primary" @click="recipientsList(0)">领用</el-button>
            <el-button type="primary" @click="recipientsList(1)">变更领用人</el-button>
            <el-button @click="copyClick">复制</el-button>
            <el-button>模板下载</el-button>
            <el-button>导入</el-button>
            <el-button>导出</el-button>
            <el-button>标签打印</el-button>
          </div>
          <div class="bannerright">
            <el-button @click="filter = !filter">检索</el-button>
          </div>
        </div>
      <div class="filter" v-show="filter">
        <div class="table-wrapper" style="font-size: 12px;vertical-align: middle">
          <el-row>
            <el-col :span="22"><div class="grid-content bg-purple-dark">
              <el-row>
                <el-col :span="6"><div class="grid-content bg-purple-dark">
                  <el-form ref="form"  label-width="110px">
                    <el-form-item label="资产编号">
                      <el-input v-model="assetsCode"></el-input>
                    </el-form-item>
                  </el-form>
                </div></el-col>
                <el-col :span="6"><div class="grid-content bg-purple-dark">
                  <el-form ref="form"  label-width="110px">
                    <el-form-item label="使用人">
                      <el-select v-model="usePersonName" placeholder="请选择">
                        <el-option
                          v-for="item in personList"
                          :key="item.staffId"
                          :label="item.name"
                          :value="item.staffId">{{item.name}}
                        </el-option>
                      </el-select>


                    </el-form-item>
                  </el-form>
                </div></el-col>
                <el-col :span="6"><div class="grid-content bg-purple-dark">
                  <el-form ref="form"  label-width="110px">
                    <el-form-item label="管理员">
                      <el-select v-model="adminId" placeholder="请选择">
                        <el-option
                          v-for="item in adminList"
                          :key="item.id"
                          :label="item.name"
                          :value="item.id">{{item.name}}
                        </el-option>
                      </el-select>
                    </el-form-item>
                  </el-form>
                </div></el-col>
                <el-col :span="6"><div class="grid-content bg-purple-dark">
                  <el-form ref="form"  label-width="110px">
                    <el-form-item label="资产名称">
                      <el-input v-model="assetsName"></el-input>
                    </el-form-item>
                  </el-form>
                </div></el-col>
              </el-row>
              <el-row>
                <el-col :span="6"><div class="grid-content bg-purple-dark">
                  <el-form ref="form"  label-width="110px">
                    <el-form-item label="资产类别">
                      <!--<el-select v-model="assetsTypeId" placeholder="请选择">-->
                        <!--<el-option-->
                          <!--v-for="item in typeList"-->
                          <!--:key="item.id"-->
                          <!--:label="item.id"-->
                          <!--:value="item.id">{{item.classifyName}}-->
                        <!--</el-option>-->
                      <!--</el-select>-->
                      <el-select v-model="assetsTypeId" placeholder="请选择">
                        <el-option-group
                          v-for="group in typeList"
                          :key="group.classifyName"
                          :label="group.classifyName">
                          <el-option
                            v-for="item in group.options"
                            :key="item.id"
                            :label="item.classifyName"
                            :value="item.id">{{item.classifyName}}
                          </el-option>
                        </el-option-group>
                      </el-select>
                    </el-form-item>
                  </el-form>
                </div></el-col>
                <el-col :span="6"><div class="grid-content bg-purple-dark">
                  <el-form ref="form"  label-width="110px">
                    <el-form-item label="资产品牌">
                      <el-input></el-input>
                    </el-form-item>
                  </el-form>
                </div></el-col>
                <el-col :span="6"><div class="grid-content bg-purple-dark">
                  <el-form ref="form"  label-width="110px">
                    <el-form-item label="请购批次">
                      <el-input v-model="batchNumber"></el-input>
                    </el-form-item>
                  </el-form>
                </div></el-col>
                <el-col :span="6"><div class="grid-content bg-purple-dark">
                  <el-form ref="form"  label-width="110px">
                    <el-form-item label="规格型号">
                      <el-input v-model="model"></el-input>
                    </el-form-item>
                  </el-form>
                </div></el-col>
              </el-row>
              <el-row>
                <el-col :span="6"><div class="grid-content bg-purple-dark">
                  <el-form ref="form"  label-width="110px">
                    <el-form-item label="SN号">
                      <el-input v-model="snNumber"></el-input>
                    </el-form-item>
                  </el-form>
                </div></el-col>
                <el-col :span="6"><div class="grid-content bg-purple-dark">
                  <el-form ref="form"  label-width="110px">
                    <el-form-item label="使用部门">
                      <el-select  v-model="useDepartmentCode" placeholder="请选择">
                        <el-option
                          v-for="item in officeList"
                          :key="item.id"
                          :label="item.officeName"
                          :value="item.id">{{item.officeName}}
                        </el-option>
                      </el-select>
                    </el-form-item>
                  </el-form>
                </div></el-col>
                <el-col :span="12"><div class="grid-content bg-purple-dark">
                  <el-form ref="form"  label-width="110px">
                    <el-form-item label="购买日期日期">
                      <el-date-picker
                        v-model="dataarr"
                        type="daterange"
                        start-placeholder="开始日期"
                        end-placeholder="结束日期"
                        :default-time="['00:00:00', '23:59:59']">
                      </el-date-picker>
                    </el-form-item>
                  </el-form>
                </div></el-col>
              </el-row>
            </div></el-col>
            <el-col :span="2"><div class="grid-content bg-purple-dark">
              <div class="button-wrapper">
                <el-row>
                  <el-col :span="24"><div class="grid-content bg-purple-dark">
                    <el-button  type="primary" size="mini" @click="searchClick">查询</el-button>
                  </div></el-col>
                </el-row>
                <el-row style="margin-top: 10px">
                  <el-col :span="24"><div class="grid-content bg-purple-dark">
                    <el-button size="mini" @click="resetClick">重置</el-button>
                  </div></el-col>
                </el-row>
              </div>
            </div></el-col>
          </el-row>
        </div>

      </div>
        <div class="table center-table">
          <el-table
            :data="tableData"
            style="width: 100%"
            @selection-change="handleSelectionChange"
            >
            <el-table-column
              type="selection"
              width="55">
            </el-table-column>
            <el-table-column
              prop="status"
              label="状态"
              align="center"
              >
               <template slot-scope="scope">
                <el-tag
                  :type="!scope.row.useDepartmentName? 'primary' : 'success'"
                  disable-transitions>{{scope.row.useDepartmentName?'已领用':'闲置'}}</el-tag>
              </template>
            </el-table-column>
            <el-table-column
              prop="picture"
              align="center"
              label="照片"
              >
              <template slot-scope="scope">
                <img :src="scope.row.picture" alt="" class="table-img">
              </template>
            </el-table-column>
            <el-table-column
              prop="assetsCode"
              label="资产编号"
              align="center"
              >
            </el-table-column>
            <el-table-column
              prop="assetsName"
              label="资产名称"
              align="center"
              >
              <template slot-scope="scope">
                <el-button  @click="showGoodsInfo(scope.row.id)" type="text">{{scope.row.assetsName}}</el-button>
              </template>
            </el-table-column>
            <el-table-column
              prop="classifyName"
              align="center"
              label="资产类别"
              >
            </el-table-column>
            <el-table-column
              prop="brandName"
              label="资产品牌"
              align="center"
              >
            </el-table-column>
            <el-table-column
              prop="batchNumber"
              label="请购批号"
              align="center"
              >
            </el-table-column>
            <el-table-column
              prop="model"
              label="规格型号"
              align="center"
              >
            </el-table-column>
            <el-table-column
              prop="buyDate"
              label="购入时间"
              align="center"
              >
            </el-table-column>
            <el-table-column
              prop="useDepartmentName"
              align="center"
              label="使用部门"
              >
            </el-table-column>
            <el-table-column
              prop="usePersonName"
              align="center"
              label="使用人"
              >
            </el-table-column>

            <el-table-column
              prop="address"
              fixed="right"
              align="center"
              width="240"
              label="操作">
              <template slot-scope="scope">
                <el-button v-if="(!scope.row.useDepartmentName)" @click="changeClick(scope.row)" type="primary" size="mini">编辑</el-button>
                <el-button v-if="(scope.row.useDepartmentName)" type="info" size="mini" @click="returnback(scope.row)">退还</el-button>
                <el-button type="info" size="mini" @click="baofeiClick(scope.row)">报废</el-button>
              </template>
            </el-table-column>
          </el-table>
          <div class="pagination" style="text-align: right">
            <el-pagination
              background
              layout="prev, pager, next"
              @current-change="changePage"
              :total="sum">
            </el-pagination>
          </div>
          
        </div>
        <mask-shadow v-if="showGoodsDetail">
            <goods-detail :showedit="showedit" :uersdata='uersdata' :uersdatack='uersdatack' @close="closeGoodsDetail" @editClickpose="editClickpose" ></goods-detail>
        </mask-shadow>
        <mask-shadow style="z-index:99;" v-if="addShow">
            <add-assetlist :edititem="edititem" :editadd="editadd" :areaList="areaList" @close="closeGoodsDetail" :officeList="officeList" :personList="personList" :adminList="adminList" :typeList="typeList" @addSure="addSure"></add-assetlist>
        </mask-shadow>
        <mask-shadow v-if="recipientsPage">
            <recipients :type="type" @recipentSure="recipentSure" :areaList="areaList" :personList="personList"  @close="closeGoodsDetail" :lingyongList="lingyongList" :officeList="officeList"></recipients>
        </mask-shadow>
        <mask-shadow style="z-index:9" v-if="copyShow">
            <copy  @close="closeGoodsDetail" @copyList="copyList" :multipleSelection="multipleSelection"></copy>
        </mask-shadow>
        <mask-shadow v-if="changeAsset">
            <change-assetlist @close="closeGoodsDetail"></change-assetlist>
        </mask-shadow>
        <mask-shadow style="z-index:99" v-if="baofeiShow">
            <baofei @close="closeGoodsDetail" :witchone="witchone"></baofei>
        </mask-shadow>
        <mask-shadow style="z-index:99" v-if="tuihuanShow">
            <tuihuan @close="closeGoodsDetail" :witchone="witchone" :areaList="areaList"></tuihuan>
        </mask-shadow>

      
    </div>
</template>

<script>
import GoodsDetail from '@/base/goods-detail'
import MaskShadow from '@/base/mask-shadow'
import addAssetlist from '@/base/add-assetlist'
import recipients from '@/base/recipients'
import copy from '@/base/copy'
import changeAssetlist from '@/base/change-assetlist'
import baofei from '@/base/baofei'
import tuihuan from '@/base/tuihuan'
export default {
    name: "AssetList",
    data:function () {
      return{
        showedit:true,
        filter: false,
        showGoodsDetail: false,
        addShow: false,
        recipientsPage: false,
        copyShow: false,
        changeAsset: false,
        baofeiShow: false,
        tuihuanShow:false,
        info: '',
        value: '',
        currentPage:1,
        sum:0,
        options: [{
          value: '选项1',
          label: '办公设备'
        }, {
          value: '选项2',
          label: '冷冻设施'
        }, {
          value: '选项3',
          label: '办公设备'
        }, {
          value: '选项4',
          label: '冷冻设施'
        }, {
          value: '选项5',
          label: '办公设备'
        }],
        tableData: [],
        uersdata:'',
        uersdatack:'',
        collarlist:'',
        multipleSelection:'',
        witchone:'',

        //  搜索
        assetsCode:'',
        assetsName:'',
        assetsTypeId:'',
        batchNumber:'',
        model:'',
        snNumber:'',
        useDepartmentCode:'',
        usePersonName:'',
        adminId:'',
        dataarr:[],
      //  管理员信息
        adminList:[],
      //  设备分类
        typeList:[],
      //  使用人
        personList:[],
        lingyongList:[],
      //  bumen
        officeList:[],
      //  区域
        areaList:[],
        type:0,
        editadd:'add',
        edititem:''

      }
    },
    mounted(){
      var data={
        unitCode:'BJSCSYGJ',
        hospitalCode:'ZXYSHJ',
        userId:'4a00ebaa0219423daa55e07046f2edf9',
        userName:'张鹏',
        roleCode:'corpAdmin,default',
        pageSize:10,
        currentPage:1,
        assetsTypeCode:''
      }
      var url='/assetsInfo/listData';
      this.ajax(url,data,1)
      this.requestAdminInfo()
      this.requestType()
      this.requestPerson()
      this.requestOffice()
      this.requestArea()
    },
    methods:{
      editClickpose(){
        this.addShow = true
        this.editadd='edit'
        this.edititem = this.uersdata
      },
      //区域
      requestArea(){
        var vm = this
        vm.$http.post('http://218.247.12.42:8196/hospitalController/getHospitalGridInfo',{
          unitCode:'ZXYSZGDW',
          hospitalCode:'zkzxysyy',
        }).then(res=>{
          if(res.code=='200'){
            vm.areaList = res.data
          }
        })
      },
      //部门
      requestOffice(){
        var vm = this
        vm.$http.post('http://218.247.12.42:8196/hospitalController/getHospitalOfficeInfo',{
          unitCode:'ZXYSZGDW',
          hospitalCode:'zkzxysyy',
        }).then(res=>{
          if(res.code=='200'){
            vm.officeList = res.data
          }
        })
      },
      //使用人
      requestPerson(){
        var vm = this
        vm.$http.post('http://218.247.12.42:8196/staffController/getHospitalStaffList',{
          unitCode:'ZXYSZGDW',
          hospitalCode:'zkzxysyy',
        }).then(res=>{
          if(res.code=='200'){
            vm.personList = res.data
          }
        })
      },
      addSure(obj){
        var vm =this
        var params=obj
        params.unitCode='BJSCSYGJ',
        params.hospitalCode='ZXYSHJ',
        params.userId='4a00ebaa0219423daa55e07046f2edf9',
        params.userName='张鹏',
        vm.$http.post('/assetsInfo/save',params).then(res=>{
          debugger
        })
      },
      //获取管理员信息
      requestAdminInfo(){
        var vm =this
        vm.$http.post('/userLoginController/getAdminUser',{
          unitCode:'BJSCSYGJ',
          hospitalCode:'ZXYSHJ',
          userId:'4a00ebaa0219423daa55e07046f2edf9',
          userName:'张鹏',
        }).then(res=>{
          if(res.code=='200'){
            vm.adminList = res.data
          }
        })
      },
      //资产类别
      requestType(){ //assrtsClassify/listData
        var vm =this
        vm.$http.post('/assrtsClassify/listData',{
          unitCode:'BJSCSYGJ',
          hospitalCode:'ZXYSHJ',
          userId:'4a00ebaa0219423daa55e07046f2edf9',
          userName:'张鹏',
        }).then(res=>{
          if(res.code=='200'){
            vm.typeList = res.data.list
            // vm.listData = res.data.list
            var arrparent = []
            var obj={}
            vm.typeList.forEach(function (item, index) {
              if(item.parentId=='#'){
                item.options = []
                arrparent.push(item)
              }else{
                if(obj[item.parentId]){
                  obj[item.parentId].push(item)
                }else{
                  obj[item.parentId]=[]
                  obj[item.parentId].push(item)
                }
              }
            })
            for(var i in obj){
              var _index;
              arrparent.forEach(function (item,index) {
                if(item.id == i){
                  arrparent[index].options=obj[i]
                }
              })
            }
            vm.typeList = arrparent
          }
        })
      },
      searchClick(){
        var data={
          unitCode:'BJSCSYGJ',
          hospitalCode:'ZXYSHJ',
          userId:'4a00ebaa0219423daa55e07046f2edf9',
          userName:'张鹏',
          roleCode:'corpAdmin,default',
          pageSize:10,
          currentPage:1,
          assetsTypeCode:''
        }
        var url='/assetsInfo/listData';
        this.ajax(url,data,1)
      },
      resetClick(){
          this.assetsCode=''
          this.assetsName=''
          this.assetsTypeId=''
          this.batchNumber=''
          this.model=''
          this.snNumber=''
          this.useDepartmentCode=''
          this.usePersonName=''
          this.adminId=''
        this.dataarr=[]
        var data={
          unitCode:'BJSCSYGJ',
          hospitalCode:'ZXYSHJ',
          userId:'4a00ebaa0219423daa55e07046f2edf9',
          userName:'张鹏',
          roleCode:'corpAdmin,default',
          pageSize:10,
          currentPage:1,
          assetsTypeCode:''
        }
        var url='/assetsInfo/listData';
        this.ajax(url,data,1)
      },
      changePage(num){
        var data={
          unitCode:'BJSCSYGJ',
          hospitalCode:'ZXYSHJ',
          userId:'4a00ebaa0219423daa55e07046f2edf9',
          userName:'张鹏',
          roleCode:'corpAdmin,default',
          pageSize:10,
          currentPage:num,
          assetsTypeCode:''
        }
        var url='/assetsInfo/listData';
        this.currentPage = num
        this.ajax(url,data,1)

      },
      closeGoodsDetail: function(bool){
        this.showGoodsDetail = false
        this.addShow = false
        this.recipientsPage = false
        this.copyShow = false
        this.changeAsset = false
        this.baofeiShow = false
        this.tuihuanShow = false
        this.editadd = 'add'
        if(bool){
          var data={
            unitCode:'BJSCSYGJ',
            hospitalCode:'ZXYSHJ',
            userId:'4a00ebaa0219423daa55e07046f2edf9',
            userName:'张鹏',
            roleCode:'corpAdmin,default',
            pageSize:10,
            currentPage:1,
            assetsTypeCode:''
          }
          var url='/assetsInfo/listData';
          this.ajax(url,data,1)
        }
      },
     
      showGoodsInfo: function(id){
        var vm =this
        var data={
          unitCode:'BJSCSYGJ',
          hospitalCode:'ZXYSHJ',
          userId:'4a00ebaa0219423daa55e07046f2edf9',
          userName:'张鹏',
          id:id
        }
        var url='/assetsInfo/view';
        this.ajax(url,data,2)
        // var data1={
        //   unitCode:'BJSCSYGJ',
        //   hospitalCode:'ZXYSHJ',
        //   userId:'4a00ebaa0219423daa55e07046f2edf9',
        //   userName:'张鹏',
        //   pageSize:10,
        //   currentPage:1,
        //   assetsId:id
        // }
        //
        // var url1='/disposeRecord/listData';
        // this.ajax(url1,data1,3)
        vm.uersdatack = id
        setTimeout(function () {
          vm.showGoodsDetail = true

        })
      },
      addAssetlist: function(){
        this.addShow = true
      },
      //领用
      recipientsList: function(type){
        var vm =this
        if(!this.multipleSelection){
          vm.$message({
            message: '请先选择资产',
            type: 'warn'
          });
          return
        }
        this.recipientsPage = true
        var arr = []
        vm.lingyongList=vm.multipleSelection.split(',')
        vm.lingyongList.forEach(function (item) {
          vm.tableData.forEach(function (i) {
            if(item == i.id){
              arr.push(i)
            }
          })
        })
        vm.type=type
        vm.lingyongList= arr
      },
      //领用请求
      recipentSure(obj){
        var vm =this
        var params = obj
        params.unitCode='BJSCSYGJ'
          params.hospitalCode='ZXYSHJ'
          params.userId='4a00ebaa0219423daa55e07046f2edf9'
          params.userName='张鹏'
          params.assrtIds = vm.multipleSelection
        vm.$http.post('/receiveRecord/saveOrChangeAssetUse',params).then(res=>{
          if(res.code=='200'){
            vm.$message({
              message: res.message,
              type: 'success'
            });
            vm.recipientsPage=false
            var data={
              unitCode:'BJSCSYGJ',
              hospitalCode:'ZXYSHJ',
              userId:'4a00ebaa0219423daa55e07046f2edf9',
              userName:'张鹏',
              roleCode:'corpAdmin,default',
              pageSize:10,
              currentPage:1,
              assetsTypeCode:''
            }
            var url='/assetsInfo/listData';
            this.ajax(url,data,1)



          }
        })
      },
      //退换
      returnback(data){
        this.tuihuanShow = true
        this.witchone = data
      },
      handleSelectionChange(val){  
          var string="";
          //console.log(val)
          for(var i=0;i<val.length;i++){
            string+=val[i].id;
            if((i+1)<val.length){
              string+=',';
            }
          }
          this.multipleSelection=string;
        },
      //复制
      copyList: function(num){
        let url = '/assetsInfo/copyAssets'
        let data = {
          unitCode:'BJSCSYGJ',
          hospitalCode:'ZXYSHJ',
          roleCode:'corpAdmin,default',
          userId:'4a00ebaa0219423daa55e07046f2edf9',
          userName:'张鹏',
          id:this.multipleSelection,
          number:num,
        }
        this.$http.post(url,data).then(res=>{
            if(res.code==200){
              this.$message.success(res.message)
              this.copyShow = false

              var data={
                unitCode:'BJSCSYGJ',
                hospitalCode:'ZXYSHJ',
                userId:'4a00ebaa0219423daa55e07046f2edf9',
                userName:'张鹏',
                roleCode:'corpAdmin,default',
                pageSize:10,
                currentPage:1,
                assetsTypeCode:''
              }
              var url='/assetsInfo/listData';
              this.ajax(url,data,1)


            }
        })
      },
      copyClick: function(){
        this.copyShow = true
      },
      //编辑
      changeClick: function(edititem){
        this.addShow = true
        this.editadd='edit'
        this.edititem = edititem
      },
      baofeiClick: function(data){
        this.baofeiShow = true
        this.witchone = data
      },
      ajax:function(url,data,type){
        var vm=this;
        if(type==1){
          data.assetsCode=vm.assetsCode
          data.assetsName=vm.assetsName
          data.assetsTypeId=vm.assetsTypeId
          data.batchNumber=vm.batchNumber
          data.model=vm.model
          data.snNumber=vm.snNumber
          data.useDepartmentCode=vm.useDepartmentCode
          data.usePersonName=vm.usePersonName
          data.adminId=vm.adminId
          data.beginReceiveDate=vm.dataarr[0]
          data.endReceiveDate=vm.dataarr[1]
        }
        this.$http.post(url,data).then(res=>{
            if(res.code==200){
              if(type==1){
                vm.tableData = res.data.list;
                vm.sum = res.data.sum;
              }else if(type==2){
                vm.uersdata = res.data[0]
              }else if(type==3){
                debugger
                vm.uersdatack = res.data
              }
            }
        })
      }
    },
    components: {
      GoodsDetail,
      MaskShadow,
      addAssetlist,
      recipients,
      copy,
      changeAssetlist,
      baofei,
      tuihuan
    }
}
</script>

<style scoped lang="scss">
.assetlist{
  .banner{
    text-align: left;
    height: 80px;
    position: relative;
    .bannerleft{
      width: 80%;
      float: left;
    }
    .bannerright{
      width: 20%;
      text-align: right;
      float: left;
    }
    .filter{
      width: 100%;
      background: white;
      right: 0;
      /*height: 230px;*/
      color: #000;
      border: 1px solid #eee;
      box-sizing: border-box;
      border-radius: 4px;
      padding-top: 20px;
      .operator{
        border-bottom: 1px solid #eee;
        padding: 10px 15px;
        overflow: hidden;
        .back{
          width: 10%;
          float: left;
          margin-top: 5px;
        }
        .button-wrapper{
          text-align: right;
          float: left;
          width: 90%;
        }
      }
      table{
        font-size:14px;
        margin-top:20px;
        .table-title{
          margin-left:20px;
        }
      }
      .form-detail{
        padding: 15px;
        .title{
          margin-bottom: 15px;
          font-size: 16px;
        }
        .form{
          .line-list{
            margin-bottom: 10px;
          }
        }
      }
    }
  }
  .pagination{
    margin-top: 10px;
  }
  .table{
    .cell{
      .table-img{
        width: 60px;
        height: 60px;
      }
    }
  }
}
</style>
