---
title: 小程序
date: 2022/08/07 20:48:01
tags: [小程序]
categories: 技术
cover: static\img\ok.jpg
---
### uni上传功能

```
//上传图片
			chooseImage() {
				let that = this;
				let imgArr = that.imgArr;
				let imgTemp = ''
				//选择图片文件
				uni.chooseImage({
					count:5, //最多可以选择的图片数量
					sourceType: ['album'], //图片来源是相册   相机:'camera'
					success(res) {
						uni.showToast({
						        title: '正在上传...',
						        icon: 'loading',
						        mask: true,
						        duration: 2000
						      })
						if(imgArr.length < 5){
							imgArr.push(...res.tempFilePaths); //不满5张追加到数组
							// console.log(222,imgArr)
							imgTemp = res.tempFilePaths[0]
						}else{
							return uni.showToast({
								title: '最多可以选择5张图片',
								icon: 'none'
							})
						}
						// 上传图片uni.uploadFile
						  uni.uploadFile({
						  	url: api.baseApi + 'sys/common/upload',
						  	filePath: imgTemp,
							name: 'file',
						  	fileType: 'image',
						  	formData: {
						      biz: 'temp'
						  	},
						  	header: {
						  	        'X-Access-Token': that.$store.state.token
						  	      },
						  	success(res) {
						  		console.log('上传成功')
						  		let { data } = res //res.data是字符串类型，需要转换为对象
								let { message } = JSON.parse(data)
								that.imgList.push(message)							
						  	},
						  	fail(error) {
						  		uni.showToast({
						  		  title: `上传失败`,
								  icon: 'none'
						  		})
						  	}
						  })
	
					}
				})
			},
			// 点击预览图片
			imgbox(item) {
				uni.previewImage({
					current:item,
					urls: this.imgArr
				})
			},
			// 删除图片
			deleteImg(index) {
				var that = this;
				uni.showModal({
					title: '删除',
					content: '确认删除当前图片',
					success(res) {
						// 用户点击确认时
						if(res.confirm) {
							that.imgList = that.imgList.filter((item,key) => key !== index)
							that.imgArr.splice(index,1);
							// console.log(that.imgList)
						}else if(res.cancel) { 
						// 用户点击取消时
							uni.showToast({
							  title: '已取消',
							  icon: 'none'
							})
						}
					}
				})
			},
```

