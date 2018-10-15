##  ngInfiniteScroll
- [x]  ++css【weui：https://weui.io/】++

- [x]  ++lazy-img++

参数名 | 描述 |取值(示例)	默认值
------|---|------
animate-visible | 是否以渐入的方式显示图片（包括初始化加载的图片） | “true”	“false”
animate-speed | 渐入显示的速度（图片透明度从0到1） | “0.5s”	“1s”



```
<img lazy-src="{{imgUrl}}" animate-visible="true" animate-speed="0.5s" alt="" />

局部滚动或路由切换无加载，滚动scroll添加class：lazyLoadContainer
            
```
- 示例

```
<div class="scrolly-wbk lazyLoadContainer" id="tody_scroll" >
        <div class="weui_leftbar" click-tv-left >
            <div class="weui_leftbar_item {{$index==0?'weui_bar_item_on':''}}" ng-repeat="v in tb" ng-click="changetb(v.channelId)" on-finish-render-filters repeat-id="r1">
                <p class="img">
                    <img class="lazy" lazy-src="{{v.icon2}}"  animate-visible="true" animate-speed="0.5s" err-src="img/ico_df.png" style="display: block;width: .36rem;margin: 0 auto;">
                </p>
                <p class="c33 fs13 ellipsis b0 l10" ng-bind="v.channelName"></p>
            </div>
        </div>
    </div>
    
    err-src="img/ico_df.png" 错误图片、404、null 替换默认
```
- [x] ++底部无限加载infiniteScroll (directive in module infinite-scroll)++

http://sroze.github.io/ngInfiniteScroll/documentation.html


- 示例,参考choose.js

```
<ANY infinite-scroll='{expression}'
     [infinite-scroll-distance='{number}']
     [infinite-scroll-disabled='{boolean}']
     [infinite-scroll-immediate-check='{boolean}']
     [infinite-scroll-listen-for-event='{string}']
     [infinite-scroll-container='{HTMLElement | [] | string}']
     [infinite-scroll-use-document-bottom='{boolean}']
     [infinite-scroll-parent]>
</ANY>

```

```
<div id="J_content ">
		<div class="actlist pt10 lazyLoadContainer" infinite-scroll="getAjax()" infinite-scroll-disabled="isc" infinite-scroll-distance="0">
			<ul class="list ovh " id="J_list">
				<li class="li" ng-repeat="v in list" >
					<a href="#/detail?columnType={{v.videoType}}&id={{v.id}}" class="lia">
						<p class="size-cover ptr ovh">
							<img class="loading pta lazy" lazy-src="{{v.image}}"  animate-visible="true">
							<span ng-if="v.edition" class="J_icotv cff dsb alic">{{v.edition}}</span>
						</p>
						<p class="sec-text alic ellipsis pt5">{{v.title}}</p>
						<!--播放量展示-->
						<!--<p class="thi-text lh16">播放:{{v.playCount|to_wan}}</p>-->
					</a>
				</li>
			</ul>
		</div>
		<p class="alic ptb" id="J_loading" ng-bind="loadtext"></p>
	</div>
	
$scope.getAjax = function () {
            $scope.isc = true;
			var senturl = webset.apiurl + 'search/siftings.json',
				data = {
					"queryStr": encodeURIComponent($scope.seting.type.split(',')[1] ? $scope.seting.type.split(',')[1] : ''),
					"region": $scope.seting.region,
					"year": $scope.seting.year,
					"tag": $scope.seting.tag,
					"cpid": $scope.cpid,
					'type': $scope.seting.type,
					'level': $scope.seting.level,
					"pageSize": 15,
					"pageNo": $scope.seting.pageNo
				};
			var transform = function (data) {
				return $.param(data);
			};
			$http.post(senturl, data, {
				headers: {
					'Content-Type': 'application/x-www-form-urlencoded; charset=UTF-8'
				},
				transformRequest: transform
			}).success(function (res) {
				if (res.response.responseHeader.code == '200') {
					$scope.list = $scope.list.concat(res.response.responseBody.list);
					if (res.response.responseBody.list.length == 15) {
						$scope.isc = false;
						$scope.seting.pageNo++;
						$scope.loadtext = '正在加载...';
					} else {
						$scope.loadtext = '无更多内容';
					}
				} else {
					$scope.loadtext = '暂无内容';
				}
			});
		};
```
##  小程序-play公共组件
```
play_type：0回看；1直播；2点播

1）回看参数说明：
_channelId：接口channelId
_title:接口title
_channelName：频道中文
_startTime:2018-05-21 07:53
_endTime:2018-05-21 07:53
_code :接口playcode

示例
<play  play_type='1' _channelId="{{options.channelId}}" _title="{{}}" _channelName="{{}}" _startTime="{{}}" _endTime="{{}}" _code={{}}>
 插槽当前推屏按钮
</play>

2）直播参数说明：
_channelId：接口channelId
示例：
<play  play_type='1' _channelId="{{options.channelId}}" >
  插槽当前推屏按钮
</play>


3）点播参数说明：
_assetId：接口code
_providerId：接口cpCode
_id:接口id

示例：
<play  play_type：2 _assetId="{{fadedmt[0].code}}" _providerId="{{fadedmt[0].cpCode}}" _id="{{options.id}}">
  <view class='tvplay cff fs30 z0' >推屏</view>
</play>
```
