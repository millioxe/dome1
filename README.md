<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="utf-8">
    <meta name="viewport"
          content="width=device-width, initial-scale=1.0, minimum-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <!--
<meta charset="UTF-8" name="viewport" content="width=device-width, initial-scale=1.0, minimum-scale=0.5, maximum-scale=2.0, user-scalable=yes"  >
-->
    <title>millixe</title>
    <!--    <script src="http://i0.sinaimg.cn/dy/js/jquery/jquery-1.7.2.min.js"></script>-->
    <script src="/zd/js/jquery-3.6.0.min.js"></script>
    <link rel="stylesheet" href="/zd/css/head.css">

    <!--    <script src="https://github.com/blueimp/JavaScript-MD5/blob/master/js/md5.min.js" type="text/javascript" ></script>-->
    <script type="text/javascript">
			var ticket_type = "深圳";
			var ticket_info = "";
			var now_host = "http://"+window.location.host;
			var order_into_who = "木子"





    </script>
    <script type="text/javascript">
			function getclock() {
				var query = window.location.search.substring(1);
                if (query.length > 0) {
                    var param_arr = query.split("&");
                    var searchJson = {};
                    for (var i = 0; i < param_arr.length; i++) {
                        param_str = param_arr[i].split("=");
                        console.log(param_str);
                        searchJson[param_arr[i].split("=")[0]] = param_arr[i].split("=")[1];
<!--								console.log(searchJson);-->

                        try{

                            if(param_str[0]=="order_type"){
                                for (var i_end = 0; i_end < gList.length; i_end++) {
                                    console.log(gList[i_end].info_id);
                                    if (gList[i_end].info_id==decodeURI(param_str[1])){
                                        document.getElementById("order_parce").value = '¥ ' + gList[i_end].ticket_agency_price;
                                        document.querySelector("#order_parce1 > span").textContent = '¥ ' + gList[i_end].ticket_agency_price;
                                        document.querySelector("#original_price").value = '¥ ' + gList[i_end].ticket_original_price;
                                        document.querySelector("#original_price").style.textDecoration = 'line-through';
                                        break;
                                        }
                                }
                            } else if (param_str[0]=="info_id"){
                                $.ajax({
                                    type: "post",
                                    url: now_host + "/zd/ticket_order_info",
                                    data: {
                                        info_id: param_str[1],
                                    },
                                    dataType: "json",
                                    success: function(data) {
                                        let jsonres = data;
                                        console.log(jsonres);
                                        let gList = jsonres.data.text;
                                        for (var i = 0; i < gList.length; i++) {
                                            let studentName = gList[i].ticket_type;
                                            let student_Name = gList[i].info_id;;
                                            var obj = document.getElementById("order_type");
                                            var varItem = new Option(studentName, student_Name);
                                            obj.options.add(varItem);
                                            if (i == 0) {
                                                document.getElementById("order_parce").value = '¥ ' + gList[i].ticket_discount_price;
                                                document.querySelector("#order_parce1 > span").textContent = '¥ ' + gList[i].ticket_discount_price;
                                                document.querySelector("#original_price").value = '¥ ' + gList[i].ticket_original_price;
                                                document.querySelector("#original_price").style.textDecoration = 'line-through';

                                            }
                                        }
                                    }
                                });

                       
                            }else if (param_str[0]=="order_date"){
                                console.log("11111111111111111111"+param_str[1].toString());
                                console.log(document.getElementById("order_date").value);
                                document.getElementById("order_date").value = param_str[1].toString();
                                console.log("2222222222");
                            }else{
                                if (document.getElementById(param_str[0]).type==null){
                                    document.getElementById(param_str[0]).innerHTML=decodeURI(param_str[1]);
                                }else{
                                    document.getElementById(param_str[0]).disabled=true;
                                    document.getElementById(param_str[0]).value=decodeURI(param_str[1]);
                                }
                            }
							

							

                        }catch (error){};
                    };
                    console.log(searchJson);
                    if ("ticket_id" in searchJson ){
                        // document.getElementById("alertorder_text").style.display = "none";
                        // document.getElementById("shibie").style.display = "none";
                        document.getElementById("z-detail-button").style.display = "none";
                        document.getElementById("page-title").innerHTML = "订单详情";
                        document.getElementById("pay_tail").style.display = ""; //支付状态数据展示
                        if (document.getElementById("order_pay_status").innerHTML =='已支付'){
                            document.getElementById("pay_status_dia").innerHTML ='请等待,出票中。。。。';
                            document.getElementById("pay_status_button").style.display = "none";
                        };
                        document.getElementById("get_order_commit_button").onclick = function () {
                            console.log(searchJson);
                            window.location.href=now_host +"/zd/pays?ticket_id="+searchJson.ticket_id;
                        }

                    }



                };






			}

			// getclock();






    </script>
    <!--		<link rel="stylesheet" href="../../css/head.css">-->
