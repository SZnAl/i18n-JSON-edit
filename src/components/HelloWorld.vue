<template>
	<div class="hello">
		<span>选择翻译的目标语言：</span
		><el-select v-model="languageVlue" placeholder="选择语言">
			<el-option
				v-for="item in languageList"
				:key="item.value"
				:label="item.label"
				:value="item.value"
			>
			</el-option>
		</el-select>
		<div class="content" v-if="languageVlue != undefined">
			<div v-if="obj === undefined">
				<el-button type="primary" @click="importText">导入TXT文件</el-button>
				<div class="tips">*请先导入TXT文件后进行翻译</div>
			</div>
			<div class="fixed">
				<h3>字段名 - 翻译后的内容</h3>
				<div v-show="obj != undefined">
					<div class="tips">翻译完成或未完成需要保存请点击：</div>
					<el-button type="success" @click="exportText">导出TXT文件</el-button>
				</div>
			</div>

			<div class="topBtnBox">
				<el-tooltip
					class="item"
					effect="dark"
					content="点击回到页面顶部"
					placement="top"
				>
					<el-button
						class="iconBtn"
						type="success"
						icon="el-icon-top"
						@click="clickTop"
						circle
					></el-button>
				</el-tooltip>
				<el-tooltip
					class="item"
					effect="dark"
					content="点击显示帮助信息"
					placement="top"
				>
					<el-button
						class="iconBtn"
						icon="el-icon-info"
						circle
						@click="dialogVisible = true"
					></el-button>
				</el-tooltip>
			</div>

			<div
				v-for="(value1, key1, index1) in this.obj"
				:key="index1"
				class="flex"
				:id="'area' + index1"
			>
				<h2>所属板块：{{ key1 }}</h2>

				<div
					v-for="(value2, key2, index2) in value1"
					:key="index2"
					:class="index2 % 2 == 0 ? 'green' : 'pink'"
					style="display: flex"
				>
					<div class="left" style="min-width: 220px">
						<span>{{ key2 }}:</span>
					</div>
					<div class="right">
						<input style="min-width: 250px" v-model="obj[key1][key2]" />
					</div>
				</div>
			</div>
		</div>

		<!-- 点击跳转到指定模块 -->
		<div class="elevator">
			<div class="elevatorTitle">
				<el-tooltip
					class="item"
					effect="dark"
					content="点击下方板块名，快速跳转到对应板块"
					placement="left"
				>
					<div>快速跳转对应板块</div>
				</el-tooltip>
			</div>
			<div v-for="(value1, key1, index1) in this.obj" :key="index1">
				<div class="elevatorItem">
					<a :href="'#area' + index1">{{ index1 + 1 + ' : ' + key1 }}</a>
				</div>
			</div>
		</div>

		<el-dialog title="使用提示" :visible.sync="dialogVisible" width="40%">
			<div class="tipsText">
				1.选择要翻译的语言后，<span class="tips">导入.txt文件</span>
			</div>
			<div class="tipsText">
				2.右侧快速跳转区域为：<span class="tips"
					>点击需要翻译的板块后快速跳转到对应板块</span
				>
			</div>
			<div class="tipsText">
				3.底部绿色圆形向上箭头状按钮为<span class="tips"
					>[快速跳转到页面顶部]</span
				>
			</div>
			<div class="tipsText">
				4.翻译完成后一定要点击<span class="tips">[导出TXT文件]</span
				>按钮，将翻译后的文件进行导出
			</div>

			<span slot="footer" class="dialog-footer">
				<el-button type="primary" @click="dialogVisible = false"
					>确 定</el-button
				>
			</span>
		</el-dialog>
	</div>
</template>

