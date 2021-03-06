# 高德地图使用详解

```
安装：npm i wqr-amap -S/--save
```

```
安装：yarn add wqr-amap -S/--save
```

```
使用：import MapService from 'wqr-amap'
```

## 版本日志
  版本号|新增功能/纠正错误      |        日期
---|---------------------|----------------
 1.0|配置地图、地图展示、地图控件、地理定位、周边搜索、关键字搜索、点击获取经纬度、经纬度转真实地址    |   2017-01-19
 2.0|修改部分文档说明，改善点击地图获取经纬度方法，补充经纬度转真实地址使用方法；新增出行路线规划及调用原生APP导航功能、地图中心点设置、地图缩放解级别设置、通过其他系统获取的gps经纬度坐标转换成高德地图经纬度坐标、计算地图上两点之间的距离    |   2017-02-06
 3.0|新增免实例化对象将经纬度转真实地址的方法|   2017-02-20


#### 注意：在使用该服务前，请确保您的项目环境支持es6语法（欢迎到[ GitHub ](https://github.com/WangQiangrong/Amap.git "65165165")fork，如若发现问题也欢迎提交到[ GitHub ](https://github.com/WangQiangrong/Amap.git "65165165")  )
## 1.配置地图基本项
```
引入示例：（具体引入地址视您项目而定）


传参示例：

  let obj = {                                     
      id: 'container',                            
      key: '1234567890',
      mapStyle: {                                
          zoom: 14,                             
          resizeEnable: true,                     
          center: [117.000923, 36.675807]
      },
  }

传参解释：（非必传的参数都是可选的，可有可无）

  id:   地图容器id (必传)
  key:    您的开发者key（这里已提供开发者已有的key供大家测试使用，需要去注册获取）（必传）
  mapStyle.zoom:   地图缩放等级（3-18） （必传）
  mapStyle.resizeEnable:    是否支持鼠标滚轮缩放 （非必传，默认值为true，可缩放）
  center:   地图中心的经纬度，如果项目有地理定位需求，该项可不配置，因为下面的定位服务，默认以定位点为地图中心 （非必传，默认值为你当前市）

使用示例：

  let mapService = new MapService(obj)

```

## 2.初始化并展示地图

```
使用示例：

  mapService.initMap().then(()=>{

  })

备注：该地图服务为异步执行，因此进入页面需要立即执行的相关地图操作请放在本回调中执行。需要在该回调中执行的有：
1、地图控件的展示 2、定位 3、周边搜索（如果他是进入页面就要获取周边信息的话）

```

## 3.地图控件展示

```
常用控件：鹰眼、比例尺、地图工具条

传参示例：

  let showTool={
      isOverView: true,          
      isScale: true,              
      isToolBar: true             
  }

传参解释：（非必传的参数都是可选的，可有可无）

  isOverView:   是否展示鹰眼（非必传，无默认值，需要使用配置即可）
  isScale:    是否展示比例尺插件（非必传，无默认值，需要使用配置即可）
  isToolBar:    暂时地图工具条（非必传，无默认值，需要使用配置即可）

使用示例：（因为该功能通常为进入页面就要展示所以记得放在初始化地图的回调中执行哦！）

  mapService.initMap().then(()=>{
    mapService.showMapTool(showTool)
  })

备注：后期考虑是否添加各控件的配置（比如说相对于地图的放置位置，期待更新...）

```

## 4.地理定位

```

1、如果您使用该服务只是做定位展示，可不做任何操作，直接调用即可。

使用示例：

  mapService.initMap().then(()=>{
    mapService.showLocation()
  })

2、如果您还需要得到定位点的信息（经纬度、地址等等。信息比较多，可视情况根据返回对象内容取其值），那么您可以定义一个参数接受该方法返回的地理信息。

使用示例：（定位服务返回的是一个Promise对象：在成功的回调中可以接收定位信息，在失败的回调中说明定位失败）

  let addressData
  mapService.initMap().then(()=>{
    mapService.showLocation().then((result)=>{
      addressData=result
    },(result)=>{
      alert(result)
    })
  })

```   

## 5.周边搜索功能