</head>
<body>
<div class="big-border" height="device-height">
    <h3 class="page-title" id="page-title">填写预订信息</h3>
    <table width=device-width>
        <form action="">
            <fieldset style="display:none" id="pay_tail">
                <legend>支付信息:</legend>
                <div id="pay_status" style="text-align:center;font-weight: 700;font-size: 5.3333333333vmin;">
                    <tip id="order_status">12</tip>
                    <tip id="order_pay_status">12</tip>
                </div>
                <br/>
                <div id="pay_status_dia">请在5分钟内完成支付，过时订单自动取消</div>
                <br/>
                <div id="ticket_add_time"></div>
                <br/>
                <div id="pay_status_button">
                    <button type="button" id="get_order_commit_button" class='btn-confirm'
                            style="display:inline;float:right;background-color: #F5F5F5;border-radius: 10px 10px 10px 10px;width: 100%;height:45px;color: #f47f02;border: 1px solid #f47f02;">
                        去支付
                    </button>
                </div>
                <br/>

            </fieldset>
            <fieldset style="">
                <legend>简介:</legend>
                <button type="button" id="select_order_type" class='btn-confirm'
                        style="display:inline;float:right;background-color: #F5F5F5;border-radius: 10px 10px 10px 10px;">
                    类型详情
                </button>
                类型<select id="order_type" class="order_type"></select><br/>
                日期<input type="date" id="order_date" class="order_date" value="2015-09-24"
                         style="display:inline;"/><br/>
                市场价<input type="text" id="original_price" value="10" style="display:inline;width:30%;"
                          disabled/><br/>
                优惠价<input type="text" id="order_parce" value="10" style="display:inline;width:30%;"
                          disabled/><br/>
                数量<input type="text" id="order_count" value="1" style="display:inline;width:30%;" disabled/>
            </fieldset>
            <fieldset style="">
                <legend>预定信息:</legend>
                <textarea rows="5" cols="200" style="width: 70%; display: none;" id="alertorder_text"
                          placeholder="粘贴信息，自动拆分&#10;示例：&#10;   姓名:XXX&#10;   手机号码：135XXXXXXXX&#10;   证件号码：XXXXXXXXXXXXXXXXXX"></textarea>
                <button type="button" id="shibie" class='btn-confirm'
                        style="display:inline;background-color: #F5F5F5;border-radius: 10px 10px 10px 10px; display: none;">
                    识别拆分
                </button>
                <br/>
                联系姓名<input type="text" id="order_name" placeholder="请输入出行人姓名(必填)" oninput="checkInput(this)"/><br/>

                证件类型<input type="text" id="order_idtype" value="身份证" style="display:inline;width:15%;"
                           disabled/><br/>
                证件号码<input type="text" id="order_idcard" placeholder="请输入出行人证件号码(必填)" oninput="checkInput(this)"  maxlength="18"
                           style="display:inline;width:70%;"/><br/>
                手机号码<input type="text" id="order_phone" class="order_phone" placeholder="请输入出行人手机号码(非必填)"
                           maxlength="11" style="width:60%;display:inline;"/><br/>
            </fieldset>
            <fieldset style="">
                兑换码密<input type="text" id="order_kami" placeholder="请填写有效的兑换码(非必填)"/><br/>
            </fieldset>
            <fieldset style="">
                留言<input type="text" id="order_mark" placeholder="如有特殊需要，请在这里留言(非必填)"
                         style="display:inline;width:70%;"/><br/>
            </fieldset>


        </form>
    </table>
</div>
<br/>
<div class="z-detail-button" id="z-detail-button">
    <div class="z-button-left" id="order_parce1" style="float:left; width: 50%;">
        <span data-v-139a1ade=""><i data-v-139a1ade="" style="font-size: 14px;">¥</i> 99 </span>
    </div>
    <div class="z-button-right" style="margin-left: 51%;">
        <button type="button" id="get_order_commit" class='btn-confirm'
                style="display:inline;background-color: #F5F5F5;border-radius: 10px 10px 10px 10px; width: 100%;">
            提交
        </button>
    </div>