<script>
export default {
	name: 'HelloWorld',
	props: {},
	data() {
		return {
			dialogVisible: true,
			obj: undefined,
			input: ['', '', ''],
			languageVlue: undefined,
			languageList: [
				{ label: '英文', value: 'en_US' },
				{ label: '日文', value: 'ja_JP' },
				{ label: '中文繁体', value: 'zh_HK' },
				{ label: '德语', value: 'de_DE' },
			],
		};
	},
	mounted() {
		// this.obj = {
		// 	// 系统公共
		// 	system: {
		// 		appTitle: 'SuperHii Cloud Platform',
		// 		title: 'SuperHii Cloud Platform',
		// 		allMarket: 'All',
		// 		logo_text: 'SuperHii Cloud Platform',
		// 		loading_simple: 'Loading，please wait....',
		// 		loading: 'Loading，please wait....',
		// 		logout: 'Confirm to log out ?',
		// 		invalid_session: 'Invalid session，please login again.',
		// 		login_invalid: 'Fail to login or login timeout. Please try again.',
		// 		tips_repeat_submit: 'Do not resubmit',
		// 		success: 'Success',
		// 		fail: 'Fail',
		// 		signOut: 'Signout',
		// 		tips: 'Tips',
		// 		confirm: 'Confirm',
		// 		cancel: 'Cancel',
		// 		home: 'Home',
		// 		search: '查询',
		// 		reset: 'Reset',
		// 		more: 'More',
		// 		more_filter: 'MoreFilter',
		// 		retract: 'Retract',
		// 		operate: 'Operate',
		// 		add: 'Add',
		// 		edit: 'Edit',
		// 		remove: 'Remove',
		// 		disable: 'Disable',
		// 		enable: 'Enable',
		// 		details: 'Details',
		// 		import: 'Import',
		// 		export: 'Export',
		// 		clean: 'Clean',
		// 		close: 'Close',
		// 		refresh_cache: 'RefreshCache',
		// 		tree_operate: 'Open/Fold',
		// 		tree_select: 'Select All/Not At All',
		// 		tree_linkage: 'Cascade',
		// 		batch_import: 'Batch Import',
		// 		batch_enable: 'Batch Enable',
		// 		all_export: 'Batch Export',
		// 		click_upload: 'Upload',
		// 		tips_file_select:
		// 			'Current limit selection {limitCount} file，now selection {fileSelectCount} file，total selection {fileTotalCount} file',
		// 		tips_file_remove: 'Sure to remove {fileName} ?',
		// 		tips_file_format:
		// 			'The file format is incorrect, please upload {0} type file!',
		// 		tips_file_size: 'Upload file cannot exceed {0} MB!',
		// 		download_template: 'Download template',
		// 		system_tips: 'System prompt',
		// 		refreshSelectedTag: 'Refresh',
		// 		closeSelectedTag: 'Close',
		// 		closeOthersTags: 'CloseOthers',
		// 		closeLeftTags: 'CloseLeft',
		// 		closeRightTags: 'CloseRight',
		// 		closeAllTags: 'CloseAll',
		// 		changePassword: 'ChangePassword',
		// 		switchLanguage: 'SwitchLanguage',
		// 		switchToChinese: '中文',
		// 		switchToEnglish: 'English',
		// 		login_again: 'Login again',
		// 		networkError: 'Network Error',
		// 		timeout: 'Timeout',
		// 		serviceError: 'Service{errCode}',
		// 		downloading_data: 'Downloading，please wait...',
		// 		download_file_error: 'Download Error！',
		// 		import_template: 'Import Template',
		// 		loading_upload_file: 'Uploading，please wait...',
		// 		upload_image_size: 'Upload only {0} files',
		// 		upload_fail_tip: 'Upload fail，please try again',
		// 		processing: 'Processing，please wait...',
		// 		tips_file_xls: 'Only xls, xlsx format files are allowed to be imported',
		// 		tips_file_sel: 'Drag files here，or <em>click upload</em>',
		// 		index: 'Home',
		// 		tips_file_width_height: 'Upload pictures is limited to {0} * {1} PX!',
		// 		switch_lang: 'Succeeded！',
		// 		tips_image: '请上传大小不超过{0}MB, 尺寸为{1}*{2}PX的图片文件',
		// 		insertImageFail: '图片插入失败',
		// 		selectAll: 'Select All',
		// 		print: '打印',
		// 		save: '保存',
		// 		refresh: '刷新',
		// 		open: '开',
		// 		shut: '关',
		// 		noData: '暂无数据',
		// 	},
		// 	// 登录页
		// 	login: {
		// 		title: 'Sign In',
		// 		slider: 'Please slider to rightmost',
		// 		slider_success: 'Successful',
		// 		signIn: 'Sign In',
		// 		logging: 'Logging',
		// 		captcha: 'Validation Code',
		// 		username: 'Username',
		// 		passwd: 'Password',
		// 	},
		// 	// 菜单搜索组件
		// 	menuSearch: {
		// 		menuSearch: 'Menu Navigation',
		// 		pleaseInput: '请输入菜单名关键字进行搜索,点击后跳转对应页面',
		// 	},
		// 	placeholder: {
		// 		startTime: 'StartTime',
		// 		endTime: 'EndTime',
		// 		select: 'Please Select',
		// 		input: 'Please Input',
		// 		textarea: 'Text Example',
		// 		sys_gen: '系统生成',
		// 		startDate: '开始日期',
		// 		endDate: '结束日期',
		// 	},
		// 	validate: {
		// 		username: 'Please enter user name',
		// 		passwd: 'Please enter password',
		// 		captcha: 'Please enter verification Code',
		// 		sliding: 'Please slide to verify',
		// 		required: 'Cannot be empty',
		// 		exists: 'Already exists',
		// 		positive_number: 'Please enter positive integer',
		// 		positive_zero_number: 'Please enter nonnegative integer',
		// 		number_range: 'Please enter {0}to{1} positive integer',
		// 		decimal: 'Please enter {0} decimal places',
		// 		decimal_range: 'Required{0}to{1}  {1} digits decimal',
		// 		number_capital: 'Please enter  integer or letter combination',
		// 		maxlength: 'Can not exceed {0} characters',
		// 		minlength: 'Can not less than {0} characters',
		// 		range_length: 'Enter  length of {0} to {1} character',
		// 		zh_letter: 'Can only enter English or Chinese',
		// 		zh_letter_number: 'Can only enter English or Chinese or numbers',
		// 		number_abc: 'Only English or numbers can be entered',
		// 		number_abc_sup: 'Only enter English, numbers and special characters',
		// 		code_exist: 'code already exists',
		// 		number_bigger: '只能输入大于{0}的数字',
		// 		chinese_error: '密码不能输入中文或空格',
		// 		letter_line: '只能输入大小写字母以及下划线',
		// 		check_complete: '请输入完整信息',
		// 	},
		// 	// 公共业务
		// 	common: {
		// 		orgCode: 'Market',
		// 		marketCode: 'Store',
		// 		startTime: 'StartTime',
		// 		endTime: 'EndTime',
		// 		createdBy: 'Created By',
		// 		createdTime: 'Created Time',
		// 		updatedBy: 'Updated By',
		// 		updatedTime: 'Updated Time',
		// 		enableStatus: 'Status',
		// 		changeBy: 'ChangeBy',
		// 		updatedRecord: 'ChangeRecord',
		// 		changeContent: 'ChangeContent',
		// 		fieldName: 'Field name',
		// 		changeBefore: 'Before',
		// 		changeAfter: 'After',
		// 		look: 'Details',
		// 		remarks: 'Remarks',
		// 		orderNum: 'Order',
		// 		yes: 'Yes',
		// 		no: 'No',
		// 		langKey: 'LangKey',
		// 		changeTime: 'ChangeTime',
		// 		monday: 'Mon.',
		// 		tuesday: 'Tues.',
		// 		wednesday: 'Wed.',
		// 		thursday: 'Thur.',
		// 		friday: 'Fri.',
		// 		saturday: 'Sat.',
		// 		sunday: 'Sun.',
		// 		success: 'Success',
		// 		fail: 'Fail',
		// 		allMarketTips: '当前图例不支持多门店查看',
		// 	},
		// 	// 车辆管理
		// 	shoppingCart: {
		// 		businessName: 'CarName',
		// 		deviceNum: 'Num',
		// 		authCode: 'WifiMac',
		// 		inputAuthCode: '请输入WifiMac',
		// 		cartSn: 'Car SN',
		// 		createdBy: '创建人',
		// 		deviceModel: 'Car Model',
		// 		ipaidType: 'Pad Type',
		// 		mainBoardModel: '主板型号',
		// 		appVersion: 'AppVersion',
		// 		iotVersion: 'IOTVersion',
		// 		mcuVersion: 'MCUVersion',
		// 		useScene: 'Usage Scenario',
		// 		rfId: 'RFID',
		// 		bluetoothMac: 'BluetoothMac',
		// 		batterySn: 'Battery Sn',
		// 		batteryModel: 'Battery Model',
		// 		batteryType: 'Battery Type',
		// 		ratedCapacity: 'RatedCapacity',
		// 		standardVoltage: 'StandardVoltage',
		// 		cartWarrantyTime: 'CartWarranty',
		// 		batteryWarrantyTime: 'BatteryWarranty',
		// 		batteryProductionDate: '电池出厂时间',
		// 		month: 'Month',
		// 		remarks: 'Remarks',
		// 		startTime: 'StartTime',
		// 		endTime: 'EndTime',
		// 		device_file: 'Car_file',
		// 		timingSwitchTime: 'Timing switch time',
		// 		isEnableTimingSwitch: 'Timing switch',
		// 		device_createdTime: 'CreatedTime',
		// 		device_enableTime: 'EnableTime',
		// 		systemVersion: '安卓版本',
		// 		moduleNumber: 'ModuleNumber',
		// 		kernelVersion: 'KernelVersion',
		// 		changeType: 'ChangeType',
		// 		changeTime: 'ChangeTime',
		// 		batteryLevel: 'BatteryLevel',
		// 		tips_enable_device: 'Are you sure you will activate it? ',
		// 		tips_disable_device: 'Are you sure you will deactivate it？',
		// 		tips_import_device:
		// 			'Tip: only "xls" or "xlsx" format files can be imported, and only shopping cart file can be imported',
		// 		tips_HanShuo: '上报汉朔失败',
		// 		tips_import_file: 'Please upload the file',
		// 		tips_batch_enable: 'Cart enable',
		// 		tips_deviceNum_has: 'Num is exists',
		// 		erpDeviceNum: '款台号',
		// 		allocation: '调拨',
		// 		cartAllocation: '车辆调拨',
		// 		allocationOrg: '调拨超市',
		// 		allocationMarket: '调拨门店',
		// 		allocationSuccess: '调拨成功',
		// 		allocationFaild: '调拨失败',
		// 		allocationFaildExist: '调拨失败-车辆已存在',
		// 		allocationFaildHS: '上报购物车信息到汉朔失败',
		// 		marketCodeCheck: '相同门店的车辆不能进行调拨',
		// 		exchangeOrgCodeTips: '所属超市不能为空',
		// 		exchangeMarketCodeTips: '所属门店不能为空',
		// 		exchangeCartNumTips: '车辆编号不能为空',
		// 		exchangeEnabledTips: '请先禁用该车辆再进行调拨',
		// 	},
		// 	// 商品管理
		// 	goods: {
		// 		businessName: 'GoodsName',
		// 		goodsType: 'GoodsType',
		// 		hasWeight: 'IsCheck',
		// 		hasBarCode: 'HasBarCode',
		// 		isSpecial: 'IsSpecial',
		// 		goodsCode: 'BarCode',
		// 		calcPrice: 'Pomotion Setting',
		// 		goodsName: 'GoodsName',
		// 		goodsPrice: 'Original Price',
		// 		promotions: 'Promotions',
		// 		imageUrl: 'GoodsImage',
		// 		promotionType: 'Promotion Type',
		// 		discountType: 'Discount Type',
		// 		discountCount: 'Discount Count',
		// 		discountAmount: 'Discount Amount',
		// 		promotionDateRang: 'Promotion Date',
		// 		fileType: 'File Type',
		// 		excelFile: 'Goods File',
		// 		zipFile: 'Image File',
		// 		marketTips: '请先选择所属门店',
		// 		import_excel_tip:
		// 			'Tip: only "xls" or "xlsx" format files can be imported, and only goods file can be imported,Up to 10000 pieces of data can be imported at one time',
		// 		import_excel_Num_tip:
		// 			'Tip:最多一次可导入10000条数据，当前商品文件数据条数为：',
		// 		import_Num_tip:
		// 			'提示：最多一次可导入10000条数据，当前商品文件数据条数已超限制。',
		// 		import_zip_tip:
		// 			'Tip: only "zip" format files can be imported, and the picture name must be named after the commodity barcode',
		// 		upload_amount_limit_tip:
		// 			'提示：单次仅能上传1个文件,且最多一次可导入10000条数据',
		// 		enterWeight: 'Enter Weight',
		// 		enterWeightTitle: 'EnterWeight Record',
		// 		learnWeight: 'LearnWeight',
		// 		learnWeightTitle: 'LearnWeight Record',
		// 		standardWeight: '标准重量',
		// 		saleType: '特殊类型',
		// 		checkWeight: '是否鉴重',
		// 		goodsCategory: '商品品类',
		// 		isLightWeight: '是否轻质',
		// 		allowLearn: '允许学习',
		// 		delGoodsTips: '确认删除该商品？',
		// 	},
		// 	agent: {
		// 		businessName: 'Agent',
		// 		agentName: 'Agent Name',
		// 		agentCode: 'Agent',
		// 	},
		// 	org: {
		// 		businessName: 'Supermarket',
		// 		orgName: 'Market Name',
		// 		orgCode: 'Market Code',
		// 		agentCode: 'Agent',
		// 	},
		// 	market: {
		// 		businessName: 'Store',
		// 		marketCode: 'Store Code',
		// 		marketName: 'Store Name',
		// 		marketType: 'Store Type',
		// 		erpMarketCode: 'ERP MarketCode',
		// 		erpStoreCode: 'ERP StoreCode',
		// 		marketCountry: 'Country',
		// 		serviceChannel: '服务渠道',
		// 		marketTimeZone: 'TimeZone',
		// 		marketLogoUrl: 'Store Logo',
		// 		comment: 'Remark',
		// 		amountConvertRate: '金额换算 数据库金额 ➗',
		// 		salary: '人销比',
		// 	},
		// 	customer: {
		// 		startLoginTime: 'Start Time',
		// 		endLoginTime: 'End Time',
		// 		CreateTime: '创建时间',
		// 		startCreateTime: 'Start Time',
		// 		endCreateTime: 'End Time',
		// 		customerCode: '用户ID',
		// 		customerType: '用户身份',
		// 		isVip: 'IsVip',
		// 		nickName: '用户名称',
		// 		phone: 'Phone',
		// 		vipNo: 'VipNo',
		// 		sex: 'Gender',
		// 		man: '男',
		// 		women: '女',
		// 		age: 'Age',
		// 		lastLoginTime: 'Login Time',
		// 		lastLogoutTime: 'Logout Time',
		// 	},
		// 	device: {
		// 		deviceNum: '设备编号',
		// 		deviceType: '设备类型',
		// 		taskDevice: '任务设备',
		// 		wifiMac: 'wifiMac',
		// 		authCode: 'wifiMac',
		// 	},
		// 	// 日志模块 任务、信息
		// 	logs: {
		// 		taskId: '任务ID',
		// 		uploadStatus: '上传状态',
		// 		download: '下载',
		// 		invalid: '失效',
		// 		commandType: '命令类型',
		// 		deviceNum: '设备编号',
		// 		commandValue: '命令值',
		// 		createdTime: '创建时间',
		// 		expirationDate: '过期时间',
		// 		logsDate: '日志日期',
		// 		status: '执行状态',
		// 		progress: '返回值',
		// 		invalidTips: '确认将此条任务失效吗?',
		// 		checkUniqueiotTips: '该设备任务已存在',
		// 		addToListSuccess: '操作成功，设备已添加至列表',
		// 		addToListFaild: '操作失败，设备未添加至列表！',
		// 		logsDateTime: '日志时间',
		// 		logsUploadScene: '上传时间',
		// 		startTime: '日志开始时间',
		// 		endTime: '日志结束时间',
		// 	},
		// 	//用户管理
		// 	user: {
		// 		nickName: 'NickName',
		// 		workNo: '工号',
		// 		status: 'Status',
		// 		dataScopeName: 'Belongs to the client',
		// 		userName: 'Account',
		// 		password: 'Password',
		// 		resetPwd: 'ResetPwd',
		// 		role: 'Role',
		// 		mail: '用户邮箱',
		// 		mailTips: '请输入正确的邮箱地址',
		// 		tips_update_exists: 'Update existing user data?',
		// 		tips_enable_or_disable: 'Confirm for {0}"{1}"client？',
		// 		tips_select_market_invalid: 'Please select a valid store',
		// 		tips_input_new_pass: 'Please enter new password {0}',
		// 		tips_update_pass_success: 'Update success，new password：{0}',
		// 		phoneNumber: '手机号',
		// 	},
		// 	//角色管理
		// 	role: {
		// 		roleId: 'Role Id',
		// 		roleName: 'Role Name',
		// 		status: 'Status',
		// 		roleKey: 'Role Key',
		// 		dataScope: 'Data Scope',
		// 		addUser: 'Assign users',
		// 		addDataPermissions: 'Assign data permissions',
		// 		menuPermissions: 'Menu permissions',
		// 		dataPermissions: 'Data permissions',
		// 		removePermissions: 'Cancel authorization',
		// 		selectUser: 'Select User',
		// 		batchRemovePermissions: 'Cancel Authorization in Batches',
		// 		tips_enable_disable: 'Confirm for{0}"{1}"user？',
		// 		tips_delete: 'Confirm to delete user ID"{0}"data？',
		// 		tips_select_user: 'Please select the user to assign',
		// 		tips_remove_user_auth: 'Confirm to cencel user ID"{0}"role？',
		// 		tips_batch_remove_permissions:
		// 			'Confirm to deselect user authorization data？',
		// 		add_auth: 'Authorization',
		// 	},
		// 	//菜单管理
		// 	menu: {
		// 		homeDirectory: 'MainCatalogue',
		// 		menuName: 'MenuName',
		// 		status: 'Status',
		// 		tips_status:
		// 			'Select Disable and the route will not appear in the sidebar and cannot be accessed',
		// 		typeTitle: 'TypeTitle',
		// 		parentMenu: 'ParentMenu',
		// 		menuType: 'MenuType',
		// 		menuType_M: 'Catalogue',
		// 		menuType_C: 'Menu',
		// 		menuType_F: 'Button',
		// 		icon: 'Icon',
		// 		isFrame: 'IsFrame',
		// 		tips_isFrame:
		// 			'If yes For the external chain, the routing address needs to start with HTTP(s)://',
		// 		path: 'Path',
		// 		tips_path:
		// 			'Access route address，Examples：`user`，If the external network address needs internal chain access Start with HTTP(s)://',
		// 		component: 'Component',
		// 		tips_component:
		// 			'Component path accessed，Examples：`system/user/index`， default is in the views directory',
		// 		perms: 'Perms',
		// 		tips_perms:
		// 			'Authority characters defined in the controller，Examples：@PreAuthorize(`@ss.hasPermi("system:user:list")`)',
		// 		query: 'Query',
		// 		tips_query:
		// 			'Default delivery parameters for access routes，Examples：`{"id": 1, "name": "ry"}`',
		// 		isCache: 'IsCache',
		// 		tips_isCache:
		// 			'Is selected, it will be cached by keep alive，The name and address of the matching component should be consistent',
		// 		cache: 'Cache',
		// 		noCache: 'NoCache',
		// 		visible: 'Visible',
		// 		tips_visible:
		// 			'Select hide, the route will not appear in the sidebar, but can still be accessed',
		// 		tips_remove: 'Sure to delete the name{0} ？',
		// 	},
		// 	//字典管理
		// 	dict: {
		// 		dictDataTitle: 'DictData',
		// 		dictId: 'DictId',
		// 		dictName: 'DictName',
		// 		dictCode: 'DictCode',
		// 		dictLabel: 'DictLabel',
		// 		dictValue: 'DictValue',
		// 		dictSort: 'DictSort',
		// 		dictType: 'DictType',
		// 		cssClass: 'Style properties',
		// 		listClass: 'Echo style',
		// 		status: 'Status',
		// 		class_default: 'Default',
		// 		class_primary: 'Primary',
		// 		class_success: 'Success',
		// 		class_info: 'Info',
		// 		class_warning: 'Warning',
		// 		class_danger: 'Danger',
		// 		tips_remove: 'Sure to delete dictionary number "{0}"？',
		// 	},
		// 	//系统配置管理
		// 	config: {
		// 		configName: 'ConfigName',
		// 		configKey: 'ConfigKey',
		// 		configType: 'ConfigType',
		// 		configValue: 'ConfigType',
		// 		tips_remove: 'Sure to delete parameter number "{0}"？',
		// 	},
		// 	//公告管理
		// 	notice: {
		// 		noticeId: 'NoticeId',
		// 		noticeTitle: 'NoticeTitle',
		// 		noticeType: 'NoticeType',
		// 		status: 'NoticeType',
		// 		noticeContent: 'Content',
		// 		tips_remove: 'Sure to delete the announcement number "{0}" ？',
		// 	},
		// 	//登录日志管理
		// 	logininfor: {
		// 		ipaddr: 'Content',
		// 		userName: 'UserName',
		// 		status: 'Status',
		// 		infoId: 'InfoId',
		// 		loginLocation: 'LoginLocation',
		// 		browser: 'Browser',
		// 		os: 'OS',
		// 		msg: 'Message',
		// 		loginTime: 'LoginTime',
		// 		tips_remove: 'Sure to delete the access number "{0}" ？',
		// 		tips_remove_all: 'Sure to clear all login log data items ？',
		// 	},
		// 	//操作日志管理
		// 	operlog: {
		// 		operId: 'OperId',
		// 		title: 'Title',
		// 		businessType: 'OperType',
		// 		requestMethod: 'RequestMethod',
		// 		operName: 'OperName',
		// 		operIp: 'OperIp',
		// 		operLocation: 'OperLocation',
		// 		status: 'Status',
		// 		operTime: 'OperTime',
		// 		operInfo: 'OperInfo',
		// 		method: 'Method',
		// 		operParam: 'Method',
		// 		jsonResult: 'Method',
		// 		errorMsg: 'ErrorMsg',
		// 		tips_remove: 'Sure you want to delete log No "{0}" ？',
		// 		tips_remove_all: 'Sure to clear all operation log data items ？',
		// 	},
		// 	//在线用户
		// 	online: {
		// 		index: 'Order',
		// 		tokenId: 'TokenId',
		// 		userName: 'UserName',
		// 		deptName: 'DeptName',
		// 		ipaddr: 'Ipaddr',
		// 		loginLocation: 'LoginLocation',
		// 		browser: 'Browser',
		// 		os: 'OS',
		// 		loginTime: 'LoginTime',
		// 		logout: 'Logout',
		// 		tips_logout: 'Sure that the forced logout name is "{0}"？',
		// 	},
		// 	//定时任务
		// 	job: {
		// 		jobId: 'TaskId',
		// 		jobName: 'TaskName',
		// 		jobGroup: 'TaskGroup',
		// 		invokeTarget: 'Scheduling method',
		// 		tips_bean: 'Bean Examples.：ryTask.ryParams("ry")',
		// 		tips_class:
		// 			'Class Examples.：com.ruoyi.quartz.task.RyTask.ryParams("ry")',
		// 		tips_params:
		// 			'Parameter description：support String，boolean，long，double，int',
		// 		cronExpression: 'Cron expression',
		// 		status: 'Status',
		// 		genCronTool: 'Cron Tool',
		// 		genCron: 'Generate expression',
		// 		nextValidTime: 'Next execution time',
		// 		concurrent: 'Concurrent',
		// 		misfirePolicy: 'MisfirePolicy',
		// 		log: 'Log',
		// 		jobMessage: 'LogMessage',
		// 		jobLogId: 'JobLogId',
		// 		exec_default: 'Exec_default',
		// 		exec_promptly: 'Exec_promptly',
		// 		exec_once: 'Exec_once',
		// 		exec_quit: 'Exec_quit',
		// 		execTime: 'ExecTime',
		// 		allow: 'Allow',
		// 		forbid: 'Forbid',
		// 		normal: 'Normal',
		// 		fail: 'Fail',
		// 		exceptionInfo: 'ExceptionInfo',
		// 		tips_enable_disable: 'Sure to "{0}""{1}" task？',
		// 		tips_remove: 'Sure to delete the scheduled task"{0}" ？',
		// 		tips_exec_once:
		// 			'Confirm that you want to execute this task it immediately "{0}" ？',
		// 		tips_remove_log_all: 'Sure to clear all dispatch log data items？',
		// 		tips_remove_log: 'Confirm to delete the dispatch log number"{0}" ？',
		// 	},
		// 	cache: {
		// 		loadding: 'Loading，please wait...！',
		// 		baseInfo: 'BaseInfo',
		// 		redis_version: 'RedisVersion',
		// 		redis_mode: 'RedisMode',
		// 		tcp_port: 'Port',
		// 		connected_clients: 'Clients',
		// 		uptime_in_days: 'Uptime(day)',
		// 		used_memory_human: 'Used Memory',
		// 		used_cpu_user_children: 'Used CPU',
		// 		maxmemory_human: 'ROM',
		// 		aof_enabled: 'AOF Enabled',
		// 		rdb_last_bgsave_status: 'RDB IsSuccess',
		// 		dbSize: 'Key Quantity',
		// 		instantaneous_input_kbps: 'Input/Export',
		// 		commandstats: 'Command statistics',
		// 		usedmemory: 'MemoryInfo',
		// 		command: 'Command',
		// 		used_memory_peak: 'Peak value',
		// 		stand_alone: 'Stand-alone',
		// 		colony: 'Colony',
		// 	},
		// 	server: {
		// 		cpu: 'CPU',
		// 		cpu_field: 'cpu_field',
		// 		cpu_value: 'cpu_value',
		// 		cpu_cpuNum: 'cpu_Num',
		// 		cpu_used: 'cpu_used',
		// 		cpu_sys: 'cpu_sys',
		// 		cpu_free: 'cpu_free',
		// 		mem: 'memory',
		// 		mem_field: 'cpu_field',
		// 		mem_sys: 'cpu_ROM',
		// 		mem_jvm: 'cpu_JVM',
		// 		mem_total: 'cpu_total',
		// 		mem_used: 'cpu_used',
		// 		mem_free: 'cpu_free',
		// 		mem_usage: 'cpu_usage',
		// 		sys: 'sysInfo',
		// 		sys_computerName: 'sys_name',
		// 		sys_osName: 'sys_osName',
		// 		sys_computerIp: 'sys_computerIp',
		// 		sys_osArch: 'sys_osArch',
		// 		jvm: 'JVM',
		// 		jvm_name: 'jvm_name',
		// 		jvm_version: 'jvm_version',
		// 		startTime: 'startTime',
		// 		runTime: 'runTime',
		// 		jvm_home: 'jvm_home',
		// 		jvm_userDir: 'jvm_userDir',
		// 		jvm_inputArgs: 'jvm_inputArgs',
		// 		sysFile: 'sysFile',
		// 		sysFile_dirName: 'sysFile_dirName',
		// 		sysFile_sysTypeName: 'sysFile_sysTypeName',
		// 		sysFile_typeName: 'sysFile_typeName',
		// 		sysFile_total: 'sysFile_total',
		// 		sysFile_free: 'sysFile_free',
		// 		sysFile_used: 'sysFile_used',
		// 		sysFile_usage: 'sysFile_usage',
		// 		loadding: 'Loading，please wait...！',
		// 	},
		// 	// 订单管理
		// 	order: {
		// 		businessName: 'Order',
		// 		billNo: '流水号',
		// 		orderId: '订单编号',
		// 		orderStatus: 'OrderStatus',
		// 		payStatus: 'PayStatus',
		// 		customerCode: '用户ID',
		// 		goodsName: 'GoodsName',
		// 		goodsCode: 'GoodsCode',
		// 		goodsCount: 'GoodsCount',
		// 		subAmount: 'SubAmount',
		// 		disAmount: 'DisAmount',
		// 		saleAmount: 'SaleAmount',
		// 		customerType: 'CustomerType',
		// 		orderDetails: 'OrderDetails',
		// 		orderAmount: 'OrderAmount',
		// 		totalAmount: '订单总金额',
		// 		totalOrderCount: '订单数',
		// 		totalGoodsCount: '购买商品数',
		// 		goodsCountAvg: '平均商品数',
		// 		customersPriceAvg: '平均客单价',
		// 		cardTip1: '订单金额总和',
		// 		cardTip2: '订单数量总和',
		// 		cardTip3: '订单商品数量总和',
		// 		cardTip4: '订单商品数量总和 / 订单总和',
		// 		cardTip5: '订单金额总和/ 订单总和',
		// 		payMethod: '支付方式',
		// 		payType: '支付类型',
		// 		paidTime: '支付时间',
		// 		goodsListLength: '商品件数',
		// 	},
		// 	// 订单支付管理
		// 	orderPay: {
		// 		businessName: 'Order Payment',
		// 		outTradeNo: 'TradeNo',
		// 		payMethod: 'PayMethod',
		// 		payAmount: 'Amount',
		// 		payType: 'PayType',
		// 		payTime: 'PayTime',
		// 		totalPaymentCount: '支付数',
		// 		totalPaymentAmount: '支付金额',
		// 		paymentDurationAvg: '平均支付时长(S)',
		// 		cardTip1: '支付信息数量',
		// 		cardTip2: '支付金额总和',
		// 		cardTip3: '(结算时间总和-支付完成时间差总和)/订单数',
		// 	},
		// 	// 充放电信息
		// 	batteryCharge: {
		// 		chargeStatus: 'ChargeStatus',
		// 		reportTime: 'ReportTime',
		// 		batteryModel: 'BatteryModel',
		// 		chargeTime: 'ChargeTime',
		// 		batteryLevel: 'BatteryLevel',
		// 		chargeTimeAvg: '平均充电时长',
		// 		cardTip1: '（放电时间 - 充电时间差总和）/ 放电次数总和',
		// 	},
		// 	// IOT配置管理
		// 	iotConfig: {
		// 		noMarketTips: '请在顶部选择要进行配置的门店后再操作',
		// 		switchMachineConfig: '开关机',
		// 		batterySafeConfig: '电池安全',
		// 		volumeSizeConfig: '音量',
		// 		monitor: '监控',
		// 		batteryModel: '电池模式',
		// 		chargeMode: 'ChargeMode',
		// 		startTime: 'StartTime',
		// 		endTime: 'EndTime',
		// 		offTime: 'OffTime',
		// 		onTime: 'OnTime',
		// 		sleepMode: 'SleepMode',
		// 		restartMode: 'Restart settings',
		// 		restartDay: 'Custom',
		// 		everyDay: 'EveryDay',
		// 		custom: 'Custom',
		// 		chargeQuantity: 'ChargeQuantity',
		// 		chargeQuantityTip: '（Input limit【80-100】）',
		// 		disChargeQuantity: 'DisChargeQuantity',
		// 		disChargeQuantityTip: '（Input limit【10-20】）',
		// 		highVoltageWarning: 'HighVoltageWarning',
		// 		highVoltageWarningTip: '（Input limit【12.50-12.55】）',
		// 		electricCurrentWarning: '高电流预警值',
		// 		electricCurrentWarningTip: '（阈值限定在【1.5-2】之间）',
		// 		electricCurrentAlarm: '高电流报警值',
		// 		electricCurrentTip: '（阈值限定在【2-3】之间）',
		// 		temperatureChangeWarning: 'TempChangeWarning',
		// 		temperatureChangeWarningTip: '（Input limit【5-10】）',
		// 		temperatureChangeAlarm: 'TempChangeAlarm',
		// 		temperatureChangeAlarmTip: '（Input limit【11-20】）',
		// 		temperatureProtectWarning: 'HightTempWarning',
		// 		temperatureProtectWarningTip: '（Input limit【35-42】）',
		// 		temperatureProtectAlarm: 'HightTempAlarm',
		// 		temperatureProtectAlarmTip: '（Input limit【42-55】）',
		// 		batteryHealthWarning: 'HealthWarning',
		// 		batteryHealthWarningTip: '（Input limit【80-95】）',
		// 		batteryHealthAlarm: 'HealthAlarm',
		// 		batteryHealthAlarmTip: '（Input limit【60-79】）',
		// 		configType: 'ConfigType',
		// 		marketCode: 'StoreId',
		// 		marketType: 'StoreType',
		// 		volumeSize: '系统音量',
		// 		cartMonitorConfig: '监控配置',
		// 		exerciseDuration: '移动持续时间',
		// 		staticDuration: '静止持续时间',
		// 		reportFrequency: '定时任务时间',
		// 		netConnectTimeout: '网络超时时间',
		// 		shutdownBackCount: '低电关机倒计时',
		// 		highTempDisChargeAlarm: '电池高温放电告警值',
		// 		highTempDisChargeAlarmTips: '有效范围【45~60】默认:55',
		// 		highTempDisChargeWarn: '电池高温放电预警值',
		// 		highTempDisChargeWarnTips: '有效范围【45~60】默认:50',
		// 		dischargeElectricAlarm: '放电最大电流告警值',
		// 		dischargeElectricAlarmTips: '有效范围【1450~8000】默认:1450',
		// 		dischargeElectricWarn: '放电最大电流预警值',
		// 		dischargeElectricWarnTips: '有效范围【500~1450】默认:1400',
		// 		highTempChargeAlarm: '电池高温充电告警值',
		// 		highTempChargeAlarmTips: '有效范围【45~60】默认:50',
		// 		highTempChargeWarn: '电池高温充电预警值',
		// 		highTempChargeWarnTips: '有效范围【45~60】默认:45',
		// 		chargeElectricAlarm: '充电最大电流告警值',
		// 		chargeElectricAlarmTips: '有效范围【1500~8000】默认:3000',
		// 		chargeElectricWarn: '充电最大电流预警值',
		// 		chargeElectricWarnTips: '有效范围【1500~8000】默认:2500',
		// 		lowTempDisChargeWarn: '电池低温放电预警值',
		// 		lowTempDisChargeWarnTips: '有效范围【-5~10】默认:0',
		// 		lowTempChargeWarn: '电池低温充电预警值',
		// 		lowTempChargeWarnTips: '有效范围【-5~10】默认:0',
		// 		highVoltageAlarm: '最大电压告警值',
		// 		highVoltageAlarmTips: '有效范围【12.15~13.05】默认:12.8',
		// 		highVoltageWarn: '最大电压预警值',
		// 		highVoltageWarnTips: '有效范围【12.15~13.05】默认:12.7',
		// 	},
		// 	// 版本信息管理
		// 	version: {
		// 		versionNum: '版本号',
		// 		versionType: '应用类型',
		// 		versionDesc: '版本说明',
		// 		versionNumInvalid: '版本号输入非法',
		// 		mad5Value: 'MD5摘要',
		// 		versionUrl: '版本地址',
		// 		apkSizeTip: '只能上传APK文件，且不超过100MB',
		// 		chooseVersionTip: '请先选择应用类型',
		// 		chooseVersionNumTip: '请先输入版本号',
		// 		versionFileName: '升级文件',
		// 	},
		// 	upgradeTask: {
		// 		addTaskDevice: '任务设备',
		// 		notAdded: '未添加',
		// 		added: '已添加',
		// 		basicInfo: '基础信息',
		// 		chooseDevice: '选择设备',
		// 		transferSearchTip: '请输入设备号',
		// 		repeatTaskTip: '请勿重复添加任务',
		// 		status: '状态',
		// 		closeTask: '关闭任务',
		// 		closeTaskConfirm: '确定要关闭升级任务吗?',
		// 		chooseDeviceTip: '请选择新增设备!',
		// 	},
		// 	upgradeInfo: {
		// 		upgradeStatus: '升级结果',
		// 	},
		// 	orderLoss: {
		// 		businessName: '核损信息',
		// 		lossNode: '核损节点',
		// 		orderId: '订单编号',
		// 		createdTime: '核损时间',
		// 		customerCode: '用户ID',
		// 		itemWeight: '订单重量',
		// 		totalWeight: '实际重量',
		// 		excessWeight: '超出重量',
		// 		detail: '核损详情',
		// 		abnormalDetails: '异常信息',
		// 		shoppingGoodsDetails: '购物清单',
		// 		abnormalType: '异常类型',
		// 		abnormalWeight: '异常重量',
		// 		abnormalImage: '图片标记',
		// 		abnormalTime: '异常时间',
		// 		totalStandardWeight: '标重总重',
		// 		abnormalCount: '异常数量',
		// 		orderLossTotalWeight: '订单总重',
		// 		totalExcessWeight: '超出总重',
		// 		cardTip1: '核损异常总数（核损列表数量）',
		// 		cardTip2: '订单重量（清单重量）总和',
		// 		cardTip3: '放入重量（车内总重）总和',
		// 		cardTip4: '超出重量（放入重量 - 订单重量）总和',
		// 		isSpecialGoods: '是否特殊商品',
		// 		behDetails: '行为详情',
		// 		noScannCount: '未扫码',
		// 		excessWeightCount: '核重异常',
		// 		specialCount: '特殊商品',
		// 		bagCount: '存包',
		// 		noWeightCount: '无重商品',
		// 		time: '时间',
		// 		putWeight: '放入重量',
		// 		speGoods: '特殊商品',
		// 		goodsName: '商品名称',
		// 		goodsCode: '商品条码',
		// 		speType: '特殊类型',
		// 		noSale: '限售提醒',
		// 		stanWeight: '标准重量',
		// 		cartWeight: '车筐重量',
		// 		originAmount: '订单原价：',
		// 	},
		// 	shoppingRecord: {
		// 		businessName: '购物行为',
		// 		ultralightCount: '超轻拦截次数',
		// 		totalOverweightCount: '超重拦截次数',
		// 		interceptDegree: '购物拦截',
		// 		isSettlement: '结算拦截',
		// 		isAppraisalWeight: '删除鉴重',
		// 		detail: '行为详情',
		// 		loginType: '登录方式',
		// 		packageWeight: '存包重量',
		// 		savePackageCount: '存包次数',
		// 		standardWeight: '商品标重',
		// 		warnImage: '查看图片',
		// 		exceedWeight: '超阈重量',
		// 		takeOutWeight: '取出重量',
		// 		editQuantity: '编辑数量',
		// 		detailsWeight: '清单总重',
		// 		putWeight: '放入总重',
		// 		detailsPrice: '清单总价',
		// 		exitType: '退出方式',
		// 		customerCount: '顾客数',
		// 		shoppingTimeAvg: '平均购物时长',
		// 		shoppingTimeAvgTips: '（登录时间-结算时间 总和）/订单数',
		// 		abnormalCount: '异常次数',
		// 		abnormalCountTips: '用户购买异常行为总和',
		// 		noScannCount: '未扫码次数',
		// 		putErrorCount: '扫码放错次数',
		// 		takeErrorCount: '删除取错次数',
		// 		customerCountTip: '查询条件下所有顾客数量总和',
		// 		shoppingTimeAvgTip: '查询条件下购物时长平均数',
		// 		abnormalCountTip: '查询条件下所有异常次数总和',
		// 		noScannCountTip: '查询条件下所有未扫码放入次数总和',
		// 		putErrorCountTip: '查询条件下所有扫码放错次数总和',
		// 		takeErrorCountTip: '查询条件下所有删除放错次数总和',
		// 	},
		// 	businessConfig: {
		// 		interfaceResources: '界面配置',
		// 		standbyPageTitle: '待机页样式配置',
		// 		themeTitle: '主题色配置',
		// 		themeColor: '主题色',
		// 		textColor: '字体&图标颜色',
		// 		black: '黑色',
		// 		white: '白色',
		// 		previewStyle: '主题色样例',
		// 		menuConfig: '图标配置',
		// 		instructionsTitle: '操作指引配置',
		// 		isEnable: '引导页是否开启',
		// 		isEnableTips:
		// 			'开启后，在购物车上点击开始购物后会激活引导页。 同一用户完成支付身份后，下次不再展示！',
		// 		expiryDate: '失效日期',
		// 		expiryDateTips: '到期后自动关闭引导页功能，默认有效期为半年。',
		// 		marketIcon: '营销导航',
		// 		functionIcon: '功能导航',
		// 		topIcon: '快捷入口顶部',
		// 		footerIcon: '快捷入口低部',
		// 		menuConfigIllegal: '图标配置不合法',
		// 		shoppingBag: '购物袋配置',
		// 		bagType: '购物袋类型',
		// 		smallBag: '小型袋子',
		// 		mediumBag: '中型袋子',
		// 		largeBag: '大型袋子',
		// 		bagName: '购物袋名称',
		// 		bagCode: '购物袋编码',
		// 		bagPrice: '购物袋价格',
		// 		priceUnit: '元',
		// 		countLimitTip: '最多可配置两种不同类型的购物袋',
		// 		emptyFieldTip: '已勾选购物袋名称、编号和价格不能为空',
		// 		codeError: '购物袋条码长度为6~18位数字',
		// 		priceError: '价格格式有误',
		// 		smallBagType: '小购物袋类型',
		// 		smallBagName: '小购物袋名称',
		// 		smallBagCode: '小购物袋条码',
		// 		smallBagPrice: '小购物袋价格',
		// 		largeBagType: '大购物袋类型',
		// 		largeBagName: '大购物袋名称',
		// 		largeBagCode: '大购物袋条码',
		// 		largeBagPrice: '大购物袋价格',
		// 		bagPriceTips: '(按照最小金额单位)',
		// 		weightThreshold: '重量响应配置',
		// 		weightThresholdTips: '设置正常的阈值范围',
		// 		payConfig: '支付配置',
		// 		payMethod: '支付方式',
		// 		payType: '支付类型',
		// 		paymentCode: '反扫',
		// 		scanPay: '正扫',
		// 		facePay: '刷脸',
		// 		cardPay: '卡支付',
		// 		settlementCodePay: '结算机(结算码)',
		// 		settlementQrCodePay: '结算机(二维码)',
		// 		bltCardPay: 'BLT卡支付',
		// 		payMethodEmptyTips: '支付方式不能为空',
		// 		payTypeEmptyTips: '已勾选支付方式的支付类型不能为空',
		// 		cartCount: '车辆数量',
		// 		warnRuleConfig: '商品配置',
		// 		noSaleIntercept: '禁售拦截',
		// 		limitSaleWarn: '限售提醒',
		// 		noCheckWeightGoods: '不鉴定商品',
		// 		specialGoods: '特别关注商品',
		// 		priceGreaterThanOrEqualTo: '最低价格',
		// 		ruleType: '规则分类',
		// 		byGoodsCategoryCategory: '按商品品类',
		// 		classifyByPrice: '按价格分类',
		// 		categorySelection: '品类选择',
		// 		cigarette: '香烟',
		// 		wine: '酒类',
		// 		medicine: '药品',
		// 		classifyPrice: '分类价格',
		// 		typeSelection: '类型选择',
		// 		priceGoods: '计件商品',
		// 		weightGoods: '称重商品',
		// 		standardGoods: '标准商品',
		// 		categoryEmptyTip: '品类选择不能为空',
		// 		warnPriceEmptyTip: '分类价格不能为空',
		// 		noSaleRuleType: '禁售拦截规则分类',
		// 		noSaleCategory: '禁售拦截品类选择',
		// 		noSalePrice: '禁售拦截分类价格',
		// 		limitSaleRuleType: '限售提醒规则分类',
		// 		limitSaleCategory: '限售提醒品类选择',
		// 		limitSalePrice: '限售提醒分类价格',
		// 		noCheckWeightGoodsType: '不鉴定商品类型选择',
		// 		followGoodsPrice: '特别关注商品价格',
		// 		followStatus: '特别关注商品状态',
		// 		noCheckWeightStatus: '不鉴重商品状态',
		// 		limitSaleStatus: '限售商品状态',
		// 		noSaleStatus: '禁售商品状态',
		// 		changeLog: '历史记录',
		// 		bagExistTips: '购物袋编码重复，请修改购物袋编码',
		// 	},
		// 	// 广告配置
		// 	advert: {
		// 		advertName: '广告名称',
		// 		advertType: '广告位',
		// 		status: '展示状态',
		// 		startTime: '配置开始时间',
		// 		endTime: '配置结束时间',
		// 		cartType: '购物车类型',
		// 		triggerMode: '触发方式',
		// 		userScanCode: '用户扫码',
		// 		fixedRotation: '固定轮播',
		// 		positioningArea: '定位区域',
		// 		duration: '广告持续时间',
		// 		uploadAdvert: '上传广告',
		// 		fileUploadTips: '( 支持图片和视频 )',
		// 		advertDate: '配置时间',
		// 		advertVersion: '适用版本',
		// 		ipaidType: 'PAD样式',
		// 		second: '秒',
		// 		goodsCodePlaceholder: '输入商品条码,多个条码以英文分号隔开',
		// 		beaconType: '定位类型',
		// 		beaconId: '定位点标识',
		// 		locationKey: '广告key',
		// 		resourceEmptyTips: '广告资源不能为空，请上传广告',
		// 		goodsCodeErrorTips: '商品条码格式有误',
		// 		resourceTypeTips: '(支持图片和视频)',
		// 		standbyPageStyle: '待机页样式',
		// 		tips_enable_advert: '您确定要启用吗?',
		// 		tips_disable_advert: '您确定要禁用吗?',
		// 		resourceType: '广告类型',
		// 		basicInfo: '基础信息',
		// 		resourceInfo: '资源信息',
		// 	},
		// 	managerResources: {
		// 		managerResourcesTitle: '管理员密码',
		// 		passwordMsy: '管理员MSY',
		// 		passwordChaohi: '超嗨管理员密码',
		// 		passwordLt: '雷霆管理员密码',
		// 		updatedBy: '更新人',
		// 		updatedTime: '更新时间',
		// 		passwordTip: '请先输入正确的密码再生成二维码',
		// 		marketCheckTip: '当前门店已设置管理员密码',
		// 	},
		// 	configMould: {
		// 		configId: '配置编号',
		// 		configType: '配置类型',
		// 		configName: '配置名称',
		// 		configValue: '配置值',
		// 		configUnit: '单位',
		// 		remarks: '备注',
		// 		modelType: '展示类型',
		// 		configDictText: '配置字典文本',
		// 		configDictValue: '配置字典值',
		// 		isDefault: '是否默认值',
		// 		dissimilarity: '配置编号和配置类型不符',
		// 		deleteTip: '请选择要删除的行',
		// 		invalidDict: '请输入完整的配置字典项',
		// 		noDefaultValue: '请选择默认字典值',
		// 		addDictTip: '请先添加配置字典项',
		// 		onlyOneDefault: '单选框只能选择一个默认值',
		// 		dictValueRepeat: '配置字典文本或字典值不能重复',
		// 		deleteConfigMouldConfirm:
		// 			'删除配置模板同时会删除门店对应配置信息，确认要删除吗？',
		// 		configMouldTypeBtn: '配置分类列表',
		// 		configMouldTypeId: '配置分类编号',
		// 		configMouldTypeName: '配置分类名称',
		// 		deleteConfigMouldTypeConfirm: '您确定要删除配置分类吗？',
		// 		deleteConfigMouldTypeTip:
		// 			'对不起，该配置分类下有基础配置模板，无法删除!',
		// 	},
		// 	currencyStyle: {
		// 		currencyName: '货币名称',
		// 		currencyCode: '货币ISO代码',
		// 		currencySymbol: '货币符号',
		// 		multiple: '金额倍数',
		// 		preSuffix: '前/后缀',
		// 		currencyFormat: '货币格式',
		// 		currencyExample: '货币示例展示',
		// 		useType: '符号类型',
		// 		deleteCurrencyStyleTip: '删除后将影响门店货币配置，您确定要删除吗？',
		// 	},
		// 	internationalResources: {
		// 		queryContent: '标识key或语言内容',
		// 		resourcesKey: '标识key',
		// 		languageCode: '语言代码',
		// 		languageName: '语言名称',
		// 		languageContent: '国际化内容',
		// 		addDictTip: '请先添加语言类型字典值',
		// 		resourcesName: '国际化资源名称',
		// 		ipaidType: 'paid样式',
		// 		language: '语言',
		// 		picture: '图片',
		// 		voice: '语音',
		// 		deleteLanguageTip:
		// 			'删除会将该key对应所有语言全部删除，您确认要删除吗？',
		// 		import_excel_tip: '仅允许导入“xls”或“xlsx”格式文件。',
		// 		importType: '导入方式',
		// 		excelFile: '国际化资源文件',
		// 		importTip: '纵向导入请先选择语言代码',
		// 		verticalType: '新增方式',
		// 		horizontally: '字段新增',
		// 		vertical: '语言新增',
		// 	},
		// 	// 故障信息管理 - 故障上报
		// 	report: {
		// 		businessName: '故障上报',
		// 		failureDeclaration: '故障录入',
		// 		deviceNum: '设备编号',
		// 		deviceSignAll: '设备编号/wifiMac/SN',
		// 		cartSn: 'SN码',
		// 		authCode: 'WifiMac',
		// 		deviceType: '设备类型',
		// 		deviceSign: '设备标识',
		// 		faultType: '故障类型',
		// 		reportMethod: '上报方式',
		// 		faultReason: '故障原因',
		// 		faultPhenomenon: '故障现象',
		// 		processingProgress: '处理进度',
		// 		processingProgressNo: '暂无处理进度',
		// 		progressUpdateTime: '进度更新时间',
		// 		faultTime: '故障时间',
		// 		entryBy: '录入人',
		// 		entryTime: '录入时间',
		// 		reportBy: '上报人',
		// 		report: '上报',
		// 		reported: '已上报',
		// 		reportTime: '上报时间',
		// 		faultStatus: '故障状态',
		// 		processingTime: '处理时间',
		// 		processingCompletionTime: '处理完成时间',
		// 		revoke: '撤销',
		// 		saveReport: '保存并上报',
		// 		uploadImg: '上传图片',
		// 		saveDraft: '保存草稿',
		// 		tips_report_cancel: '已取消',
		// 		tips_report_delconfirm: '确认删除该条记录吗，删除后不可恢复！',
		// 		tips_report_revokeconfirm: '确认撤销该条记录吗？',
		// 		tips_report_confirm: '故障上报后不可修改，您确认要上报该记录吗？',
		// 		tips_revoke_confirm: '确认撤销该条记录吗？',
		// 		tips_deviceNum_err: '请输入五位设备编号',
		// 		tips_deviceSN_err: '请输入SN码',
		// 		resourceTypeTips: '(最多可上传9张图片或视频)',
		// 		deviceSignTypeTips: '请先选择设备类型',
		// 		deviceSignTips: '请先选择设备标识',
		// 		'non-existent': '不存在',
		// 		deviceNumTips: '编号不能为空',
		// 	},
		// 	// 故障信息管理 - 故障处理
		// 	handle: {
		// 		businessName: '故障处理',
		// 		deviceType: '设备类型',
		// 		deviceNum: '设备编号',
		// 		deviceNumMacSN: '设备编号/WiFiMac/SN码',
		// 		faultType: '故障类型',
		// 		faultPhenomenon: '故障现象',
		// 		reportMethod: '上报方式',
		// 		processMethod: '处理方式',
		// 		faultStatus: '故障状态',
		// 		reportTime: '上报时间',
		// 		processingProgress: '处理进度',
		// 		processingTime: '处理时间',
		// 		processingCompletionTime: '处理完成时间',
		// 		deviceModel: '设备型号',
		// 		mainBoardModel: '主板型号',
		// 		systemVersion: '安卓版本',
		// 		appVersion: '应用版本号',
		// 		reportBy: '上报人',
		// 		faultReason: '故障原因',
		// 		faultImg: '故障图片',
		// 		processBy: '处理人',
		// 		faultCategory: '故障大类',
		// 		faultPoint: '故障点',
		// 		faultLevel: '故障级别',
		// 		faultAmount: '故障数量',
		// 		faultRate: '故障率',
		// 		faultRateFormula: '故障数/启用设备数',
		// 		cartServiceLife: '车辆使用年限',
		// 		quarter: '季度',
		// 		processCompleteResult: '处理完成结果说明',
		// 		processResult: '处理结果说明',
		// 		responseTime: '响应时长(h)',
		// 		processTime: '处理时长(h)',
		// 		process: '故障处理',
		// 		editProcess: '编辑处理',
		// 		editProcessComplete: '编辑处理完成',
		// 		statistics: '是否统计',
		// 		processingCompletion: '处理完成',
		// 	},
		// 	// 一体机
		// 	deviceExport: {
		// 		businessName: '一体机',
		// 		deviceNum: '设备编号',
		// 		authCode: 'WifiMac信息',
		// 		deviceType: '出口类型',
		// 		appVersion: '应用版本号',
		// 		useScene: '使用场景',
		// 		remarks: '备注',
		// 		changeType: '变更方式',
		// 		tips_deviceNum_has: '该一体机编号已存在',
		// 		onlyScreenNum_has: '该单独大屏编号已存在',
		// 	},
		// 	// 首页-图表
		// 	home: {
		// 		enableCartCount: '实时启用车辆数',
		// 		saleAmount: '销售额',
		// 		saleOrderCount: '订单',
		// 		customerAmountAvg: '客单价',
		// 		economizeAmount: '节省金额',
		// 		orderLossAmount: '防损金额',
		// 		newMembersCount: '新增会员',
		// 		loginMembersCount: '登录用户数',
		// 		saleAmountTips: '订单支付成功的实付金额',
		// 		saleOrderCountTips: '已支付订单数量',
		// 		customerAmountAvgTips: '销售额/订单数',
		// 		economizeAmountTips: '人销比*销售额',
		// 		orderLossAmountTips: '商品平均重量价值*报警订单超重克数',
		// 		newMembersCountTips: '新增会员数',
		// 		loginMembersCountTips: '游客+会员登陆用户数',
		// 		isIncreaseOfDay: '日环比',
		// 		isIncreaseOfWeek: '周环比',
		// 		saleOrder: '销售额/订单数',
		// 		sales: '销售额',
		// 		orders: '订单数',
		// 		orderAvgTrend: '客单价/车均金额',
		// 		orderAvg: '客单价',
		// 		cartAvg: '车均金额',
		// 		orderLossTrend: '防损金额/节省金额',
		// 		memberTrend: '登录用户/新增会员',
		// 		loginMembers: '登录用户',
		// 		paymentTypeCake: '支付方式占比',
		// 		weChat: '微信支付',
		// 		aliPay: '支付宝支付',
		// 		dMall: '多点支付',
		// 		memberPay: '会员支付',
		// 		unionPay: '银联云闪付',
		// 		payPal: 'PayPal支付',
		// 		card: '卡支付',
		// 		cash: '现金支付',
		// 		other: '其他支付',
		// 		googlePay: 'GooglePay支付',
		// 		wingPay: '翼支付',
		// 		orderLossCake: '防损占比',
		// 		normalOrderLoss: '正常',
		// 		abnormalOrderLoss: '拦截',
		// 		bagOrderLoss: '存包',
		// 		saleAmountTop10: '门店成交金额TOP10',
		// 	},
		// 	// 顾客评价任务
		// 	commentsTask: {
		// 		businessName: '顾客评价任务',
		// 		useScene: '应用场景',
		// 		isScore: '是否评分',
		// 		scoreCorrelation: '评分关联',
		// 		enableStatus: '启用状态',
		// 		configureDate: '配置日期',
		// 		startDate: '配置有效日期开始日期',
		// 		endDate: '配置有效日期结束日期',
		// 		effectiveDate: '生效日期',
		// 		creatTime: '创建时间',
		// 		addTask: '新增任务',
		// 		evaluationTemplate: '评价模板',
		// 		relatedCondition: '关联条件',
		// 		startTime: '开始日期',
		// 		endTime: '结束日期',
		// 		templateName: '模板名称',
		// 		isOpenScore: '是否开启评分',
		// 		enable: '开启',
		// 		disable: '关闭',
		// 		isScoreQuestion: '评分是否关联问题',
		// 		relatedQuestion: '关联问题条件',
		// 		star: '星以内',
		// 		questionStar: '关联问题条件的星级',
		// 		question: '问题',
		// 		selectType: '选择类型',
		// 		singleChoice: '单选',
		// 		multipleChoice: '多选',
		// 		answer: '答案',
		// 		questionList: '问题列表',
		// 		isEnable: '是否启用',
		// 		dateCoincidence: '有效期重合',
		// 		tipsDisabled: '已禁用',
		// 		tipsAbled: '已启用',
		// 		dateTips: '请先选择所属超市、所属门店、应用场景',
		// 		enableTips: '启用该任务后会恢复使用，您确定要启用该任务吗？',
		// 		disabledTips: '禁用该任务后将无法使用，您确定要禁用该任务吗？',
		// 	},
		// 	// 评分统计
		// 	questionRecord: {
		// 		businessName: '评分统计',
		// 		useScene: '应用场景',
		// 		effectiveDate: '生效日期',
		// 		creatTime: '创建时间',
		// 		startTime: '开始日期',
		// 		endTime: '结束日期',
		// 		templateName: '模板名称',
		// 		scoreTimes: '评分次数',
		// 	},
		// 	// 问题统计
		// 	questionStatistics: {
		// 		businessName: '问题统计',
		// 		content: '问题内容',
		// 		answerStatistics: '回答统计',
		// 		answerContent: '选项内容',
		// 		questionName: '问题名称',
		// 		useScene: '应用场景',
		// 		effectiveDate: '生效日期',
		// 		creatTime: '创建时间',
		// 		startTime: '开始日期',
		// 		endTime: '结束日期',
		// 		templateName: '模板名称',
		// 		scoreTimes: '评分次数',
		// 	},
		// 	// 数据转发任务
		// 	sendDataTask: {
		// 		businessName: '问题统计',
		// 		sendModel: '转发模块',
		// 		sendTimeType: '转发时机',
		// 		sendTitle: '转发主题',
		// 		agentName: '所属服务商',
		// 		creatTime: '创建时间',
		// 		updatedTime: '更新时间',
		// 		sendTarget: '组织级别',
		// 		Agent: '服务商',
		// 		Org: '超市',
		// 		Market: '门店',
		// 		sendAgent: '转发服务商',
		// 		sendOrg: '转发超市',
		// 		sendMarket: '转发门店',
		// 		sendUser: '转发用户',
		// 		emailTitle: '邮件标题',
		// 		emailContent: '邮件内容',
		// 		notAdded: '未添加',
		// 		added: '已添加',
		// 		repeatTaskTip: '请输入用户ID',
		// 		addUserTip: '请添加转发用户',
		// 	},
		// 	// 定位信息
		// 	beacon: {
		// 		beaconType: '定位类型',
		// 		beaconName: '定位名称 ',
		// 		bluetoothMac: '蓝牙Mac',
		// 		beaconLocation: '区域',
		// 		rfId: 'RFID信息',
		// 		locationKey: '定位key',
		// 		intensity: '设备强度',
		// 		frequency: '重复次数',
		// 		beaconId: '定位标识值',
		// 		locationKeyExist: '该门店定位key已存在',
		// 		RFIDExist: '该RFID已存在',
		// 		bluetoothExist: '该蓝牙Mac已存在',
		// 		x: 'X坐标',
		// 		y: 'Y坐标',
		// 		positionId: '点位SID值',
		// 		floor: '楼层',
		// 	},
		// 	// 热力图
		// 	wda: {
		// 		timeType: '时间类型',
		// 		date: '日期',
		// 		FilterByHour: '按小时筛选',
		// 		FilterByDay: '按天筛选',
		// 		timeInterval: '时间区间',
		// 		marketTips: '请先选择所属门店',
		// 	},
		// 	// 车辆盘点
		// 	deviceCartCheck: {
		// 		totalCount: '总车辆数',
		// 		onlineCount: '在场车辆',
		// 		departureCount: '离场车辆',
		// 		offlineCount: '离线车辆',
		// 		checkStatus: '盘点状态',
		// 		positionName: '定位名称',
		// 		positionValue: '定位标识',
		// 		checkTime: '盘点时间',
		// 		distance: '距离范围(m)',
		// 	},
		// 	// 演示优惠券
		// 	demoCoupon: {
		// 		orgName: '发放超市',
		// 		marketName: '发放门店',
		// 		validType: '生效范围',
		// 		promptType: '提示时机',
		// 		dateRang: '优惠券日期',
		// 		couponName: '优惠券名称',
		// 		discountAmount: '优惠券金额',
		// 		discountAmountTips: '(按照最小金额单位)',
		// 		validGoodsCode: '生效商品条码',
		// 		couponCode: '优惠券编号',
		// 		couponRule: '核销规则',
		// 		promptStr: '提示时机值',
		// 		positionStr: '定位选择值',
		// 		couponExis: '该条码已存在，请重新输入',
		// 		promptStrTips: '输入商品条码，多个条码以英文分号隔开',
		// 		startDate: '开始日期',
		// 		endDate: '结束日期',
		// 		over: '满',
		// 		canUse: '可使用',
		// 		discountAmountFlag: '满减金额必须大于优惠券金额',
		// 		positionSelect: '定位选择',
		// 	},
		// 	// 日期筛选器
		// 	datePicker: {
		// 		yesterday: '昨日',
		// 		last7: '近7日',
		// 		last30: '近30日',
		// 	},
		// 	//用户概览
		// 	customerView: {
		// 		customerFunnel: '会员资产',
		// 		registerMember: '会员',
		// 		huangjin: '黄金会员',
		// 		baijin: '白金会员',
		// 		zuanshi: '钻石会员',
		// 		registerMemberTips: '所有会员人数',
		// 		huangjinTips: '累计积分达100的会员人数',
		// 		baijinTips: '累计积分达1000的会员人数',
		// 		zuanshiTips: '累计积分达10000的会员人数',
		// 		buyCount: '会员活跃频次',
		// 		commentData: '会员评分统计占比',
		// 		consumeRange: '会员消费区间',
		// 		storePeople: '会员到店时间',
		// 		loginMember: '新增会员数',
		// 		briskMember: '活跃会员',
		// 		briskMemberTips: '活跃:平均60天购买5次以上',
		// 		ordinaryMember: '一般会员',
		// 		ordinaryMemberTips: '一般:平均60天购买4次以下',
		// 		sleepMember: '沉睡会员',
		// 		sleepMemberTips: 'Dormant Member: no purchase on average for 60 days',
		// 		star1: '1星',
		// 		star2: '2星',
		// 		star3: '3星',
		// 		star4: '4星',
		// 		star5: '5星',
		// 		commentAvg: '平均分值：',
		// 		totalCommentCount: '评分次数：',
		// 		singular: '单数',
		// 		peopNum: '人数',
		// 		memberNum: '会员数',
		// 	},
		// 	orderView: {
		// 		keyData: '关键数据',
		// 		dataChart: '数据图表',
		// 		saleAmount: '销售额',
		// 		saleOrderCount: '订单',
		// 		customerAmountAvg: '客单价',
		// 		shoppingTime: '平均购物时长',
		// 		saleAmountTips: '订单支付成功的实付金额',
		// 		saleOrderCountTips: '已支付订单数量',
		// 		customerAmountAvgTips: '销售额/订单数',
		// 		shoppingTimeTips: '购物时长/订单数',
		// 		isIncreaseOfDay: '日环比',
		// 		isIncreaseOfWeek: '周环比',
		// 		orderStatusData: '订单支付状态占比',
		// 		salesTrend: '销售趋势',
		// 		orderNum: '订单数',
		// 		memberOrderAvg: '客单价趋势',
		// 		unPaidCount: '未支付',
		// 		paidCount: '已支付',
		// 	},
		// 	orderlossView: {
		// 		keyData: '关键数据',
		// 		dataChart: '数据图表',
		// 		alarmOrderLossRate: '报警订单占比',
		// 		specialGoodsRate: '特殊商品占比',
		// 		orderLossAmount: '防损金额',
		// 		alarmOrderLossRateTips:
		// 			'报警订单数/所有已支付订单数(包含特殊商品的订单也属于报警订单)',
		// 		specialGoodsRateTips: '特殊商品数/总商品数',
		// 		orderLossAmountTips: '商品平均重量价值*报警订单超重克数',
		// 		isIncreaseOfDay: '日环比',
		// 		isIncreaseOfWeek: '周环比',
		// 		orderStatusData: '异常订单占比',
		// 		abnormalOrderLossCount: '拦截',
		// 		bagOrderLossCount: '提醒',
		// 		normalOrderLossCount: '正常',
		// 		behaviorTypeData: '拦截订单行为类型占比',
		// 		specialBehaviorCount: '特殊商品',
		// 		exceedThresholdBehaviorCount: '夹带/替换',
		// 		noScanBehaviorCount: '漏扫',
		// 		bagBehaviorCount: '存包',
		// 		noWeightBehaviorCount: '无重商品',
		// 		specialBehaviorCountTips: '特殊商品购买次数',
		// 		exceedThresholdBehaviorCountTips: '超重+超轻的拦截次数',
		// 		noScanBehaviorCountTips: '未扫码拦截次数',
		// 		bagBehaviorCountTips: '存包次数',
		// 		noWeightBehaviorCountTips: '无重商品购买次数',
		// 	},
		// 	goodsView: {
		// 		keyData: '关键数据',
		// 		dataChart: '数据图表',
		// 		isIncreaseOfDay: '日环比',
		// 		isIncreaseOfWeek: '周环比',
		// 		saleGoodsCount: '累计商品数',
		// 		saleGoodsAvgCount: '平均商品数',
		// 		saleSpecialGoodsCount: '特殊商品数',
		// 		goodsPriceAvg: '平均商品价格',
		// 		saleGoodsCountTips: '购买的商品数量',
		// 		saleGoodsAvgCountTips: '商品购买个数/订单数',
		// 		saleSpecialGoodsCountTips: '购买特殊商品数量',
		// 		goodsPriceAvgTips: '总销售额/累计商品数',
		// 		goodsTop20: '商品销售数量TOP20',
		// 	},
		// 	abnormalMonitorRecord: {
		// 		deviceNum: '车辆编号',
		// 		abnormalMonitorRecord: '报警信息',
		// 		abnormalStatus: '告警类型',
		// 		abnormalLevel: '告警级别',
		// 		abnormalValue: '告警值',
		// 		createdTime: '发生时间',
		// 	},
		// 	batteryMonitor: {
		// 		abnormalStatus: 'BatteryLevel',
		// 		batteryStatus: '充放电状态',
		// 		abnormalVoltage: 'AbnormalVoltage',
		// 		abnormalTemperature: 'AbnormalTemp',
		// 		abnormalTemperatureChange: 'TemChange',
		// 		abnormalBatteryHealthy: 'AbnormalHealthy',
		// 		chargeChangeType: '变化状态',
		// 		batteryMonitor: '电池参数监控信息',
		// 		batteryMonitorChangeLog: '电池参数监控(历史记录)信息',
		// 		deviceNum: '车辆编号',
		// 		chargeStatus: '电池状态',
		// 		changeLog: '历史记录',
		// 		batteryLevel: '电量',
		// 		batteryElectricCurrent: '电流mA',
		// 		batteryVoltage: '电压V',
		// 		batteryTemperature: '温度­°C',
		// 		batteryHealth: '健康值',
		// 		cyclesCount: '循环次数',
		// 		reportTime: '发生时间',
		// 		charging: '充电中',
		// 		uncharging: '放电中',
		// 	},
		// 	deviceCartMonitor: {
		// 		freeCount: '闲置',
		// 		shopping: '购物中',
		// 		charging: '充电中',
		// 		disable: '禁用',
		// 		faultAlarm: '故障报警',
		// 		lowBatteryCar: '低电车辆',
		// 		batteryHealthReminder: '电池健康提醒',
		// 		deviceId: '设备ID',
		// 		orderCount: '订单数',
		// 		payAmount: '结算金额',
		// 		shoppingCount: '购物次数',
		// 		shoppingTime: '购物时长',
		// 		useTime: '占用时长',
		// 		useCount: '占用中',
		// 		orgName: '所属超市',
		// 		marketName: '所属门店',
		// 		updatedTime: '上报时间',
		// 		dataCreate: '创建时间',
		// 		batteryMonitor: '车辆监控信息',
		// 		batteryMonitorChangeLog: '车辆监控(历史记录)信息',
		// 		deviceNum: '车辆编号',
		// 		cartSn: 'SN码',
		// 		authCode: 'wifiMac',
		// 		cartStatus: '启用状态',
		// 		faultStatus: '故障状态',
		// 		changeLog: '历史记录',
		// 		batteryLevel: '车辆电量',
		// 		batteryHealth: '电池健康值',
		// 		switchStatus: '开关机状态',
		// 		networkStatus: '网络状态',
		// 		runStatus: '运行状态',
		// 		deviceModel: '车辆型号',
		// 		reportTime: '发生时间',
		// 		normal: '正常',
		// 		unusual: '异常',
		// 		all: '全部',
		// 		start: '开机',
		// 		Shutdown: '关机',
		// 		network: '联网',
		// 		disNetwork: '断网',
		// 		leave: '闲置',
		// 		occupied: '占用中',
		// 		screenStatus: '亮屏状态',
		// 		screenDuration: '亮屏累计时间（天）',
		// 	},
		// 	scanGoodsRecommend: {
		// 		scanPage: '扫码成功页',
		// 		goodsName: '商品名称',
		// 		goodsCode: '商品条码',
		// 		goodsPrice: '商品原价',
		// 		goodsType: '商品类别',
		// 		specialType: '特殊类型',
		// 		status: '启用状态',
		// 		scanGoodsName: '扫码商品名称',
		// 		scanGoodsCode: '扫码商品条码',
		// 		recommendType: '推荐类型',
		// 		goodsId: '选择商品',
		// 		scangoodId: '扫码商品',
		// 		relationId: '推荐商品',
		// 		inputKeyWords: '请输入商品名称或商品条码',
		// 		img: '图片',
		// 		uploadImg: '上传图片',
		// 		uploadImgTips: '仅允许上传10M内,格式为:jpg,gif,png,jpeg的图片',
		// 		noUploadImgTips: '*请上传商品图片',
		// 		recommendGoodsTips: '最多关联2个推荐商品',
		// 	},
		// 	putGoodsRecommend: {
		// 		putPage: '加购成功页',
		// 		goodsName: '商品名称',
		// 		goodsCode: '商品条码',
		// 		goodsPrice: '商品原价',
		// 		goodsType: '商品类别',
		// 		specialType: '特殊类型',
		// 		recommendType: '推荐类型',
		// 		status: '启用状态',
		// 		putGoods: '加购商品',
		// 		putGoodsName: '加购商品名称',
		// 		putGoodsCode: '加购商品条码',
		// 		goodsId: '选择商品',
		// 		cookbook: '菜谱',
		// 		addCookbook: '添加菜谱',
		// 		cookbookName: '菜谱名称',
		// 		cookbookImg: '菜谱图片',
		// 		materialName: '用料名称',
		// 		usageAmount: '用量(g)',
		// 		recommendCookBook: '推荐菜谱',
		// 		recommendGoods: '推荐商品',
		// 		inputKeyWords: '请输入商品名称或商品条码',
		// 		recommendCookbookTips: '最多关联2个推荐菜谱',
		// 		recommendGoodsTips: '最多关联2个推荐商品',
		// 		img: '图片',
		// 		uploadImg: '上传图片',
		// 		uploadImgTips: '仅允许上传10M内,格式为:jpg,gif,png,jpeg的图片',
		// 		goodsExistTips: '该商品已存在，请更换商品',
		// 		nogoodsTips: '最少添加一件推荐商品',
		// 	},
		// 	wdaGoodsRecommend: {
		// 		wdaPage: '位置推荐页',
		// 		goodsName: '商品名称',
		// 		goodsCode: '商品条码',
		// 		goodsPrice: '商品原价',
		// 		goodsType: '商品类别',
		// 		specialType: '特殊类型',
		// 		recommendType: '推荐类型',
		// 		status: '启用状态',
		// 		putGoods: '加购商品',
		// 		putGoodsName: '加购商品名称',
		// 		putGoodsCode: '加购商品条码',
		// 		goodsId: '选择商品',
		// 		recommendGoods: '推荐商品',
		// 		inputKeyWords: '请输入商品名称或商品条码',
		// 		recommendGoodsTips: '最多关联2个推荐商品',
		// 		img: '图片',
		// 		uploadImg: '上传图片',
		// 		uploadImgTips: '仅允许上传10M内,格式为:jpg,gif,png,jpeg的图片',
		// 		noUploadImgTips: '*请上传商品图片',
		// 		nogoodsTips: '最少添加一件推荐商品',
		// 		title: '标题',
		// 		beaconName: '定位名称',
		// 		beacon: 'Mac地址',
		// 		beaconExist: '该定位已存在，请更换定位点',
		// 	},
		// 	operateView: {
		// 		marketTips: '请先选择所属门店',
		// 		cartUseRate: '车辆使用率',
		// 		cartPaidOrderAvg: '车均订单数',
		// 		cartAmountAvg: '车均销售额',
		// 		cartUseRateTips: '已支付购物车数/启用车辆数',
		// 		cartPaidOrderAvgTips: '已支付订单数/已支付购物车数',
		// 		cartAmountAvgTips: '已支付订单额/已支付购物车数',
		// 		loginsCount: '登录次数',
		// 		shoppingCount: '购物次数',
		// 		unpaidCound: '生成待支付订单个数',
		// 		paidCount: '支付成功订单个数',
		// 		funnelChart: '转化漏斗模型',
		// 		filterIdentityFunnelChart: '筛选登录购物车的身份',
		// 		tourist: '游客',
		// 		member: '会员',
		// 		filterSessionFunnelChart: '筛选购物中的会话时长',
		// 		'5Minutes': '5分钟以下',
		// 		'5-30Minutes': '5~30分钟',
		// 		over30Minutes: '30分钟以上',
		// 		filterBehaviorFunnelChart: '筛选行为状态类型',
		// 		abnormalBehavior: '异常行为',
		// 		normal: '正常',
		// 		filterOrderFunnelChart: '筛选订单异常类型',
		// 		alarm: '报警',
		// 		remind: '提醒',
		// 		filterPaidFunnelChart: '筛选已支付订单交易金额',
		// 		'10min': '10以下',
		// 		'10-100min': '10~100',
		// 		'100-300min': '100~300',
		// 		'300-500min': '300~500',
		// 		'500-1000min': '500~1000',
		// 		over100min: '1000以上',
		// 		cartLoadAvgTrend: '车均使用负荷压力趋势图',
		// 		orderTrend: '车均订单数趋势图',
		// 		orderAvgTrendChart: '车均销售额',
		// 		activeCartTopChart: '活跃车辆Top20',
		// 		unusedCartTopChart: '闲置车辆Top20',
		// 		conversionRate: '转化率：',
		// 		useRate: '使用率',
		// 		cartAvgOrder: '车均订单',
		// 		paymentRate: '支付率',
		// 		loginCount: '登录次数',
		// 		stayTime: '闲置时间',
		// 		day: '天',
		// 	},
		// };
	},

	methods: {
		exportAsTextFile(textToExport, fileName) {
			const blob = new Blob([textToExport], { type: 'text/plain' });
			const url = window.URL.createObjectURL(blob);
			const a = document.createElement('a');
			a.style.display = 'none';
			a.href = url;
			a.download = fileName;
			document.body.appendChild(a);
			a.click();
			document.body.removeChild(a);
			window.URL.revokeObjectURL(url);
		},

		// 导出txt
		exportText() {
			const objectToExport = JSON.stringify(this.obj);

			// 获取当前时间日期
			const now = new Date();

			// 将日期转为字符串
			const date =
				now.getFullYear() + '-' + (now.getMonth() + 1) + '-' + now.getDate();

			// 将时间转为字符串
			const time =
				now.getHours() + ':' + now.getMinutes() + ':' + now.getSeconds();

			// 日期和时间拼接
			const dateTime = date + ' ' + time;
			const fileName = `${this.languageVlue}(${dateTime}).txt`;
			this.exportAsTextFile(objectToExport, fileName);

			this.$message({
				message: '导出成功！',
				type: 'success',
			});
		},

		importText() {
			const input = document.createElement('input');
			input.type = 'file';
			input.accept = 'text/plain';
			input.onchange = (e) => {
				const file = e.target.files[0];
				const reader = new FileReader();
				reader.readAsText(file);
				reader.onload = (e) => {
					this.obj = JSON.parse(e.target.result);
				};
			};
			input.click();
		},

		// 返回顶部
		clickTop() {
			window.scrollTo(0, 0);
		},
	},
};
</script>

