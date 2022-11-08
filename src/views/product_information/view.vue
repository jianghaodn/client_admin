<template>
	<el-main class="bg">
		<el-form ref="form" :model="form" status-icon label-width="120px" v-if="is_view()">
			<el-col v-if="user_group === '管理员' || $check_field('get','trade_name') || $check_field('add','trade_name') || $check_field('set','trade_name')" :xs="24" :sm="12" :lg="8">
				<el-form-item label="商品名称" prop="trade_name">
					<el-input id="trade_name" v-model="form['trade_name']" placeholder="请输入商品名称"
							  v-if="user_group === '管理员' || (form['product_information_id'] && $check_field('set','trade_name')) || (!form['product_information_id'] && $check_field('add','trade_name'))" :disabled="disabledObj['trade_name_isDisabled']"></el-input>
					<div v-else-if="$check_field('get','trade_name')">{{form['trade_name']}}</div>
				</el-form-item>
			</el-col>
			<el-col v-if="user_group === '管理员' || $check_field('get','product_cover') || $check_field('add','product_cover') || $check_field('set','product_cover')" :xs="24" :sm="12" :lg="8">
				<el-form-item label="商品封面" prop="product_cover">
					<el-upload :disabled="disabledObj['product_cover_isDisabled']" id="product_cover" class="avatar-uploader" drag
						accept="image/gif, image/jpeg, image/png, image/jpg" action="" :http-request="upload_product_cover"
						:show-file-list="false" v-if="user_group === '管理员' || (form['product_information_id'] && $check_field('set','product_cover')) || (!form['product_information_id'] && $check_field('add','product_cover'))">
						<img v-if="form['product_cover']" :src="$fullUrl(form['product_cover'])" class="avatar">
						<i v-else class="el-icon-plus avatar-uploader-icon"></i>
					</el-upload>
					<el-image v-else-if="$check_field('get','product_cover')" style="width: 100px; height: 100px"
						:src="$fullUrl(form['product_cover'])" :preview-src-list="[$fullUrl(form['product_cover'])]">
						<div slot="error" class="image-slot">
							<img src="../../../public/img/error.png" style="width: 90px; height: 90px" />
						</div>
					</el-image>
				</el-form-item>
			</el-col>
			<el-col v-if="user_group === '管理员' || $check_field('get','commodity_price_') || $check_field('add','commodity_price_') || $check_field('set','commodity_price_')" :xs="24" :sm="12" :lg="8">
				<el-form-item label="商品价钱" prop="commodity_price_">
					<el-input id="commodity_price_" v-model="form['commodity_price_']" placeholder="请输入商品价钱"
							  v-if="user_group === '管理员' || (form['product_information_id'] && $check_field('set','commodity_price_')) || (!form['product_information_id'] && $check_field('add','commodity_price_'))" :disabled="disabledObj['commodity_price__isDisabled']"></el-input>
					<div v-else-if="$check_field('get','commodity_price_')">{{form['commodity_price_']}}</div>
				</el-form-item>
			</el-col>
			<el-col v-if="user_group === '管理员' || $check_field('get','merchandise_inventory') || $check_field('add','merchandise_inventory') || $check_field('set','merchandise_inventory')" :xs="24" :sm="12" :lg="8">
				<el-form-item label="商品库存" prop="merchandise_inventory">
					<el-input id="merchandise_inventory" v-model="form['merchandise_inventory']" placeholder="请输入商品库存"
							  v-if="user_group === '管理员' || (form['product_information_id'] && $check_field('set','merchandise_inventory')) || (!form['product_information_id'] && $check_field('add','merchandise_inventory'))" :disabled="disabledObj['merchandise_inventory_isDisabled']"></el-input>
					<div v-else-if="$check_field('get','merchandise_inventory')">{{form['merchandise_inventory']}}</div>
				</el-form-item>
			</el-col>
			<el-col v-if="user_group === '管理员' || $check_field('get','commodity_type') || $check_field('add','commodity_type') || $check_field('set','commodity_type')" :xs="24" :sm="12" :lg="8">
				<el-form-item label="商品类型" prop="commodity_type">
					<el-select id="commodity_type" v-model="form['commodity_type']"
						v-if="user_group === '管理员' || (form['product_information_id'] && $check_field('set','commodity_type')) || (!form['product_information_id'] && $check_field('add','commodity_type'))">
						<el-option v-for="o in list_commodity_type" :key="o['commodity_type']" :label="o['commodity_type']"
							:value="o['commodity_type']">
						</el-option>
					</el-select>
					<div v-else-if="$check_field('get','commodity_type')">{{form['commodity_type']}}</div>
				</el-form-item>
			</el-col>
			<el-col v-if="user_group === '管理员' || $check_field('get','product_description') || $check_field('add','product_description') || $check_field('set','product_description')" :xs="24" :sm="12" :lg="8">
				<el-form-item label="商品描述" prop="product_description">
					<el-input type="textarea" id="product_description" v-model="form['product_description']" placeholder="请输入商品描述"
						v-if="user_group === '管理员' || (form['product_information_id'] && $check_field('set','product_description')) || (!form['product_information_id'] && $check_field('add','product_description'))" :disabled="disabledObj['product_description_isDisabled']"></el-input>
					<div v-else-if="$check_field('get','product_description')">{{form['product_description']}}</div>
				</el-form-item>
			</el-col>
			<el-col v-if="user_group === '管理员' || $check_field('get','product_introduction') || $check_field('add','product_introduction') || $check_field('set','product_introduction')" :xs="24" :sm="24" :lg="24">
				<el-form-item label="商品介绍" prop="product_introduction">
					<quill-editor v-model.number="form['product_introduction']"
						v-if="user_group === '管理员' || (form['product_information_id'] && $check_field('set','product_introduction')) || (!form['product_information_id'] && $check_field('add','product_introduction')) ">
					</quill-editor>
					<div v-else-if="$check_field('get','product_introduction')" v-html="form['product_introduction']"></div>
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
				field: "product_information_id",
				url_add: "~/api/product_information/add?",
				url_set: "~/api/product_information/set?",
				url_get_obj: "~/api/product_information/get_obj?",
				url_upload: "~/api/product_information/upload?",

				query: {
					"product_information_id": 0,
				},

				form: {
					"trade_name":'', // 商品名称
					"product_cover":'', // 商品封面
					"commodity_price_":'', // 商品价钱
					"merchandise_inventory":'', // 商品库存
					"commodity_type":'', // 商品类型
					"product_description":'', // 商品描述
					"product_introduction":'', // 商品介绍
					"product_information_id": 0, // ID

				},
				disabledObj:{
					"trade_name_isDisabled": false,
					"product_cover_isDisabled": false,
					"commodity_price__isDisabled": false,
					"merchandise_inventory_isDisabled": false,
					"commodity_type_isDisabled": false,
					"product_description_isDisabled": false,
					"product_introduction_isDisabled": false,
				},
				//商品类型选项列表
				list_commodity_type: [],

			}
		},
		methods: {
			/**
			 * 上传商品封面
			 * @param {Object} param图片参数
			 */
			upload_product_cover(param){
				this.uploadFile(param.file, "product_cover");
			},
			/**
			 * 获取商品类型列表
			 */
			async get_list_commodity_type() {
				var json = await this.$get("~/api/commodity_category/get_list?");
				if(json.result && json.result.list){
					this.list_commodity_type = json.result.list;
				}
				else if(json.error){
					console.error(json.error);
				}
			},

			/**
			 * 获取对象之前
			 * @param {Object} param
			 */
			get_obj_before(param) {
				var form = "";
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
					bl = this.$check_action('/product_information/table','add');
					console.log(bl ? "你有表格添加权限视作有添加权限" : "你没有表格添加权限");
				}
				if(!bl){
					bl = this.$check_action('/product_information/table','set');
					console.log(bl ? "你有表格添加权限视作有修改权限" : "你没有表格修改权限");
				}
				if(!bl){
					bl = this.$check_action('/product_information/view','add');
					console.log(bl ? "你有视图添加权限视作有添加权限" : "你没有视图添加权限");
				}
				if(!bl){
					bl = this.$check_action('/product_information/view','set');
					console.log(bl ? "你有视图修改权限视作有修改权限" : "你没有视图修改权限");
				}
				if(!bl){
					bl = this.$check_action('/product_information/view','get');
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
			this.get_list_commodity_type();
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
