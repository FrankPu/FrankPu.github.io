---
title: ReactNative原生组件FlatList实现上拉加载
date: 2019-07-03 11:42:35
tags: 
- ReactNative
- 上拉加载
categories: ReactNative
---

我们可以利用官方组件`RefreshControl`实现下拉刷新功能，但官方并没有提供相应的上拉加载组件，所以我们需要通过自己手动实现。

这里用到了`FlatList`组建的`onEndReached`与`onEndReachedThreshold`属性来实现相应效果。



<!-- more -->



## 上拉加载

### 思路

上拉加载一般应用于分页加载的情况，当`FlatList`滑动到底部时：

1. 页面`pageIndex`数加1，触发请求新一页的数据
2. 更新到组件`state`中的数据源`dataArray`中，`dataArray`也作为`FlatList`的数据源`data`

滑动到底部触发请求新一页的数据通过`FlatList`的`onEndReached`和`onEndReachedThreshold`属性来实现



### 具体实现

#### onEndReached

上拉加载的关键是`onEndReached`，通过设置距离底部距离`onEndReachedThreshold`的值（值为比例，0.5表示距离底部一半），来触发`onEndReached`事件。

`onEndReached`方法在当列表被滚动到距离最底部**小于**`onEndReachedThreshold`的值时调用。

```javascript
/**
 * 上拉加载
 */
_onEndReached = () => {
  /** 如果是正在加载中或没有更多数据了，则返回 */
  if (this.state.showFoot !== 0 ) return;

  /** 如果当前页大于或等于总页数，那就是到最后一页了，返回 */
  if ((this.state.pageIndex !== 1) && (this.state.showFoot === 1)) {
    return;
  } else {
    this.setState({pageIndex: this.state.pageIndex + 1});
  }

  /** 底部显示正在加载更多数据 */
  this.setState({showFoot: 2});
  /** 获取数据 */
  this._fetchPrev();
}
```



#### 初始State

```javascript
constructor(props) {
    super(props);
    this.state = {
        isLoading: true,
        // 网络请求失败
        fetchError: false,
        errorInfo: "",
        // 列表数据
        dataArray: [],
        /**
        * 是否显示底部组件
        * 0：隐藏footer
        * 1：已加载完成,没有更多数据
        * 2：显示加载中
        */
        showFoot:0,
        // 下拉刷新控制
        isRefreshing:false,
        // 当前页码
        pageIndex: 1,
        // 每页个数
        pageSize: 10, 
    }
}
```



#### 数据获取

获取到新一页的数据时，把`pageIndex`加1，

当获取到数据`data`时，拼接在已有的`dataArray`上：

```javascript
componentDidMount() {
    //请求数据
    this.fetchData();
}

this.setState({
    //复制数据源
    dataArray:this.state.dataArray.concat(data),
    isLoading: false,
    showFoot:foot,
    isRefreshing:false,
});
```



#### 组件渲染

```javascript
/**
 * 加载等待页
 */
_renderLoadingView = () => {
  return (
    <ActivityIndicator
      animating={this.state.isLoading}
      toast={true}
      color='#108ee9'
      size="large"
    />
  );
}

/**
 * 加载失败页
 */
_renderErrorView = () => {
  return (
    <View style={styles.fetchErrContainer}>
      <Text>网络请求失败</Text>
    </View>
  );
}

render() {
    // 第一次加载等待的view
    if (this.state.isLoading && !this.state.error) {
    	// 加载等待页
    	return this.renderLoadingView();
    } else if (this.state.error) {
        // 网络请求失败
        return this.renderErrorView();
    }

    // 列表
    return (
    	<FlatList
            data={this.state.dataArray}
            keyExtractor={(item, index) => `${item.id} - ${index}`}
            renderItem={this._renderItem}
            ListFooterComponent={this._renderFooter}
            onEndReached={this._onEndReached}
            onEndReachedThreshold={0.5}
            refreshing={this.state.refreshing}
            onRefresh={this._onRefresh}
            ItemSeparatorComponent={this._renderSeparator}
        />
    )
}
```



#### 每个Item组件

`renderItem`根据行数据`dataArray`来渲染每个Item组件

```javascript
_renderItem = ({ item, index, separator }) => {
    return (
    	<View>
    		<Text style={styles.title}>
    			name: {item.value.name} ({item.value.stargazers_count}stars)
            </Text>
            <Text style={styles.content}>
            	description: {item.value.description}
            </Text>
        </View>
    );
}
```



#### 底部上拉加载loading组件

`ListFooterComponent`为尾部组件的渲染

```javascript
/**
 * 底部提示组件
 */
_renderFooter = () => {
  if (this.state.showFoot === 1) {
    return (
      <View>
        <Text style={{color: '#aaa', fontSize: 14, textAlign: 'center', height: 80, marginBottom: 10}}>
          没有更多数据了
        </Text>
      </View>
    );
  } else if (this.state.showFoot === 2) {
    return (
      <View style={{flexDirection: 'row', justifyContent: 'center', alignItems: 'center', height: 80, marginBottom: 10}}>
        <ActivityIndicator />
        <Text>正在加载更多数据...</Text>
      </View>
    );
  } else if (this.state.showFoot === 0) {
    return (
      <View>
        <Text></Text>
      </View>
    );
  }
}
```



#### 每个Item之间的分隔线

`ItemSeparatorComponent`是行与行之间的分隔线组件,不会出现在第一行之前和最后一行之后

```javascript
_renderSeparator(){
    return(
    	<View style={{height:1,backgroundColor:'#999999'}}/>
    )
}
```



## ActivityIndicator加载指示器

用原生组件`ActivityIndicator`来实现Loading的效果。

```javascript
<ActivityIndicator
	// { string } 是否显示
    animating={true}
    color='red'
    // enum('small', 'large')
    size="large"
/>
```

