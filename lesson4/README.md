#iOS 发布流程和分支使用方法

##Branches

* stable - 当前app store线上版本最后commit，所有版本的tag应该从此分支标记，在app store最后发版时刻tag
* master - 当前app store待发版本开发分支，所有针对当前待发版本做的修改，一般应该从master checkout一个新分支，之后开Merge Request进行review，然后merge进入master
* testflight - testflight 打包分支，所有在testflight上发布的版本应当从此分支打包，并且标记tag
* x.x.x - 临时发版branch，当某次发版时，如果master中的commits有一些不想进入下一版本，则临时建立某分支用于发版，发版后删除。

##发布流程
比如，当前APP STORE线上版本为 4.9.10，待发的下一版本为4.9.12，有一个正在长期做的feature新导航页在navigation分支，则：

* Stable分支为4.9.10的最后一个commit，发布后打tag4.9.10
* 4.9.10发布后，马上将 testflight分支置为master，并且修改版本为4.9.12.0，并且向苹果申请testflight审核（最长一周）
* 所有开发在自己的feature或者bug fix分支做开发，之后通过MR，在规定时间如周四中午前，merge进入master
* 开发负责人，在周四code freeze之后，确认master的修改，然后merge进入testflight
* 测试对比stable和当前testflight之间的commits，确认所有commits都是针对我们选择要发版的feature或者bug fix
* 测试修改testflight分支上版本号为 4.9.12.1并打tag，上传到testflight（这时应该已经通过了4.9.12.x的审核，可以马上测试）
* testflight参与者进行测试，报bug，默认为p1，测试和开发商议是否为 p0或者p2，并修复
* 修复时，同样按照bug fix => testflight MR的形式review和merge，完成后通知测试，测试将testflight分支版本修改为4.9.12.2，上传并再次测试
* 反复测试直到没问题
* 测试和开发确认，然后在app store上发布
* 测试将当前testflight merge到stable，然后打tag 4.9.12
* 开发将当前testflight merge到master，继续下一版本开发

##特殊情况
比如，在上述情况下，在进入正式发版code freeze之前，navigation分支想让大家先测试一下，则：

* 该开发reset testflight分支到navigation分支，通知测试
* 测试修改testflight分支版本号（如4.9.12.1），打tag，并打包上传到testflight，通知测试参与者 Code freeze之后则原则上不允许新的feature内测，并且code freeze之后，版本从4.9.12.2开始（累计）。

比如，在上述情况下，4.9.12需要做一次紧急发版仅仅包括少量fix

* 开发从stable checkout出4.9.12分支，cherry-pick适当的master中的修改，然后所有其他修改通过 bug_fix => 4.9.12
* 4.9.12好了之后 reset testflight为 4.9.12，并按上述过程测试打包发布

比如，在上述情况下，4.9.12需要做一次紧急发版仅仅包括少量fix，且时间不够testflight审核

* 开发从stable checkout出4.9.12分支，cherry-pick适当的master中的修改，然后所有其他修改通过 bug_fix => 4.9.12
* 测试直接从4.9.12打包并通过开发者设备测试
* 测试完成直接提交审核
* 发布后，4.9.12 merge 到 stable，master，打tag，之后删除此分支