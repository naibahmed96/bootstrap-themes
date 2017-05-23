// 设置主菜单选中状态
$("body").on('click', '[lazyLoadTo]', 
  function(e) {
	var primary = $(this).parents('.nav-primary');
	if (primary) {
		primary.find('.active').removeClass('active');
		$(this).parents('li').addClass('active');
	}
  }
);

// 设置页面打开时的菜单选中状态
function setMenu() {
	$('.nav-primary li').each(function () {
	    var a = $(this).children('a').first();
	    if (a.attr('href') == LazyLoadFrom[0]) {
	        $('.nav-primary').find('.active').removeClass('active');
	        $(this).addClass('active');
	    }
	});
}