	//validate 选项***********************************************************  
        $("form").validate({  
  
            debug:true  //进行调试模式（表单不提交）  
            rules:{  
                name:"required", //自定义规则,key:value的形式,key是要验证的元素,value可以是字符串或对象  
                email:{  
                    //内置验证方式  
                    required:true, //必填项  
                    required:"#aa:checked"表达式的值为真,则必填项  
                    required:function(){}返回为真,则必填项  
                    email:true,   //验证电子邮箱格式  
                    minlength:5,  //设置最小长度  
                    maxlength:10, //设置最大长度  
                    rangelength:[5,10],//设置一个长度范围[min,max]  
                    min:2,        //设置最小值  
                    max:8,       //设置最大值  
                    range:[2,8]      //设置值的范围  
                    url:true,         //验证URL格式  
                    date:true,    //验证日期格式(类似30/30/2008的格式,不验证日期准确性只验证格式)  
                    dateISO:true, //验证ISO类型的日期格式 例如：2009-06-23  
                    dateDE:true,  //验证德式的日期格式（29.04.1994 or 1.1.2006）  
                    number:true,  //验证十进制数字（包括小数的）  
                    digits:true,  //验证整数  
                    creditcard:true, //验证信用卡号  
                    accept:""     //请输入拥有合法后缀名的字符串（上传文件的后缀）  
                    equalTo:"id名" //验证两个输入框的内容是否相同  
                    phoneUS:true   //验证美式的电话号码  
                    regex:/正则表达式/     //上面addMethod扩展的验证规则  
                }  
            }  
  
            messages:{  
                name:"Name不能为空",  //自定义的提示信息key:value的形式key是要验证的元素,值是字符串或函数  
                email:{  
                   required:"E-mail不能为空",  
                   email:"E-mail地址不正确"  //validate 内置验证有默认的英语提示 此处为重新自定义  
                }  
            }  
  
            errorPlacement: function(error,element) {  
                element.parents('.form-group').children(".help-block").html(error); //错误显示的位置 element验证的元素error错误提示  
            }  
  
            submitHandler:function(form) {//通过验证后运行的函数,里面要加上表单提交的函数,否则表单不会提交  
                $(form).ajaxSubmit();    
                //form.submit();  
            }  
  
            success:"类名"   //要验证的元素通过验证后的动作,跟一个字符串,会给输出错误的元素追加一个css类  
            ignore:".ignore" //对某些元素不进行验证  
            onsubmit:false   //是否提交时验证 默认:true  
            onfocusout:false //是否在获取焦点时验证 默认:true   
            onkeyup:false    //是否在敲击键盘时验证 默认:true  
            onclick:false    //是否在鼠标点击时验证(一般验证checkbox,radiobox) 默认:true  
            focusInvalid:false //提交表单后,未通过验证的表单(第一个或提交之前获得焦点的未通过验证的表单)会获得焦点 默认:true  
            focusCleanup:true  //当未通过验证的元素获得焦点时,并移除错误提示（避免和 focusInvalid.一起使用）默认:false  
            errorClass:"类名"  //指定错误提示的css类名,可以自定义错误提示的样式 默认:"error"  
            errorElement:"标签" //使用什么标签标记错误 默认:"label"  
            wrapper:"标签"      //使用什么标签再把上边的errorELement包起来  
            errorLabelContainer:"容器id"  //把错误信息统一放在一个容器里面  
            showErrors:function(errorMap,errorList) { //跟一个函数,可以显示总共有多少个未通过验证的元素  
                $("#summary").html("Your form contains " + this.numberOfInvalids() + " errors,see details below.");  
                this.defaultShowErrors();  
            }  
        })  
  
  
        //validate方法 返回一个Validator对象,它有很多方法, 让你能使用引发校验程序或者改变form的内容**************  
  
        $.validator.setDefaults({//设置validator默认  
            debug:true,  //把调试模式设置为默认 如果一个页面中有多个表单一般用这种方式  
            errorClass: "txt-impt", //设置默认错误提示的css类名  
            errorElement: "div"     //设置默认错误提示的标签  
        })  
  
        //addMethod(name,method,message)方法：name(自定义rules的key) method(自定义验证方法) message(报错输出的提示)  
        jQuery.validator.addMethod("regex",function(value, element, params){  //扩展方法示例:　　　　　　　　　　　  
            var exp = new RegExp(params); //params rules的value传入的正则表达式  
            return exp.test(value);       //value  被验证的input传入的值  
        },"输入格式有误");  
  
        //扩展rules规则  
        jQuery.validator.addClassRules("name", {  
            required: true,  
            minlength: 2  
        });  
        jQuery.validator.addClassRules({  
            name: {  
                required: true,  
                minlength: 2  
            },  
            zip: {  
                required: true,  
                digits: true,  
            }  
        });  
        $("#myinput").rules("add", {    
            required: true,    
            minlength: 2,    
            messages: {    
                required: "Required input",    
                minlength: jQuery.format("Please, at least {0} characters are necessary")    
            }    
        });   
        $("#myinput").rules("remove"); //全部移除验证规则  
        $("#myinput").rules("remove", "min max") //移除 min max  
  
        var form=$('form');  
        $(".formBtn").click(function(){ //按钮type非submit or submit按钮不在form内  
            console.log("Valid: " + form.valid()) //form.valid() 验证成功返回true  
            var validator = $("form").validate(setValidate);  
            var formState=validator.form();      //判断验证状态 返回Boolean  
                //validator.element("id名") 验证某个元素 返回Boolean  
                //validator.resetForm()  把前面验证的FORM恢复到验证前原来的状态  
                /*validator.showErrors({ 
                    "firstname": "I know that your firstname is Pete, Pete!" 
                }); 显示自定义的错误信息 */  
  
            if(formState==false){  
                return;  
            }else{  
                //do someing...  
            }     
        })  
  
        //使用ajax方式进行验证，默认会提交当前验证的值到远程地址，如果需要提交其他的值，可以使用data选项 后台只允许返回false和true  
        remote: "check-email.php"    
        remote: {    
            url: "check-email.php",     //后台处理程序    
            type: "post",               //数据发送方式    
            dataType: "json",           //接受数据格式       
            data: {                     //要传递的数据    
                username: function() {    
                    return $("#username").val();    
                }    
            }    
        }    
  
  
        //meta String方式***************************************************************  
  
        //引入js  
        <script type="text/javascript" src="js/jquery.metadata.js"></script>  
        <script type="text/javascript" src="js/jquery.validate.js"></script>  
  
        //dom上验证规则写法  
        <input type="text" name="email" class="{validate:{ required:true,email:true }}" />  
  
        //设置为meta String验证方式  
        $("#myform").validate({  
           meta:"validate",  
           submitHandler:function() { alert("Submitted!") }  
        }) 