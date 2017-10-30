---
title: vue项目总结（搜索电影）
---
#### 1. mounted 里获取DOM元素 ####
	mounted () {
      this.video = this.$el.querySelector(`#${this.videoId}`)
    },
#### 2. 子组件调用父组件函数 ####
需求：视频子组件的弹窗有下载按钮，父组件已定义了下载的方法。怎么调用父组件的方法？

1.子组件（video-player）定义方法，emit方法到父组件
	
	html:
	<span class="video-cover-btn" @click="downloadZapya" id="video-cover-playbtn"></span>

	js:
    downloadZapya: function () {
    	this.$emit('dwzapya')
  	}

2.父组件接受子组件的方法
	
	<video-player @dwzapya="downloadZapya"></video-player>
	
	js:
	downloadZapya: function () {
		//do something
	}
扩展： $emit, $on的使用

$on 监听当前实例上的事件（包括自定义事件），事件可有$emit触发。回调函数会接收所有传入事件触发函数的额外参数。

	vm.$on( event, callback )

$emit 触发当前实例上的事件，附加参数都会传给监听器回调。
	vm.$emit( event, […args] )

[摘自：http://www.jb51.net/article/115624.htm](摘自：http://www.jb51.net/article/115624.htm)

3.具名slot

	子组件(modal)定义name
	<div class="modal-header">
	  <slot name="header">
	  </slot>
	</div>

	父组件
	<modal><div class="video-box" slot="body"></modal>

4.watch监测数据变化（数据是异步获取）

需求:搜索按钮点击获取后台数据（axios请求），通过判断搜索结果(sMovies)的数量来显示有无结果的不同界面。

最开始是想着在，点击事件请求里面判断serchResult(它的值是由sMovies决定的)的值，结果发现serchResult只在第一次获取后，再次点击就不会发生改变。

解决：通过查阅资料，发现watch可监测异步操作数据的变化，于是设置watch事件。

	methods：（获取sMovies的的值）
	showSearchResult： funciton(){
	    getSearch(sWords).then(res => {
	      this.sMovies = res.data.shockings
	      if (this.sMovies && this.sMovies.length > 1) {
	        //this.serchResult = true (这里设置的话下次点击的时候数据不会同步更新) 
	        this.sMovies = this.sMovies.length > 4 ? this.sMovies.slice(0, 4) : this.sMovies
	      }
	    })	
	}

	watch: (监测sMovie的变化来改变serchResult的变化)
	watch: {
	  //需要监测的值
	  'sMovies': { 
	    //值变化时的回调函数，参数（新值，旧值）
		handler: function (val, oldVal) { 
		  //注: handler如果采用箭头函数，this的值不会指向vue实例
	      if (val.length === 0) {
	        this.serchResult = false
	      }
	    }
	  }
	}

5.ref的使用

需求：视频列表点击，当前视频播放，其余视频暂停。

解决：通过视频组件（video-player），设置ref。然后在methods通过$refs获取到视频列表的DOM元素。（注：v-for生成的ref是数组）

	<video-player ref="video"></video-player>

	methods:
		
		let videos = this.$refs.video
        let currentVideo = videos[this.currentMovie]
        videos.forEach((video) => {
          video.pause()
        })
        currentVideo.play()