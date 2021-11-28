# **考勤打卡页面 sign_in**



**onload** 

- 获取当前时间，用户显示打卡区域的时间跳动
- 获取微信经纬度以及调用百度地图api获取地址
- 获取当前用户的考勤信息和职位信息
- 查询群发消息接口的消息并且查询消息状态



**业务逻辑**

显示当前用户的职位信息；

点击反馈活动图片按钮可以跳转到活动图片功能；

通过接口字段显示用户是否打卡，没有打卡就显示打卡界面，打卡了就显示打卡信息

功能；

给用户提供打卡功能，打卡需要填写备注信息和上传图片，

查看考勤范围跳转到 sales_promoter/range/Range 

反馈现场活动跳转到/sales_promoter/activity_picture/activity_picture 



**有两个跳转链接**

- /sales_promoter/activity_picture/activity_picture   活动图片
- /sales_promoter/range/Range    考勤范围

   