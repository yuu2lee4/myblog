## 学习环境： 
nodejs ：4.4.3，express ：4.13.4，mongoDB ： 3.2.4，robomongo: 0.9.0-RC7

##  依赖
![package.json](https://dn-cnode.qbox.me/FiRDWBofIK4psu5Usq6bCShH6Rni)

## 学习例子：
**nswbmw**的[一个简单的博客](https://github.com/nswbmw/N-blog/tree/backup)

接下来，我将按照例子中的章节依次列出遇到的问题以及解决的方法。

### 第1章 一个简单的博客
1. windows下用set DEBUG=express:* node ./bin/www设置DEBUG环境变量，再用npm start访问页面，但是输出的信息太多了，所以我们干脆不设（使用“set DEBUG=”清除DEBUG环境变量），就用npm start，此时在bin/www里面的debug('Listening on ’ + bind)下面添加console.log('Listening on ’ + bind)；
2. 不必按照作者那样把建立服务器也弄到app.js里面来，就让它在bin/www里就好；
3. 建议使用nodemon代替supervisor；
4. app.use(flash());要放在app.use(session({}))之前；
5. 控制台会提示express-session的配置里面最好设置resave、saveUninitialized；
6. 注册用户成功后，回调里面的user[0]改为user.ops[0]，可以将user的结构打出来看看。

### 第3章 增加文件上传功能
如果你跟我一样使用的1.1.0的multer，那么请按照以下步骤增加上传功能：
1. index.js
```javascript
var multer  = require('multer');
var storage = multer.diskStorage({
  destination: function (req, file, cb) {
  	cb(null, './public/images')
  }
})
app.post('/upload', checkLogin);
app.post('/upload',multer({ storage: storage }).array('photos', 12), function (req, res) {
  req.flash('success', '文件上传成功!');
  res.redirect('/upload');
});
```
2. upload.ejs  
所有file input的name都改为photos

### 第14章 增加头像
因为http://www.gravatar.com被墙了，所以把http://www.gravatar.com换成http://gravatar.duoshuo.com

### 第15章 增加转载功能和转载统计
给转载链接注册了路由响应并不需要Post.edit()
