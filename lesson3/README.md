#Code Review
众所周知code review有诸多好处，直接影响包括代码质量和项目的稳定性。最主要目的是为了加快开发的进度，避免由于实现中的不一致、不了解历史问题而造成的错误行为，由于低级代码错误造成的返工，等等。

##Code Review 的主要关注点
我们的code review过程中首先要保证code review的高效率。一般来说我们应当关注：

 * 代码实现正确性，比如是否会造成NullReference，是否会有内存泄露，是否会有死锁，是否有不合理的计算量等。
 * 代码一致性，主要是feature main owner控制，比如UI上的显示应当遵守同样的方式，API设计一致等等
 * interface一致性，包括命名尽量一致。

不应该关注或者少关注：

* 语法错误，各个项目应当通过linter工具或者编译来保证
* 非interface的命名
* code style，应该通过format工具保证
* 设计选择，比如使用什么db，使用什么语言，应该在写代码之前讨论。

##Code Review的途径
多数code review应当通过merge request发出后离线进行。但如果是特别大的修改，可以发了以后坐一起过代码更方便。特别小的修改也可以临时看一下而不发MR。

##Code Review的主要流程？
以做一个中等size的feature为例

* 从main branch（master或者本组特别设定的branch）checkout出新的工作branch： `git checkout -b feature1 origin/master`
* 在feature1 branch上进行开发，随时push commits到gitlab server：`git push -u origin feature1`（第一次），`git push`（之后）
* 开发到一个阶段后，需要进行review，则先将feature1对master做rebase：`git checkout feature1`， `git base master`
* 在gitlab上的project 主页边栏中选择 Merge Request，点击 "+ New Merge Request"，Source branch选择feature1，Target branch选择master，点击continue
* review代码并反复迭代
* review完成，开发者再次rebase feature1，保证当前branch以当前master为base。
* 通知master branch owner "Accept Merge Request"，如果开发者本身可以push到master也可以直接Accept















