<template>
	<el-main class="bg">
		<el-form ref="form" :model="form" status-icon label-width="120px" v-if="is_view()">
			<el-col v-if="user_group === '管理员' || $check_field('get','order_number') || $check_field('add','order_number') || $check_field('set','order_number')" :xs="24" :sm="12" :lg="8">
				<el-form-item label="订单编号" prop="order_number">
					<el-input id="order_number" v-model="form['order_number']" placeholder="请输入订单编号"
							  v-if="user_group === '管理员' || (form['order_information_id'] && $check_field('set','order_number')) || (!form['order_information_id'] && $check_field('add','order_number'))" :disabled="true"></el-input>
					<div v-else-if="$check_field('get','order_number')">{{form['order_number']}}</div>
				</el-form-item>
			</el-col>
			<el-col v-if="user_group === '管理员' || $check_field('get','purchase_user') || $check_field('add','purchase_user') || $check_field('set','purchase_user')" :xs="24" :sm="12" :lg="8">
				<el-form-item label="购买用户" prop="purchase_user">
						<div v-if="user_group !== '管理员'">
							{{ get_user_session_purchase_user(form['purchase_user']) }}
							<!--<el-input id="business_name" v-model="form['purchase_user']" placeholder="请输入购买用户"-->
							<!--v-if="user_group === '管理员' || (form['order_information_id'] && $check_field('set','purchase_user')) || (!form['order_information_id'] && $check_field('add','purchase_user'))" :disabled="disabledObj['purchase_user_isDisabled']"></el-input>-->
							<!--<div v-else-if="$check_field('get','purchase_user')">{{form['purchase_user']}}</div>-->
							<el-select v-if="user_group === '管理员' || (form['order_information_id'] && $check_field('set','purchase_user')) || (!form['order_information_id'] && $check_field('add','purchase_user'))" id="purchase_user" v-model="form['purchase_user']" :disabled="disabledObj['purchase_user_isDisabled']">
								<el-option v-for="o in list_user_purchase_user" :key="o['username']" :label="o['nickname'] + '-' + o['username']"
										   :value="o['user_id']">
								</el-option>
							</el-select>
							<el-select v-else-if="$check_field('get','purchase_user')" id="purchase_user" v-model="form['purchase_user']" :disabled="true">
								<el-option v-for="o in list_user_purchase_user" :key="o['username']" :label="o['nickname'] + '-' + o['username']"
										   :value="o['user_id']">
								</el-option>
							</el-select>
						</div>
						<el-select v-else id="purchase_user" v-model="form['purchase_user']" :disabled="disabledObj['purchase_user_isDisabled']">
							<el-option v-for="o in list_user_purchase_user" :key="o['username']" :label="o['nickname'] + '-' + o['username']"
									   :value="o['user_id']">
							</el-option>
						</el-select>
				</el-form-item>
			</el-col>
			<el-col v-if="user_group === '管理员' || $check_field('get','user_name') || $check_field('add','user_name') || $check_field('set','user_name')" :xs="24" :sm="12" :lg="8">
				<el-form-item label="用户姓名" prop="user_name">
					<el-input id="user_name" v-model="form['user_name']" placeholder="请输入用户姓名"
							  v-if="user_group === '管理员' || (form['order_information_id'] && $check_field('set','user_name')) || (!form['order_information_id'] && $check_field('add','user_name'))" :disabled="disabledObj['user_name_isDisabled']"></el-input>
					<div v-else-if="$check_field('get','user_name')">{{form['user_name']}}</div>
				</el-form-item>
			</el-col>
			<el-col v-if="user_group === '管理员' || $check_field('get','subscriber_telephone') || $check_field('add','subscriber_telephone') || $check_field('set','subscriber_telephone')" :xs="24" :sm="12" :lg="8">
				<el-form-item label="用户电话" prop="subscriber_telephone">
					<el-input id="subscriber_telephone" v-model="form['subscriber_telephone']" placeholder="请输入用户电话" type="tel"
						v-if="user_group === '管理员' || (form['order_information_id'] && $check_field('set','subscriber_telephone')) || (!form['order_information_id'] && $check_field('add','subscriber_telephone'))">
					</el-input>
					<div v-else-if="$check_field('get','subscriber_telephone')">{{form['subscriber_telephone']}}</div>
				</el-form-item>
			</el-col>
			<el-col v-if="user_group === '管理员' || $check_field('get','user_address') || $check_field('add','user_address') || $check_field('set','user_address')" :xs="24" :sm="12" :lg="8">
				<el-form-item label="用户地址" prop="user_address">
					<el-input id="user_address" v-model="form['user_address']" placeholder="请输入用户地址"
							  v-if="user_group === '管理员' || (form['order_information_id'] && $check_field('set','user_address')) || (!form['order_information_id'] && $check_field('add','user_address'))" :disabled="disabledObj['user_address_isDisabled']"></el-input>
					<div v-else-if="$check_field('get','user_address')">{{form['user_address']}}</div>
				</el-form-item>
			</el-col>
			<el-col v-if="user_group === '管理员' || $check_field('get','trade_name') || $check_field('add','trade_name') || $check_field('set','trade_name')" :xs="24" :sm="12" :lg="8">
				<el-form-item label="商品名称" prop="trade_name">
					<el-input id="trade_name" v-model="form['trade_name']" placeholder="请输入商品名称"
							  v-if="user_group === '管理员' || (form['order_information_id'] && $check_field('set','trade_name')) || (!form['order_information_id'] && $check_field('add','trade_name'))" :disabled="disabledObj['trade_name_isDisabled']"></el-input>
					<div v-else-if="$check_field('get','trade_name')">{{form['trade_name']}}</div>
				</el-form-item>
			</el-col>
			<el-col v-if="user_group === '管理员' || $check_field('get','commodity_price_') || $check_field('add','commodity_price_') || $check_field('set','commodity_price_')" :xs="24" :sm="12" :lg="8">
				<el-form-item label="商品价钱" prop="commodity_price_">
					<el-input id="commodity_price_" v-model="form['commodity_price_']" placeholder="请输入商品价钱"
							  v-if="user_group === '管理员' || (form['order_information_id'] && $check_field('set','commodity_price_')) || (!form['order_information_id'] && $check_field('add','commodity_price_'))" :disabled="disabledObj['commodity_price__isDisabled']"></el-input>
					<div v-else-if="$check_field('get','commodity_price_')">{{form['commodity_price_']}}</div>
				</el-form-item>
			</el-col>
			<el-col v-if="user_group === '管理员' || $check_field('get','purchase_quantity') || $check_field('add','purchase_quantity') || $check_field('set','purchase_quantity')" :xs="24" :sm="12" :lg="8">
				<el-form-item label="购买数量" prop="purchase_quantity">
					<el-input id="purchase_quantity" v-model="form['purchase_quantity']" placeholder="请输入购买数量"
							  v-if="user_group === '管理员' || (form['order_information_id'] && $check_field('set','purchase_quantity')) || (!form['order_information_id'] && $check_field('add','purchase_quantity'))" :disabled="disabledObj['purchase_quantity_isDisabled']"></el-input>
					<div v-else-if="$check_field('get','purchase_quantity')">{{form['purchase_quantity']}}</div>
				</el-form-item>
			</el-col>
			<el-col v-if="user_group === '管理员' || $check_field('get','total') || $check_field('add','total') || $check_field('set','total')" :xs="24" :sm="12" :lg="8">
				<el-form-item label="总计" prop="total">
					<el-input id="total" v-model="form['total']" placeholder="请输入总计"
							  v-if="user_group === '管理员' || (form['order_information_id'] && $check_field('set','total')) || (!form['order_information_id'] && $check_field('add','total'))"  @focus="set_total()" :disabled="disabledObj['total_isDisabled']"></el-input>
					<div v-else-if="$check_field('get','total')">{{form['total']}}</div>
				</el-form-item>
			</el-col>
			<el-col :xs="24" :sm="12" :lg="8">
				<el-form-item>
					<el-button type="primary" @click="submit()">提交</el-button>
					<el-button @click="cancel()">取消</el-button>
				</el-form-item>
			</el-col>

		</el-form>
	</el-main>
