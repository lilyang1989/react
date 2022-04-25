## Day1

* babel 能将jsx转成js

* react development是react的核心

* react-dom  react扩展库

* 只有先引入核心库才能引入扩展库

* 标签type必须写babel

* 标签内不能加单引号





### jsx规则：

1.定义虚拟DOM时，不要加引号

2.标签中混入js表达式，要用{}

3.样式的类名指定不要用class，要用classname

4.内联样式。要用style={{key:value}}的形式去写

5.只能有一个根标签，即剩下的必须嵌套在里面

6.标签必须闭合

7.标签首字母：

​										1）若小写开头 则将该标签转换为html中的同名元素，若无对应的同名元素则报错								

​										2）若大写开头 则找react的对应组件  没有定义则报错

### git TEST