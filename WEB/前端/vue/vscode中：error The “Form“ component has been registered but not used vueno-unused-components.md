问题描述：
组件导入了，且在componets中注册了，也在模板中使用了 ，但是依然报错：error The “Form” component has been registered but not used vue/no-unused-components，组件注册了，但是没有使用。



报错信息：

解决方法：


直接将componets中注册的组件名在模板使用即可，不用改成小写加连字符

