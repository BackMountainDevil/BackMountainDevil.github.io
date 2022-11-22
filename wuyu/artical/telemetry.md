# telemetry 匿名信息反馈/遥测 与 收集用户信息
- date: 2022-11-22
- lastmod: 2022-11-22

遥测一次最早在 vscode 看到，最后也因此投奔了 vscodium。在使用 Plasma 时，在系统设置的用户反馈里可以设置反馈频率和内容。Firefox 也可以在 设置/隐私与安全 中关闭数据收集。当然我关注这个问题倒不是因为前边提到的软件，而是我在日常使用中无意中发现的，比如通过 火绒安全的网络控制、opensnitch 发现了某些软件在后台偷偷地发送数据，并未在显眼的位置告知用户收集了哪些数据以及如何关闭遥测数据功能。

关于用户统计数据和崩溃报告的一篇文章 [Should I Let Apps Send “Usage Statistics” and “Error Reports”? Chris Hoffman](https://www.howtogeek.com/166021/htg-explains-why-do-so-many-apps-want-to-send-usage-statistics-and-should-i-let-them/)，解释了这些数据的用处以及崩溃报告可能存在泄漏隐私的例子。

# 应用分析
## KDE Plasma

在 系统设置-用户反馈 里可以设置反馈频率和内容，全选目前是 9 个数据共2KB，一周反馈一次，九个数据条目分别是：QPA信息、Qt版本信息、平台信息、应用程序版本、编译器版本、使用时间、OpenGL信息、屏幕参数、面板数量，数据存储目录为 `~/.local/share/plasmashell/kuserfeedback/audit/`，数据都有 desc 说明，可以直观了解数据含义。

- 数据量：9 条 共 2 KB
- 关闭步骤数：4 步

## Firefox

在地址栏输入 about:telemetry 即可查看遥测数据的相关信息，数据密密麻麻我看不太懂，只是看到有点多，当然有一部分是火狐账户的同步数据(sync)。在“Firefox 遥测数据的收集和删除”一文中，也简要的介绍了收集哪类数据以及如何关闭遥测。

交互数据我是不想被遥测的，技术数据倒是有助于程序的维护，不过在关闭设置里，这两是同时关闭或开启的，在关于崩溃报告的说明里包含敏感数据、网页数据、技术数据。

- 数据量：
- 关闭上传步骤数：3 步（关闭上传服务并不会关闭收集信息服务）

完全关闭收集信息服务的步骤比较复杂，可以参阅 [How to See (and Disable) the Telemetry Data Firefox Collects About You. Brady Gavin. Feb 6, 2020](https://www.howtogeek.com/557929/how-to-see-and-disable-the-telemetry-data-firefox-collects-about-you/)，和 [how to turn off telemetry completely ? 2018](https://support.mozilla.org/en-US/questions/1197144) 存在出入，也都没有提到这样设置的参考手册来源

如果想手动同步火狐的配置文件，可以阅读 [备份你的信息](https://support.mozilla.org/zh-CN/kb/%e5%a4%87%e4%bb%bd%e4%bd%a0%e7%9a%84%e4%bf%a1%e6%81%af)、[用户配置文件](https://support.mozilla.org/zh-CN/kb/%E7%94%A8%E6%88%B7%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6)

## 爱思助手
## WPS

# 相关阅读及其快照备份

[遥测/匿名信息反馈--自21w38a以来一直在强制收集用户身份数据 Voidsd 2021年11月23日 搬运自 Reddit 原作者u/LapisDemon](https://mcbar.club/d/454-21w38a):援引欧盟的《通用数据保护条例》
<details>
<summary>原文备份</summary>

Minecraft玩家们，还有Mojang和微软员工们，如果他们可能会看到这篇文章的话，大家好。
在快照21w38a开始，遥测功能被重新添加回来，但a在用户界面/启动器中并没有添加关闭该功能的选项。
总体上来看，微软至少侵犯了GDPR的数项权利（更多内容见下文）。

遥测，以前被称为匿名信息反馈（Snooper Settings），在不久前曾被移除，不过在那时你可以在设置中禁用它。
你可以通过21w38a更新日志来了解21w38a通过遥测收集哪些数据。

正如我在已关闭的bugpost遥测无法被关闭上所评论的那样，对这种更改的处理方式并不理想。

遥测应该是可选的，而且为了符合GDPR的要求，它必须是可选的。
GDPR的同意要求：https://gdpr.eu/gdpr-consent-requirements
GDPR数据主体的权利：https://gdpr.eu/tag/chapter-3
感谢/u/11people5提供的链接!
GDPR“被遗忘权”：https://gdpr.eu/right-to-be-forgotten
以及更多
如果我列出微软违反的所有GDPR的规定，那么这篇文章将会变得更长，因此我不会在这里列出。

我特别关注的是发送用户ID（XUID），即用户标识符。
这意味着用户可以被追踪，不仅是他们的电子邮件，一般来说，所有有被追踪的价值的数据都与这个用户标识符有关，例如，像聊天记录、年龄组、位置、你的IP、你的Xbox账户中连接/添加的家庭/朋友、你的游戏、游戏中的统计数据，以及更多数据，可能是比我们所知道的更多的数据挖掘（在服务条款中，它往往只是以一种含糊的、美化的、无害的方式被提及），至于哪些是出于商业原因的数据挖掘，我们最多只是在微软的服务条款和Xbox服务条款中被浅显地告知。

XUID是用于标识每个Xbox帐户的唯一用户标识符。
你用这个帐户做的任何事情都需要用到XUID，而不是仅限于Minecraft Java版。

编辑 再次说明一下这个问题，我已经被私下问过几次了。
如果游戏性能是（至少目前）遥测的唯一原因，那么XUID也被追踪就没有意义了。
肯定有其他原因，而我还没有找到Mojang或微软官方的明确、透明的声明，能够说明他们的理由到底是什么的声明。

鉴于微软的地位，以及XUID的用处，目前这似乎是一个-不为人知的-与商业有关的决定，用基本的逻辑加上基本的代码知识，你就会发现情况正收集玩家的XUID并不能帮助Mojang优化游戏。
事实上，在遥测快照发布的第二天，就出现了几个绕过遥测的MOD。
之后的几天，出现了越来越多的帮助玩家绕过遥测的mod。这应该足以说明问题。

Minecraft社区应该被公开告知，关于遥测的每一个细节，没有被美化过的诚实的细节，关于收集XUID的理由的细节。玩家应当自行决定自己是否同意遥测。

追踪XUID，找着“这可能提升游戏性能”的借口去追踪用户的个人信息，在我个人看来这并不是好事，因此我坚决反对强制性的遥测。

在快照中，唯一暗示XUID被用于改善游戏性能以外的用途的措辞是。

    “为了更好地了解我们的玩家并改善他们的体验”

然后，它继续说世界性能。

    “具体来说，我们希望在今年晚些时候的洞穴和悬崖更新的第二部分中，确保更强的世界生成性能。”

这使得它看起来--对于外行人来说--好像遥测仅仅是为了改善游戏性能而采取的，由于我们都喜欢Minecraft和Mojang，我们当然愿意在这里帮忙。
我自己也是这样做的，早在Mojang不属于微软的时候，我就自愿启用了遥测功能。

但是，"为了更好地了解我们的玩家 "和 "改善他们的体验 "是极其广泛的解释，这听起来比它实际上可能更无害。Mojang不会说：
“我们也会追踪你的XUID，虽然不需要它来提高游戏性能，但出于商业原因，微软想知道关于Minecraft用户的详情，他们只能通过将XUID添加到遥测中来达成这一目的” - 他们会指着那一句短语，并认为他们已经把关于收集XUID的理由告诉了玩家。

大多数用户不知道如何理解商业上的或政治上的言论。
如果他们知道这一切意味着什么，他们中的一些人至少会不同意，不顺从，不玩那些 "间谍版本"，不迁移。除非他们可以拒绝微软/Xbox对用户数据的一切挖掘，除非数据挖掘是100%匿名的，100%为了改进游戏，0%为了商业原因。我想自己决定是否希望微软提供给我一些商业相关的东西，而不是让我的Xbox配置文件/ XUID告诉微软我将是微软的X产品或Y产品的完美候选人。

最糟的是，大多数用户信任Mojang。
说实话，他们喜欢他们的开发者。

我个人感到失望的是-据我所知-没有一个Mojang员工向社区解释这到底意味着什么。- 顺带一提，根据GDPR的规定，Mojang必须要解释收集用户数据的原因。
我不知道Mojang是否在内部向微软表达了他们的担忧。
如果他们这么做了，但微软禁止他们公开发表任何言论，那么我收回我刚刚写的内容。
这样的话就100%是微软的问题了，至少Mojang尝试过为玩家发声。

更让我担心的是，更新日志中写道。
“目前仅在世界加载阶段有效” - “目前”，“世界加载阶段”。这暗示了将来会有更多的内容，其他的 “阶段”，更多的遥测数据被收集，而且我们还不知道我们是否可以选择拒绝这些数据收集。

我不喜欢有人在收集所有这些数据以及在未来收集那些我们不清楚的数据之前，不通过某种形式的协议文本彻底通知我，并确保我没有遗漏一些东西，比如我必须勾选的协议文本，其中向我解释了收集这些数据的所有原因（不是含糊不清的“为了更好地了解我们的玩家”，而是实际的潜在商业原因，如果这也是一个原因的话，除了“性能改进”之外的原因），我的数据被储存在哪里，传输的安全性如何，我的数据何时会被自动删除，以及是否匿名等等。我甚至不想勾选“同意所有的服务协议条款”，只想单独勾选部分内容，或者可以选择不勾选涉及隐私以及只是数据挖掘的内容。

我明白优化世界性能的重要性，但这种优化性能的方式引起了对用户隐私和数据保护的担忧，因此必须加入选择退出的选项，或者在已经存在这个选项的情况下指出它的存在。

此外，在已经收集了数据的情况下，可以选择删除这些数据，也可以选择要求对自己被收集风数据进行全面存档。

Minecraft/Xbox未被列入https://docs.microsoft.com/en-us/compliance/regulatory/gdpr-data-subject-requests
由于遥测只适用于迁移的玩家（当然也包括那些使用Microsoft账户的玩家），这应当得到官方的承认。

我强调迁移的原因是，如果玩家选择迁移，他们必须充分意识到迁移所带来的问题。所有问题。不仅是目前的问题，还有迁移后未来微软的计划。
微软究竟会收集哪些数据，在哪里收集，收集多长时间，谁会因为什么原因得到哪部分数据，以及这些个人用户数据会被如何处理。在这之后Minecraft玩家才能对是否接受它做出明智的决定。

由于遥测技术仅用于已迁移的账户（当然还有基岩版玩家），在微软的登录系统中必须有一个选择退出的选项（迁移后），而且还必须非常清楚地说明它在哪里，并被放在明显的位置，最好是在登录时。也许选择退出的选项也可以通过启动器/用户界面来实现，尽管我怀疑微软宁愿在Xbox账户的某个地方这样做，这样用户就会留在他们的生态体系中。此外，目前的Xbox账户是如此不清晰，如此难以浏览，所以用户不会马上看到它来选择退出，除非社区中的大部分玩家都对此感到不满并要求微软在登录时增加明显的选择退出选项。

此外，在我个人看来，整个迁移过程不够透明，在多次联系尝试和要求澄清之后，有关迁移的具体问题没有得到答复。

最后但同样重要的是，https://www.minecraft.net/zh-hans/privacy/gdpr 已经不是最新的了，应该尽快调整。

感谢您的阅读。如果Mojang的任何人碰巧发现了这个帖子，请考虑尝试改变这一切。
Meri

展望未来：除了遥测微软还整了哪些损人利己的东西？

Voidsd： https://www.privacycompany.eu/blogpost-en/impact-assessment-shows-privacy-risks-microsoft-office-proplus-enterprise，https://www.forbes.com/sites/emmawoollacott/2019/10/22/microsofts-eu-contracts-breach-gdpr/amp 等
</details>


[Firefox 遥测数据的收集和删除](https://support.mozilla.org/zh-CN/kb/telemetry-clientid)：默认开启、可关闭、可浏览数据
<details>
<summary>原文备份</summary>

Firefox 默认情况下会收集遥测数据。我们收集此数据是为了帮助提高 Firefox 的性能和稳定性。遥测数据由两个数据集组成：交互数据和技术数据。
什么是交互和技术数据？

交互数据包括你与 Firefox 的交互信息，如打开的标签页和窗口的数量、访问的网页数量、安装的 Firefox 附加组件的数量和类型、会话时长，以及 Mozilla 或我们的合作伙伴提供的 Firefox 功能，如与 Firefox 搜索功能和搜索合作伙伴推荐的交互数据。

技术数据包括有关你的 Firefox 版本和语言、设备操作系统和硬件配置、内存、有关崩溃和错误的基本信息、自动化过程（如更新、安全浏览）的结果的信息。当 Firefox 向我们发送数据时，你的 IP 地址将作为我们服务器日志的一部分临时收集。IP 地址每14天删除一次。
我可以了解有关遥测数据收集的更多信息吗？

可以! 你可以通过在浏览器的地址栏中输入 about:telemetry 来了解我们的遥测数据收集。该页面将向你显示当前会话中发送到 Firefox 的遥测数据。你还可以查看已存档的信息。

请阅读 Firefox 数据文档概览 和 桌面版遥测数据文档 来学习我们如何处理你的数据。你可以从我们供用户公开使用的 探针词典 了解有关特定探针的更多信息。

任何数据收集或共享都遵守我们的 Firefox 数据收集政策。

遥测收集 - Windows 默认浏览器趋势 - 了解我们如何为 Windows 用户收集和使用遥测数据的一个例子。
遥测数据将保留多长时间？

我们将遥测数据保留13个月。
如何选择退出或删除遥测数据？

你可以随时选择 不发送 任何 Firefox 遥测信息。如果你选择不发送遥测数据，我们还将将此视为删除先前收集的任何数据的请求。选择退出后的30天内，数据将被删除。

要选择不发送任何 Firefox 遥测信息：

    点击菜单按钮 Fx89menuButton 并选择 设置。
    选择 隐私和安全 面板.
    滚动到 Firefox 数据收集与使用 部分。
    取消勾选 允许 Firefox 向 Mozilla 发送技术信息及交互数据 复选框。 

要了解有关 Mozilla 接收什么数据以及如何使用它们的更多信息，请参阅 Firefox 隐私声明。
</details>


[谁让你的个人信息在“裸奔”——部分APP“过分”收集用户信息调查 2019-05-24 中国法院网](https://www.chinacourt.org/article/detail/2019/05/id/3955440.shtml)
<details>
<summary>文章备份</summary>

谁让你的个人信息在“裸奔”——部分APP“过分”收集用户信息调查 2019-05-24 10:25:11 | 来源：新华网 | 作者：胡林果 毛鑫

看小说的APP要读取用户短信、贷款类APP要访问摄像头并拍照、上网类APP要读取用户通讯录……伴随着移动互联网的飞速发展，大量APP在不知不觉中收集了一些与自身业务无关的信息，个人信息在网络空间中“裸奔”的现象屡禁不绝。

　　8亿用户的APP被曝超范围收集用户信息

　　一款号称可“一键连接WiFi”的APP WiFi万能钥匙已成为不少人的手机必备。公开数据显示，其月活跃用户已经达到8亿。然而，这款被视为“蹭网利器”的APP，近期却被曝光存在超范围收集用户信息的行为。

　　广东省公安厅近日公布，2019年一季度，广东警方共监测发现1670余款APP存在超范围收集用户信息行为。其中WiFi万能钥匙、钱聚易等10款APP问题突出，特别是WiFi万能钥匙问题最多，共超范围收集了7类信息。据通报，WiFi万能钥匙（4.3.56版本）存在读取用户短信或彩信、联系人，收集用户设备上已知账号，使用用户设备摄像头或麦克风等问题。

　　APP超范围收集用户信息现象并不少见。根据爱加密大数据中心提供的数据，截至2019年3月底，该中心已收录安卓应用270多万个，iOS应用190多万个，30%以上的APP存在不同程度的越权、超范围收集等行为。

　　“这么做多是为了收集用户的经济状况、消费偏好、活动区域等信息，对用户进行精细的人物画像，以支持产品研发更新，或精准推送广告。”广东省公安厅网警总队案件科副科长黄建邦说。

　　暨南大学网络空间安全学院院长翁健表示，“获取用户设备上已知账号列表”易泄露用户隐私。攻击者有可能利用掌握的账号，实行撞库等网络攻击，即从安全性较弱的账号中获取的用户名密码，来推测强安全措施的账号和密码。

　　此外，调用权限发送短信也是手机木马的主要传播方式之一。翁健介绍，应用程序可通过该种方式将带有病毒的链接放入短信中，并依次发送给用户相关联系人，一旦有人点击该链接，则会感染病毒。

　　过度索权套路多 “迷魂阵”里走不出

　　一款主打便捷连网的APP为啥要索取这些“八竿子打不着”的信息权限？

　　记者在安卓手机应用市场上下载该APP后发现，页面弹出的窗口询问“WiFi万能钥匙需要以下权限，是否允许？”包括用户位置、电话、信息、通讯录、相机等权限，但只有点击“允许”才可下载。

　　该产品有关负责人表示，目前，WiFi万能钥匙除了提供WiFi连接以外，产品中也提供资讯、社交等其他服务，广东警方提到的额外获取的权限，是所有社交软件都会需要获取的正常权限。

　　记者发现，安卓版本的WiFi万能钥匙产品内，确有所谓的社交功能“附近的人”，然而记者点击后发现，使用“附近的人”功能需要另外下载“连信”APP。不仅如此，该APP的下载安装并未经过应用市场。截至记者发稿时，“连信”APP在安卓应用市场内仍无法通过关键字搜索出来。

　　业内人士表示，此举意味着WiFi万能钥匙的相关产品“连信”APP未经过安卓应用市场的审核，即使用户可以自主选择提供或不提供相应权限，但用户下载使用仍存在一定的风险。

　　此外，WiFi万能钥匙调用的权限实际上是为另一款APP使用，这是为其他APP规避监管“打掩护”，既侵犯了用户的知情权，同时也把用户拉进了“迷魂阵”——难以辨别哪一类信息权限是与产品服务直接相关的。

　　那么是否关掉所有的权限即可保护用户个人信息呢？事实上并不容易。

　　翁健表示，有的权限看似与APP运行无关，其实后台的服务需要这些权限。然而，有的开发者故意将APP的超范围权限与正常权限的模块“打包”，导致在阻隔了APP的超范围权限后，正常程序无法运行。

　　专家建议从源头端加强公民个人信息保护

　　黄建邦表示，正因为企业收集这些信息的成本远低于可能的收益，导致很多APP索权无度，也就有了“不管有用没用，先收集来再说”的心态和做法。

　　对于如何从源头端保护用户个人信息，专家建议，首先应用商店要做好把关，对上架下载量大的APP要求经过人工检测，并确立一个原则——每个APP只给最低的信息收集权限，且每次信息收集都要获得用户许可。

　　近日，由中央网信办、工信部、公安部、市场监管总局指导成立的APP专项治理工作组起草了《APP违法违规收集使用个人信息行为认定方法（征求意见稿）》，该征求意见稿将APP违法违规收集使用个人信息分为7种情形。翁健认为，该文件明确了违规APP行为的具体认定标准，有助于网络安全法的相关要求真正落地见效。

　　黄建邦呼吁，从立法的角度对用户个人信息进行分类分级管理，明确APP收集哪一类信息需要哪些授权程序，可探索信息收集备案制，信息收集只有经过法律授权而非简单用户授权才可行。 
</details>


[Impact assessment shows privacy risks Microsoft Office ProPlus Enterprise 11/13/2018](https://www.privacycompany.eu/blogpost-en/impact-assessment-shows-privacy-risks-microsoft-office-proplus-enterprise)：Microsoft 大规模秘密收集并存储用户内容数据、功能数据、诊断数据
<details>
<summary>原文备份</summary>

On behalf of the Ministry of Security and Justice, Privacy Company carried out a DPIA on DPIA on Microsoft Office ProPlus (Office 2016 MSI and Office 365 CTR). At the request of the Ministry, we publish this blog about the findings. For questions about the research you can contact SLM Rijk (Strategic Vendor Management Microsoft Rijk), accessible via press release from the Ministry of Justice, 070 370 73 45.

The SLM Rijk conducts negotiations with Microsoft for approximately 300.000 digital work stations of the national government. The Enterprise version of the Office software is deployed by different governmental organisations, such as ministries, the judiciary, the police and the taxing authority.

The results of this Data Protection Impact Assessment (DPIA) are alarming. Microsoft collects and stores personal data about the behaviour of individual employees on a large scale, without any public documentation. The DPIA report (in English) as published by the Ministry is available here

Starting today, and with the help of Microsoft, SLM Rijk offers zero exhaust settings to admins of government organisations. During the writing of this DPIA, Microsoft has committed to take a number of other important measures to lower the data protection risks.

‍
Office 2016 and Office 365

Most government organisations in the Netherlands use versions of Office 2016 and Office 365 (or even older versions) that are installed on the computers of the government employees. The organisations store the content data locally, in their own data centres (on premise). But this will change. SLM Rijk conducts a pilot with data storage in the Microsoft cloud, in SharePoint, and in OneDrive. There is also a test with the web-only version of Office 365, where the software is no longer installed on the end-user devices.

‍
Large scale and covert collection of personal data

Microsoft systematically collects data on a large scale about the individual use of Word, Excel, PowerPoint and Outlook. Covertly, without informing people. Microsoft does not offer any choice with regard to the amount of data, or possibility to switch off the collection, or ability to see what data are collected, because the data stream is encoded. Similar to the practice in Windows 10, Microsoft has included separate software in the Office software that regularly sends telemetry data to its own servers in the United States. For example, Microsoft collects information about events in Word, when you use the backspace key a number of times in a row, which probably means you do not know the correct spelling. But also the sentence before and after a word that you look up in the online spelling checker or translation service. Microsoft not only collects use data via the inbuilt telemetry client, but also records and stores the individual use of Connected Services. For example, if users access a Connected Service such as the translate service through the Office software, Microsoft can store the personal data about this usage in so called system-generated event logs.

‍
Difference between content, diagnostic, and functional data

Microsoft provides services over the Internet. From a technical perspective, it is inevitable that you have to provide data to Microsoft, such as the header of your e-mail and your IP address in order to be able to use the services. But Microsoft should not store these transient, functional data, unless the retention is strictly necessary, for example, for security purposes. In this DPIA report (data protection impact assessment report), the data which Microsoft collects via Office ProPlus are divided in three categories:

    Content data: the content of files and communication that you store in your own datacenter or on cloud computers of Microsoft
    Functional data: the data you have to transmit over het Internet to be able to connect to Microsoft’s internet services
    Diagnostic data: the data that Microsoft stores for analysis of the usage of the services

In the report, Privacy Company uses these three categories of data in analogy with the division of communications data in ePrivacy law in Europe. This legislation distinguishes between (i) content, (ii) traffic/location data that are generated as a result of using the communication services, and (iii) data that are strictly necessary to transmit the communication, but have to be erased or anonymised immediately afterwards.

Microsoft emphasises that the company does not use these categories. Microsoft uses, amongst others, the categories of ‘Customer Data’ and ‘Personal Data’. Microsoft only uses the term Diagnostic Data for the specific telemetry data collected via the inbuilt software client in the locally installed Office software.

‍
23.000 to 25.000 types of events

Microsoft does not (yet) offer a possibility to inspect the contents of the diagnostic data flow. Microsoft has explained that 23.000 to 25.000 types of events are sent to Microsoft’s servers, and that 20 to 30 engineer teams work with these data. The engineers can dynamically add new events to the data stream from all computers with Office ProPlus. This collection of data is much more specific than in Windows 10 telemetry. If the telemetry is set to ‘full’ in Windows 10, it involves one thousand up to twelve hundred types of events. And 10 teams with engineers. The Dutch DPA conducted an investigation in 2017 of the processing of telemetry data in the consumer and small business versions of Windows 10 (Home and Pro).

The Dutch DPA concluded that Microsoft violated data protection law on many counts, amongst others through the lack of transparency and purpose limitation, and the lack of a legal ground for the processing.

In response to that investigation, Microsoft made some adjustments in the spring 2018 release of the software. The Dutch DPA concluded (prior to the actual release of the software, press release in Dutch only) that the improvement plan presented by Microsoft would end all violations. The Dutch DPA did not investigate data processing via the Office software.

‍
Microsoft as a (joint) controller and not as a data processor

Microsoft determines the purposes of the processing of the diagnostic data in the Office software, and the retention period of the data (30 days up to 18 months, or even longer if deemed necessary by Microsoft). The DPIA report shows that Microsoft processes the diagnostic data for 7 purposes, and for all other purposes Microsoft deems to be compatible with those purposes. Because Microsoft determines the purposes and the means (of the retention period), Microsoft acts as a controller, and not as a data processor.

The 7 purposes are:

    Security (identifying and mitigating security threats and risks as quickly as possible through updates to Office ProPlus Applications and remediation of connected services)
    Up to Date (delivering and installing the latest updates to the Office ProPlus Applications without disruption to the experience)
    Performing Properly (identifying and mitigating anomalies, “bugs,” and other product issues as quickly as possible through updates to the Office ProPlus Applications and remediation of connected services)
    Product development (learning to add new features)
    Product innovation (business intelligence, develop new services)
    General inferences based on long-term analysis, support machine learning
    Showing targeted recommendations on screen to the user
    Purposes Microsoft deems compatible with any these 7 purposes.

The Office ProPlus software includes the use of a number of online services. But Microsoft also offers so called ‘discretionary’ (voluntary) Connected Services, such as the online spelling checker and the translation service. Microsoft only considers itself to be a data controller when people use these discretionary Connected Services. In that case, Microsoft processes the personal data about the use of these services for all 12 purposes listen in its general privacy statement.

‍
High data protection risks for data subjects

The DPIA report provides an extensive description of 8 high data protection risks for data subjects. The government organisations that use Office should, however, determine themselves what the specific risks are, based on the specific personal data they process. This DPIA report is meant to assist, not to replace.

During the writing of this DPIA report, Microsoft has already made commitments to SLM Rijk to make important adjustments to lower the risks. Microsoft has developed zero-exhaust settings. Microsoft also intends to provide adequate information, include a data viewer tool for the telemetry data from Office and provide an option to administrators to determine the desired level of telemetry. Additionally, SLM Rijk and Microsoft office will jointly work on the correct qualification of Microsoft as a (joint) controller or data processor.

Some residual risks can be mitigated if the government organisations will use the newly developed settings to minimise the processing of telemetry data. There are 6 remaining high risks for data subjects.

    The unlawful storage of sensitive/classified/special categories of data, both in metadata and in, for example, subject lines of e-mails
    The incorrect qualification of Microsoft as a data processor, instead of as joint controller as defined in article 26 of the GDPR
    The lack of purpose limitation, both for the processing of historically collected diagnostic data and the possibility to dynamically add new types of events
    The transfer of (all kinds of) diagnostic data outside of the EEA, while the current legal ground for Office ProPlus is the Privacy Shield and the validity of this agreement is subject of a procedure at the European Court of Justice
    The indefinite retention period of diagnostic data and the lack of a tool to delete historical diagnostical data

What can the admins do now to lower the risks? Admins of the Enterprise version of Office ProPlus can already take a number of specific measures to lower the privacy risks for employees and other people in the Netherlands.

    Apply the new zero-exhaust settings
    Centrally prohibit the use of Connected Services
    Centrally prohibit the option for users to send personal data to Microsoft to ‘improve Office’
    Do not use SharePoint Oneline / OneDrive
    Do not use the web-only version of Office 365
    Periodically delete the Active Directory account of some VIP users, and create new accounts for them, to ensure that Microsoft deletes the historical diagnostic data
    Consider using a stand-alone deployment without Microsoft account for confidential/sensitive data
    Consider conducting a pilot with alternative software, after having conducted a DPIA on that specific processing This could be a pilot with alternative open source productivity software. This would be in line with the Dutch government policy to promote open standards and open source software.

These measure are not in all cases realistic or feasible. It is not possible for the (Enterprise) customers of Office to solve all problems. With regard to the contracts and transfer of personal data to the USA, a European solution must be sought.
</details>


[Win10优化：手把手教你如何禁止系统收集你的数据 2018-06-27](https://post.smzdm.com/p/720270/)

[]()
