
# AI Coding 工具怎么用？看看滴滴人怎么说？

> 来源：https://yunyinghui.feishu.cn/docx/Vk5edbWfGoxhMKxiOcjcZCtEnDg

> 来源👸号：滴滴技术

> 推荐理由：篇文章是滴滴技术团队推出的《技术桔有话说》专栏内容，围绕 AI Coding 工具的应用、技巧与未来展望展开讨论，邀请了多位一线开发者（“技术桔”）分享实践经验。

![](img/d64695d69ec6aa17a76d5190b02d4742.png)

深夜，盯着屏幕上报错的代码，咖啡早已见底，是否曾想过能有位“AI 队友”接过手里的键盘，帮助一起解决问题该多好？

如今，AI Coding 工具正全球爆发式涌现，在大模型的助推下，AI Coding 正从单纯的代码补全向更智能化的方向演进，重塑开发者的工作流。

尽管存在生成式 AI 对开发者编程技能价值影响的担忧，但普遍的观点认为，AI 在增强而非取代开发者，使其能够更专注于投入到更具创造性的工作。

### 《技术桔有话说》特邀 5 位桔厂“技术桔”，一起来聊聊 AI Coding 带给开发者的工作范式转变？

## “技术桔”有话说

### 哪些场景会用到 AI Coding？

![](img/304914e89df9c367e9bc97cd8bade459.png)

阳光青桔 小图

▶ 复杂业务中寻求实现思路 ：在处理多状态的 UI 组件或复杂的数据流转换时，会先把需求和现有的代码结构告诉 AI，让它提供几种实现思路，再根据项目的技术栈和性能要求选择合适方案。

▶ 代码重构 ：项目中有些历史代码比较乱，在维护时会用 AI 来分析代码结构，识别可优化的地方。特别是一些可以抽象成公共组件或者应用设计模式的地方，AI 的建议往往很有参考价值。如 `RecycleView` 使用的优化。

![](img/6db3baf3a42355cc8b669d18a97bd76d.png)

▶ `API` 集成和第三方库的使用 ：虽然官方文档很详细，但有时想快速了解最佳实践或者常见的坑点，AI 能给出比较实用的建议和代码示例。

▶ 性能优化 ：当发现某个页面加载慢或者内存占用高时，会把相关代码和 `profiling` 数据给 AI 分析，它通常能指出一些没注意到的优化点。

▶ 解决 `bug` ：`logcat` 里的错误信息难理解时，会把 `stack trace` 发给 AI，它通常能解释原因和提供解决方向。当然，有了参考思路，具体怎么修还是要自己动手。

![](img/09dba27e70979fa6b3f1acdbcd9e61b6.png)

资深算法桔 橙橘

▶ 代码重构与优化 ：针对历史遗留代码，AI 能够进行静态代码分析并识别重构机会。在优化一个使用传统 `ListView` 的页面时，AI 不仅指引使用 `RecyclerView`，还详细说明了 `ViewHolder` 模式的实现细节和缓存策略配置。页面加载速度提升了 40%，滚动流畅度提高了 30%。

▶ 性能调优与问题排查 ：当遇到内存泄漏或卡顿问题时，结合 `Profiler` 数据和 AI 分析能快速定位问题。在优化一个图片编辑功能时，AI 通过分析内存快照指出了 `Bitmap` 对象未及时回收的问题，并提供了使用 `BitmapPool` 和弱引用的解决方案，使内存占用降低 60%。

▶ 跨平台开发支持 ：在开发多平台应用时，AI 能够提供不同技术栈的实现方案对比。例如在选择 `Flutter` 还是 `React Native` 时，AI 从性能、生态、学习曲线等多个维度进行详细分析，并针对具体业务场景推荐 `Flutter` 方案，最终实现 `iOS` 和 `Android` 平台代码 85%的复用率。

![](img/259bc43934dada3d1679e528b217d2b9.png)

安全工程桔 Spring

▶ `API` 查阅与调用 ：大部分框架和第三方库都有大量 `API`，即使官方文档齐全，但查阅 `API` 或新库的文档时间成本很高，AI 能高效给出建议。

▶ 单元测试 ：接口自测的工作在整个需求的开发时间成本中占用不小的比例，包括写单元测试、造测试数据、校验逻辑是否符合预期。现在会用 AI 自动生成单元测试，并直接识别接口的入参数据格式，生成对应测试 `mock` 数据。

## 玩转 AI Coding 的实用小技巧

![](img/00064eb9f5240539d93a7aa3199a5915.png)

阳光青桔 小图

自己编码探索时，体验过 `GitHub Copilot`、`Claude` 和 `Cursor` 这几款工具，各有特色。

