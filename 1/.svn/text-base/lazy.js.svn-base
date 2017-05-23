/**
 * 实现了页面的异步加载功能
 *

= 实现了什么功能 =
实现页面的局部加载功能。

= 怎么使用 =
== 三种类型 == 
=== 自动加载 ===
使用 lazyLoadAuto 与 src （或者href、action、lazyLoadFrom）属性，页面加载完成后，将使用指定URL的内容替换放置在当前元素内。
<section src="/product_list.html" lazyLoadAuto>

=== 点击加载 ===
使用 lazyLoadClick、lazyLoadTo与 src（或者href、action、lazyLoadFrom）属性，点击指定内容后，将使用href的内容替换放置在当前元素内。
<a href="/code/create.html" lazyLoadTo=".lazy_box" lazyLoadClick>

=== 表单提交加载 ===
使用 lazyLoadSubmit、lazyLoadTo与 action（或者href、src、lazyLoadFrom）属性，点击指定内容后，将使用href的内容替换放置在当前元素内。
<a href="/code/create.html" lazyLoadTo=".lazy_box" lazyLoadSubmit>
<form action="form_action.asp" method="get"></form>

== 属性说明 ==
动作属性：lazyLoadAuto、lazyLoadClick、lazyLoadSubmit，一个元素只能选择使用一个。
内容来源属性：lazyLoadFrom、href、src、action。权重由高到低。lazyLoadClick 支持以逗号分隔的数组。
内容填充区块属性：lazyLoadTo，后面填写Jquery选择符。其中lazyLoadAuto由于是加载本身，省略这个属性。lazyLoadClick 支持以逗号分隔的数组。

== 编程接口 == 
无法通过上面三种方式实现的功能，使用编程接口实现。

== 加载内容接口 ==
=== function lazyLoadContent(lazyLoadFrom, lazyLoadTo) === 
lazyLoadFrom 内容来源，比如 "https://www.baidu.com"
lazyLoadTo Jquery选择的对象，比如$('#box')

=== function lazyLoadSubmit(lazyLoadFrom, lazyLoadTo) === 
** 开发中 ** 

 *
 */
var LazyLoadFrom = new Array();
var LazyLoadTo = new Array();
$(document).ready(function () {
	analysisUrl();
	initLazy();

	// 绑定点击事件
	$("body").on('click', '[lazyLoadClick]', function(e) {
		console.log(getLazyFrom($(this)));
		console.log(getLazyTo($(this)));
		lazyLoadContent(getLazyFrom($(this)), getLazyTo($(this)));
		setUrlAndHistory(getLazyFrom($(this)), getLazyTo($(this)));
		return false;
	});

	$(document).trigger("lazyLoadOk")
});

// 获取LazyTo
function getLazyTo($this) {
	return $this.attr('lazyLoadTo');
}

// 获取lazyFrom
function getLazyFrom($this) {
	if ($this.attr('lazyLoadFrom'))
		return $this.attr('lazyLoadFrom');
	if ($this.attr('href'))
		return $this.attr('href');
	if ($this.attr('src'))
		return $this.attr('src');
	if ($this.attr('action'))
		return $this.attr('action');
	return false;
}

// 内容加载
function lazyLoadContent(lazyFrom, lazyTo) {
	if (!lazyFrom || !lazyTo)
		return;
	var lazyFromArr = lazyFrom.split(',');
	if (!(lazyTo instanceof Object)) {
		var lazyToArr = lazyTo.split(',');
	}
	//console.log(lazyFromArr);
	//console.log(lazyTo);
	for (i = 0; i < lazyFromArr.length; i++) {
		if (lazyTo instanceof Object) {
			lazyTo.removeAttr('lazyLoadAuto');
			lazyTo.load(lazyFromArr[i], initLazy);
		} else {
			$(lazyToArr[i]).removeAttr('lazyLoadAuto');
			$(lazyToArr[i]).load(lazyFromArr[i], initLazy);
		}
	}
	return true;
}

// 表单内容加载
function lazyLoadSubmit() {
	$("[lazyLoadSubmit]").each(function() {
		lazyLoadTo = getLazyTo($(this));
		var options = {
			target: lazyLoadTo,
			success: initLazy
		};
		$(this).ajaxForm(options);
		$(this).removeAttr('lazyLoadSubmit');
	});
}

// 自动填充
function lazyLoadAuto() {
	$("[lazyLoadAuto]").each(function() {
		lazyLoadContent(getLazyFrom($(this)), $(this));
	});
}

// 系统初始化时候需要调用的函数
function initLazy() {
	// 绑定自动加载事件
	lazyLoadAuto();

	// 绑定表单时间
	lazyLoadSubmit();
}

/**
 * 以下代码用来记录操作历史，拥有回退功能
 * 基本原理就是使用 history.pushState 来修改浏览器历史记录
 */
function setUrlAndHistory(lazyFrom, lazyTo) {
	var stateObject = {};
	var title = document.title;
	var href = window.location.href;
	//console.log(href);

	exist = inArray(lazyTo, LazyLoadTo);
	console.log(exist);
	if (exist < 0)
	{
		LazyLoadTo.push(lazyTo);
		LazyLoadFrom.push(lazyFrom);
	}
	else
	{
		LazyLoadFrom[exist] = lazyFrom;
		LazyLoadTo.splice(exist+1, LazyLoadTo.length - exist);
		LazyLoadFrom.splice(exist+1, LazyLoadTo.length - exist);
	}

	if (href.indexOf('#') < 0)
		var newUrl = href + '#' + createUrl();
	else
		var newUrl = href.substr(0, href.indexOf('#')) + '#' + createUrl();

	history.pushState(stateObject, title, newUrl);
}

function analysisUrl() {
	var href = window.location.href;
	var url = href.substr(href.indexOf('#') + 1);
	if (!url)
		return;
	var urlArr = url.split(';');
	for (x in urlArr) {
		lazyTmp = urlArr[x].split(',');
		if (!lazyTmp[0] || !lazyTmp[1])
			return;

		LazyLoadFrom.push(lazyTmp[0]);
		LazyLoadTo.push(lazyTmp[1]);
		console.log(lazyTmp);
		lazyLoadContent(lazyTmp[0], lazyTmp[1]);
	}
}

function createUrl() {
	var url = new Array();
	for (x in LazyLoadFrom) {
		url.push(LazyLoadFrom[x] + ',' + LazyLoadTo[x]);
	}
	return url.join(';');
}

function inArray(val, array) {
	for (x in array) {
		if (array[x] == val)
			return x;
	}
	return -1;
}