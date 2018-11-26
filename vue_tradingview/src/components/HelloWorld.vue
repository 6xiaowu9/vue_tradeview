<template>
	<div class="hello">
		<div id="chart_container" style="background:#0D1A24;height:100%;" class="f-fill" ></div>
	</div>
</template>

<script>
	export default {
		name: "HelloWorld",
		created:function(){
			var this_vue = this;
        	this_vue.getChartData('')
		},
		mounted: function() {
			var this_vue = this;
			if(this.GetRequest().lan=='cn'||this.GetRequest().lan=='hk'){
				this_vue.lan='zh'
			}else{
				this_vue.lan='en'
			}
			this_vue.feed = this_vue.createFeed();
			TradingView.onready(function(configurationData) {
				this_vue.chart = window.tvWidget = new TradingView.widget({
					fullscreen: false,
					autosize: true,
					symbol: this_vue.currency1 + ":" + this_vue.currency2,
					container_id: "chart_container",
					datafeed: this_vue.feed,
					library_path: "static/custom_scripts/chart_main/",
					locale: this_vue.lan,
					timezone: 'Asia/Shanghai',
					charts_storage_api_version: "1.1",
					client_id: 'tradingview.com',
					user_id: 'public_user_id',
					debug: false,
					loading_screen:{ backgroundColor: "#000000",foregroundColor: "#000000", },//todo:do it
					interval: '60',
				    timeframe:this_vue.datetimes(),//todo: na koncu
					toolbar_bg: "#0D1A24",
					// study_count_limit:'10',
					// saved_data: this_vue.savedData,
					allow_symbol_change: true,
					time_frames: [
						{
							text: "1min",
							resolution: "1"
						}
					],
					drawings_access: {
						type: 'black',
						// tools: [{name: "Regression Trend"}]//todo: moje
						tools: [{
							name: "Trend Line",
							grayed: true
						}, {
							name: "Trend Angle",
							grayed: true
						}] //todo: bb
					},
					disabled_features: [
						"volume_force_overlay",
						"header_fullscreen_button",
						"left_toolbar",
						"header_screenshot",
						"show_interval_dialog_on_key_press",
						"symbol_search_hot_key",
						"study_dialog_search_control",
						"display_market_status",
						"header_compare",
						"edit_buttons_in_legend",
						"symbol_info",
						"border_around_the_chart",
						"main_series_scale_menu",
						"star_some_intervals_by_default",
						"datasource_copypaste",
						"right_bar_stays_on_scroll",
						"context_menus",
						"go_to_date",
						"compare_symbol",
						"border_around_the_chart",
						// "timezone_menu",
						"header_resolutions",//todo: przetestowac
						"header_symbol_search",
						"header_interval_dialog_button",
						// "control_bar",//todo: przetestowac
						"edit_buttons_in_legend",//todo: przetestowac
						"remove_library_container_border",
					],
					enabled_features: [
						"dont_show_boolean_study_arguments",
						"use_localstorage_for_settings",
						"remove_library_container_border",
						"save_chart_properties_to_local_storage",
						"side_toolbar_in_fullscreen_mode",
						"hide_last_na_study_output",
						"constraint_dialogs_movement", //todo: nie do końca jestem pewien
						"disable_resolution_rebuild",
					],
					studies_overrides: {
						// "volume.volume.color.0": "#a15456",
						// "volume.volume.color.1": "#668e69",
						"volume.volume.transparency": 75,
						"average true range.precision": 8,
						"moving average.precision": 8
						
					},
					overrides: {
						
						//    支持的值: large, medium, small, tiny
						"volumePaneSize": "medium",
						'scalesProperties.fontSize': 12, // 设置坐标轴字体大小
						'paneProperties.legendProperties.showLegend': false, // 隐藏左上角标题
						"symbolWatermarkProperties.color": "rgba(0,0,0, 0)",
						
						"paneProperties.background": "#0D1A24",
						"paneProperties.vertGridProperties.color": "#344568",
						"paneProperties.horzGridProperties.color": "#344568",
						"paneProperties.crossHairProperties.color": "#58637a",
						"paneProperties.crossHairProperties.style": 2,
						"mainSeriesProperties.style": this_vue.GetRequest().type=='line'?3:1,//1为K线
						"mainSeriesProperties.showCountdown": false,
						"scalesProperties.showSeriesLastValue": true,
						"mainSeriesProperties.visible": false,
						"mainSeriesProperties.showPriceLine": false,
						"mainSeriesProperties.priceLineWidth": 1,
						"mainSeriesProperties.lockScale": false,
						"mainSeriesProperties.minTick": "default",
						"mainSeriesProperties.extendedHours": false,
						editorFontsList: ["Lato", "Arial", "Verdana", "Courier New", "Times New Roman"],
						"paneProperties.topMargin": 5,
						"paneProperties.bottomMargin": 5,
						"paneProperties.leftAxisProperties.autoScale": true,
						"paneProperties.leftAxisProperties.autoScaleDisabled": false,
						"paneProperties.leftAxisProperties.percentage": false,
						"paneProperties.leftAxisProperties.percentageDisabled": false,
						"paneProperties.leftAxisProperties.log": false,
						"paneProperties.leftAxisProperties.logDisabled": false,
						"paneProperties.leftAxisProperties.alignLabels": true,
						// "paneProperties.legendProperties.showStudyArguments": true,
						"paneProperties.legendProperties.showStudyTitles": true,
						"paneProperties.legendProperties.showStudyValues": true,
						"paneProperties.legendProperties.showSeriesTitle": true,
						"paneProperties.legendProperties.showSeriesOHLC": true,
						"scalesProperties.showLeftScale": false,
						"scalesProperties.showRightScale": true,
						"scalesProperties.backgroundColor": "#0D1A24",
						"scalesProperties.lineColor": "#46587b",
						"scalesProperties.textColor": "#8f98ad",
						"scalesProperties.scaleSeriesOnly": false,
						"mainSeriesProperties.priceAxisProperties.autoScale": true,
						"mainSeriesProperties.priceAxisProperties.autoScaleDisabled": false,
						"mainSeriesProperties.priceAxisProperties.percentage": false,
						"mainSeriesProperties.priceAxisProperties.percentageDisabled": false,
						"mainSeriesProperties.priceAxisProperties.log": false,
						"mainSeriesProperties.priceAxisProperties.logDisabled": false,

						"mainSeriesProperties.candleStyle.upColor": "#668e69",
						"mainSeriesProperties.candleStyle.downColor": "#a15456",
						"mainSeriesProperties.candleStyle.drawWick": true,
						"mainSeriesProperties.candleStyle.drawBorder": true,
						"mainSeriesProperties.candleStyle.borderColor": "#668e69",
						"mainSeriesProperties.candleStyle.borderUpColor": "#668e69",
						"mainSeriesProperties.candleStyle.borderDownColor": "#a15456",
						"mainSeriesProperties.candleStyle.wickColor": "#737375",
						"mainSeriesProperties.candleStyle.wickUpColor": "#668e69",
						"mainSeriesProperties.candleStyle.wickDownColor": "#a15456",
						"mainSeriesProperties.candleStyle.barColorsOnPrevClose": false,
						
						"mainSeriesProperties.hollowCandleStyle.upColor": "#668e69",
						"mainSeriesProperties.hollowCandleStyle.downColor": "#a15456",
						"mainSeriesProperties.hollowCandleStyle.drawWick": true,
						"mainSeriesProperties.hollowCandleStyle.drawBorder": true,
						"mainSeriesProperties.hollowCandleStyle.borderColor": "#668e69",
						"mainSeriesProperties.hollowCandleStyle.borderUpColor": "#668e69",
						"mainSeriesProperties.hollowCandleStyle.borderDownColor": "#a15456",
						"mainSeriesProperties.hollowCandleStyle.wickColor": "#737375",
						"mainSeriesProperties.hollowCandleStyle.wickUpColor": "#668e69",
						"mainSeriesProperties.hollowCandleStyle.wickDownColor": "#a15456",

						"mainSeriesProperties.haStyle.upColor": "#668e69",
						"mainSeriesProperties.haStyle.downColor": "#a15456",
						"mainSeriesProperties.haStyle.drawWick": true,
						"mainSeriesProperties.haStyle.drawBorder": true,
						"mainSeriesProperties.haStyle.borderColor": "#668e69",
						"mainSeriesProperties.haStyle.borderUpColor": "#668e69",
						"mainSeriesProperties.haStyle.borderDownColor": "#a15456",
						"mainSeriesProperties.haStyle.wickColor": "#737375",
						"mainSeriesProperties.haStyle.wickUpColor": "#668e69",
						"mainSeriesProperties.haStyle.wickDownColor": "#a15456",
						"mainSeriesProperties.haStyle.barColorsOnPrevClose": false, 
						"mainSeriesProperties.barStyle.upColor": "#668e69",
						"mainSeriesProperties.barStyle.downColor": "#a15456",
						"mainSeriesProperties.barStyle.barColorsOnPrevClose": false,
						"mainSeriesProperties.barStyle.dontDrawOpen": false,
						"mainSeriesProperties.lineStyle.color": "#0cbef3",
						"mainSeriesProperties.lineStyle.linestyle": 0,
						"mainSeriesProperties.lineStyle.linewidth": 1,
						"mainSeriesProperties.lineStyle.priceSource": "close",
						"mainSeriesProperties.areaStyle.color1": "#0cbef3",
						"mainSeriesProperties.areaStyle.color2": "#0098c4",
						"mainSeriesProperties.areaStyle.linecolor": "#0cbef3",
						"mainSeriesProperties.areaStyle.linestyle": 0,
						"mainSeriesProperties.areaStyle.linewidth": 1,
						"mainSeriesProperties.areaStyle.priceSource": "close",
						"mainSeriesProperties.areaStyle.transparency": 80
					},
					 custom_css_url: 'charts.css'
				});
				this_vue.chart.onChartReady(function() {
					 this_vue.chart.chart().createStudy('Moving Average', false, true, [5], null, {'Plot.color': '#642d92'});
					 this_vue.chart.chart().createStudy('Moving Average', false, true, [10], null, {'Plot.color': '#5278a3'});
					 this_vue.chart.chart().createStudy('Moving Average', false, true, [30], null, {'Plot.color': '#238031'});
				})
			});	
		},
		methods: {
			datetimes:function(){
				var res=this.GetRequest().type;
				switch(res){
                        case "1min":
                            return "3H"
                            break;
                        case "15min":
                           return "2D"
                            break;
                        case "30min":
                            return "4D"
							break;
						case "1hour":
                            return "8D"
							break;
						case "1day":
                            return "5M"
							break;
						case "1week":
                            return "10M"
                            break;
						default:
						'3H'
						break;
                    }
			},
			GetRequest : function() {
				var url = decodeURI(location.search); //获取url中"?"符后的字串
				var theRequest = new Object();
				if (url.indexOf("?") != -1) {
					var str = url.substr(1);
					var strs = str.split("&");
					for(var i = 0; i < strs.length; i ++) {
						theRequest[strs[i].split("=")[0]]=unescape(strs[i].split("=")[1]);
					}
				}
				return theRequest;
			},
			getChartData: function(times) {
				var _this = this;
				var mytype='';
				var date={};
				if(times==""){
					mytype='&size=1000'
				}else{
					mytype='&size=1'
				}
				
				date.market=_this.GetRequest().market;
				date.assist=_this.GetRequest().money;
				date.exxvip=_this.GetRequest().exxvip;
				if(_this.GetRequest().type=='line'){
					date.type='1min'
				}else{
					date.type=_this.GetRequest().type;
				}
                $.ajax({
                    type : 'GET',
                    dataType : 'jsonp',
					url : date.exxvip+"/api/web/V1_0/charts?jsoncallback=?&needTickers=1&market="+date.market+"&type="+date.type+"&note=mainkline"+mytype+"&assist="+date.market.split("_")[1]+times,
                    success : function (data){
						setTimeout(function(){
							_this.getChartData(_this.timestamp)
						},3000)
						if(data.datas.data&&data.datas.data.length>0){
							data.datas.data.forEach((order => {
								_this.$data.bars.push({
									time: Number(order[0]),
									open: Number(order[1]),
									high: Number(order[2]),
									low: Number(order[3]),
									close: Number(order[4]),
									volume: Number(order[5]),
								})
							}))
							var st=_this.$data.bars[_this.$data.bars.length-1].time
							_this.timestamp ='&since='+st;
							// let fromTime = _this.bars[_this.bars.length - 50].time / 1000;
							// let toTime = _this.bars[_this.bars.length - 1].time / 1000;
							// _this.chart.chart().setVisibleRange({from: fromTime, to: toTime});
						}
						
					},
					error:function(data){
					}
					
                });
			},
			changePair: function() {
				let this_vue = this;
				if(this.chart && this.feed) {
					this.feed._fireEvent('pair_change');
					// this.chart.activeChart().resetData();
					
					this.chart.activeChart().setSymbol(this.currency1 + ":" + this.currency2, function() {
						// console.log("GOWNO :: proba zmiany", this_vue.currency1, this_vue.currency2);
					});
					// this_vue.Datafeed.Container.prototype.getBars=function(symbolInfo, resolution, rangeStartDate, rangeEndDate, onDataCallback, onErrorCallback){
					// 	onDataCallback(this_vue.bars, {
					// 		noData: false
					// 	})
					// }
				
				}
			},
			createFeed: function() {
				var this_vue = this;
				

				this_vue.Datafeed.DataPulseUpdater = function(datafeed, updateFrequency) {
					this._datafeed = datafeed;
					this._subscribers = {};

					this._requestsPending = 0;
					var that = this;

					var update = function() {
						if(that._requestsPending > 0) {
							return;
						}

						for(var listenerGUID in that._subscribers) {
							var subscriptionRecord = that._subscribers[listenerGUID];
							var resolution = subscriptionRecord.resolution;

							var datesRangeRight = parseInt((new Date().valueOf()) / 1000);

							//	BEWARE: please note we really need 2 bars, not the only last one
							//	see the explanation below. `10` is the `large enough` value to work around holidays
							var datesRangeLeft = datesRangeRight - that.periodLengthSeconds(resolution, 10);

							that._requestsPending++;

							(function(_subscriptionRecord) { // eslint-disable-line
								that._datafeed.getBars(_subscriptionRecord.symbolInfo, resolution, datesRangeLeft, datesRangeRight, function(bars) {
										that._requestsPending--;

										//	means the subscription was cancelled while waiting for data
										if(!that._subscribers.hasOwnProperty(listenerGUID)) {
											return;
										}

										if(bars.length === 0) {
											return;
										}

										var lastBar = bars[bars.length - 1];
										if(!isNaN(_subscriptionRecord.lastBarTime) && lastBar.time < _subscriptionRecord.lastBarTime) {
											return;
										}

										var subscribers = _subscriptionRecord.listeners;

										//	BEWARE: this one isn't working when first update comes and this update makes a new bar. In this case
										//	_subscriptionRecord.lastBarTime = NaN
										var isNewBar = !isNaN(_subscriptionRecord.lastBarTime) && lastBar.time > _subscriptionRecord.lastBarTime;

										//	Pulse updating may miss some trades data (ie, if pulse period = 10 secods and new bar is started 5 seconds later after the last update, the
										//	old bar's last 5 seconds trades will be lost). Thus, at fist we should broadcast old bar updates when it's ready.
										if(isNewBar) {
											if(bars.length < 2) {
												throw new Error('Not enough bars in history for proper pulse update. Need at least 2.');
											}

											var previousBar = bars[bars.length - 2];
											for(var i = 0; i < subscribers.length; ++i) {
												subscribers[i](previousBar);
											}
										}
										_subscriptionRecord.lastBarTime = lastBar.time;

										for(var i = 0; i < subscribers.length; ++i) {
											subscribers[i](lastBar);
										}
									},
									//	on error
									function() {
										that._requestsPending--;
									});
							})(subscriptionRecord);
						}
					};
					if(typeof updateFrequency != 'undefined' && updateFrequency > 0) {
						setInterval(update, updateFrequency);
					}
				};
				this_vue.Datafeed.DataPulseUpdater.prototype.periodLengthSeconds = function(resolution, requiredPeriodsCount) {
					var daysCount = 0;
					if(resolution === 'D') {
						daysCount = requiredPeriodsCount;
					} else if(resolution === 'M') {
						daysCount = 31 * requiredPeriodsCount;
					} else if(resolution === 'W') {
						daysCount = 7 * requiredPeriodsCount;
					} else {
						daysCount = requiredPeriodsCount * resolution / (24 * 60);
					}
					return daysCount * 24 * 60 * 60 ;
				};
				this_vue.Datafeed.DataPulseUpdater.prototype.subscribeDataListener = function(symbolInfo, resolution, newDataCallback, listenerGUID) {
					this._datafeed._logMessage('Subscribing ' + listenerGUID);
					if(!this._subscribers.hasOwnProperty(listenerGUID)) {
						this._subscribers[listenerGUID] = {
							symbolInfo: symbolInfo,
							resolution: resolution,
							lastBarTime: NaN,
							listeners: []
						};
					}
					this._subscribers[listenerGUID].listeners.push(newDataCallback);
				};
				this_vue.Datafeed.DataPulseUpdater.prototype.unsubscribeDataListener = function(listenerGUID) {
					this._datafeed._logMessage('Unsubscribing ' + listenerGUID);
					delete this._subscribers[listenerGUID];
				};
				this_vue.Datafeed.Container = function(updateFrequency) {
					this._configuration = {
						supports_search: false,
						supports_group_request: false,
						supported_resolutions: ['1',  '15', '30', '60', '1D', '1W'],
						supports_marks: true,
						supports_timescale_marks: true,
						exchanges: ['myExchange']
					};
					this._barsPulseUpdater = new this_vue.Datafeed.DataPulseUpdater(this, updateFrequency || 10 * 1000);
					// this._quotesPulseUpdater = new Datafeed.QuotesPulseUpdater(this);

					this._enableLogging = true;
					this._callbacks = {};
					this._initializationFinished = true;
					this._fireEvent('initialized');
					this._fireEvent('configuration_ready');
				};
				this_vue.Datafeed.Container.prototype._fireEvent = function(event, argument) {
					
					if(this._callbacks.hasOwnProperty(event)) {
						var callbacksChain = this._callbacks[event];
						for(var i = 0; i < callbacksChain.length; ++i) {
							callbacksChain[i](argument);
						}

						this._callbacks[event] = [];
					}
				};
				this_vue.Datafeed.Container.prototype._logMessage = function(message) {
					if(this._enableLogging) {
						var now = new Date();
						// console.log("CHART LOGS: " + now.toLocaleTimeString() + '.' + now.getMilliseconds() + '> ' + message);
					}
				};
				this_vue.Datafeed.Container.prototype.on = function(event, callback) {
					if(!this._callbacks.hasOwnProperty(event)) {
						this._callbacks[event] = [];
					}

					this._callbacks[event].push(callback);
					return this;
				};

				this_vue.Datafeed.Container.prototype.onReady = function(callback) {
					let that = this;
					if(this._configuration) {
						setTimeout(function() {
							callback(that._configuration);
						}, 0);
					} else {
						this.on('configuration_ready', function() {
							callback(that._configuration);
						});
					}
				};
				this_vue.Datafeed.Container.prototype.resolveSymbol = function(symbolName, onSymbolResolvedCallback, onResolveErrorCallback) {
					this._logMessage("GOWNO :: resolve symbol " + symbolName);
					Promise.resolve().then(() => {

						function adjustScale() {
								return 100000;
						}
						this._logMessage("GOWNO :: onResultReady inject " + this_vue.currency1 + ":" + this_vue.currency2);
						onSymbolResolvedCallback({
							"name": this_vue.currency1 + ":" + this_vue.currency2,
							"timezone": "Europe/Warsaw",
							"pricescale": adjustScale(),
							"minmov": 1,
							"minmov2": 0,
							// "volume_precision": 8,//整数显示此商品的成交量数字的小数位。0表示只显示整数。1表示保留小数位的1个数字字符
							"ticker": this_vue.currency1 + ":" + this_vue.currency2,
							"description": "",
							"session": "24x7",
							"type": "bitcoin",
							"exchange-traded": "myExchange",
							"exchange-listed": "myExchange",
							"has_intraday": true,
							"intraday_multipliers": ['60'], //It is an array containing intraday resolutions (in minutes) the datafeed wants to build by itself. E.g., if the datafeed reported he supports resolutions ["1", "5", "15"], but in fact it has only 1 minute bars for symbol X, it should set intraday_multipliers of X = [1]. This will make Charting Library to build 5 and 15 resolutions by itself.
							"has_weekly_and_monthly": false,
							"has_no_volume": false,
							"regular_session": "24x7"
						});
					})
				};
				this_vue.Datafeed.Container.prototype.getBars = function(symbolInfo, resolution, rangeStartDate, rangeEndDate, onDataCallback, onErrorCallback) {
				// 	if(rangeStartDate > 0 && (rangeStartDate + '').length > 10) {
				// 		throw new Error(['Got a JS time instead of Unix one.', rangeStartDate, rangeEndDate]);
				// 	}
			     		 
						onDataCallback(this_vue.bars, {
							noData: false
						})
						onDataCallback([], {
								noData: true
							});
						// this.on('pair_change', function() {
						// 	onDataCallback(this_vue.bars, {
						// 		noData: false
						// 	})
						// 	onDataCallback([], {
						// 		noData: true
						// 	});
						// });
						// console.log(this_vue.fghjk())
					
					
					
				// 	// onDataCallback(this_vue.bars, { noData: true , nextTime: data.nb || data.nextTime });
				
					// onResult(result.bars, result.meta);
				
                
				};
				this_vue.Datafeed.Container.prototype.subscribeBars = function(symbolInfo, resolution, onRealtimeCallback, listenerGUID, onResetCacheNeededCallback) {
					//  callback&& callback()
                    // this.on('pair_change', function() {
                    //     onResetCacheNeededCallback();
					// });
					// this_vue.bars.forEach(function (bar) { // in subscribeBars
                    //      onRealtimeCallback(bar)
                    // });
                    // this.on('pair_change', function() {
                    //     onResetCacheNeededCallback();
					// });
					

					this._barsPulseUpdater.subscribeDataListener(symbolInfo, resolution, onRealtimeCallback, listenerGUID, onResetCacheNeededCallback);
				};
				this_vue.Datafeed.Container.prototype.unsubscribeBars = function(listenerGUID) {
					this._barsPulseUpdater.unsubscribeDataListener(listenerGUID);
				};
				return new this_vue.Datafeed.Container;
			},
			adjustChart: function() {
				let chart_iframe = $("#chart_container").find("iframe");
				let chart_top = chart_iframe.contents().find(".header-chart-panel");
				let chart_top_container = chart_iframe.contents().find(".header-chart-panel-content");
				let chart_bottom = chart_iframe.contents().find(".date-range-wrapper");

				chart_bottom.appendTo(chart_top_container);
			}
		},
		watch: {
			bars: function(newVal, oldVal) {
				this.currency1 = newVal[0];
				this.currency2 = newVal[1];
				this.changePair();
			},
		},
		data: function() {
			return {
				Datafeed:{},
				currency1: 'USD',
				currency2: 'BTC',
				saved_chart: null,
				chart: null,
				feed: null,
				last_price: 1234.2365,
				bars: [],
				timestamp:'',
				lan:'zh',
				fghjk:''
			}
		}

	}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
	h1,
	h2 {
		font-weight: normal;
	}
	
	ul {
		list-style-type: none;
		padding: 0;
	}
	
	li {
		display: inline-block;
		margin: 0 10px;
	}
	
	a {
		color: #42b983;
	}
</style>