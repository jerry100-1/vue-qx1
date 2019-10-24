<template>
  <div class="main">


   <!-- <div class="wrapTheModalWWW" v-if="showTheModal">
                 
               <div style="width:400px;height:400px;position:absolute;left:50%;margin-left:-200px;top:50%;margin-top:-200px;background:#fff;z-index:4500;">
                       <el-dropdown>
                      <el-button type="primary">
                        更多菜单<i class="el-icon-arrow-down el-icon--right"></i>
                      </el-button>
                <el-dropdown-menu slot="dropdown">
                        <el-dropdown-item>黄金糕</el-dropdown-item>
                        <el-dropdown-item>狮子头</el-dropdown-item>
                        <el-dropdown-item>螺蛳粉</el-dropdown-item>
                        <el-dropdown-item>双皮奶</el-dropdown-item>
                        <el-dropdown-item>蚵仔煎</el-dropdown-item>
                </el-dropdown-menu>
              </el-dropdown>




               </div>

   </div> -->




<el-dialog :modal="true" 
   :fullscreen="fullscreen" 
   :visible.sync="dialogVisible" 
   width="30%" height="400px!important;" style="margin-top:10%;">
                <!-- 有效 -->
             <!-- <select name="" id="" >

                    <option value="1">1</option>
                    <option value="2">2</option>
                    <option value="3">3</option>
                    <option value="4">4</option>
                    <option value="5">5</option>
                    <option value="6">6</option>
             </select> -->
                 <!-- 有效 -->
              <!-- <el-dropdown>
                    <el-button type="primary" style="margin-left:30px;">
                      更多菜单<i class="el-icon-arrow-down el-icon--right"></i>
                    </el-button>
                    <el-dropdown-menu slot="dropdown">
                          <el-dropdown-item>黄金糕</el-dropdown-item>
                          <el-dropdown-item>狮子头</el-dropdown-item>
                          <el-dropdown-item>螺蛳粉</el-dropdown-item>
                          <el-dropdown-item>双皮奶</el-dropdown-item>
                          <el-dropdown-item>蚵仔煎</el-dropdown-item>
                    </el-dropdown-menu>
            </el-dropdown> -->

             <!-- <el-select v-model="value" placeholder="请选择"  style="width:300px;height:40px;margin-left:-150px;left:50%;">
                    <el-option
                      v-for="item in options"
                      :key="item.value"
                      :label="item.label"
                      :value="item.value">
                    </el-option>
            </el-select> -->
                      <!-- <el-input v-model="tuishouId"  style="width:100px;border:none;"></el-input>   -->

                       <!-- <el-input value="修改等级"  style="width:100px;border:none;"></el-input> -->
                 
                          <div>
                                <h4  class="eleft1" style="">修改状态</h4>
                                
                          </div>
                          <div>
                                <span class="eleft2" style="">手机号码 :</span>
                                <el-input :disabled="true" v-model="tuishouPhone" class="eleft4" style=""></el-input> 
                          </div>




                        <div style="margin-top:12px;">
                                <span class="eleft3" style="">修改等级 :</span>
                                <el-select  @change="selectGet" v-model="tuishouGrade"  class="filter-input" placeholder="请选择"  style="width:200px;height:40px;margin-left:-130px;left:30%;display:inline-block;vertical-align:middle;">
                                        <el-option
                                          v-for="item in typeOptions"
                                          :key="item.id"
                                          :label="item.name"
                                          :value="item.id">
                                        </el-option>
                                </el-select>
                          </div>

                      <el-button   @click="dismissTheDialog" type="primary" class="eleft5" style="">确定</el-button>

                      <div style="width:100%;height:30px;">

                      </div>


   
   </el-dialog>













      <div ref="queryContent" v-sticky="{zIndex: 2001, stickyTop: 0, disabled: false}">
        <div class="header-wrapper">
          <el-form :disabled="isListLoading" :model="queryForm" ref="queryForm" :inline="true" label-width="0" :show-message="false">
            <div class="d-flex justify-content-between">
              <div class="filter-wrapper mr-2">
                <div class="filter-container">
                  <div class="filter-item mr-3">
                    <label>手机号：</label>
                    <el-form-item prop="gzhName" class="inline-form-item">
                      <el-input v-model.trim="queryForm.mobile" class="filter-input" clearable></el-input>
                    </el-form-item>
                  </div>
                  <div class="filter-item mr-3">
                    <label>等级：</label>
                    <el-form-item prop="group" class="inline-form-item">
                      <el-select v-model="queryForm.type" class="filter-input">
                        <el-option label="全部" :value="null"></el-option>
                        <el-option
                          v-for="item in typeOptions"
                          :key="item.id"
                          :label="item.name"
                          :value="item.id">
                        </el-option>
                      </el-select>
                    </el-form-item>
                  </div>
                  <div class="filter-item">
                    <label>状态：</label>
                    <el-form-item prop="status" class="inline-form-item">
                      <el-select v-model="queryForm.status" class="filter-input">
                        <el-option label="全部" :value="null"></el-option>
                        <el-option
                          v-for="item in statusOptions"
                          :key="item.id"
                          :label="item.name"
                          :value="item.id">
                        </el-option>
                      </el-select>
                    </el-form-item>
                  </div>
                </div>
              </div>
              <div class="tools flex-shrink-0">
                <div class="filter-item">
                  <el-button :loading="isListLoading" @click="fetchData" type="primary" class="query-button" icon="el-icon-search">查询</el-button>
                  <el-button :disabled="isListLoading" @click="resetQueryForm" class="query-button">重置</el-button>
                </div>
              </div>
            </div>
          </el-form>
        </div>
      </div>
      <div class="content-wrapper">
        <div class="main-content">
          <el-table
            ref="table"
            :data="rawListData"
            v-loading="isListLoading"
            border
            fit
            highlight-current-row
            style="width: 100%;"
            >
            <el-table-column
              prop="Id"
              label="ID"
              width="50"
            >
            </el-table-column>
            <el-table-column
              prop="Mobile"
              label="手机号">
            </el-table-column>
            <el-table-column
              prop="Type"
              label="绑定微信">
              <template slot-scope="scope">
                <div style="display: flex; flex-direction: column;align-items: center;justify-content: center">
                  <img style="width: 100px; height: 100px" :src="scope.row.HeadUrl"/>
                  <span>{{scope.row.UserName}}</span>
                </div>
              </template>
            </el-table-column>
            <el-table-column
              prop="Desc"
              label="微信二维码">
              <template slot-scope="scope">
                <div style="display: flex; flex-direction: column;align-items: center;justify-content: center">
                  <img v-if='scope.row.WeixinUrl' style="width: 100px; height: 100px" :src="scope.row.WeixinUrl"/>
                </div>
              </template>
            </el-table-column>
            <el-table-column
              prop="InviteKey"
              label="推手KEY">
            </el-table-column>
            <el-table-column
              prop="InviteInfo"
              label="邀请人">
            </el-table-column>
            <el-table-column
                prop="IsPayUser"
                label="是否支付推手费"
                width="80"
            >
            </el-table-column>
            <el-table-column
              prop="Grade"
              label="等级"
              width="100"
              v-model="wwwType"      
            >
              <!-- <template slot-scope="scope">
                {{typeMap.get(scope.row.Grade)}}
              </template> -->
              <template slot-scope="scope">
                {{scope.row.Grade|formateGrade}}
              </template>



            </el-table-column>
            <el-table-column
                prop="InviteCount"
                label="邀请推手数"
                width="100"
            >
            </el-table-column>
            <el-table-column
                prop="UserCount"
                label="付费用户数"
                width="100"
            >
            </el-table-column>
            <el-table-column
                prop="Sort"
                label="总收益"
                width="100"
            >
              <template slot-scope="scope">
                {{scope.row.TotalCash / 100}}元
              </template>
            </el-table-column>
            <el-table-column
                prop="Sort"
                label="可提现收益"
                width="100"
            >
              <template slot-scope="scope">
                {{scope.row.WithdrawCash / 100}}元
              </template>
            </el-table-column>
            <el-table-column
                prop="Sort"
                label="待确认收益"
                width="100"
            >
              <template slot-scope="scope">
                {{(scope.row.Cash - scope.row.WithdrawCash) / 100 }}元
              </template>
            </el-table-column>
            <el-table-column
                prop="CreateTime"
                label="注册时间"
                width="100"
            >
            </el-table-column>


              <el-table-column prop="" label="操作" width="100px;">
                      <template slot-scope="scope">
                          <el-button
                                  size="mini"
                                  type="primary"
                                  @click="editOpen(scope.row)" style="display:inline-block">编辑</el-button>

                      </template>
                </el-table-column>


          </el-table>
          <div v-show="!isListLoading" class="text-center py-3">
            <el-pagination
              @current-change="handleCurrentChange"
              @size-change="handlePageSizeChange"
              :current-page.sync="tableCurrentPage"
              :page-sizes="tablePageSizeOption"
              :page-size="tablePageSize"
              layout="total, sizes, prev, pager, next, jumper"
              :total="tableTotalSize"
              background>
            </el-pagination>
          </div>
        </div>
      </div>
  </div>
