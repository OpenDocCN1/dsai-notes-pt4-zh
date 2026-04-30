# Hackers And Painters

> 来源：[https://docs.feishu.cn/docx/NYtudR8wboDSuIxVZIQcC5yLnOg](https://docs.feishu.cn/docx/NYtudR8wboDSuIxVZIQcC5yLnOg)

> http://paulgraham.com/hp.html

(This essay is derived from a guest lecture at Harv[ard, which inco](http://paulgraham.com/taste.html)r[porated ](http://www.apa.org/journals/features/psp7761121.pdf)an earlier talk at Northeastern.)

When I finished grad school in computer sci[ence I w](http://paulgraham.com/piraha.html)ent to a[rt schoo](http://paulgraham.com/power.html)l to study painting. A lot of people seemed surprised that someone interested in computers would also be interested in painting. They seemed to think that hacking and painting were very different kinds of work-- that hacking was cold, precise, and methodical, and that painting was the frenzied expression of some primal urge.

Both of these images are wrong. Hacking and painting have a lot in common. In fact, of all the different types of people I've known, hackers and painters are among the most alike.

What hackers and painters have in common is that they're both makers. Along with composers, architects, and writers, what hackers and painters are trying to do is make good things. They're not doing research per se, though if in the course of trying to make good things they discover some new technique, so much the better.

I've never liked the term "computer science." The main reason I don't like it is that there's no such thing. Computer science is a grab bag of tenuously related areas thrown together by an accident of history, like Yugoslavia. At one end you have people who are really mathematicians, but call what they're doing computer science so they can get DARPA grants. In the middle you have people working on something like the natural history of computers-- studying the behavior of algorithms for routing data through networks, for example. And then at the other extreme you have the hackers, who are trying to write interesting software, and for whom computers are just a medium of expression, as concrete is for architects or paint for painters. It's as if mathematicians, physicists, and architects all had to be in the same department.

Sometimes what the hackers do is called "software engineering," but this term is just as misleading. Good software designers are no more engineers than architects are. The border between architecture and engineering is not sharply defined, but it's there. It falls between what and how: architects decide what to do, and engineers figure out how to do it.

What and how should not be kept too separate. You're asking for trouble if you try to decide what to do without understanding how to do it. But hacking can certainly be more than just deciding how to implement some spec. At its best, it's creating the spec-- though it turns out the best way to do that is to implement it.

Perhaps one day "computer science" will, like Yugoslavia, get broken up into its component parts. That might be a good thing. Especially if it meant independence for my native land, hacking.

Bundling all these different types of work together in one department may be convenient administratively, but it's confusing intellectually. That's the other reason I don't like the name "computer science." Arguably the people in the middle are doing something like an experimental science. But the people at either end, the hackers and the mathematicians, are not actually doing science.

The mathematicians don't seem bothered by this. They happily set to work proving theorems like the other mathematicians over in the math department, and probably soon stop noticing that the building they work in says ``computer science'' on the outside. But for the hackers this label is a problem. If what they're doing is called science, it makes them feel they ought to be acting scientific. So instead of doing what they really want to do, which is to design beautiful software, hackers in universities and research labs feel they ought to be writing research papers.

In the best case, the papers are just a formality. Hackers write cool software, and then write a paper about it, and the paper becomes a proxy for the achievement represented by the software. But often this mismatch causes problems. It's easy to drift away from building beautiful things toward building ugly things that make more suitable subjects for research papers.

Unfortunately, beautiful things don't always make the best subjects for papers. Number one, research must be original-- and as anyone who has written a PhD dissertation knows, the way to be sure that you're exploring virgin territory is to to stake out a piece of ground that no one wants. Number two, research must be substantial-- and awkward systems yield meatier papers, because you can write about the obstacles you have to overcome in order to get things done. Nothing yields meaty problems like starting with the wrong assumptions. Most of AI is an example of this rule; if you assume that knowledge can be represented as a list of predicate logic expressions whose arguments represent abstract concepts, you'll have a lot of papers to write about how to make this work. As Ricky Ricardo used to say, "Lucy, you got a lot of explaining to do."

The way to create something beautiful is often to make subtle tweaks to something that already exists, or to combine existing ideas in a slightly new way. This kind of work is hard to convey in a research paper.

So why do universities and research labs continue to judge hackers by publications? For the same reason that "scholastic aptitude" gets measured by simple-minded standardized tests, or the productivity of programmers gets measured in lines of code. These tests are easy to apply, and there is nothing so tempting as an easy test that kind of works.

Measuring what hackers are actually trying to do, designing beautiful software, would be much more difficult. You need a good sense of design to judge good design. And there is no correlation, except possibly a negative one, between people's ability to recognize good design and their confidence that they can.

The only external test is time. Over time, beautiful things tend to thrive, and ugly things tend to get discarded. Unfortunately, the amounts of time involved can be longer than human lifetimes. Samuel Johnson said it took a hundred years for a writer's reputation to converge. You have to wait for the writer's influential friends to die, and then for all their followers to die.

I think hackers just have to resign themselves to having a large random component in their reputations. In this they are no different from other makers. In fact, they're lucky by comparison. The influence of fashion is not nearly so great in hacking as it is in painting.

There are worse things than having people misunderstand your work. A worse danger is that you will yourself misunderstand your work. Related fields are where you go looking for ideas. If you find yourself in the computer science department, there is a natural temptation to believe, for example, that hacking is the applied version of what theoretical computer science is the theory of. All the time I was in graduate school I had an uncomfortable feeling in the back of my mind that I ought to know more theory, and that it was very remiss of me to have forgotten all that stuff within three weeks of the final exam.

Now I realize I was mistaken. Hackers need to understand the theory of computation about as much as painters need to understand paint chemistry. You need to know how to calculate time and space complexity and about Turing completeness. You might also want to remember at least the concept of a state machine, in case you have to write a parser or a regular expression library. Painters in fact have to remember a good deal more about paint chemistry than that.

I've found that the best sources of ideas are not the other fields that have the word "computer" in their names, but the other fields inhabited by makers. Painting has been a much richer source of ideas than the theory of computation.

For example, I was taught in college that one ought to figure out a program completely on paper before even going near a computer. I found that I did not program this way. I found that I liked to program sitting in front of a computer, not a piece of paper. Worse still, instead of patiently writing out a complete program and assuring myself it was correct, I tended to just spew out code that was hopelessly broken, and gradually beat it into shape. Debugging, I was taught, was a kind of final pass where you caught typos and oversights. The way I worked, it seemed like programming consisted of debugging.

For a long time I felt bad about this, just as I once felt bad that I didn't hold my pencil the way they taught me to in elementary school. If I had only looked over at the other makers, the painters or the architects, I would have realized that there was a name for what I was doing: sketching. As far as I can tell, the way they taught me to program in college was all wrong. You should figure out programs as you're writing them, just as writers and painters and architects do.

Realizing this has real implications for software design. It means that a programming language should, above all, be malleable. A programming language is for thinking of programs, not for expressing programs you've already thought of. It should be a pencil, not a pen. Static typing would be a fine idea if people actually did write programs the way they taught me to in college. But that's not how any of the hackers I know write programs. We need a language that lets us scribble and smudge and smear, not a language where you have to sit with a teacup of types balanced on your knee and make polite conversation with a strict old aunt of a compiler.

While we're on the subject of static typing, identifying with the makers will save us from another problem that afflicts the sciences: math envy. Everyone in the sciences secretly believes that mathematicians are smarter than they are. I think mathematicians also believe this. At any rate, the result is that scientists tend to make their work look as mathematical as possible. In a field like physics this probably doesn't do much harm, but the further you get from the natural sciences, the more of a problem it becomes.

A page of formulas just looks so impressive. (Tip: for extra impressiveness, use Greek variables.) And so there is a great temptation to work on problems you can treat formally, rather than problems that are, say, important.

If hackers identified with other makers, like writers and painters, they wouldn't feel tempted to do this. Writers and painters don't suffer from math envy. They feel as if they're doing something completely unrelated. So are hackers, I think.

If universities and research labs keep hackers from doing the kind of work they want to do, perhaps the place for them is in companies. Unfortunately, most companies won't let hackers do what they want either. Universities and research labs force hackers to be scientists, and companies force them to be engineers.

I only discovered this myself quite recently. When Yahoo bought Viaweb, they asked me what I wanted to do. I had never liked the business side very much, and said that I just wanted to hack. When I got to Yahoo, I found that what hacking meant to them was implementing software, not designing it. Programmers were seen as technicians who translated the visions (if that is the word) of product managers into code.

This seems to be the default plan in big companies. They do it because it decreases the standard deviation of the outcome. Only a small percentage of hackers can actually design software, and it's hard for the people running a company to pick these out. So instead of entrusting the future of the software to one brilliant hacker, most companies set things up so that it is designed by committee, and the hackers merely implement the design.

If you want to make money at some point, remember this, because this is one of the reasons startups win. Big companies want to decrease the standard deviation of design outcomes because they want to avoid disasters. But when you damp oscillations, you lose the high points as well as the low. This is not a problem for big companies, because they don't win by making great products. Big companies win by sucking less than other big companies.

So if you can figure out a way to get in a design war with a company big enough that its software is designed by product managers, they'll never be able to keep up with you. These opportunities are not easy to find, though. It's hard to engage a big company in a design war, just as it's hard to engage an opponent inside a castle in hand to hand combat. It would be pretty easy to write a better word processor than Microsoft Word, for example, but Microsoft, within the castle of their operating system monopoly, probably wouldn't even notice if you did.

The place to fight design wars is in new markets, where no one has yet managed to establish any fortifications. That's where you can win big by taking the bold approach to design, and having the same people both design and implement the product. Microsoft themselves did this at the start. So did Apple. And Hewlett-Packard. I suspect almost every successful startup has.

So one way to build great software is to start your own startup. There are two problems with this, though. One is that in a startup you have to do so much besides write software. At Viaweb I considered myself lucky if I got to hack a quarter of the time. And the things I had to do the other three quarters of the time ranged from tedious to terrifying. I have a benchmark for this, because I once had to leave a board meeting to have some cavities filled. I remember sitting back in the dentist's chair, waiting for the drill, and feeling like I was on vacation.

The other problem with startups is that there is not much overlap between the kind of software that makes money and the kind that's interesting to write. Programming languages are interesting to write, and Microsoft's first product was one, in fact, but no one will pay for programming languages now. If you want to make money, you tend to be forced to work on problems that are too nasty for anyone to solve for free.

All makers face this problem. Prices are determined by supply and demand, and there is just not as much demand for things that are fun to work on as there is for things that solve the mundane problems of individual customers. Acting in off-Broadway plays just doesn't pay as well as wearing a gorilla suit in someone's booth at a trade show. Writing novels doesn't pay as well as writing ad copy for garbage disposals. And hacking programming languages doesn't pay as well as figuring out how to connect some company's legacy database to their Web server.

I think the answer to this problem, in the case of software, is a concept known to nearly all makers: the day job. This phrase began with musicians, who perform at night. More generally, it means that you have one kind of work you do for money, and another for love.

Nearly all makers have day jobs early in their careers. Painters and writers notoriously do. If you're lucky you can get a day job that's closely related to your real work. Musicians often seem to work in record stores. A hacker working on some programming language or operating system might likewise be able to get a day job using it. [1]

When I say that the answer is for hackers to have day jobs, and work on beautiful software on the side, I'm not proposing this as a new idea. This is what open-source hacking is all about. What I'm saying is that open-source is probably the right model, because it has been independently confirmed by all the other makers.

It seems surprising to me that any employer would be reluctant to let hackers work on open-source projects. At Viaweb, we would have been reluctant to hire anyone who didn't. When we interviewed programmers, the main thing we cared about was what kind of software they wrote in their spare time. You can't do anything really well unless you love it, and if you love to hack you'll inevitably be working on projects of your own. [2]

Because hackers are makers rather than scientists, the right place to look for metaphors is not in the sciences, but among other kinds of makers. What else can painting teach us about hacking?

One thing we can learn, or at least confirm, from the example of painting is how to learn to hack. You learn to paint mostly by doing it. Ditto for hacking. Most hackers don't learn to hack by taking college courses in programming. They learn to hack by writing programs of their own at age thirteen. Even in college classes, you learn to hack mostly by hacking. [3]

Because painters leave a trail of work behind them, you can watch them learn by doing. If you look at the work of a painter in chronological order, you'll find that each painting builds on things that have been learned in previous ones. When there's something in a painting that works very well, you can usually find version 1 of it in a smaller form in some earlier painting.

I think most makers work this way. Writers and architects seem to as well. Maybe it would be good for hackers to act more like painters, and regularly start over from scratch, instead of continuing to work for years on one project, and trying to incorporate all their later ideas as revisions.

The fact that hackers learn to hack by doing it is another sign of how different hacking is from the sciences. Scientists don't learn science by doing it, but by doing labs and problem sets. Scientists start out doing work that's perfect, in the sense that they're just trying to reproduce work someone else has already done for them. Eventually, they get to the point where they can do original work. Whereas hackers, from the start, are doing original work; it's just very bad. So hackers start original, and get good, and scientists start good, and get original.

The other way makers learn is from examples. For a painter, a museum is a reference library of techniques. For hundreds of years it has been part of the traditional education of painters to copy the works of the great masters, because copying forces you to look closely at the way a painting is made.

Writers do this too. Benjamin Franklin learned to write by summarizing the points in the essays of Addison and Steele and then trying to reproduce them. Raymond Chandler did the same thing with detective stories.

Hackers, likewise, can learn to program by looking at good programs-- not just at what they do, but the source code too. One of the less publicized benefits of the open-source movement is that it has made it easier to learn to program. When I learned to program, we had to rely mostly on examples in books. The one big chunk of code available then was Unix, but even this was not open source. Most of the people who read the source read it in illicit photocopies of John Lions' book, which though written in 1977 was not allowed to be published until 1996.

Another example we can take from painting is the way that paintings are created by gradual refinement. Paintings usually begin with a sketch. Gradually the details get filled in. But it is not merely a process of filling in. Sometimes the original plans turn out to be mistaken. Countless paintings, when you look at them in xrays, turn out to have limbs that have been moved or facial features that have been readjusted.

Here's a case where we can learn from painting. I think hacking should work this way too. It's unrealistic to expect that the specifications for a program will be perfect. You're better off if you admit this up front, and write programs in a way that allows specifications to change on the fly.

(The structure of large companies makes this hard for them to do, so here is another place where startups have an advantage.)

Everyone by now presumably knows about the danger of premature optimization. I think we should be just as worried about premature design-- deciding too early what a program should do.

The right tools can help us avoid this danger. A good programming language should, like oil paint, make it easy to change your mind. Dynamic typing is a win here because you don't have to commit to specific data representations up front. But the key to flexibility, I think, is to make the language very abstract. The easiest program to change is one that's very short.

This sounds like a paradox, but a great painting has to be better than it has to be. For example, when Leonardo painted the portrait of Ginevra de Benci in the National Gallery, he put a juniper bush behind her head. In it he carefully painted each individual leaf. Many painters might have thought, this is just something to put in the background to frame her head. No one will look that closely at it.

Not Leonardo. How hard he worked on part of a painting didn't depend at all on how closely he expected anyone to look at it. He was like Michael Jordan. Relentless.

Relentlessness wins because, in the aggregate, unseen details become visible. When people walk by the portrait of Ginevra de Benci, their attention is often immediately arrested by it, even before they look at the label and notice that it says Leonardo da Vinci. All those unseen details combine to produce something that's just stunning, like a thousand barely audible voices all singing in tune.

Great software, likewise, requires a fanatical devotion to beauty. If you look inside good software, you find that parts no one is ever supposed to see are beautiful too. I'm not claiming I write great software, but I know that when it comes to code I behave in a way that would make me eligible for prescription drugs if I approached everyday life the same way. It drives me crazy to see code that's badly indented, or that uses ugly variable names.

If a hacker were a mere implementor, turning a spec into code, then he could just work his way through it from one end to the other like someone digging a ditch. But if the hacker is a creator, we have to take inspiration into account.

In hacking, like painting, work comes in cycles. Sometimes you get excited about some new project and you want to work sixteen hours a day on it. Other times nothing seems interesting.

To do good work you have to take these cycles into account, because they're affected by how you react to them. When you're driving a car with a manual transmission on a hill, you have to back off the clutch sometimes to avoid stalling. Backing off can likewise prevent ambition from stalling. In both painting and hacking there are some tasks that are terrifyingly ambitious, and others that are comfortingly routine. It's a good idea to save some easy tasks for moments when you would otherwise stall.

In hacking, this can literally mean saving up bugs. I like debugging: it's the one time that hacking is as straightforward as people think it is. You have a totally constrained problem, and all you have to do is solve it. Your program is supposed to do x. Instead it does y. Where does it go wrong? You know you're going to win in the end. It's as relaxing as painting a wall.

The example of painting can teach us not only how to manage our own work, but how to work together. A lot of the great art of the past is the work of multiple hands, though there may only be one name on the wall next to it in the museum. Leonardo was an apprentice in the workshop of Verrocchio and painted one of the angels in his Baptism of Christ. This sort of thing was the rule, not the exception. Michelangelo was considered especially dedicated for insisting on painting all the figures on the ceiling of the Sistine Chapel himself.

As far as I know, when painters worked together on a painting, they never worked on the same parts. It was common for the master to paint the principal figures and for assistants to paint the others and the background. But you never had one guy painting over the work of another.

I think this is the right model for collaboration in software too. Don't push it too far. When a piece of code is being hacked by three or four different people, no one of whom really owns it, it will end up being like a common-room. It will tend to feel bleak and abandoned, and accumulate cruft. The right way to collaborate, I think, is to divide projects into sharply defined modules, each with a definite owner, and with interfaces between them that are as carefully designed and, if possible, as articulated as programming languages.

Like painting, most software is intended for a human audience. And so hackers, like painters, must have empathy to do really great work. You have to be able to see things from the user's point of view.

When I was a kid I was always being told to look at things from someone else's point of view. What this always meant in practice was to do what someone else wanted, instead of what I wanted. This of course gave empathy a bad name, and I made a point of not cultivating it.

Boy, was I wrong. It turns out that looking at things from other people's point of view is practically the secret of success. It doesn't necessarily mean being self-sacrificing. Far from it. Understanding how someone else sees things doesn't imply that you'll act in his interest; in some situations-- in war, for example-- you want to do exactly the opposite. [4]

Most makers make things for a human audience. And to engage an audience you have to understand what they need. Nearly all the greatest paintings are paintings of people, for example, because people are what people are interested in.

Empathy is probably the single most important difference between a good hacker and a great one. Some hackers are quite smart, but when it comes to empathy are practically solipsists. It's hard for such people to design great software [5], because they can't see things from the user's point of view.

One way to tell how good people are at empathy is to watch them explain a technical question to someone without a technical background. We probably all know people who, though otherwise smart, are just comically bad at this. If someone asks them at a dinner party what a programming language is, they'll say something like ``Oh, a high-level language is what the compiler uses as input to generate object code.'' High-level language? Compiler? Object code? Someone who doesn't know what a programming language is obviously doesn't know what these things are, either.

Part of what software has to do is explain itself. So to write good software you have to understand how little users understand. They're going to walk up to the software with no preparation, and it had better do what they guess it will, because they're not going to read the manual. The best system I've ever seen in this respect was the original Macintosh, in 1985. It did what software almost never does: it just worked. [6]

Source code, too, should explain itself. If I could get people to remember just one quote about programming, it would be the one at the beginning of Structure and Interpretation of Computer Programs.

> Programs should be written for people to read, and only incidentally for machines to execute.

You need to [have emp](http://paulgraham.com/montefeltro.html)**athy ****not ju**st for your users, but for your readers. It's in your interest, because you'll be one of them. Many a hacker has written a program only to find on returning to it six months later that he has no idea how it works. I know several people who've sworn off Perl after such experiences. [7]

Lack of empathy is associated with intelligence, to the point that there is even something of a fashion for it in some places. But I don't think there's any correlation. You can do well in math and the natural sciences without having to learn empathy, and people in these fields tend to be smart, so the two qualities have come to be associated. But there are plenty of dumb people who are bad at empathy too. Just listen to the people who call in with questions on talk shows. They ask whatever it is they're asking in such a roundabout way that the hosts often have to rephrase the question for them.

So, if hacking works like painting and writing, is it as cool? After all, you only get one life. You might as well spend it working on something great.

Unfortunately, the question is hard to answer. There is always a big time lag in prestige. It's like light from a distant star. Painting has prestige now because of great work people did five hundred years ago. At the time, no one thought these paintings were as important as we do today. It would have seemed very odd to people at the time that Federico da Montefeltro, the Duke of Urbino, would one day be known mostly as the guy with the strange nose in a painting by Piero della Francesca.

So while I admit that hacking doesn't seem as cool as painting now, we should remember that painting itself didn't seem as cool in its glory days as it does now.

What we can say with some confidence is that these are the glory days of hacking. In most fields the great work is done early on. The paintings made between 1430 and 1500 are still unsurpassed. Shakespeare appeared just as professional theater was being born, and pushed the medium so far that every playwright since has had to live in his shadow. Albrecht Durer did the same thing with engraving, and Jane Austen with the novel.

Over and over we see the same pattern. A new medium appears, and people are so excited about it that they explore most of its possibilities in the first couple generations. Hacking seems to be in this phase now.

Painting was not, in Leonardo's time, as cool as his work helped make it. How cool hacking turns out to be will depend on what we can do with this new medium.

Notes

[1] The greatest damage that photography has done to painting may be the fact that it killed the best day job. Most of the great painters in history supported themselves by painting portraits.

[2] I've been told that Microsoft discourages employees from contributing to open-source projects, even in their spare time. But so many of the best hackers work on open-source projects now that the main effect of this policy may be to ensure that they won't be able to hire any first-rate programmers.

[3] What you learn about programming in college is much like what you learn about books or clothes or dating: what bad taste you had in high school.

[4] Here's an example of applied empathy. At Viaweb, if we couldn't decide between two alternatives, we'd ask, what would our competitors hate most? At one point a competitor added a feature to their software that was basically useless, but since it was one of few they had that we didn't, they made much of it in the trade press. We could have tried to explain that the feature was useless, but we decided it would annoy our competitor more if we just implemented it ourselves, so we hacked together our own version that afternoon.

[5] Except text editors and compilers. Hackers don't need empathy to design these, because they are themselves typical users.

[6] Well, almost. They overshot the available RAM somewhat, causing much inconvenient disk swapping, but this could be fixed within a few months by buying an additional disk drive.

[7] The way to make programs easy to read is not to stuff them with comments. I would take Abelson and Sussman's quote a step further. Programming languages should be designed to express algorithms, and only incidentally to tell computers how to execute them. A good programming language ought to be better for explaining software than English. You should only need comments when there is some kind of kludge you need to warn readers about, just as on a road there are only arrows on parts with unexpectedly sharp curves.

Thanks to Trevor Blackwell, Robert Morris, Dan Giffin, and Lisa Randall for reading drafts of this, and to Henry Leitner and Larry Finkelstein for inviting me to speak.

当我完成计算机科学研究生学业时，我去了艺术学校学习绘画。很多人似乎很惊讶，对计算机感兴趣的人也会对绘画感兴趣。他们似乎认为黑客和绘画是截然不同的工作——黑客是冷酷的、精确的、有条不紊的，而绘画是某种原始冲动的疯狂表达。

这两个图像都是错误的。黑客和绘画有很多共同点。事实上，在我认识的所有不同类型的人中，黑客和画家是最相似的。

**黑客和绘画有很多共同点。**事实上，在我认识的所有不同类型的人中，黑客和画家是最相似的。

黑客和画家的共同点是他们都是**制造者。与作曲家、建筑师和作家一起，黑客和画家试图做的是创造好东西。他们本身并**不是在做研究，但如果在试图创造好东西的过程中，他们发现了一些新技术，那就更好了。

我从来不喜欢“计算机科学”这个词。我不喜欢它的主要原因是没有这样的东西。计算机科学是历史偶然事件（如南斯拉夫）拼凑起来的脆弱相关领域的抓手。一方面，你有一些人是真正的数学家，但称他们正在做的事情是计算机科学，这样他们就可以获得DARPA的资助。在中间，你让人们从事类似计算机自然史的工作，例如研究通过网络路由数据的算法的行为。在另一个极端，你有黑客，他们试图编写有趣的软件，对他们来说，计算机只是一种表达媒介，就像混凝土之于建筑师或油漆之于画家一样。就好像数学家、物理学家和建筑师都必须在同一个部门。

有时黑客所做的事情被称为“软件工程”，但这个术语同样具有误导性。优秀的软件设计师并不比架构师更像是工程师。建筑和工程之间的界限没有明确定义，但它就在那里。它介于什么和如何做之间：建筑师决定做什么，工程师弄清楚如何做。

什么和如何不应该分开得太分开。如果你试图在不了解如何去做的情况下决定做什么，你就是在自找麻烦。但是，黑客攻击肯定不仅仅是决定如何实现某些规范。在最好的情况下，它正在创建规范——尽管事实证明，最好的方法是实现它。

也许有一天，“计算机科学”会像南斯拉夫一样，被分解成它的组成部分。这可能是一件好事。特别是如果这意味着我的祖国独立，黑客。

**将所有这些不同类型的工作捆绑在一个部门中可**能在行政上很方便，但在智力上却令人困惑。这是我不喜欢“计算机科学”这个名字的另一个原因。可以说，中间的人正在做一些类似实验科学的事情。但两端的人，黑客和数学家，实际上并没有在做科学。

数学家们似乎对此并不在意。他们兴高采烈地开始像数学系的其他数学家一样证明定理，可能很快就会注意到他们工作的大楼外面写着“计算机科学”。但对于黑客来说，这个标签是一个问题。如果他们所做的事情被称为科学，这会让他们觉得他们应该表现得很科学。因此，大学和研究实验室的黑客没有做他们真正想做的事情，即设计漂亮的软件，而是觉得他们应该写研究论文。

在最好的情况下，**这些文件只是一种形式。**黑客写了很酷的软件，然后写了一篇关于它的论文，这篇论文就成了软件所代表的成就的代表。但这种不匹配往往会导致问题。人们很容易从建造美丽的东西转向建造丑陋的东西，使研究论文的主题更合适。

不幸的是，美丽的事物并不总是成为论文的最佳主题。第一，研究必须是原创的——任何写过博士论文的人都知道，确保你正在探索处女地的方法是在一块没人想要的土地上放样。第二，研究必须是实质性的——笨拙的系统会产生更丰满的论文，因为你可以写下你必须克服的障碍才能完成工作。没有什么比从错误的假设开始更能产生肉质问题了。大多数人工智能都是这条规则的一个例子;如果你假设知识可以表示为谓词逻辑表达式的列表，其参数表示抽象概念，那么你将有很多关于如何使它起作用的论文要写。正如瑞奇·里卡多（Ricky Ricardo）曾经说过的那样，“露西，你有很多解释工作要做。

**创**造美丽事物的方法通常是对已经存在的东西进行微妙的调整，或者以一种稍微新颖的方式组合现有的想法。这种工作很难在研究论文中传达。

那么，为什么大学和研究实验室继续通过出版物来判断黑客呢？出于同样的原因，“学术能力”是通过头脑简单的标准化测试来衡量的，或者程序员的生产力是用代码行来衡量的。这些测试很容易应用，没有什么比简单的测试更诱人的了。

衡量黑客实际想要做什么，设计漂亮的软件，会困难得多。你需要有良好的设计感来判断好的设计。人们识别优秀设计的能力和他们对自己能够识别的能力之间没有相关性，除了可能是负的相关性。

唯一的外部考验是时间。随着时间的流逝，美丽的事物往往会茁壮成长，而丑陋的事物往往会被丢弃。不幸的是，所涉及的时间可能比人类的寿命还要长。塞缪尔·约翰逊（Samuel Johnson）说，作家的声誉需要一百年才能收敛。你必须等待作家有影响力的朋友死去，然后等待他们所有的追随者死去。

我认为黑客只需要让自己的声誉中有一个很大的随机成分。在这一点上，它们与其他制造商没有什么不同。事实上，相比之下，他们是幸运的。时尚对黑客的影响远不如绘画那么大。

还**有比让人们误解你的工作更糟糕的事情。更糟糕的危险是，你自己会误解你的工作。相关领域是你寻找想法的地方。如果你发现**自己在计算机科学系，就会有一种自然的诱惑，相信，例如，黑客是理论计算机科学的应用版本。在我读研究生的时候，我脑海里有一种不舒服的感觉，我应该知道更多的理论，而在期末考试后的三周内忘记了所有这些东西，这对我来说是非常失职的。

**现在我意识到我错了。黑客需要了解**计算理论，就像画家需要了解油漆化学一样。你需要知道如何计算时间和空间复杂度以及图灵完备性。您可能还需要至少记住状态机的概念，以防您必须编写解析器或正则表达式库。事实上，画家必须记住更多关于油漆化学的知识。

我发现，最好的创意来源不是名称中带有“计算机”一词的其他领域，而是创客居住的其他领域。绘画是比计算理论更丰富的思想来源。

例如，**我在大学里被教导说，一个人应该在靠近计算机之前完全在纸上**弄清楚一个程序。我发现我不是这样编程的。我发现我喜欢坐在电脑前编程，而不是一张纸。更糟糕的是，我没有耐心地写出一个完整的程序，并保证自己是正确的，我倾向于只是吐出那些无可救药的代码，然后逐渐把它打成形状。我被教导说，调试是一种最后的通过，你可以发现错别字和疏忽。我的工作方式，似乎编程包括调试。

在很长一段时间里，我为此感到难过，就像我曾经为我没有像他们在小学时教我的那样握住铅笔而感到难过一样。如果我只看看其他的制造商，画家或建筑师，我就会意识到我正在做的事情有一个名字：素描。据我所知，他们在大学里教我编程的方式都是错误的。你应该在编写程序时弄清楚它们，就像作家、画家和建筑师一样。

意识到这一点对软件设计具有真正的意义。这意味着编程语言首先应该是可塑的。编程语言是用来思考程序的，而不是用来表达你已经想到的程序的。它应该是铅笔，而不是钢笔。如果人们真的按照他们在大学里教我的方式编写程序，那么静态类型将是一个好主意。但是，我认识的任何黑客都不是这样编写程序的。我们需要一种让我们涂鸦、涂抹和涂抹的语言，而不是一种你必须坐在膝盖上平衡的茶杯，与一个严格的编译器老阿姨进行礼貌交谈的语言。

当我们讨论静态类型的主题时，认同制造商将使我们免于另一个困扰科学的问题：数学嫉妒。科学界的每个人都暗地里相信数学家比他们更聪明。我想数学家也相信这一点。无论如何，结果是科学家倾向于让他们的工作看起来尽可能数学化。在像物理学这样的领域，这可能不会造成太大的伤害，但你离自然科学越远，它就会成为一个更大的问题。

一页公式看起来令人印象深刻。（提示：为了给人留下更多印象，请使用希腊变量。因此，有一种很大的诱惑，即致力于解决你可以正式处理的问题，而不是那些重要的问题。

如果大学和研究实验室阻止黑客做他们想做的那种工作，也许他们的地方就在公司里。不幸的是，大多数公司也不会让黑客为所欲为。大学和研究实验室强迫黑客成为科学家，公司强迫他们成为工程师。

我自己最近才发现这一点。当雅虎收购Viaweb时，他们问我想做什么。我从来都不太喜欢商业方面，并说我只是想破解。当我来到雅虎时，我发现黑客对他们来说意味着实现软件，而不是设计它。程序员被视为将产品经理的愿景（如果真是这样的话）转化为代码的技术人员。

这似乎是大公司的默认计划。他们这样做是因为它降低了结果的标准差。只有一小部分黑客能够真正设计软件，而经营公司的人很难挑选出这些软件。因此，大多数公司没有将软件的未来委托给一位杰出的黑客，而是将其设置为由委员会设计，而黑客只是实现设计。

如果你想在某个时候赚钱，请记住这一点，因为这是初创公司获胜的原因之一。大公司希望降低设计结果的标准差，因为他们希望避免灾难。但是，当你抑制振荡时，你就会失去高点和低点。这对大公司来说不是问题，因为他们不会通过制造出色的产品来取胜。大公司通过比其他大公司吸得更少而获胜。

因此，如果你能找到一种方法，与一家足够大的公司进行设计战，以至于其软件是由产品经理设计的，他们将永远无法跟上你的步伐。然而，这些机会并不容易找到。在设计战中与一家大公司交战是很困难的，就像在城堡内与对手进行肉搏战一样。例如，写一个比Microsoft Word更好的文字处理器是很容易的，但微软，在他们的操作系统垄断的城堡里，可能甚至不会注意到你是否注意到了。

**打**设计战的地方是在新市场，那里还没有人设法建立任何防御工事。这就是您可以通过采用大胆的设计方法并让相同的人设计和实施产品来赢得大奖的地方。Microsoft自己一开始就这样做了。苹果也是如此。还有惠普。我怀疑几乎每个成功的创业公司都有。

因此，构建出色软件的一种方法是开始自己的创业公司。不过，这有两个问题。一个是，在创业公司中，除了编写软件之外，你还必须做很多事情。在Viaweb，我认为自己很幸运，如果我能有四分之一的时间破解。而我必须在另外四分之三的时间里做的事情从乏味到可怕不等。我对此有一个基准，因为我曾经不得不离开董事会会议来填补一些空洞。我记得我坐在牙医的椅子上，等待钻头，感觉自己在度假。

**创业公司的另一个问题是，赚钱的软件和编写有趣的软**件之间没有太多重叠。编程语言写起来很有趣，事实上，Microsoft的第一个产品就是其中之一，但现在没有人会为编程语言付费。如果你想赚钱，你往往会被迫处理那些对任何人都无法免费解决的太讨厌的问题。

所有制造商都面临这个问题。价格是由供求关系决定的，对有趣工作的东西的需求不如对解决个人客户平凡问题的东西的需求多。在非百老汇戏剧中表演的报酬不如在贸易展上穿着大猩猩套装在某人的展位上。写小说的报酬不如写垃圾处理的广告文案。黑客编程语言的回报不如弄清楚如何将某些公司的遗留数据库连接到他们的 Web 服务器。

我认为，就软件而言，这个问题的答案是几乎所有创客都知道的一个概念：日常工作（the day job）。这句话始于在夜间表演的音乐家。更一般地说，这意味着你有一种工作是为了钱，另一种是为了爱。

几乎所有的创客在职业生涯的早期都有日常工作。众所周知，画家和作家会这样做。如果幸运的话，你可以找到一份与你的实际工作密切相关的日常工作。音乐家似乎经常在唱片店工作。从事某种编程语言或操作系统的黑客同样可以使用它获得日常工作。[1]

当我说答案是让黑客有日常工作，并同时开发漂亮的软件时，我并不是把这当作一个新想法。这就是开源黑客的全部内容。我想说的是，开源可能是正确的模式，因为它已经得到了所有其他制造商的独立确认。

令我感到惊讶的是，任何雇主都不愿意让黑客从事开源项目。在Viaweb，我们不愿意雇用任何不这样做的人。当我们采访程序员时，我们最关心的是他们在业余时间写了什么样的软件。除非你喜欢它，否则你无法很好地做任何事情，如果你喜欢黑客，你将不可避免地从事自己的项目。[2]

因为黑客是制造者而不是科学家，所以寻找隐喻的正确位置不是在科学领域，而是在其他类型的制造者中。关于黑客，绘画还能教给我们什么？因为黑客是制造者而不是科学家，所以寻找隐喻的正确位置不是在科学领域，而是在其他类型的制造者中。关于黑客，绘画还能教给我们什么？

我**们可以从绘画的例子中学到，或者至少证实一件事，那就是如何学习黑客**。你主要通过做画来学习画画。黑客攻击也是如此。大多数黑客不会通过参加大学编程课程来学习黑客攻击。他们在 13 岁时通过编写自己的程序来学习黑客。即使在大学课堂上，你也主要通过黑客来学习黑客。[3]

因为画家在他们身后留下了工作痕迹，所以您可以看着他们边做边学。如果你按时间顺序看一个画家的作品，你会发现每幅画都是建立在以前的作品之上的。当一幅画中有一些东西效果很好时，你通常可以在一些早期的画作中以较小的形式找到它的第一个版本。

我认为大多数创客都是这样工作的。作家和建筑师似乎也是如此。也许黑客表现得更像画家，并定期从头开始，而不是继续在一个项目上工作多年，并试图将他们后来的所有想法作为修订。

黑客通过这样做来学习黑客这一事实是黑客与科学有多么不同的另一个迹象。科学家不是通过做科学来学习科学，而是通过做实验和问题集来学习科学。科学家开始做完美的工作，从某种意义上说，他们只是试图复制别人已经为他们完成的工作。最终，他们达到了可以做原创工作的地步。而黑客从一开始就在做原创工作;这很糟糕。因此，黑客从原创开始，然后变得优秀，而科学家从优秀开始，并变得原创。

创客学习的另一种方式是从例子中学习。对于画家来说，博物馆是技术的参考图书馆。数百年来，临摹大师的作品一直是画家传统教育的一部分，因为临摹迫使你仔细观察一幅画的制作方式。

作家也这样做。本杰明·富兰克林（Benjamin Franklin）通过总结艾迪生（Addison）和斯蒂尔（Steele）文章中的观点，然后尝试复制它们来学习写作。雷蒙德·钱德勒（Raymond Chandler）对侦探故事做了同样的事情。

同样，黑客也可以通过查看好的程序来学习编程——不仅仅是他们做了什么，还有源代码。开源运动的一个鲜为人知的好处是，它使学习编程变得更加容易。当我学习编程时，我们不得不主要依靠书本上的例子。当时可用的一大块代码是 Unix，但即使这样也不是开源的。大多数阅读该资料的人都是在约翰·莱恩斯（John Lions）的书的非法影印本中阅读的，该书虽然写于1977年，但直到1996年才被允许出版。

我们可以从绘画中借鉴的另一个例子是绘画是通过逐渐完善来创作的。绘画通常以素描开始。渐渐地，细节被填满了。但这不仅仅是一个填充的过程。有时最初的计划被证明是错误的。无数的画作，当你在X光片中观察它们时，发现它们的四肢已经移动，或者面部特征被重新调整。

这是一个我们可以从绘画中学习的案例。我认为黑客攻击也应该以这种方式工作。期望程序的规范是完美的是不现实的。如果你提前承认这一点，并以一种允许规范即时更改的方式编写程序，你会更好。

（大公司的结构使他们很难做到这一点，所以这是初创公司具有优势的另一个地方。

现在每个人大概都知道过早优化的危险。我认为我们也应该担心过早的设计——过早地决定一个程序应该做什么。

正确的工具可以帮助我们避免这种危险。一门好的编程语言应该像油画颜料一样，让你很容易改变主意。动态类型在这里是一个胜利，因为您不必预先提交特定的数据表示形式。但我认为，灵活性的关键是使语言非常抽象。最容易更改的程序是一个非常短的程序。

这听起来像是一个悖论，但一幅伟大的画作必须比它必须的更好。例如，当莱昂纳多在国家美术馆画吉内夫拉·德·本奇的肖像时，他把一棵杜松灌木放在她的头后。在里面，他仔细地画了每一片叶子。许多画家可能会想，这只是放在背景中框住她的头的东西。没有人会仔细观察它。

不是莱昂纳多。他在一幅画上有多努力，根本不取决于他希望任何人仔细观察它。他就像迈克尔·乔丹。毫不留情。

坚持不懈之所以获胜，是因为总的来说，看不见的细节变得可见。当人们走过吉内夫拉·德·本奇的肖像时，他们的注意力往往会立即被它吸引，甚至在他们看到标签并注意到上面写着莱昂纳多·达·芬奇之前。所有这些看不见的细节结合在一起，产生了令人惊叹的东西，就像一千个几乎听不见的声音都在唱歌。

同样，伟大的软件需要对美的狂热奉献。如果你看看好的软件内部，你会发现没有人应该看到的部分也很漂亮。我并不是说我写了很棒的软件，但我知道，当涉及到代码时，如果我以同样的方式对待日常生活，我的行为方式将使我有资格获得处方药。看到缩进严重或使用丑陋的变量名称的代码，这让我发疯。

如**果黑客只是一个实**现者，将规范转化为代码，那么他就可以像挖沟一样从一端到另一端。但是，如果黑客是创造者，我们必须考虑灵感。

在黑客中，就像绘画一样，工作是有周期的。有时你对一些新项目感到兴奋，你想每天工作十六个小时。其他时候，似乎没有什么有趣的事情。

要做好工作，你必须考虑这些周期，因为它们会受到你对它们的反应的影响。当您在山坡上驾驶配备手动变速箱的汽车时，有时您必须松开离合器以避免熄火。退缩同样可以防止雄心壮志停滞不前。在绘画和黑客攻击中，有些任务是可怕的雄心勃勃，而另一些则是令人欣慰的例行公事。最好将一些简单的任务留到其他时候，否则会停滞不前。

在黑客攻击中，这实际上意味着保存错误。我喜欢调试：这是黑客攻击像人们想象的那样简单的时候。你有一个完全受限的问题，你所要做的就是解决它。你的程序应该做 x。相反，它做 y。哪里出了问题？你知道你最终会赢。这就像粉刷墙壁一样放松。

绘画的例子不仅可以教会我们如何管理自己的工作，还可以教会我们如何合作。过去许多伟大的艺术作品都是多手的作品，尽管博物馆旁边的墙上可能只有一个名字。莱昂纳多是韦罗基奥工作室的学徒，并在他的《基督洗礼》中画了一位天使。这种事情是规则，而不是例外。米开朗基罗被认为特别致力于坚持在西斯廷教堂的天花板上画所有人物。

据我所知，当画家们一起创作一幅画时，他们从未在相同的部分上工作过。师傅画主要人物，助手画其他人物和背景是很常见的。但是你从来没有让一个人在另一个人的作品上画画。

我认为这也是软件协作的正确模式。不要把它推得太远。当一段代码被三四个不同的人入侵时，没有人真正拥有它，它最终会像一个公共休息室。它往往会感到凄凉和被遗弃，并积累残渣。我认为，正确的协作方式是将项目划分为明确定义的模块，每个模块都有一个明确的所有者，并且它们之间的接口经过精心设计，如果可能的话，就像编程语言一样清晰。

**像绘画一样，大多数软件都是为人类观众准备的**。因此，黑客和画家一样，必须有同理心才能完成真正伟大的工作。你必须能够从用户的角度看问题。

当我还是个孩子的时候，我总是被告知要从别人的角度看问题。在实践中，这总是意味着做别人想做的事，而不是我想要的事。这当然给同理心带来了不好的名声，我特意不去培养它。

男孩，我错了。**事实证明，从别人的角度看问题实际上**是成功的秘诀。这并不一定意味着自我牺牲。远非如此。了解别人如何看待事物并不意味着你会为他的利益行事;在某些情况下，例如在战争中，你想做完全相反的事情。[4]

大多数制造商为人类观众制作东西。为了吸引观众，你必须了解他们需要什么。例如，几乎所有最伟大的画作都是人物的画作，因为人们感兴趣的东西就是人。

同理心可能是好黑客和优秀黑客之间最重要的区别。一些黑客非常聪明，但在同理心方面实际上是唯我论者。这样的人很难设计出伟大的软件[5]，因为他们无法从用户的角度看待事物。

判断人们同理心有多好的一种方法是观察他们向没有技术背景的人解释技术问题。我们可能都知道一些人，尽管他们很聪明，但在这方面却很滑稽。如果有人在晚宴上问他们什么是编程语言，他们会说：“哦，高级语言是编译器用作生成目标代码的输入。高级语言？编译器？目标代码？不知道编程语言是什么的人显然也不知道这些东西是什么。

软件要做的部分工作就是解释自己。因此，要编写好的软件，你必须了解用户理解的程度。他们会在没有准备的情况下走到软件前，它最好按照他们猜测的去做，因为他们不会阅读手册。在这方面，我见过的最好的系统是 1985 年的原始 Macintosh。它做了软件几乎从未做过的事情：它只是工作。[6]

源代码也应该解释自己。如果我能让人们记住一句关于编程的名言，那就是《计算机程序的结构和解释》开头的那句话。

程序应该写给人阅读，而只是顺便写给机器执行。

你不仅需要对你的用户有同理心，还需要对你的读者有同理心。这符合你的利益，因为你将成为他们中的一员。许多黑客编写了一个程序，六个月后才发现他不知道它是如何工作的。我认识几个人，他们在经历过这样的经历后发誓不再使用 Perl。[7]

缺乏同理心与智力有关，以至于在某些地方甚至有一种时尚。但我不认为有任何相关性。你可以在数学和自然科学方面做得很好，而不必学习同理心，而这些领域的人往往很聪明，所以这两种品质已经联系在一起。但是也有很多愚蠢的人不善于同理心。只要听听那些在脱口秀节目中打电话提问的人。他们以迂回的方式问他们问的是什么，以至于主持人经常不得不为他们重新表述问题。

那么，如果黑客像绘画和写作一样工作，那么它很酷吗？毕竟，你只有一次生命。你还不如把时间花在一些伟大的事情上。

不幸的是，这个问题很难回答。声望总是有很大的时间滞后。这就像来自遥远恒星的光。绘画现在有声望，因为人们在五百年前所做的伟大工作。当时，没有人认为这些画作像我们今天一样重要。在当时的人们看来，乌尔比诺公爵费德里科·达·蒙特费尔特罗（Federico da Montefeltro）有一天会以皮耶罗·德拉·弗朗西斯卡（Piero della Francesca）的画作中那个鼻子奇怪的家伙而闻名，这似乎很奇怪。

因此，虽然我承认黑客攻击现在看起来不像绘画那么酷，但我们应该记住，绘画本身在其辉煌时期似乎并不像现在这样酷。

我们可以自信地说，这是黑客攻击的辉煌时期。在大多数领域，伟大的工作都是在早期完成的。1430 年至 1500 年间创作的画作仍然无与伦比。莎士比亚的出现恰逢专业戏剧诞生之时，它推动了这种媒介的发展，以至于此后的每一位剧作家都不得不生活在他的阴影下。阿尔布雷希特·丢勒（Albrecht Durer）在版画上做了同样的事情，简·奥斯汀（Jane Austen）在小说上也做了同样的事情。

我们一遍又一遍地看到相同的模式。一种新的媒介出现了，人们对它感到非常兴奋，以至于他们在前几代中探索了它的大部分可能性。黑客攻击现在似乎处于这个阶段。

**在莱昂纳多的时代，绘画并不像他的作品那样酷。黑客攻击有**多酷将取决于我们能用这种新媒体做些什么。