</template>

<script>
	import mixin from "@/mixins/page.js";

	export default {
		mixins: [mixin],
		data() {
			return {
				field: "order_information_id",
				url_add: "~/api/order_information/add?",
				url_set: "~/api/order_information/set?",
				url_get_obj: "~/api/order_information/get_obj?",
				url_upload: "~/api/order_information/upload?",

				query: {
					"order_information_id": 0,
				},

				form: {
					"order_number":this.$get_stamp(), // 订单编号
					"purchase_user": 0, // 购买用户
					"user_name":'', // 用户姓名
					"subscriber_telephone":'', // 用户电话
					"user_address":'', // 用户地址
					"trade_name":'', // 商品名称
					"commodity_price_":'', // 商品价钱
					"purchase_quantity":'', // 购买数量
					"total":'', // 总计
					"order_information_id": 0, // ID

				},
				disabledObj:{
					"order_number_isDisabled": false,
					"purchase_user_isDisabled": false,
					"user_name_isDisabled": false,
					"subscriber_telephone_isDisabled": false,
					"user_address_isDisabled": false,
					"trade_name_isDisabled": false,
					"commodity_price__isDisabled": false,
					"purchase_quantity_isDisabled": false,
					"total_isDisabled": false,
				},
				// 用户列表
				list_user_purchase_user: [],
				// 用户组
				group_user_purchase_user: "",

			}
		},
		methods: {
			/**
			 * 获取普通用户用户列表
			 */
			async get_list_user_purchase_user() {
                // if(this.user_group !== "管理员" && this.form["purchase_user"] === 0) {
                //     this.form["purchase_user"] = this.user.user_id;
                // }
                var json = await this.$get("~/api/user/get_list?user_group=普通用户");
                if(json.result && json.result.list){
                    this.list_user_purchase_user = json.result.list;
                }
                else if(json.error){
                    console.error(json.error);
                }
			},
			/**
			 * 获取普通用户用户组
			 */
			async get_group_user_purchase_user() {
				this.form["purchase_user"] = this.user.user_id;
				var json = await this.$get("~/api/user_group/get_obj?name=普通用户");
				if(json.result && json.result.obj){
					this.group_user_purchase_user = json.result.obj;
				}
				else if(json.error){
					console.error(json.error);
				}
			},
			get_user_session_purchase_user(id){
				var _this = this;
				var user_id = {"user_id":id}
				var url = "~/api/"+_this.group_user_purchase_user.source_table+"/get_obj?"
				this.$get(url, user_id, function(res) {
					if (res.result && res.result.obj) {
						var arr = []
						for (let key in res.result.obj) {
							arr.push(key)
						}
						var arrForm = []
						for (let key in _this.form) {
							arrForm.push(key)
						}
						for (var i=0;i<arr.length;i++){
							for (var j=0;j<arrForm.length;j++){
								if (arr[i]===arrForm[j]){
									if (arr[i]!=="purchase_user") {
										_this.form[arrForm[j]] = res.result.obj[arr[i]]
										_this.disabledObj[arrForm[j] + '_isDisabled'] = true
										break;
									}else {
										_this.disabledObj[arrForm[j] + '_isDisabled'] = true
									}
								}
							}
						}
					}
				});
			},
			get_user_purchase_user(id){
				var obj = this.list_user_purchase_user.getObj({"user_id":id});
				var ret = "";
				if(obj){
					if(obj.nickname){
						ret = obj.nickname;}
					else{
						ret = obj.username;
					}
				}
				return ret;
			},
			set_total(){
				this.form.total =this.form.commodity_price_ * this.form.purchase_quantity
			},

			/**
			 * 获取对象之前
			 * @param {Object} param
			 */
			get_obj_before(param) {
				var form = "";
				// 获取缓存数据附加
				form = $.db.get("form");
				$.push(this.form ,form);
				/**
				* 请求列表前
				* @param {Object} param
				*/
				var user_group = this.user.user_group;
				if (user_group !== "管理员") {
					switch (user_group) {
						case "购买用户":
							if(param["purchase_user"] > 0){
								param["purchase_user"] = this.user.user_id;
							}
							break;
					}
				}
				if(this.form && form){
					Object.keys(this.form).forEach(key => {
						Object.keys(form).forEach(dbKey => {
							// if(dbKey === "charging_standard"){
							// 	this.form['charging_rules'] = form[dbKey];
							// 	this.disabledObj['charging_rules_isDisabled'] = true;
							// };
							if(key === dbKey){
								this.disabledObj[key+'_isDisabled'] = true;
							}
						})
					})
				}
				$.db.del("form");
				return param;
			},

			/**
			 * 获取对象之后
			 * @param {Object} json
			 * @param {Object} func
			 */
			get_obj_after(json, func){

			},

			is_view(){
				var bl = this.user_group == "管理员";

				if(!bl){
					bl = this.$check_action('/order_information/table','add');
					console.log(bl ? "你有表格添加权限视作有添加权限" : "你没有表格添加权限");
				}
				if(!bl){
					bl = this.$check_action('/order_information/table','set');
					console.log(bl ? "你有表格添加权限视作有修改权限" : "你没有表格修改权限");
				}
				if(!bl){
					bl = this.$check_action('/order_information/view','add');
					console.log(bl ? "你有视图添加权限视作有添加权限" : "你没有视图添加权限");
				}
				if(!bl){
					bl = this.$check_action('/order_information/view','set');
					console.log(bl ? "你有视图修改权限视作有修改权限" : "你没有视图修改权限");
				}
				if(!bl){
					bl = this.$check_action('/order_information/view','get');
					console.log(bl ? "你有视图查询权限视作有查询权限" : "你没有视图查询权限");
				}

				console.log(bl ? "具有当前页面的查看权，请注意这不代表你有字段的查看权" : "无权查看当前页，请注意即便有字段查询权限没有页面查询权限也不行");

				return bl;
			},
			/**
			 * 上传文件
			 * @param {Object} param
			 */
			uploadimg(param) {
				this.uploadFile(param.file, "avatar");
			},

		},
		created() {
			this.get_list_user_purchase_user();
			this.get_group_user_purchase_user();
		}
	}
</script>

<style>
	.avatar-uploader .el-upload {
		border: 1px dashed #d9d9d9;
		border-radius: 6px;
		cursor: pointer;
		position: relative;
		overflow: hidden;
	}

	.avatar-uploader .el-upload:hover {
		border-color: #409EFF;
	}

	.avatar-uploader-icon {
		font-size: 28px;
		color: #8c939d;
		width: 178px;
		height: 178px;
		line-height: 178px;
		text-align: center;
	}

	.avatar {
		width: 178px;
		height: 178px;
		display: block;
	}
</style>