</template>

<script>
import VueSticky from 'vue-sticky'
import RecommendApi from '@/api/recommend'
import swal from 'sweetalert2'
import { deepClone } from '@/utils'
//  推手列表
export default {
  name: 'user-list',
  directives: {
    sticky: VueSticky
  },
  data () {
    const statusOptions = [
      {
        id: '0',
        name: '未注册'
      }, {
        id: '1',
        name: '已注册'
      }
    ]
    const typeOptions = [
      {
        id: '1',
        name: '黄金推手'
      }, {
        id: '2',
        name: '铂金推手'
      }, {
        id: '3',
        name: '钻石推手'
      }
    ]
    const mobile = null
    const status = null
    const type = null

    return {
      tuishouPhone:"",
      wwwType:"",
      tsGradeNum:"",
       tuishouGrade:"",
       tuishouId:"",
       myType:"",
       form: {
        region: ""
      },
     options: [{
          value: '选项1',
          label: '黄金糕'
        }, {
          value: '选项2',
          label: '双皮奶'
        }, {
          value: '选项3',
          label: '蚵仔煎'
        }, {
          value: '选项4',
          label: '龙须面'
        }, {
          value: '选项5',
          label: '北京烤鸭'
        }],
      value: '',
      dialogVisible:false,
      fullscreen:false,
      showTheModal:false,
      queryForm: {
        // 公众号名称
        mobile: mobile,
        // 类型
        type: type,
        // 状态
        status: status
      },
      // 状态选项
      statusOptions: statusOptions,
      typeOptions: typeOptions,
      statusMap: new Map(statusOptions.map(item => [item.id, item.name])),

      // 图表数据加载状态
      isListLoading: false,
      // 保存之前的请求参数
      query: deepClone({ mobile, type, status }),
      // 图表原始数据
      rawListData: [],

      // 表格当前页
      tableCurrentPage: 1,
      // 表格一页条数可选项
      tablePageSizeOption: [10, 20, 30],
      // 表格一页条数
      tablePageSize: 20,
      // 总条数
      tableTotalSize: 0
    }
  },
  filters: {
        formateGrade(item){
           if(item==1)
           {
             return "黄金推手";
           }else if(item==2)
           {
             return "铂金推手";
           }else{
             return "钻石推手";
           }
        }
  },
  computed: {
    cachedViews () {
      return this.$store.state.tagsView.cachedViews
    },
    typeMap () {
      return new Map(this.typeOptions.map(item => [item.id, item.name]))
    }
  },
  created () {
    this.fetchData()
  },
  activated () {
    if (this.$route.query.refresh === 1) {
      this.fetchList()
    }
  },
  methods: {


        selectGet(vId){
              let obj = {};
              obj = this.typeOptions.find((item)=>{//这里的selectList就是上面遍历的数据源
                  return item.id === vId;//筛选出匹配数据          
              });
                    //this.Grade=obj.id;
              console.log("得到的type的name是"+obj.name);//我这边的name就是对应label的
              console.log("得到的type的id是"+obj.id);//这里的id其实就是Grade
              this.tuishouGrade=obj.id;

               // console.log("得到的type的Id是"+obj.Id);//这里的id其实就是Grade
               //this.wwwType=obj.id;
              //  this.$nextTick(function(){
              //     this.tuishouGrade=obj.name;
              //  })
              
            },
            

         getTheSeleType()
         {
           console.log("当前的myType是"+this.value);
         },
          dismissTheDialog(){


              this.dialogVisible = false;
              console.log("你好,要发送请求之前的两个重要参数之一的推手id是"+this.tuishouId);
              console.log("你好,要发送请求之前的两个重要参数之一的推手等级是"+this.tuishouGrade);
               
                
                
                const postData = {
                  id: this.tuishouId,
                  grade: this.tuishouGrade
                }
                RecommendApi.modifyTuiShouGrade(postData).then(({ data }) => {
                  //this.isListLoading = false
                  if (data.Status== 200) {
                    console.log("发送修改推手等级请求后接口返回的数据是",data);
                  }
                  this.fetchData();
                    // this.rawListData = data.Result.List
                    // console.log("你好,得到的rawListData的详细数据如下:",data.Result)

                // this.tableTotalSize = parseInt(data.Result.Total)
                });

                
          },

      showDialog() {
        this.dialogVisible = true;
      },

         //编辑病症名称
            editOpen(row)
            {
              var myGrade="";

              if(row.Grade==1)
              {
                  this.tuishouGrade="黄金等级";
              }else if(row.Grade==2)
              {
                  this.tuishouGrade="铂金等级";
              }else{
                  this.tuishouGrade="钻石等级";
              }

               console.log("你好,你获取的推手等级是"+row.Grade);
               console.log("你好,你获取的推手ID是"+row.Id);
               this.tuishouId=row.Id;
               this.tuishouPhone=row.Mobile;
               //this.tuishouGrade=row.Grade;
               this.showTheModal=true;
               this.showDialog();

                // this.$prompt('请输入新的账号名称', '', {
                //     confirmButtonText: '确定',
                //     cancelButtonText: '取消',
                //     // inputPattern: /[\w!#$%&'*+/=?^_`{|}~-]+(?:\.[\w!#$%&'*+/=?^_`{|}~-]+)*@(?:[\w](?:[\w-]*[\w])?\.)+[\w](?:[\w-]*[\w])?/,
                //     inputErrorMessage: '邮箱格式不正确',
                //     inputPlaceholder:row.name
                // }).then(({ value }) => {
                //     this.$message({
                //         type: 'success',
                //         message: '请输入新的账号名称: ' + value
                //     });

                //     let token=localStorage.getItem('token');
                //     console.log(token);

                //     this.$http.get('/admin/symptom/addtype',{
                //         params:{
                //             token,
                //             apitype:1,
                //             title:value,
                //             id:row.id
                //         }
                //     }).then(res=>{
                //         // console.log('删除',res)
                //         console.log(res);
                //         if(res.data.status==1){
                //             this.$message.success('编辑症状成功!');
                //             // window.location.reload();
                //             this.getDiseaseSeries();
                //             console.log("此处语句被执行了!");
                //         }else{
                //             console.log("此处语句被执行o!");
                //             this.$message.error(res.data.message);
                //         }
                //     })

                // }).catch(() => {
                //     this.$message({
                //         type: 'info',
                //         message: '取消输入'
                //     });
                // });

            },


    // 重置查询
    resetQueryForm () {
      // this.$refs.queryForm.resetFields()
      this.queryForm.mobile = null
      this.queryForm.type = null
      this.queryForm.status = null
    },
    fetchData () {
      // 克隆当前查询条件
      console.log(this.queryForm)
      this.query = deepClone(this.queryForm)
      this.tableCurrentPage = 1
      this.tableTotalSize = 0

      this.fetchList()
    },
    fetchList () {
      // 获取图表数据
      this.isListLoading = true
      this.rawListData = []
      console.log(this.query)
      const { status, mobile, type } = this.query

      const postData = {
        mobile: mobile,
        grade: type,
        status: status,
        page: this.tableCurrentPage,
        limit: this.tablePageSize
      }
      RecommendApi.getUserList(postData).then(({ data }) => {
        this.isListLoading = false
        if (data.Status !== 200) {
          swal({
            text: data.Result.ErrorMsg,
            type: 'error',
            timer: 2000,
            showConfirmButton: false
          })
          return
        }
        this.rawListData = data.Result.List
console.log("你好,得到的rawListData的详细数据如下:",data.Result)

        this.tableTotalSize = parseInt(data.Result.Total)
      })
    },
    handleCurrentChange (value) {
      this.fetchList()
    },
    handlePageSizeChange (value) {
      this.tableCurrentPage = 1
      this.tablePageSize = value
      this.fetchList()
    }
  }
}
</script>