`GitHub Copilot` 的代码补全很智能，特别是在写一些标准的 `Android` 代码时，通常会把它的建议作为起点，然后根据项目规范调整。

`Claude` 在代码分析和技术讨论方面表现很好。当我需要理解某个架构设计的思路，或者比较不同技术方案的优劣时，它能给出比较深入的分析。

`Cursor` 的项目级理解能力让我印象深刻。在重构模块间的依赖关系时，它能准确理解各个组件的职责和相互关系，给出比较全面的重构建议。

一般会根据具体需求组合使用这些工具，并不会完全依赖某一个。

除了以上提及，国内也有一些不错的 AI Coding 工具，例如 `Trae`、`通义灵码` 等。这里整理了一些不同工具的适用场景，仅供大家参考。

![](img/8c4808a4fac6c215e82e87919a8b87ef.png)

在这里，不做具体产品推荐，而是根据不同工具特性，给出一些选择思路。

![](img/352daab48c8f31a000b22ca47edb2925.png)

资深研发桔 Jog

AI Coding 产品不少，不过受 AI 发展的局限性影响，会存在”抽盲盒”等问题，即输出不稳定，前后不一致性较强。

这里提供一个 AI Coding 的使用技巧——规则设定，从而限制 AI 自由发挥。

例如对于移动端的开发，如下规则设定，可作为多次复用的限制条件。