```
传参示例：

  let nearByObj = {
      key:'',                             
      pageSize: 50,                         
      pageIndex: 1,                         
      lngLat:[116.405467, 39.907761], 
      showResultInMap:true,      
      type: '',                         
  }

传参解释：（非必传的参数都是可选的，可有可无）

  key:    搜索的关键字    （非必传，默认值为空）
  pageSize:   一页展示的搜索结果条数（1-50），超出50按50计   （必传）
  pageIndex:    搜索结果的页码（每次搜索返回的结果中有总条数信息，可以根据该总条数判断最大页码数）   （必传）
  lngLat:   搜索中心点的经纬度    （必传）
  showResultInMap:    表示是否把结果展示在地图上   （非必传，默认为不展示）
  type:   搜周边的类型   （非必传，默认值：餐饮服务、商务住宅、生活服务）
        兴趣点类别，多个类别用“|”分割，如“餐饮|酒店|电影院”
        POI搜索类型共分为以下20种：
        汽车服务|汽车销售|汽车维修|摩托车服务|餐饮服务|购物服务|生活服务|体育休闲服务|
        医疗保健服务|住宿服务|风景名胜|商务住宅|政府机构及社会团体|科教文化服务|
        交通设施服务|金融保险服务|公司企业|道路附属设施|地名地址信息|公共设施

返回参数示例：

{
  count:865,
  pageIndex:1,
  pageSize:50,
  pois:[****]
}

返回参数解释：

  count:    表示搜索该关键字返回结果总条数
  pageIndex:    返回的页码数
  pageSize:   本页的结果条数
  pois:   返回的结果数组，数组元素为一个对象，每个对象里面包含了每条结果的具体信息（如：地址，经纬度），可供您后续操作


使用示例：（该服务返回的也是一个）

  mapService.searchNearBy(nearByObj).then((result)=>{
    console.log(result)    
  },(result)=>{
    console.log(result)
  })
使用备注：如果您的搜索周边是在进入一个页面就执行并且经纬度参数是靠定位获取时，请把该方法放在定位方法的回调中去执行，那样才能正常获取定位经纬度执行搜周边
```


## 6.关键字搜索
```
传参示例：

  let searchByKeyword = {
      key:'',                             
      pageSize: 50,                         
      pageIndex: 1, 
      showResultInMap:true                         
  }

传参解释：（非必传的参数都是可选的，可有可无）

  key:    搜索的关键字    （必传，如果为空或者搜索无结果会返回错误提示）
  pageSize:   一页展示的搜索结果条数（1-50），超出50按50计   （必传）
  pageIndex:    搜索结果的页码（每次搜索返回的结果中有总条数信息，可以根据该总条数判断最大页码数）   （必传）
  showResultInMap:    表示是否把结果展示在地图上   （非必传，默认为不展示）

返回参数示例：

{
  count:865,
  pageIndex:1,
  pageSize:50,
  pois:[****]
}

返回参数解释：

  count:    表示搜索该关键字返回结果总条数
  pageIndex:    返回的页码数
  pageSize:   本页的结果条数
  pois:   返回的结果数组，数组元素为一个对象，每个对象里面包含了每条结果的具体信息（如：地址，经纬度），可供您后续操作


使用示例：（该服务返回的也是一个）

  mapService.searchByKeyword(searchByKeyword).then((result)=>{
    console.log(result)    
  },(result)=>{
    console.log(result)
  })
使用备注：如果您的搜索周边是在进入一个页面就执行并且经纬度参数是靠定位获取时，请把该方法放在定位方法的回调中去执行，那样才能正常获取定位经纬度执行搜周边

```

## 7.点击地图获取经纬度
```
返回值：

lnglat：   点击处的经纬度(注意该值是作为实例化地图服务的一个属性返回的，获取时请使用：如：mapService.lnglat形式)

使用示例：（因为该点击需要地图初始化完成后才能生效，所以请放在地图初始化回调中执行）

  1、获取经纬度：（适用于放在vue实例之外执行，需要传一个回调函数，您可以在该回调函数中获取点击的经纬度）

  mapService.initMap().then(() => {
    mapService.clickMap(()=>{
      console.log(mapService.lnglat)
    })
  })

  2、点击地图容器获取该经纬度：（适用于在Vue实例中使用，点击事件绑定到地图的容器上面）

  methods: {
    clickMap(){
      console.log(mapService.lnglat)    
    }
  }
```

## 8.经纬度转真实地址（需要地图初始化完毕后才有效）
```
传参示例：

  let lnglat=[110.365,36.2154]

传参解释：

  该参数为一个经纬度，格式必须为数组形式，该经纬度您可以从地图点击获得，也可通过定位获得等等途径

使用示例：（该方法返回结果为Promise对象，在成功回调结果中可以获取地址信息，信息量过大，在此不多赘述，请自行查找返回内容中
          您自己需要的内容）

mapService.initMap().then(() => {
  mapService.lngLatToAddress(lnglat).then((result)=>{
    console.log(result)    
  },(result)=>{
    console.log(result)    
  }) 
})
```

