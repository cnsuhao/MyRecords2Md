---
title: Extjs结合JfreeChart批量生成图片用于用户选择组件时预览（接昨天博文）
date: 2012-12-17 19:39:59
categories: "开发"
tags:
	- Ext
	- js
	- Chart

---

这是今天做的统计图的预览图片功能，用于选择组件时,用户可以看到统计图的静态展示样式。

``````````
/***********************************
	 * 处理过程
	 **********************************/
	@SuppressWarnings("unchecked")
	@Override
	public void handler() {
				
		List list = this.componentForImageDao.queryComponentDao("pie");// 查询图形为pie的组件

		for (int i = 0, len = list.size(); i < len; i++) {
			HashMap obj = (HashMap) list.get(i);
			String comType = (String) obj.get("componenttype");
			String zjId = (String) obj.get("id");// 用于图片命名
			String zjMc = (String) obj.get("mc");// 用于标题
			
			List zbList = this.componentForImageDao.queryZbData(zjId);
			List wdList = this.componentForImageDao.queryWbData(zjId);
			List fwList = this.componentForImageDao.queryFwData(zjId);
			// 范围 维度
			String zbSQL = ((Map) zbList.get(0)).get("sql").toString();// 指标一个
			String wdSQL = ((Map) wdList.get(0)).get("sql").toString();
			String wdCxzdm = ((Map) wdList.get(0)).get("cxzdm").toString();

			DefaultPieDataset dpd = new DefaultPieDataset();
			if (fwList.size() >= 1 && zbList.size() <= 1) {
				for (int f = 0, lenF = fwList.size(); f < lenF; f++) {
					String fwMc = ((Map) fwList.get(f)).get("mc").toString();
					String fwtj = ((Map) fwList.get(f)).get("fwtj").toString();
					List _wdList = this.componentForImageDao
							.queryComputerResult(wdSQL);
					for (int k = 0, lenK = _wdList.size(); k < lenK; k++) {
						StringBuilder sb = new StringBuilder(zbSQL);
						Object[] _wdResult = (Object[]) _wdList.get(k);
						Object id = _wdResult[0];
						Object mc = _wdResult[1];
						sb.append(" and ").append(wdCxzdm).append("=").append(
								id);
						sb.append(" and " + fwtj);// fanwei

						List result = this.componentForImageDao
								.queryComputerResult(sb.toString());
						BigDecimal resultData = (BigDecimal) result.get(0);
						dpd.setValue(mc.toString(), Double.valueOf(resultData
								.toString()));
					}
				}
			} else if (fwList.size() <= 1 && zbList.size() >= 1) {
                
			}
			this.generatePie2d(zjMc, dpd, zjId);
		}
	}

	// 生成2d图形
	private void generatePie2d(String name, DefaultPieDataset dpd, String id) {
		try {
			int width = 580, height = 370;
			JFreeChart chart = ChartFactory.createPieChart("输出图", dpd, true,
					true, false);
			chart.setTitle(new TextTitle(name, new Font("宋体", Font.BOLD
					+ Font.ITALIC, 20)));

			LegendTitle legend = chart.getLegend(0);// 设置Legend
			legend.setItemFont(new Font("宋体", Font.BOLD, 14));
			PiePlot plot = (PiePlot) chart.getPlot();// 设置Plot
			plot.setLabelFont(new Font("隶书", Font.BOLD, 16));

			OutputStream os = new FileOutputStream(
					"WebRoot/testImage/out_put_image_" + id.toString() + ".gif");// 图片是文件格式的，故要用到FileOutputStream用来输出。
			ChartUtilities.writeChartAsJPEG(os, chart, width, height);

			os.close();// 关闭输出流

		} catch (Exception e) {
			e.printStackTrace();
		}
	}
``````````

这是后台,批量生成后放置在webroot下的testImage文件夹下,命名使用了固定的样式和id表示，格式可以是出gif外的其他图片格式（jpeg png我试了都可以）。

前台是这么用的：

``````````
onMouseOut : function() {
		this.moveWin.hide();
	},
	onMouseOver : function(id) {
		var me = this, textColor = this.textColor = '#23FAB3';
		var image = 'url(./testImage/out_put_image_' + id + '.gif);';
		var winHeight = 400, winWidth = 600;
		if (this.moveWin && this.moveWin != null) {

		} else {
			var window = this.moveWin = Ext.create('Ext.window.Window', {
						title : 'imagin',
						height : winHeight,
						width : winWidth
					});
			window.addListener({
						close : function() {
							me.moveWin = null;
						}
					});
			var docHeight = document.documentElement.clientHeight;
			var docWidth = document.documentElement.clientWidth;
			window.setPosition(docWidth - winWidth, docHeight - winHeight);
		}
		// 再设置位置
		me.moveWin.focus();// 总是置于最上层
		me.moveWin.setTitle(id);
		me.moveWin.setBodyStyle('background-color:' + textColor
				+ ';background-image:' + image);
		me.moveWin.show();
	}
``````````


使用id做图片的唯一标识。

这样每当鼠标移动至该组件上即可显示该组件的图片。～～

\-----------------------------------------------牵涉到很多问题,仅将部分贴出,具体思路和做法可加我Q群讨论---------------------------------------------------------

这是效果：


![EMJF-I2MA-JM22.jpg][](生成的某张图图例)![E2IM-UFUZ-R32E.jpg][](批量生成的结果截图)![RU77-ZR3E-YRUZ.jpg][]（使用截图）







[EMJF-I2MA-JM22.jpg]: /pro/os/crawler/EMJF-I2MA-JM22.jpg
[E2IM-UFUZ-R32E.jpg]: /pro/os/crawler/E2IM-UFUZ-R32E.jpg
[RU77-ZR3E-YRUZ.jpg]: /pro/os/crawler/RU77-ZR3E-YRUZ.jpg