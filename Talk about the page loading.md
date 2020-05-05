# 谈一下页面加载过程
## 浏览器组成
### 一、渲染引擎
* 不同浏览器有不同的渲染引擎
* 操作过程
  1. **解析HTML** 拿到HTML文本开始解析，产生DOM Tree
  2. **解析CSS** 遇到css，则异步请求css，并解析，产生CSS Rule Tree(CSSOM)
  * **注：** 即HTML与css同时解析、互不影响
  3. **对象合成** 将产生的DOM Tree 和 CSS Rule Tree综合产生Render Tree
  * **注：** Render Tree是一部分一部分显示的.</br>只要有一部分DOM Tree和CSS Rule Tree都解析完了，相应的那部分Render Tree就开始布局了，完事儿就可以绘制在页面了。</br>即：不是所有HTML文档解析完毕才一起展示(也就是页面是一部分一部分加载出来的)
  4. **布局** 计算出渲染树的布局（layout）
  5. **绘制** 将渲染树绘制到屏幕
### 二、js引擎（js解释器）
* js引擎与渲染引擎互斥
* 默认遇到js脚本时立即将渲染权交给js引擎，解析并执行完成脚本后才继续渲染引擎的任务
* **所以产生了js阻塞**
#### 2.1 设置defer属性
1. HTML解析过程中，发现带有defer属性的<script>元素
2. 浏览器继续往下解析 HTML 网页
3. 同时，并行下载<script>元素加载的外部脚本
4. 浏览器完成解析 HTML 网页
5. 此时再回过头执行已经下载完成的js脚本
* **注：**  
   * defer可以**保证执行顺序**就是它们在页面上出现的顺序
   * 对于内置而不是加载外部脚本的script标签，以及动态生成的script标签，defer属性不起作用
   * 使用defer加载的外部脚本不应该使用document.write方法
#### 2.2 设置async属性
1. HTML 解析过程中，发现带有async属性的<script>标签
2. 浏览器继续往下解析 HTML 网页
3. 同时，并行下载<script>标签中的外部脚本
4. js脚本下载完成
5. 浏览器立刻暂停解析 HTML 网页，开始执行下载的js脚本
6. js脚本执行完毕，浏览器继续解析 HTML 网页
* **注：**  
   * async**无法保证脚本的执行顺序**
   * 对于内置script标签、以及动态生成的script标签，async属性也起作用
   * 使用async加载的外部脚本也不应该使用document.write方法
   * 当与defer属性同时存在时，async的优先级更高
#### 2.3 defer和async属性使用场景
1. 使用async —— 脚本之间没有依赖关系
2. 使用defer —— 脚本之间有依赖关系