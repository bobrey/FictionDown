# FictionDown

用于下载盗版网络小说

[![License](https://img.shields.io/github/license/ma6254/FictionDown.svg)](https://raw.githubusercontent.com/ma6254/FictionDown/master/LICENSE)
[![release_version](https://img.shields.io/github/release/ma6254/FictionDown.svg)](https://github.com/ma6254/FictionDown/releases)
[![last-commit](https://img.shields.io/github/last-commit/ma6254/FictionDown.svg)](https://github.com/ma6254/FictionDown/commits)
[![Download Count](https://img.shields.io/github/downloads/ma6254/FictionDown/total.svg)](https://github.com/ma6254/FictionDown/releases)

[![godoc](https://img.shields.io/badge/godoc-reference-blue.svg)](https://godoc.org/github.com/ma6254/FictionDown/)
[![QQ 群](https://img.shields.io/badge/qq%E7%BE%A4-934873832-orange.svg)](https://jq.qq.com/?_wv=1027&k=5bN0SVA)

[![travis-ci](https://www.travis-ci.org/ma6254/FictionDown.svg?branch=master)](https://travis-ci.org/ma6254/FictionDown)

## 特性

1. 以起点为样本，多站点多线程爬取校对
2. 支持导出markdown，可以用pandoc转换成epub，保留书本信息、卷结构、作者信息
3. 内置简单的广告过滤（现在还不完善）
4. 用Golang编写，安装部署方便，外部依赖只有PhantomJS

## 使用流程

1. 输入起点链接
2. 获取到书本信息，开始爬取每章内容，遇到vip章节放入`Example`中作为校对样本
3. 手动设置笔趣阁等盗版小说的对应链接，`tamp`字段
4. 再次启动，开始爬取，只爬取VIP部分，并跟`Example`进行校对
5. 手动编辑对应的缓存文件，手动删除广告和某些随机字符(有部分是关键字,可能会导致pandoc内存溢出或者样式错误)
6. `d -f md`生成markwown
7. 用pandoc转换成epub，`pandoc -o xxxx.epub xxxx.md`

### Example

```bash
> ./FictionDown --url https://book.qidian.com/info/3249362 d # 获取正版信息
> vim 一世之尊.FictionDown # 加入盗版小说链接
> ./FictionDown -i 一世之尊.FictionDown d -f md # 获取盗版内容
> pandoc -o 一世之尊.epub 一世之尊.md
```

## 未实现

- 爬取起点的时候带上`Cookie`，用于爬取已购买章节
- 支持刺猬猫（即“欢乐书客”）
- 支持直接输出epub，不需要pandoc
- 支持小说站内搜索
- 多线程转换md
- 整理main包中的面条逻辑
- 整理命令行参数风格
- 在windows下，md转换到epub时有路径问题
- 完善广告过滤
- 简化使用步骤
- 优化log输出

## 编译

包管理采用godep

1. `dep ensure -v`
2. `gox github.com/ma6254/FictionDown/cmd/FictionDown`

## 支持的盗版站点

随机挑选了几个，实际上有些过于旧的书和作者频繁修改的书是爬不全的

- www.biqiuge.com
- www.biquge5200.cc
- www.bqg5200.com
- www.booktxt.net
- www.81new.com

## 盗版内容广告

记录一下可能的广告内容，可以在`使用流程`中手动删除广告内容

- 段落最后`\r`
- 段落最后`rs`
- `天才一秒记住`

### 章节信息

章节内可能会有书名、章节名等，一般在该章第一段最前，或者最末一端

```yaml
- 第587章飞机事变
```

### 分割线

```yaml
- '-----这是华丽的分割线--'
- 小说网友请提示:长时间阅读请注意眼睛的休息。推荐阅读：
- '----这是华丽的分割线---'
- …………
```

### 网站广告

```yaml
- “战争，毫无人性可言！！”2k阅读网 # 章节最后一端的最后
```

### APP下载

```yaml
- Ps:书友们，我是火之高兴，推荐一款免费小说App，支持小说下载、听书、零广告、多种阅读模式。请您关注微信公众号：书友们快关注起来吧！
- 公告：笔趣阁APP安卓，苹果专用版，告别一切广告，请关注微信公众号进入下载安装：appxsyd
```

### 各种在线看

特指黄色广告，有时是微信号有时是网站，有时是单独一行，有时是跟在最后一行末尾

```yaml
- 泰国最胸女主播全新激_情视频曝光扑倒男主好饥_渴!!在线看:!!
- G_罩杯女星偶像首拍A_V勇夺冠军在线观看!!:meinvlu123!!
- 翘_臀女神张雪馨火辣丁_字_裤视频曝光！！请关注微信公众号在线看：baixingsiyu66！！
- 童颜巨_Ru香汗淋漓大_尺_度双球都快溢_出来的大_胆视频在线看!!请关注微信公众号：meinvmei222！！
- 泰国最胸女主播全新激_情视频曝光扑倒男主好饥_渴!!在线看:!!
- G_罩杯女星偶像首拍A_V勇夺冠军在线观看！！：
- 厉害的屁股丰满迷人的身材！微信公众：meinvmeng22你懂我也懂！
```

### 微信公众号or微信号

有时作者也会推荐一些微信公众号，但是这里指的不是这个，是盗版网站自己加的营销推广广告

```yaml
- 推荐一个分享天猫淘宝购物优惠券公众号:guoertejia先领券再购物最高可以节省90%。打开微信添加微信公众号:guoertejia
- 推荐一个淘宝天猫内部折扣优惠券的微信公众号:guoertejia每天人工筛选上百款特价商品。打开微信添加微信公众号:guoertejia省不少辛苦钱。
```

### xxx说

xxx是当前小说中的某个角色，一般为主角，后跟单独一行一个10以内的数字

### 网址

```yaml
- 记住手机版网址：m.
- 先给自己定个小目标：比如收藏：.手机版网址：m.
- 破防盗完美章节，请用搜索引擎各种小说任你观看
- 搜索引擎各种小说任你观看，
- https:///book_73598/l
- /book_66099/l
- 天才一秒记住本站地址：。笔趣阁手机版阅读网址：
- 'https:'
- 天才一秒记住本站地址：。顶点：88106
- 热门推荐：
- 如您已阅读到此章节，请移步到wωw.999wχ.cοm阅读最新章节,手机同步阅读请访问sj.999wχ.coμ,清爽无广告。敬请记住我们新的网址999wχ.coμ
- 最快更新，无弹窗阅读请。
- 纯文字在线本站域名手机同步请访问
- 提供全文字在线，更新速度更快文章质量更好，如果您觉得网不错就多多分享本站谢谢各位读者的支持
- 高速首发一世之尊，本章节是地址为如果你觉的本章节还不错的话请不要忘记向您qq群和微博里的朋友推荐哦
- http:///txt/73/73869/
- 。_手机版阅读网址：
- 请记住本书首发域名：.com。妙书屋手机版阅读网址：.com
- 。妙书屋
- http:///txt/73869/
- 一秒★小△说§网..Org】，精彩小说无弹窗免费阅读！
- m.。
- m.手机最省流量,无广告的站点。
- 搜索：9--9--9--w--x阅读最新章节-绿色无广告-快速稳定-免费阅读
- 手机用户请浏览m阅读，。
- 更新快无广告。
- 唉，这真是搞什么搞啊！李琳是完全搞不明白现在的状况，这本来直来直去的事情，非要中途拐弯。*菠⿻萝⿻小*说
- ±菠∨萝∨小±说
- “而且像地狱火这样的战争傀儡，我目前可就这一个。无广告的站点。”埃文森一脸肉疼的表情说道“而且他是一次性用品，不能回收。”
- “一秒★小△说§网..Org】，精彩小说无弹窗免费阅读！”
- “将毁灭万物当作企业文化的燃烧军团，从来不相信有什么东西是杀不死的，有什么东西是不可以毁灭的。本文由ｗｗｗ。lwχｓ520。ｃｏｍ首发如果有的话，那只是因为你的力量不够大，或者是没有找对方法。”
```

### 空行

不明意义的空行，有时会有省略号

```yaml
- 。
- “”
- "18119"
- '!!:!!'
- a
- :,,gegegengxin!!
- :,,!!
- ':'
- '&#&#&#&#&#&#&#&#&#&#&#&#&#&#&#&#&#&#&#&#&#&#&#&#&#&#&#'
- 8)
- ♂！
```

### 特殊的结尾

- 以.结尾

### 屏蔽词问题

有时小说内会出现用*号代替的屏蔽词，当小说中连续出现`*xxx*`之类的就会被识别为markdown中的强调样式
用`□`或者`■`代替

### 随机字符

一般是章节正文第一行的开头或者末尾，有时会有像`[]`、`()`等等的关键字，不删除可能会引起pandoc内存溢出

```yaml
- 时间已经很晚了，石磊虽然激动难耐，但他还是将那些字画全都收拢起来，不打算继续观看ＷwＷ..lā
- “什么时候轮到丫环指挥主人做事了”孟奇拿捏着腔调，不放过任何一个机会打击顾小桑。。。看最新最全小说
- 砰一声，门板被撞飞，四个人如旋风般冲了进来，站定于剑将军身前。.xs.org看最新最全小说
- 接下来要出场的是，来自阿斯嘉德的自由体操选手希芙，请大家欣赏她精彩的~щww~~lā
- 墨菲斯托饶有兴趣地看着，正仔细的审视着契约的埃森“小子，你很专щww”
- “你看人家托尼的那套3d投影系统多上档次，那左手右手一个慢动作，不光把该办的事情办完了，还能装逼耍帅。㈠你这神盾局局长就不能跟人家好好学学？”
```

![boom_img](https://raw.githubusercontent.com/ma6254/FictionDown/master/doc_img/boom.png)