</div>
<script>
    async function customAlert(message) {
        // 创建背景遮罩
        const overlay = document.createElement('div');
        overlay.style.position = 'fixed';
        overlay.style.top = 0;
        overlay.style.left = 0;
        overlay.style.right = 0;
        overlay.style.bottom = 0;
        overlay.style.backgroundColor = 'rgba(0, 0, 0, 0.5)';
        document.body.appendChild(overlay);

        // 创建弹窗
        const popup = document.createElement('div');
        popup.style.position = 'absolute';
        popup.style.top = '50%';
        popup.style.left = '50%';
        popup.style.transform = 'translate(-50%, -50%)';
        popup.style.backgroundColor = 'white';
        popup.style.padding = '20px';
        popup.style.borderRadius = '5px';
        popup.innerHTML = message;

        // 创建确认按钮
        // const confirmButton = document.createElement('button');
        // confirmButton.textContent = confirmButtonText || 'OK';
        // confirmButton.style.marginTop = '20px';
        // confirmButton.addEventListener('click', function() {
        // 	document.body.removeChild(overlay);
        // 	document.body.removeChild(popup);
        // if (onConfirm) onConfirm();
        // });
        // popup.appendChild(confirmButton);


        document.body.appendChild(popup);

        // 倒计时
        var count = 4;
        var timer = setInterval(function() {

            count -= 1;
            popup.innerHTML=message+count;
            if (count <= 0) {
                document.body.removeChild(overlay);
                document.body.removeChild(popup);
                clearInterval(timer);
            }
        }, 1000);
    }

