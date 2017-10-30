---
title: ajax,jsonp的理解
---
##Ajax原生实现
  ####1.创建 XMLHttpRequest实例

    if(window.XMLHttpRequest)
    {
        var oAjax=new XMLHttpRequest();
    }
    else
    {
        var oAjax=new ActiveXObject('Microsoft.XMLHttp');
    }
####2.连接

    if(type=='get')
    {
        oAjax.open('get', url+'?'+arr.join('&'), true);
        oAjax.send();
    }
    else
    {
        oAjax.open('post', url, true);
        oAjax.setRequestHeader('content-type', 'application/x-www-form-urlencoded');
        oAjax.send(arr.join('&'));
    }
####3.接收

    oAjax.onreadystatechange=function ()
    {
        if(oAjax.readyState==4)
        {	
            if(oAjax.status>=200 && oAjax.status<300 || oAjax.status==304)
            {
                fnSucc && fnSucc(oAjax.responseText);
            }
            else
            {
                fnFaild && fnFaild();
            }
        }
    };
##jsonp原理

1.页面直接定义回调函数，用script标签传入回调函数名

	 <script type="text/javascript">  
	     function jsonpCallback(result) {  
			console.log(reult); 
	     }  
	 </script>
	<script src="remote.php?callback=jsonpCallback">  
	
 或者动态生成script标签

	<script type="text/javascript">  
    function jsonpCallback(result) {
		console.log(result);  
    }  
    var JSONP=document.createElement("script");   
    JSONP.src="remote.php?callback=jsonpCallback";  
    document.getElementsByTagName("head")[0].appendChild(JSONP);  
	</script> 
服务器输出

	echo $callback."($result)";

2.实现封装
	
	function json2url(json){
		var arr=[];
		for(var name in json){
			arr.push(name+'='+json[name]);
		}
		return arr.join('&');
	}
	function jsonp(json){
		json=json || {};
		if(!json.url)return;
		json.data=json.data || {};
		json.cbName=json.cbName || 'cb';
		
		
		var fnName='jsonp'+Math.random();
		fnName=fnName.replace('.','');
		window[fnName]=function(data){
			json.success && json.success(data);
			
			//删除
			oHead.removeChild(oS);
			window[fnName]=null;
		};
		
		json.data[json.cbName]=fnName;
		
		
		var oS=document.createElement('script');	
		oS.src=json.url+'?'+json2url(json.data);
		var oHead=document.getElementsByTagName('head')[0];
		oHead.appendChild(oS);
		
	}