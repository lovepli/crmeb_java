<template>
  <div class="divBox">
    <el-card class="box-card">
      <div slot="header" class="clearfix">
        <el-tabs v-model="loginType" @tab-click="getList">
          <el-tab-pane :label="item.name" :name="item.type.toString()" v-for="(item,index) in headeNum" :key="index"/>
        </el-tabs>
        <div class="container">
          <el-form inline size="small" :model="userFrom" ref="userFrom" :label-position="labelPosition"  label-width="100px">
            <el-row>
              <el-col :xs="24" :sm="24" :md="24" :lg="18" :xl="18">
                <el-col v-bind="grid">
                  <el-form-item label="用户搜索：">
                    <el-input v-model="userFrom.keywords" placeholder="请输入" clearable  class="selWidth"/>
                  </el-form-item>
                </el-col>
                <!--<el-col :span="24">-->
                  <!--<el-col v-bind="grid">-->
                    <!--<el-form-item label="会员搜索：">-->
                      <!--<el-input v-model="userFrom.keywords" placeholder="请输入" clearable  class="selWidth"/>-->
                    <!--</el-form-item>-->
                  <!--</el-col>-->
                <!--</el-col>-->
              </el-col>
              <template v-if="collapse">
                <el-col  :xs="24" :sm="24" :md="24" :lg="18" :xl="18">
                  <el-col v-bind="grid">
                    <el-form-item label="用户等级：">
                      <el-select v-model="levelData" placeholder="请选择"  class="selWidth" clearable filterable multiple>
                        <el-option :value="item.id" v-for="(item, index) in levelList" :key="index" :label="item.name"></el-option>
                      </el-select>
                    </el-form-item>
                  </el-col>
                  <el-col v-bind="grid">
                    <el-form-item label="用户分组：">
                      <el-select v-model="groupData" placeholder="请选择"  class="selWidth" clearable filterable multiple>
                        <el-option :value="item.id" v-for="(item, index) in groupList" :key="index" :label="item.groupName"></el-option>
                      </el-select>
                    </el-form-item>
                  </el-col>
                  <el-col v-bind="grid">
                    <el-form-item label="用户标签：">
                      <el-select v-model="labelData" placeholder="请选择"  class="selWidth" clearable filterable multiple>
                        <el-option :value="item.id" v-for="(item, index) in labelLists" :key="index" :label="item.name"></el-option>
                      </el-select>
                    </el-form-item>
                  </el-col>
                </el-col>
                <el-col :xs="24" :sm="24" :md="24" :lg="18" :xl="18">
                  <el-col v-bind="grid">
                    <el-form-item label="国家：">
                      <el-select v-model="userFrom.country" placeholder="请选择"  class="selWidth" clearable @on-change="changeCountry">
                        <el-option value="" label="全部"></el-option>
                        <el-option value="domestic" label="中国"></el-option>
                        <el-option value="abroad" label="外国"></el-option>
                      </el-select>
                    </el-form-item>
                  </el-col>
                  <el-col v-bind="grid" v-if="userFrom.country ==='domestic'">
                    <el-form-item label="省份：">
                      <el-cascader :options="addresData" :props="propsCity" v-model="address" @change="handleChange"  class="selWidth"></el-cascader>
                    </el-form-item>
                  </el-col>
                </el-col>
                <el-col :xs="24" :sm="24" :md="24" :lg="18" :xl="18">
                  <el-col v-bind="grid">
                    <el-form-item label="性别：">
                      <el-radio-group v-model="userFrom.sex" type="button"  class="selWidth">
                        <el-radio-button label="">
                          <span>全部</span>
                        </el-radio-button>
                        <el-radio-button label="1">
                          <span>男</span>
                        </el-radio-button>
                        <el-radio-button label="2">
                          <span>女</span>
                        </el-radio-button>
                        <el-radio-button label="0">
                          <span>保密</span>
                        </el-radio-button>
                      </el-radio-group>
                    </el-form-item>
                  </el-col>
                  <el-col v-bind="grid">
                    <el-form-item label="身份：">
                      <el-radio-group v-model="userFrom.isPromoter" type="button"  class="selWidth">
                        <el-radio-button label="">
                          <span>全部</span>
                        </el-radio-button>
                        <el-radio-button label="1">
                          <span>推广员</span>
                        </el-radio-button>
                        <el-radio-button label="0">
                          <span>普通会员</span>
                        </el-radio-button>
                      </el-radio-group>
                    </el-form-item>
                  </el-col>
                </el-col>
                <el-col :xs="24" :sm="24" :md="24" :lg="18" :xl="18">
                  <el-col v-bind="grid">
                    <el-form-item label="访问情况：">
                      <el-select v-model="userFrom.accessType" placeholder="请选择"  class="selWidth" clearable>
                        <el-option value="" label="全部"></el-option>
                        <el-option value="visitno" label="时间段未访问"></el-option>
                        <el-option value="visit" label="时间段访问过"></el-option>
                        <el-option value="add_time" label="首次访问"></el-option>
                      </el-select>
                    </el-form-item>
                  </el-col>
                  <el-col v-bind="grid">
                    <el-form-item label="消费情况：">
                      <el-select v-model="userFrom.payCount" placeholder="请选择"  class="selWidth" clearable>
                        <el-option value="" label="全部"></el-option>
                        <el-option value="-1" label="0"></el-option>
                        <el-option value="0" label="1+"></el-option>
                        <el-option value="1" label="2+"></el-option>
                        <el-option value="2" label="3+"></el-option>
                        <el-option value="3" label="4+"></el-option>
                        <el-option value="4" label="5+"></el-option>
                      </el-select>
                    </el-form-item>
                  </el-col>
                </el-col>
                <el-col :xs="24" :sm="24" :md="24" :lg="18" :xl="18">
                  <el-col v-bind="grid">
                    <el-form-item label="时间选择：" class="timeBox">
                      <el-date-picker
                        v-model="timeVal"
                        align="right"
                        unlink-panels
                        value-format="yyyy-MM-dd"
                        format="yyyy-MM-dd"
                        size="small"
                        type="daterange"
                        placement="bottom-end"
                        placeholder="自定义时间"
                        class="selWidth"
                        :picker-options="pickerOptions"
                        @change="onchangeTime"
                      />
                    </el-form-item>
                  </el-col>
                </el-col>
              </template>
              <el-col  :xs="24" :sm="24" :md="24" :lg="6" :xl="6" class="text-right userFrom">
                <el-form-item>
                  <el-button type="primary" icon="ios-search" label="default" class="mr15" size="small"   @click="userSearchs">搜索</el-button>
                  <el-button class="ResetSearch mr10" @click="reset('userFrom')"  size="small">重置</el-button>
                  <a class="ivu-ml-8" @click="collapse = !collapse">
                    <template v-if="!collapse">
                      展开 <i class="el-icon-arrow-down"></i>
                    </template>
                    <template v-else>
                      收起 <i class="el-icon-arrow-up"></i>
                    </template>
                  </a>
                </el-form-item>
              </el-col>
            </el-row>
          </el-form>
        </div>
        <el-button class="mr10" size="small" @click="onSend">发送优惠券</el-button>
        <el-button v-show="loginType === 'wechat'" size="mini" class="mr10" @click="sendNews">发送文章</el-button>
        <el-button class="mr10" size="small" @click="setBatch('group')">批量设置分组</el-button>
        <el-button class="mr10" size="small" @click="setBatch('label')">批量设置标签</el-button>
      </div>
      <el-table
        v-loading="listLoading"
        :data="tableData.data"
        style="width: 100%"
        size="mini"
        @selection-change="onSelectTab"
        highlight-current-row
        >
        <el-table-column type="expand">
          <template slot-scope="props">
            <el-form label-position="left" inline class="demo-table-expand">
              <el-form-item label="首次访问：">
                <span>{{ props.row.createTime | filterEmpty }}</span>
              </el-form-item>
              <el-form-item label="近次访问：">
                <span>{{ props.row.lastLoginTime | filterEmpty }}</span>
              </el-form-item>
              <el-form-item label="身份证号：">
                <span>{{ props.row.cardId | filterEmpty }}</span>
              </el-form-item>
              <el-form-item label="手机号：">
                <span>{{ props.row.phone | filterEmpty }}</span>
              </el-form-item>
              <el-form-item label="真实姓名：">
                <span>{{ props.row.realName | filterEmpty }}</span>
              </el-form-item>
              <el-form-item label="标签：">
                <span>{{ props.row.tagName | filterEmpty }}</span>
              </el-form-item>
              <el-form-item label="生日：">
                <span>{{ props.row.birthday | filterEmpty }}</span>
              </el-form-item>
              <el-form-item label="地址：">
                <span>{{ props.row.addres | filterEmpty }}</span>
              </el-form-item>
              <el-form-item label="备注：">
                <span>{{ props.row.mark  | filterEmpty}}</span>
              </el-form-item>
            </el-form>
          </template>
        </el-table-column>
        <el-table-column
          type="selection"
          width="55">
        </el-table-column>
        <el-table-column
          prop="uid"
          label="ID"
          min-width="80"
        />
        <el-table-column label="头像" min-width="80">
          <template slot-scope="scope">
            <div class="demo-image__preview">
              <el-image
                style="width: 36px; height: 36px"
                :src="scope.row.avatar"
                :preview-src-list="[scope.row.avatar]"
              />
            </div>
          </template>
        </el-table-column>
        <el-table-column
          label="姓名"
          min-width="150"
        >
          <template slot-scope="scope">
            <span>{{scope.row.nickname | filterEmpty}}</span>
            <span></span>
            <span></span>
          </template>
        </el-table-column>
        <el-table-column
          label="用户等级"
          min-width="100"
        >
          <template slot-scope="scope">
            <span>{{scope.row.level | levelFilter | filterEmpty}}</span>
          </template>
        </el-table-column>
        <el-table-column
          prop="groupName"
          label="分组"
          min-width="100"
        />
        <el-table-column
          prop="spreadNickname"
          label="推荐人"
          min-width="130"
        />
        <el-table-column
          label="用户类型"
          min-width="100"
        >
          <template slot-scope="scope">
            <span>{{scope.row.userType | typeFilter}}</span>
          </template>
        </el-table-column>
        <el-table-column
          prop="nowMoney"
          label="余额"
          min-width="100"
        />
        <el-table-column
          prop="integral"
          label="积分"
          min-width="100"
        />
        <el-table-column label="操作" min-width="200" fixed="right" align="center">
          <template slot-scope="scope">
            <el-button type="text" size="small" @click="onDetails(scope.row.uid)">账户详情</el-button>
            <el-button type="text" @click="editUser(scope.row.uid)" size="small">编辑</el-button>
            <el-button type="text" @click="editPoint(scope.row.uid)" size="small">积分余额</el-button>
          </template>
        </el-table-column>
        </el-table>
      <div class="block">
        <el-pagination
          :page-sizes="[20, 40, 60, 80]"
          :page-size="userFrom.limit"
          :current-page="userFrom.page"
          layout="total, sizes, prev, pager, next, jumper"
          :total="tableData.total"
          @size-change="handleSizeChange"
          @current-change="pageChange"
        />
      </div>
    </el-card>
    <!--批量设置-->
    <el-dialog
      title="设置"
      :visible.sync="dialogVisible"
      width="500px"
      :before-close="handleClose">
      <el-form :model="dynamicValidateForm" ref="dynamicValidateForm" label-width="100px" class="demo-dynamic" v-loading="loading">
        <el-form-item
          prop="groupId"
          label="用户分组"
          :rules="[{ required: true, message: '请选择用户分组', trigger: 'change' }]"
          v-if="batchName ==='group'"
        >
          <el-select v-model="dynamicValidateForm.groupId" placeholder="请选择分组" style="width: 80%" filterable multiple>
            <el-option :value="item.id" v-for="(item, index) in groupList" :key="index" :label="item.groupName"></el-option>
          </el-select>
        </el-form-item>
        <el-form-item
          prop="groupId"
          label="用户标签"
          :rules="[{ required: true, message: '请选择用户标签', trigger: 'change' }]"
          v-else
        >
          <el-select v-model="dynamicValidateForm.groupId" placeholder="请选择标签" style="width: 80%" filterable multiple>
            <el-option :value="item.id" v-for="(item, index) in labelLists" :key="index" :label="item.name"></el-option>
          </el-select>
        </el-form-item>
      </el-form>
      <span slot="footer" class="dialog-footer">
        <el-button @click="handleClose">取 消</el-button>
        <el-button type="primary" @click="submitForm('dynamicValidateForm')">确 定</el-button>
      </span>
    </el-dialog>
    <!--编辑-->
    <el-dialog
      title="编辑"
      :visible.sync="visible"
      width="600px"
    >
      <edit-from v-if="visible" :uid="uid"></edit-from>
    </el-dialog>
    <!--积分余额-->
    <el-dialog
      title="积分余额"
      :visible.sync="VisiblePoint"
      width="500px"
      :before-close="handlePointClose">
      <el-form :model="PointValidateForm" ref="PointValidateForm" label-width="100px" class="demo-dynamic" v-loading="loadingPoint">
        <el-form-item
          label="修改余额"
          required
        >
            <el-radio-group v-model="PointValidateForm.moneyType">
              <el-radio :label="1">增加</el-radio>
              <el-radio :label="2">减少</el-radio>
            </el-radio-group>
        </el-form-item>
        <el-form-item
          label="余额"
          required
        >
          <el-input type="text" v-model.number="PointValidateForm.moneyValue"></el-input>
        </el-form-item>
        <el-form-item
          label="修改积分"
          required
        >
          <el-radio-group v-model="PointValidateForm.integralType">
            <el-radio :label="1">增加</el-radio>
            <el-radio :label="2">减少</el-radio>
          </el-radio-group>
        </el-form-item>
        <el-form-item
          label="积分"
          required
        >
          <el-input type="text" v-model.number="PointValidateForm.integralValue"></el-input>
        </el-form-item>
      </el-form>
      <span slot="footer" class="dialog-footer">
        <el-button @click="handlePointClose">取 消</el-button>
        <el-button type="primary" :loading="loadingBtn" @click="submitPointForm('PointValidateForm')">确 定</el-button>
      </span>
    </el-dialog>
    <!--账户详情-->
    <el-dialog
      title="用户详情"
      :visible.sync="Visible"
      width="1000px"
      v-if="uid"
      :before-close="Close">
      <user-details ref="userDetails" :uid="uid" v-if="Visible"></user-details>
    </el-dialog>
  </div>