<style>
.hello {
	height: 100%;
	padding: 0 200px;
}
.fixed {
	width: 250px;
	height: 150px;
	padding-left: 20px;
	position: fixed;
	right: 35vw;
	top: 80px;
	background-color: #f1f1f1;
	backdrop-filter: saturate(100%) blur(4px);
	border-radius: 10px;
}

/* 返回顶部 */
.topBtnBox {
	position: fixed;
	right: 45vw;
	bottom: 70px;
}
.topBtn {
	width: 80px;
	height: 40px;
	background: rgb(79, 107, 233);
	border: #f1f1f1 1px solid;
	border-radius: 10px;
	color: #fff;
}

.iconBtn {
	font-size: 20px !important ;
}

.tips {
	color: red;
}

.green {
	background-color: rgb(218, 231, 218);
}
.pink {
	background-color: rgb(230, 222, 223);
}

.flex {
	width: 500px;
}

/* 快速跳转 */
.elevator {
	width: 400px;
	height: 100vh;
	overflow: scroll;
	position: fixed;
	top: 0;
	right: 6vw;
	display: flex;
	flex-wrap: wrap;
	flex-direction: row;
}
.elevatorTitle {
	width: 400px;
	font-size: 22px;
	font-weight: 600;
}
.elevatorItem {
	width: 200px;
}

.el-dialog {
	border-radius: 20px !important;
}
.tipsText {
	font-size: 18px;
	font-weight: 400;
}
</style>