<style lang="less" scoped>
  
.main {
  display: table;
  table-layout: fixed;
  min-width: 1300px;
  width: 100%;
  height: 100%;
  background-color: #ececec;
  .filter-item {
    margin-bottom: 15px;
  }
  .filter-container {
    .filter-item {
      display: inline-block;
      white-space: nowrap;
    }
  }
    .eleft1{
        display:inline-block;width:100%;height:40px;text-align:left;font-size:16px;
        position:absolute;
        left:20px;
        top:20px;
    }
    .eleft2{
        display:inline-block;vertical-align:middle;margin-left:18%;
    }
    .eleft3{
       display:inline-block;vertical-align:middle;margin-left:18%;
    }
    .eleft4{
       width:200px;height:40px;margin-left:-130px;left:30%;display:inline-block;vertical-align:middle;
    }    
    .eleft5{
       position:absolute;right:60px;bottom:10px;
    }    


  .filter-input {
    width: 200px;
  }
  .query-button {
    width: 100px;
  }
  .tool-button {
    width: 100px;
  }
  .oper-button {
    width: 80px;
  }
  .header-wrapper {
    padding: 14px 20px 0;
    box-shadow: 0 1px 3px rgba(0,0,0,.1);
    background-color: #fff;
  }
  .content-wrapper {
    padding: 20px;
    .main-content {
      border-top: 1px solid #e1e3e4;
      border-left: 1px solid #e1e3e4;
      border-right: 1px solid #e1e3e4;
      background-color: #fff;
      .button-wrapper {
        padding: 20px 20px;
      }
    }
  }
  .inline-form-item {
    display: inline-block;
    margin: 0;
    vertical-align: middle;
  }
}

  .wrapTheModalWWW{
      width:100%;
      height:100%;
      position:fixed;
      top:0;
      left:0;
      right:0;
      bottom:0;
      z-index:4100;
      background:rgba(0,0,0,0.66);
    }



.gzh-info-container {
  width: 100%;
  height: 100%;
  padding: 20px 20px;
  background-color: #fff;
  .gzh-info-form {
    width: 800px;
    .gzh-info-group {
      display: table;
      table-layout: fixed;
      width: 100%;
      border-collapse: collapse;
      background: #f9f9f9;
      .gzh-info {
        display: table-row;
        height: 45px;
        .label, .value {
          display: table-cell;
          padding: 10px 20px;
          text-align: left;
          vertical-align: middle;
          border: 1px solid #dcdfe6;
          word-break: break-word;
        }
        .label {
          width: 30%;
        }
        .value {
          width: 70%;
        }
        .form-input {
          width: 300px;
        }
      }
    }
    .buttons {
      margin-top: 20px;
      text-align: center;
      .form-button {
        width: 80px;
      }
    }
  }
}
</style>