</script>
<script>
			function testid(id) {
				// 1 "验证通过!", 0 //校验不通过
				var format =
					/^(([1][1-5])|([2][1-3])|([3][1-7])|([4][1-6])|([5][0-4])|([6][1-5])|([7][1])|([8][1-2]))\d{4}(([1][9]\d{2})|([2]\d{3}))(([0][1-9])|([1][0-2]))(([0][1-9])|([1-2][0-9])|([3][0-1]))\d{3}[0-9xX]$/;
				//号码规则校验
				if (!format.test(id)) {
					return {
						'status': 0,
						'msg': '身份证号码不合规'
					};
				}
				//区位码校验
				//出生年月日校验   前正则限制起始年份为1900;
				var year = id.substr(6, 4), //身份证年
					month = id.substr(10, 2), //身份证月
					date = id.substr(12, 2), //身份证日
					time = Date.parse(month + '-' + date + '-' + year), //身份证日期时间戳date
					now_time = Date.parse(new Date()), //当前时间戳
					dates = (new Date(year, month, 0)).getDate(); //身份证当月天数
				if (time > now_time || date > dates) {
					return {
						'status': 0,
						'msg': '出生日期不合规'
					}
				}
				//校验码判断
				var c = new Array(7, 9, 10, 5, 8, 4, 2, 1, 6, 3, 7, 9, 10, 5, 8, 4, 2); //系数
				var b = new Array('1', '0', 'X', '9', '8', '7', '6', '5', '4', '3', '2'); //校验码对照表
				var id_array = id.split("");
				var sum = 0;
				for (var k = 0; k < 17; k++) {
					sum += parseInt(id_array[k]) * parseInt(c[k]);
				}
				if (id_array[17].toUpperCase() != b[sum % 11].toUpperCase()) {
					return {
						'status': 0,
						'msg': '身份证校验码不合规'
					}
				}
				return {
					'status': 1,
					'msg': '校验通过'
				}
			}

            function checkInput(input) {
                if (input.value.trim() === '') {
                    // 输入框为空，不做任何操作或标红边框
                    // 可以在这里添加代码处理输入框为空的情况
                    input.style.border = '2px solid red';
                } else {
                    // 输入框有内容，不标红边框
                    // 可以在这里添加代码处理输入框有内容的情况
                    input.style.border = '';
                }
            }

			$(function() {
				$('#get_order_commit').click(function() {
					let order_type = document.getElementById("order_type").value.trim();
					let order_date = document.getElementById("order_date").value.trim();
					let order_parce = document.getElementById("order_parce").value.trim();
					let order_count = document.getElementById("order_count").value.trim();
					let order_name = document.getElementById("order_name").value.trim();
					let order_phone = document.getElementById("order_phone").value.trim();
					let order_idtype = document.getElementById("order_idtype").value.trim();
					let order_idcard = document.getElementById("order_idcard").value.trim();
					let order_kami = document.getElementById("order_kami").value.trim();
					let order_mark = document.getElementById("order_mark").value.trim();


					if (order_type == null || order_type == '') {
						alert("类型为空。。");
						return false;
					} else if (order_date == null || order_date == '') {
						alert("日期为空。。");
						return false;
					} else if (order_parce == null || order_parce == '') {
						alert("价格 为空。。");
						return false;
					} else if (order_count == null || order_count == '') {
						alert("数量 为空。。");
						return false;
					} else if (order_name == null || order_name == '') {
					    document.getElementById("order_name").style.border = '2px solid red';
						// alert("联系姓名 为空。。");
						// customAlert("联系姓名 为空。。");
						customAlert("联系姓名 为空。。").then(result => {console.log(result);});
						return false;
					}
<!--					else if (order_phone == null || order_phone == '') {-->
<!--						alert("手机号码 为空。。");-->
<!--						return false;-->
<!--					} -->
					else if (order_idtype == null || order_idtype == '') {
						alert("证件类型 为空。。");
						return false;
					} else if (order_idcard == null || order_idcard == '') {
					    document.getElementById("order_idcard").style.border = '2px solid red';
						// alert("证件号码 为空。。");
						// customAlert("证件号码 为空。。");
						customAlert("证件号码 为空。。").then(result => {console.log(result);});
						return false;
					} else {
						var reg = /^[1][0-9]{10}$/;
						if (testid(order_idcard).status == 0) {
						    document.getElementById("order_idcard").style.border = '2px solid red';
							alert("身份证校验不通过。。");
							return false;
                        }
<!--                        else if (reg.test(order_phone) == false) {-->
<!--							alert("手机号码格式不对。。");-->
<!--							return false;-->
<!--						}-->
						else {
							console.log("提交成功" + order_kami);

                            // 处理提交卡密 兑换的情况
                            let order_pay_status="兑换中"; //卡密有值的情况
                            if (order_kami== null || order_kami == ''){
                                // kami 为空的情况
                                order_pay_status="待支付";
                            }

							console.log("提交order_pay_status----->" + order_pay_status);

							let jsonres_data = {
								"order_type": order_type,
								"order_date": order_date,
								"order_parce": order_parce,
								"order_count": order_count,
								"order_name": order_name,
								"order_phone": order_phone,
								"order_idtype": order_idtype,
								"order_idcard": order_idcard,
								"order_kami": order_kami,
								"order_mark": order_mark,
								"order_into_who": order_into_who,
								"order_status": "已提交",
								"order_pay_status": order_pay_status,


							};
							let uid = JSON.stringify(jsonres_data);
							console.log(uid);
							let end_date = post_order1(now_host + "/zd/ticket_order_update", uid);
							console.log('end_date' + JSON.stringify(end_date));
							// alert(end_date.msg)
                            // await customAlert(end_date.msg);
                            customAlert(end_date.msg).then(result => {console.log(result);});
							if (end_date.msg == "新增成功") {
							    if (order_pay_status=="兑换中"){
							        document.location.href=now_host+"/zd/ticket?ticket_id="+end_date.data.ticket_id;
							    }else{
							        document.location.href=now_host+"/zd/pays?ticket_id="+end_date.data.ticket_id;
							    }
							}
							if (end_date.code == "203") {
								document.location.href=now_host+"/zd/ticket?ticket_id="+end_date.data.ticket_id;
							}


						}
					};


				})

				$('#shibie').click(function() {
					let long_text = document.getElementById("alertorder_text").value.trim();
					console.log(long_text);
					if (long_text.length == 0) {
						return alert("空内容，拆分不了");
					};
					let regex_id = /(\d{18})|(\d{17}(\d|X|x))/;
					let id_end = long_text.match(regex_id);
					if (id_end == null) {
						return alert("没有拆分到证件号");
					};
					id_end = id_end.filter(n => n);
					let res_idcard = id_end.filter((num) => {
					 return num.length == 18;
					});
					res_idcard = [...new Set(res_idcard)];
					document.getElementById("order_idcard").value = res_idcard[0];
					console.log("res_idcard:" + res_idcard);

					let long_text_er = long_text.replace(res_idcard, "");
					let regex_mob = /[1][3,4,5,6.7,8,9][0-9]{9}/;
					let mob_end = long_text_er.match(regex_mob);
					if (mob_end == null) {
						return alert("没有拆分到手机号");
					};
					mob_end = mob_end.filter(n => n);
					let res_mob = mob_end.filter((num) => {
						return num.length == 11;
					});
					document.getElementById("order_phone").value = res_mob[0];

					long_text_name = long_text.split("\n");
					long_text_name = long_text_name.filter(n => n);
					let res_name1 = long_text_name.filter((num) => {
						return num.match(/名/) != null;
					});

					console.log("33:" + res_name1);
					if (res_name1.length == 0) {
						let regName = /[\u4e00-\u9fa5]{2,4}/;
						res_name1 = long_text_name.filter((num) => {
							return num.match(regName).length > 0;
						});

					} else {
						let long_text_name = res_name1[0].split(" ");
						res_name1 = long_text_name.filter((num) => {
							return num.match(/名/) == null;
						});
					}
					document.getElementById("order_name").value = res_name1[0];



				})

				$('#select_order_type').click(function() {
					let order_type = document.getElementById("order_type").value.trim();
					// let order_kami = document.getElementById("order_kami").value.trim();
					window.location.href="http://"+window.location.host +"/zd/ticket_details"+"?order_type="+order_type+"&info_id="+order_type;

				})

			});





