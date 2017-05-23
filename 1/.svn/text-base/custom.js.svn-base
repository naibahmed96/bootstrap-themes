/*************************************************
 * 通用
 ************************************************/
/**
 * Highcharts设置为本地时间
 */
Highcharts.setOptions({ global: { useUTC: false } });


/*************************************************
 * 模块管理
 ************************************************/
/**
 * autoShowFor 功能，在添加代码模块中有使用
 */
function autoShowFor() {
	$('[autoShowFor]').each(function () {
		var autoShowFor = $(this).attr('autoShowFor');
		var autoShowForArr = autoShowFor.split(',');
		for (x in autoShowForArr) {
			if ($(autoShowForArr[x]).is(':checked')) {
				$(this).show();
				return;
			}
		}

		$(this).removeClass('active');
		$(this).find('input').prop("checked", false);
		$(this).hide();
	}); 
}