````
# 一  你是一名 Objective-c 研发专家，代码需要满足面向对象设计的7大原则，写出的代码要满足可维护性，可拓展性，可复用性，灵活性好。7大设计原则参考建议：1.        SRP - 单一职责原则：每个类只负责一项功能（e.g. UI 与业务解耦，model/view/controller职责清晰）。2.        OCP - 开闭原则：对扩展开放，对修改关闭（通过继承、协议实现扩展而不是修改已有类）。3.        LSP - 里氏替换原则：子类可以替换父类，保证多态性成立。4.        ISP - 接口隔离原则：不要强迫实现不需要的接口（Objective-C 中表现为 protocol 精细拆分）。5.        DIP - 依赖倒置原则：高层模块不应该依赖底层模块（比如业务逻辑不依赖 UI 实现）。6.        LoD - 迪米特法则：一个对象应对其它对象有最少的了解（controller 不直接操作 view 的子控件）。7.        CRP - 合成复用原则：优先使用组合而非继承复用功能（如 cell 内使用多个子 view 而不是子类化整个 cell）。
# 二 代码规范### 2.1 通用注释的规范每一个类都要用 #pragma mark 来标记代码块，具体格式如下两行：#pragma mark - Class#pragma mark - 具体类名
对于.m中方法的pragma，分为如下几类：#pragma mark - Life Cycle#pragma mark - Public Methods  #pragma mark - Private Methods#pragma mark - Event Response#pragma mark - Delegate Methods#pragma mark - Getters &amp; Setters#pragma mark - Lazy Load
### 2.2 UIView的规范每一个继承UIView的类，建议方法顺序统一为以下结构，有助于提高代码可读性和协作效率：#pragma mark - Init- (instancetype)initWithFrame:...- (instancetype)initWithCoder:...#pragma mark - Public Method- (void)configWithDataModel:#pragma mark - Private Method- (void)setupUI- (void)setupConstraints#pragma mark - Event- (void)xxxButtonTapped
#pragma mark - Lazy Load
其中在- (instancetype)initWithFrame:(CGRect)frame {方法，其内部会调用setupUI方法；### 2.3 变量命名规范（统一风格）
建议采用以下命名约定：        •        UILabel：以 xxxLabel 结尾（如 titleLabel）        •        UIImageView：以 xxxImageView 或 xxxImg 命名        •        UIButton：以 xxxButton 命名        •        UIView：以 containerView、backgroundView 命名        •        避免使用无意义缩写，如 lbl、img，统一使用全称提升可读性
### 2.4 协议和代理规范```objc// 协议声明@protocol SomeViewDelegate &lt;NSObject&gt;@optional- (void)someView:(SomeView *)view didSelectItem:(id)item;@required- (NSInteger)numberOfItemsInSomeView:(SomeView *)view;@end

// 代理属性使用weak@property (nonatomic, weak) id&lt;SomeViewDelegate&gt; delegate;

```
### 2.5 抽象基类规范```objc// 在 initWithFrame: 中阻止抽象基类实例化- (instancetype)initWithFrame:(CGRect)frame {    self = [super initWithFrame:frame];    if (self) {        if ([self isMemberOfClass:[AbstractBaseClass class]]) {            [self doesNotRecognizeSelector:_cmd];            return nil;        }        [self setupUI];    }    return self;}```

### 2.6 分类（Category）命名规范```objc// 分类文件命名：UIView+KQYExtension.h// 分类方法前缀：kqy_@interface UIView (KQYExtension)- (void)kqy_addShadow;@end```
### 2.7 Lazy load的规范对于Lazy load的UI控件，需要遵循如下格式对于UIButton类型的Lazy Load，使用如下格式：```objc- (UIButton *)actionButton {    if (!_actionButton) {        _actionButton = [UIButton buttonWithType:UIButtonTypeCustom];        [_actionButton setTitle:@&quot;kqyBtn&quot; forState:UIControlStateNormal];        [_actionButton setTitleColor:[UIColor whiteColor] forState:UIControlStateNormal];        _actionButton.layer.cornerRadius = 4;        _actionButton.layer.masksToBounds = YES;        [_actionButton addTarget:self action:@selector(actionButtonDidClicked:) forControlEvents:UIControlEventTouchUpInside];    }    return _actionButton;}```对于UILabel类型的Lazy load，使用如下格式：```objc- (UILabel *)descLabel {    if (!_descLabel) {        _descLabel = [[UILabel alloc] init];        _descLabel.textAlignment = NSTextAlignmentLeft;        _descLabel.numberOfLines = 0;    }    return _descLabel;}
````

## AI Coding 带来哪些帮助？

![](img/1d26bc51be6f4f4d46c05c3bb91946ca.png)

阳光青桔 小图

▶ 代码质量提升 ：AI 会提醒注意易忽略的细节，例如内存泄漏的风险点、线程安全问题、性能瓶颈等。

▶ 学习效率提升 ：比如最近在学 `Compose`，AI 不仅能提供代码示例，还能解释设计思路和最佳实践，帮助快速上手。

▶ 调试效率提升 ：遇到复杂问题时，AI 能帮我快速缩小问题范围，提供几个可能的排查方向，避免很多无效尝试。

![](img/8c44e51c8e3c48ab8e2e13cbe7ba064b.png)

资深算法桔 橙橘

▶ 架构设计迭代加速 ：在设计一个微服务架构时，AI 协助完成服务边界划分、通信协议选择和容错策略设计。通过与 AI 的多轮讨论，最终确定基于事件驱动的架构方案，相比传统方式缩短了 40%的设计周期。

▶ 调试时间缩短 ：在排查一个间歇性出现的网络请求失败问题时，AI 通过分析日志和网络抓包数据，迅速定位到 `OkHttp` 连接池配置不合理的问题。整个排查过程从原来的平均 4 小时缩短至 40 分钟。

## 希望 AI Coding 功能如何再进化？

![](img/61818e4559866cdbd4d0712d959d40ce.png)

阳光青桔 小图

▶ 项目上下文的深度理解 ：现在的 AI 虽然能理解代码结构，但对业务逻辑和项目历史的理解还有限。希望未来 AI 能更好地理解项目的技术债务、性能瓶颈和发展方向。

▶ 更智能的代码审查能力 ：希望 AI 在代码提交前能发现潜在问题，不只是语法错误，还包括架构设计问题、性能风险、安全漏洞等。

![](img/ec62cd112d9f436fec2f06fdffa3c414.png)

资深算法桔 橙橘

▶ 全链路智能开发 ：期待未来 AI 能够理解完整的产品需求文档，并自动生成从数据库设计到前端 `UI` 的全链路代码，同时保证架构一致性和可扩展性。

▶ 持续学习与进化 ：期待 AI 能够学习项目特有的代码模式和业务逻辑，随着时间推移提供更加精准的建议，甚至预测潜在的技术债务。

![](img/f120f76cc52d3c30627bcccf85b5801b.png)

资深研发桔 毛一

▶ 低代码整合 ：期望 `AI Coding` 能将自然语言描述的需求，直接转化为功能完备、开箱即用的应用模块，如 `API`、`UI` 组件等。生成的代码应适配目标环境，处理好依赖关系，提供清晰的集成接口和必要配置，确保模块能够立即运行或轻松集成到现有项目中，提升开发效率和可访问性。

▶ 自适应个性化风格 ： 期望 `AI Coding` 具备持续学习能力，通过分析开发者历史代码、编辑行为及反馈，构建个人风格模型，并动态调整输出代码的命名、格式、结构、技术选型等，以高度匹配开发者习惯和偏好，提升代码的接受度、可维护性及使用体验。

`AI Coding` 是技术演进的飞跃，带来 多方位 变革。它借助大模型理解自然语言与代码逻辑，帮助开发者降低开发门槛，提升生产效率，推动编程向“智能协作”范式转变。

未来，`AI Coding` 将重塑工具链，将激发人机协同的创造力。适应技术进化，掌握 `AI` 工具，成为开发者新时代的重要竞争力。