</script>
<script>
			function order_pay_onlin() {
				$(".alert").show();
				$(".over").show();
				$(".config_list").show();
				document.getElementById("detail_submit").value = "确认修改";
				document.getElementById("alertorder_text").value = "点击「获取配置」可以显示当前的配置信息";
				var clickfunction = this;
				var td = $(this);
				$children_list = td.parent().parent().children();
				for (var i = 0, len = $children_list.length; i < len; i++) {
					$(".detail_list input")[i].value = $children_list[i].innerText;

				}


			}





</script>
<script>
			function post_order1(url, jsonres_data) {
				$.ajax({
					type: "post",
					url: url,
					async: false,
					data: jsonres_data,
					dataType: "json",
					contentType: "application/json",
					headers: {
						"Content-Type": "application/json",
					},
					success: function(result) {
						jsonres_data = result;

					},
					error: function() {
						jsonres_data = {
							"status": "201",
							"data": {
								"text": "服务器异常。。。",
								"order_count": 0,
								"__log_id_": ""
							}
						}

					}
				});
				console.log(jsonres_data);
				return jsonres_data;
			};





</script>


<script type="text/javascript">
    if (window.location.search.substring(1).indexOf('ticket_id=')==-1 || new URLSearchParams(window.location.search.substring(1)).get('ticket_id')==''){
        // 不存在订单信息时的处理
        // 先查询是否 已选订单时间，已选订单时间 则填充，未选则默认当前日期
        if (window.location.search.substring(1).indexOf('order_date=')==-1 || new URLSearchParams(window.location.search.substring(1)).get('order_date')==''){
            var date = new Date();
            var seperator1 = "-";
            var year = date.getFullYear();
            var month = date.getMonth() + 1;
            var strDate = date.getDate();
            if (month >= 1 && month <= 9) {
                month = "0" + month;
            }
            if (strDate >= 0 && strDate <= 9) {
                strDate = "0" + strDate;
            }
            var currentdate = year + seperator1 + month + seperator1 + strDate;
            document.getElementById("order_date").value = currentdate; //未选则默认当前日期
        }else(
            // 已选订单时间 则填充
            document.getElementById("order_date").value =window.location.search.match(new RegExp('order_date' + '=(.*?)(&|$)'))[1]

        )
        // 处理是否存在 info_id
        if (window.location.search.substring(1).indexOf('info_id=')==-1 || new URLSearchParams(window.location.search.substring(1)).get('info_id')==''){
            //未传 info_id
            // 查询 所有的 info_info
            let jsonres_data = {
                "order_date": document.getElementById("order_date").value.trim()  //传时间，当前后端不存在该字段
            }
            let uid = JSON.stringify(jsonres_data);
            let ticket_order_info = post_order1(now_host + "/zd/ticket_order_info", uid);
            console.log("ticket_order_info 已存在"+ticket_order_info);

            // 渲染界面
            let jsonres = ticket_order_info;
            // console.log(jsonres);
            let gList = jsonres.data.text;
            for (var i = 0; i < gList.length; i++) {
                let studentName = gList[i].ticket_type;
                let student_Name = gList[i].info_id;;
                var obj = document.getElementById("order_type");
                var varItem = new Option(studentName, student_Name);
                obj.options.add(varItem);
                if (i == 0) { //默认显示第一条数据
                    document.getElementById("order_parce").value = '¥ ' + gList[i].ticket_discount_price;
                    document.querySelector("#order_parce1 > span").textContent = '¥ ' + gList[i].ticket_discount_price;
                    document.querySelector("#original_price").value = '¥ ' + gList[i].ticket_original_price;
                    document.querySelector("#original_price").style.textDecoration = 'line-through';

                }
            }



        }else{
            //已传 info_id
            //查询单条
            let jsonres_data = {
                "order_date": document.getElementById("order_date").value.trim() , //传时间，当前后端不存在该字段 goto
                "info_id": new URLSearchParams(window.location.search.substring(1)).get('info_id') ,
            }
            let uid = JSON.stringify(jsonres_data);
            let ticket_order_info = post_order1(now_host + "/zd/ticket_order_info", uid);
            console.log("ticket_order_info 已存在"+ticket_order_info);

            // 渲染界面
            let jsonres = ticket_order_info;
            // console.log(jsonres);
            let gList = jsonres.data.text;
            for (var i = 0; i < gList.length; i++) {
                let studentName = gList[i].ticket_type;
                let student_Name = gList[i].info_id;;
                var obj = document.getElementById("order_type");
                var varItem = new Option(studentName, student_Name);
                obj.options.add(varItem);
                if (i == 0) { //默认显示第一条数据
                    document.getElementById("order_parce").value = '¥ ' + gList[i].ticket_discount_price;
                    document.querySelector("#order_parce1 > span").textContent = '¥ ' + gList[i].ticket_discount_price;
                    document.querySelector("#original_price").value = '¥ ' + gList[i].ticket_original_price;
                    document.querySelector("#original_price").style.textDecoration = 'line-through';

                }
            }


        }


        // order_kami
        if (window.location.search.substring(1).indexOf('order_kami=')==-1 || new URLSearchParams(window.location.search.substring(1)).get('order_kami')==''){


        }else{

            console.log("kami 已存在");
            document.getElementById("order_kami").value = window.location.search.match(new RegExp('order_kami' + '=(.*?)(&|$)'))[1];

            let jsonres_data = {
                "order_kami": document.getElementById("order_kami").value
            }
            let uid = JSON.stringify(jsonres_data);
            let end_text = post_order1(now_host + "/zd/order_ticket_select", uid);
            if (end_text.status=="200"){ //当前 kami 存在兑换记录，直接跳转到对应的订单信息，
                window.location.href=now_host +"/zd/ticket"+"?ticket_id="+end_text.data.text[0].ticket_id;
                // 未处理多个订单记录的情况 goto
            }

        }


    }else{ // ticket_id 存在订单记录时的处理方式
        console.log("ticket_id 已存在"+new URLSearchParams(window.location.search.substring(1)).get('ticket_id').toString());

        // 查询订单详情
        let jsonres_data = {
            "ticket_id": new URLSearchParams(window.location.search.substring(1)).get('ticket_id')
        }
        let uid = JSON.stringify(jsonres_data);
        let end_text = post_order1(now_host + "/zd/order_ticket_select", uid);
        console.log("end_text 已存在"+end_text);
        if (end_text.status=="200"){
            let gList_dict = end_text.data.text[0]; //查询第一个结果
            Object.keys(gList_dict).forEach(key => {
                if (gList_dict[key]!=""){
                    console.log(key, gList_dict[key]);
                    if (document.getElementById(key)!=null){
                        document.getElementById(key).disabled=true;
                        document.getElementById(key).value=decodeURI(gList_dict[key]);
                        if (key=='order_pay_status'){
                            document.getElementById(key).innerHTML=decodeURI(gList_dict[key]);
                            if (decodeURI(gList_dict[key]) =='已支付'){
                                document.getElementById("pay_status_dia").innerHTML ='请等待,出票中。。。。';
                                document.getElementById("pay_status_button").style.display = "none";
                            };
                            if (decodeURI(gList_dict[key]) =='兑换中'){
                                document.getElementById("pay_status_dia").innerHTML ='请等待,出票中。。。。';
                                document.getElementById("pay_status_button").style.display = "none";
                            };
                        }
                        if (key=='order_status'){
                            document.getElementById(key).innerHTML=decodeURI(gList_dict[key]);
                        }
                        if (key=='ticket_add_time'){
                            document.getElementById(key).innerHTML="订单创建时间："+decodeURI(gList_dict[key]);
                        }
                        // if ()
                    }


                }
            });

            document.getElementById("alertorder_text").style.display = "none";
            document.getElementById("shibie").style.display = "none";
            document.getElementById("z-detail-button").style.display = "none";
            document.getElementById("page-title").innerHTML = "订单详情";
            document.getElementById("pay_tail").style.display = ""; //支付状态数据展示


            let studentName = gList_dict.ticket_type; //name
            let student_id = gList_dict.order_type;//id
            var obj = document.getElementById("order_type");
            var varItem = new Option(studentName, student_id);
            obj.options.add(varItem);

            document.getElementById("select_order_type").onclick = function () {
                window.location.href=now_host +"/zd/ticket_details"+"?info_id="+student_id;
            }

            document.getElementById("get_order_commit_button").onclick = function () {
                window.location.href=now_host +"/zd/pays?ticket_id="+new URLSearchParams(window.location.search.substring(1)).get('ticket_id');
            }

        }else{
            console.log("end_text 订单不存在----->"+end_text.data.text);
            alert("订单不存在----->"+end_text.data.text)
        }

       }




