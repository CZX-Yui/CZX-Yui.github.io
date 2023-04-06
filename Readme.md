# YuiのBlog

### 关于我的博客

- 用来做知识总结：当我开始研究一个新领域时，我可以将研究路径记录下来，形成有体系的研究路线，与相关学科形成交叉图。我可以汇总资源，用自己的话总结下来，并抽取出形成一套自己可用的浓缩知识。
- 用来做经验分享、教程备注：当我准备实现某件事（如安装某某工具，配置某某环境）时，这些需要详细步骤且容易出错、且隔三岔五就要重新部署一套的东西，我希望可以作为经验教程记录下来，方便日后反复查看。遇到新版本时也方便随时迭代。
- 用来做论文、博客文章等阅读笔记：当我在网上冲浪时总会不时关注到一些新的文章，可能是感兴趣的点，可能是与研究工作领域相关的点，可以mark一下到收藏栏里随时查看，看完并用自己的话总结一遍。
- 用来做项目记录：除了学习输入总结以外，也需要做一些个人的输出。时不时有自己的想法需要实践，不妨开个项目记录帖子。
- 。。。



### TODO

- [ ] 实现云端编辑，增加随笔闪念页面

- [ ] 增加鼠标点击特效、背景代码雨、代码娘等

- [ ] 增加页面访问记录，访客记录，设置点赞选项

- [ ] 备份，开新分支

- [ ] 在线编辑



#### 项目文件组织

整个项目200多M，但其实是有3个拷贝，即共四份相同的文件，只不过是不同形式而已

- 源文件：md文档、一堆图片
- public：本地部署server，转译的格式
- .deploy_git：远程部署server的本地拷贝
- .git：本地内容同步，备份（除掉public和.deploy_git）