## 9.出行路线规划展示及调用原生APP导航（调用原生APP导航待测试验证）
```
传参示例：

let planObj={ （以两点的名称规划路线）
    wrapId:'',
    searchObj:[{keyword: '临泓路６号院',city:'北京'},{keyword: '龙潭公园',city:'北京'}],
    native:true,
    isWalking:false,
    isRiding:true,
    isDriving:false
}

let planObj={  （以两点的经纬度规划路线）
    wrapId:'',
    searchOrigin:[116.397933,39.844818],
    searchDestination:[116.440655,39.878694],
    native:true,
    isWalking:true,
    isRiding:true,
    isDriving:false
}

传参解释：（非必传的参数都是可选的，可有可无）
a、起始点名称适用：

  wrapId:   表示展示规划路线信息面板的id，如果你规划路径后要把沿途规划的地点信息让地图自动展示出来，那么你可以在你的html文件中事
            先定义一个div并赋予id，填入即可，如果不希望使用地图本来默认的展示方式，而仅仅获取沿途地点信息，您可以不填或不传
            （非必传）
  searchObj:    路线规划的起点和终点名称（必传）
  native:       是否调用原生APP进行导航 （非必传）
  isWalking:    步行路线规划（非必传）
  isRiding:     骑行路线规划（非必传）
  isDriving:    驾车路线规划（非必传）

b、起始点经纬度适用：

  wrapId:   同上
  searchOrigin:   起点经纬度（必传）
  searchDestination:    终点经纬度（必传）
  native:       是否调用原生APP进行导航 （非必传）
  isWalking:    步行路线规划（非必传）
  isRiding:     骑行路线规划（非必传）
  isDriving:    驾车路线规划（非必传）

使用示例：

mapService.planWay(planObj).then((result)=>{
    console.log(result)        
},(result)=>{
    console.log(result)        
})

```

## 10.设置地图中心点
```

传参示例：

let center=[116.440655,39.878694]

传参解释：

center:   要设置的地图中心点的经纬度（必传）

使用示例：

mapService.setMapCenter(center)

```

## 11.设置地图缩放级别
```

传参示例：

let zoom=17

传参解释：

传参解释：
center:   要设置的地图的缩放级别（必传）

使用示例：

mapService.setMapZoom(zoom)

```

## 12.把gps经纬度坐标转化成高德经纬度坐标
```
备注：坐标转换是高德地图web服务api，和其他服务不同，所以在使用该方法前请确保您已在高德地图开发者平台中申请了该服务的key

传参示例：

let gpsObj={
    key:'1234567890',
    lnglat:'116.481499,39.990475'
}

传参解释：

key:    高度地图web服务api的key（必传，需要到开发者平台自行申请）
lnglat:   您传入的经纬度（必传，格式可选：Array、String）

使用示例：（注意：本方法无需实例化MapService对象即可使用）

MapService.gpsTransform(gpsObj).then((response)=>{
    console.log(response)    //这里可以获取转换后的真实地址信息
})

```

## 13.计算地图上两点之间的距离
```
传参示例：

let twoPoints={
    one:[117.018862,36.669061],
    two:[117.019377,36.668992]
}

传参解释：

one:    第一个点的经纬度坐标 （必传）
two:    第二个的的经纬度坐标  （必传）

使用示例：

let distance=mapService.caculateDistance(twoPoints)


```
## 14.免实例化对象的经纬度转真实地址方法
```

传参示例：

let gpsObj={
    key:'1234567890',
    lnglat:[113.481499,39.990475]
}

传参解释：

    key:'1234567890',             高度地图web服务api的key（必传，需要到开发者平台自行申请）
    lnglat:[113.481499,39.990475] 您传入的经纬度（必传，格式可选：Array、String）

使用示例：（注意：本方法无需实例化MapService对象即可使用）

MapService.webLnglatToAddress(gpsObj).then((response)=>{
    console.log(response)  //这里可以获取转换后的真实地址信息      
})

```



#### 好了，到此结束，去做你的地图去吧！


###### AuthorProfile：

Mail：gcgleb22@163.com

Github：[WangQiangrong](https://github.com/WangQiangrong/wqr-amap)  欢迎 Fork 并提出宝贵意见，赏个star再好不过了，哈哈！！

NPM：[575201314](https://www.npmjs.com/~575201314)