</script>

<script>
			//监听下拉框变化
			$('select.order_type').change(() => {
				let seeType = $('select.order_type').val();
				let jsonres_data = {
					"info_id": seeType
				}
				let uid = JSON.stringify(jsonres_data);
				let end_text = post_order1(now_host + "/zd/ticket_order_info", uid);
				let gList = end_text.data.text;
				console.log(gList);
				if (gList.length == 1) {
					document.getElementById("order_parce").value = '¥ ' + gList[0].ticket_agency_price;
					document.querySelector("#order_parce1 > span").textContent = '¥ ' + gList[0].ticket_agency_price;
					document.querySelector("#original_price").value = '¥ ' + gList[0].ticket_original_price;
					document.querySelector("#original_price").style.textDecoration = 'line-through';
				} else {
					document.getElementById("order_parce").value = '¥ 0';
					document.querySelector("#order_parce1 > span").textContent = '¥ 0';
					document.querySelector("#original_price").value = '¥ 0';
					document.querySelector("#original_price").style.textDecoration = 'line-through';
				}

			});





</script>

<style>
			.over {
				display: none;
				background-color: rgba(0, 0, 0, 0.7);
				height: 100%;
				left: 0;
				position: fixed;
				top: 0;
				width: 100%;
				z-index: 998;
			}

			.alert {
				display: none;
				left: 50%;
				position: fixed;
				top: 30%;
				transform: translateX(-50%) translateY(-50%);
				width: 100%;
				height: 60%;
				z-index: 999;
			}

			.title {
				background-color: #f54c53;
				color: #fff;
				font-size: 40%;
				line-height: 80%;
				width: 100%;
			}

			.close {
				display: block;
				height: 36px;
				position: absolute;
				right: 30px;
				top: 0;
				width: 80px;
			}

			.content {
				background-color: #dddada;
				color: #747474;
				font-size: 40%;
				padding: 1%;
				margin-top: -1%;
				height: 100%
			}

			.important {
				color: #f54c53;
			}

			.over {
				hide
			}

			.detail_list input {
				display: inline-block;
				padding: 3px 6px;
				width: 33%;
				vertical-align: top;
			}

			.detail_list label {
				display: inline-block;
				padding: 3px 6px;
				text-align: right;
				width: 33%;
				vertical-align: top;
			}





</style>
<div class="over"></div>
<div class="alert" style="height:expression(this.height < 150 ? '150px' : this.height+'px');">
    <h2 class="title">
        <span style="padding-left:5%;line-height: 300%;"> 操作 </span>
        <input type="button" id="detail_cancel" value="取消"
               style="background-color: #F5F5F5;font-size: 20%;line-height: 250%; float: right;"/>
    </h2>
    <div class="content" style="">

        <br/>

    </div>
</div>

<script>

</script>
</body>
</html>