</template>

<script>
  import { userListApi, groupListApi, levelListApi, tagListApi, groupPiApi, tagPiApi, foundsApi } from '@/api/user'
  import editFrom from './edit'
  import userDetails from './userDetails'
  import * as logistics from '@/api/logistics.js'
  import Cookies from 'js-cookie'
  export default {
    name: 'UserIndex',
    components:{ editFrom, userDetails },
    // filters: {
    //   typeFilter(status) {
    //     const statusMap = {
    //       'wechat': '微信用户',
    //       'routine': '小程序用户',
    //       'h5': 'H5用户'
    //     }
    //     return statusMap[status]
    //   }
    // },
    data() {
      return {
        pickerOptions: {
          shortcuts: [
            {
              text: '今天',
              onClick(picker) {
                const end = new Date()
                const start = new Date()
                start.setTime(new Date(new Date().getFullYear(), new Date().getMonth(), new Date().getDate()))
                picker.$emit('pick', [start, end])
              }
            },
            {
              text: '昨天',
              onClick(picker) {
                const end = new Date()
                const start = new Date()
                start.setTime(start.setTime(new Date(new Date().getFullYear(), new Date().getMonth(), new Date().getDate() - 1)))
                end.setTime(end.setTime(new Date(new Date().getFullYear(), new Date().getMonth(), new Date().getDate())))
                picker.$emit('pick', [start, end])
              }
            },
            {
              text: '最近7天',
              onClick(picker) {
                const end = new Date()
                const start = new Date()
                start.setTime(start.getTime() - 3600 * 1000 * 24 * 7)
                picker.$emit('pick', [start, end])
              }
            },
            {
              text: '最近30天',
              onClick(picker) {
                const end = new Date()
                const start = new Date()
                start.setTime(start.getTime() - 3600 * 1000 * 24 * 30)
                picker.$emit('pick', [start, end])
              }
            },
            {
              text: '本月',
              onClick(picker) {
                const end = new Date()
                const start = new Date()
                start.setTime(start.setTime(new Date(new Date().getFullYear(), new Date().getMonth(), 1)))
                picker.$emit('pick', [start, end])
              }
            },
            {
              text: '本年',
              onClick(picker) {
                const end = new Date()
                const start = new Date()
                start.setTime(start.setTime(new Date(new Date().getFullYear(), 0, 1)))
                picker.$emit('pick', [start, end])
              }
            }
          ]
        },
        loadingBtn: false,
        PointValidateForm: {
          integralType: 2,
          integralValue: 0,
          moneyType: 2,
          moneyValue: 0,
          uid: ''
        },
        loadingPoint: false,
        VisiblePoint: false,
        visible: false,
        userIds: '',
        dialogVisible: false,
        levelData: [],
        groupData: [],
        labelData: [],
        selData:[],
        labelPosition:'right',
        collapse: false,
        props: {
          children: 'child',
          label: 'name',
          value: 'id',
          emitPath: false
        },
        propsCity: {
          children: 'child',
          label: 'name',
          value: 'cityId'
        },
        headeNum: [
          { 'type': '', 'name': '全部用户' },
          { 'type': 'wechat', 'name': '微信公众号用户' },
          { 'type': 'routine', 'name': '微信小程序用户' },
          { 'type': 'h5', 'name': 'H5用户' }
        ],
        listLoading: true,
        tableData: {
          data: [],
          total: 0
        },
        loginType: '',
        userFrom: {
          labelId: '',
          userType: '',
          sex: '',
          isPromoter: '',
          country: '',
          payCount: '',
          accessType: '',
          dateLimit: '',
          keywords: '',
          province: '',
          city: '',
          page: 1,
          limit: 15,
          level: '',
          groupId: ''
        },
        grid: {
          xl: 8,
          lg: 12,
          md: 12,
          sm: 24,
          xs: 24
        },
        grid2: {
          xl: 18,
          lg: 16,
          md: 12,
          sm: 24,
          xs: 24
        },
        grid3: {
          xl: 8,
          lg: 12,
          md: 12,
          sm: 24,
          xs: 24
        },
        levelList: [],
        labelLists: [],
        groupList: [],
        selectedData: [],
        timeVal: [],
        addresData: [],
        dynamicValidateForm:{
          groupId: []
        },
        loading: false,
        groupIdFrom: [],
        selectionList: [],
        batchName: '',
        uid: 0,
        Visible: false,
        keyNum: 0,
        address: []
      }
    },
    mounted() {
      this.getList()
      this.groupLists()
      this.levelLists()
      this.getTagList()
      this.getCityList()
    },
    methods: {
      reset(formName) {
        this.userFrom = {
            labelId: '',
            userType: '',
            sex: '',
            isPromoter: '',
            country: '',
            payCount: '',
            accessType: '',
            dateLimit: '',
            keywords: '',
            province: '',
            city: '',
            page: 1,
            limit: 15,
            level: '',
            groupId: ''
        }
        this.levelData = []
        this.groupData = []
        this.labelData = []
        this.getList()
      },
      // 列表
      getCityList() {
        logistics.cityListTree().then(res => {
          res.forEach((el, index) => {
            el.child.forEach((cel, j) => {
              delete cel.child
            })
          })
          this.addresData = res
        })
      },
      // 发送文章
      sendNews() {
        if (this.selectionList.length === 0) return this.$message.warning('请先选择用户')
        const _this = this
        this.$modalArticle(function(row) {
          console.log(row)
        },'send')
      },
      // 发送优惠劵
      onSend(){
        if (this.selectionList.length === 0) return this.$message.warning('请选择要设置的用户');
        const _this = this
        this.$modalCoupon('send', this.keyNum += 1, [],function(row) {
          _this.formValidate.give_coupon_ids = []
          _this.couponData = []
          row.map((item) => {
            _this.formValidate.give_coupon_ids.push(item.coupon_id)
            _this.couponData.push(item.title)
          })
        },this.userIds)
      },
      Close() {
        this.Visible = false
      },
      // 账户详情
      onDetails(id){
        this.uid = id
        this.Visible = true
      },
      // 积分余额
      editPoint(id) {
        this.uid = id
        this.VisiblePoint = true
      },
      // 积分余额
      submitPointForm(formName){
        this.$refs[formName].validate((valid) => {
          if (valid) {
            this.PointValidateForm.uid = this.uid
            this.loadingBtn = true
            foundsApi(this.PointValidateForm).then(res => {
              this.$message.success('设置成功')
              this.loadingBtn = false
              this.handlePointClose()
              this.getList()
            }).catch(() => {
              this.loadingBtn = false
            })
          } else {
            return false
          }
        })
      },
      // 积分余额
      handlePointClose() {
        this.VisiblePoint = false
        this.PointValidateForm = {
          integralType: 2,
          integralValue: 0,
          moneyType: 2,
          moneyValue: 0,
          uid: ''
        }
      },
      editUser(id) {
        this.visible = true
        this.uid = id
      },
      submitForm(formName) {
        this.$refs[formName].validate((valid) => {
          if (valid) {
            this.loading = true
            this.batchName ==='group' ? groupPiApi({groupId: this.dynamicValidateForm.groupId.join(','), id: this.userIds}).then(res => {
              this.$message.success('设置成功')
              this.loading = false
              this.handleClose()
              this.getList()
            }).catch(() => {
              this.loading = false
            }) : tagPiApi({tagId: this.dynamicValidateForm.groupId.join(','), id: this.userIds}).then(res => {
              this.$message.success('设置成功')
              this.loading = false
              this.handleClose()
              this.getList()
            }).catch(() => {
              this.loading = false
            })
          } else {
            return false;
          }
        });
      },
      setBatch(name){
        this.batchName = name
        if (this.selectionList.length === 0) return this.$message.warning('请选择要设置的用户')
        this.dialogVisible = true
      },
      handleClose(){
        this.dialogVisible = false
        this.$refs['dynamicValidateForm'].resetFields();
      },
      // 选择时间
      selectChange (tab) {
        this.timeVal = [];
        this.getList();
      },
      // 全选
      onSelectTab (selection) {
        this.selectionList = selection;
        let data = [];
        this.selectionList.map((item) => {
          data.push(item.uid)
        });
        this.userIds = data.join(',');
      },
      // 搜索
      userSearchs () {
        this.userFrom.page = 1;
        this.getList();
      },
      // 选择国家
      changeCountry () {
        if (this.userFrom.country === 'abroad' || !this.userFrom.country) {
          this.selectedData = [];
          this.userFrom.province = '';
          this.userFrom.city = '';
          this.address = [];
        }
      },
      // 选择地址
      handleChange (value) {
        this.userFrom.province = value[0];
        this.userFrom.city = value[1];
      },
      // 具体日期
      onchangeTime (e) {
        this.timeVal = e;
        console.log(e)
        this.userFrom.dateLimit = e.join(',');
      },
      // 分组列表
      groupLists () {
        groupListApi({ page: 1, limit: 9999}).then(async res => {
          this.groupList = res.list
        })
      },
      //标签列表
      getTagList () {
        tagListApi({ page: 1, limit: 9999}).then(res => {
          this.labelLists = res.list
        })
      },
      // 等级列表
      levelLists () {
        levelListApi({ page: 1, limit: 9999}).then(async res => {
          this.levelList = res.list
          localStorage.setItem('levelKey', JSON.stringify(res.list))
        })
      },
      // 列表
      getList() {
        this.listLoading = true
        this.userFrom.userType = this.loginType
        if(this.loginType == 0) this.userFrom.userType =''
        this.userFrom.level = this.levelData.join(',')
        this.userFrom.groupId = this.groupData.join(',')
        this.userFrom.labelId = this.labelData.join(',')
        userListApi(this.userFrom).then(res => {
          this.tableData.data = res.list
          this.tableData.total = res.total
          this.listLoading = false
        }).catch(() => {
          this.listLoading = false
        })
      },
      pageChange(page) {
        this.userFrom.page = page
        this.getList()
      },
      handleSizeChange(val) {
        this.userFrom.limit = val
        this.getList()
      },
      // 删除
      handleDelete(id, idx) {
        this.$modalSure().then(() => {
          productDeleteApi(id).then(() => {
            this.$message.success('删除成功')
            this.getList()
          })
        })
      },
      onchangeIsShow(row) {
        row.isShow
          ? putOnShellApi( row.id ).then(() => {
            this.$message.success('上架成功')
            this.getList()
          }) : offShellApi(row.id).then(() => {
            this.$message.success('下架成功')
            this.getList()
          })
      }
    }
  }
</script>

<style scoped lang="scss">
  /*.timeBox{*/
    /*width: 100%;*/
    /*/deep/.el-form-item__content{*/
      /*width: 87% !important;*/
    /*}*/
  /*}*/
  .text-right{
    text-align: right;
  }
  .demo-table-expand {
    font-size: 0;
  }
  .demo-table-expand label {
    width: 90px;
    color: #99a9bf;
  }
  .demo-table-expand .el-form-item {
    margin-right: 0;
    margin-bottom: 0;
    width: 33.33%;
  }
  .selWidth{
    width: 100% !important;
  }
  .seachTiele{
    line-height: 30px;
  }
  .container{
    min-width: 821px;
    /deep/.el-form-item{
      width: 100%;
    }
    /deep/.el-form-item__content{
      width: 72%;
    }
  }
  .ivu-ml-8{
    font-size: 12px;
    color: #1682e6;
  }
</style>
