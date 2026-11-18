
# OpenAI 的错位与微软的收获（OpenAI’s Misalignment and Microsoft’s Gain）

> 来源：[https://leonzhang.feishu.cn/docx/EfSPdHsOqoQ7R0xCaDacIHo4nHc](https://leonzhang.feishu.cn/docx/EfSPdHsOqoQ7R0xCaDacIHo4nHc)

> Author: Ben Thompson (本·汤普森)

I have, as you might expect, authored several ve[rsions of this Article, ](https://twitter.com/benthompson/status/1726514608234746003)both in my head and on the page, as the most extraordinary weekend of my career has unfolded. To briefly summarize:

+ On Friday, then-CEO Sam Altman was fired from OpenAI by the board that governs the non-profit; then-President Greg Brockman was removed from the board and subsequently resigned.

+ Overthe weekendrumorssurged that Altman was negotiating his return, only for OpenAI to hire former Twitch CEO Emmett Shear as CEO.

+ Finally, late Sunday night, Satya Nadella announced via tweet that Altman and Brockman, “together with colleagues”, would be joining Microsoft.

This is, quite obviously, a phenomenal outcome for Microsoft. The company already has a perpetual license to all OpenAI IP ( short of artificial general intelligence ), including source code and model weights; the question was whether it would have the talent to exploit that IP if OpenAI suffered the sort of talent drain that was threatened upon Altman and Brockman’s removal. Indeed they will, as a good portion of that talent seems likely to flow to Microsoft; you can make the case that Microsoft just acquired OpenAI for $0 and zero risk of an antitrust lawsuit. 1↳

Microsoft’s gain, meanwhile, is OpenAI’s loss, which is dependent on the Redmond-based company for both money and compute: the work its employees will do on AI will either be Microsoft’s by virtue of that perpetual license, or Microsoft’s directly because said employees joined Altman’s team. OpenAI’s trump card is ChatGPT, which is well on its way to achieving the holy grail of tech — an at-scale consumer platform — but if the reporting this weekend is to be believed, OpenAI’s board may have already had second thoughts about the incentives ChapGPT placed on the company (more on this below).

The biggest loss of all, though, is a necessary one: the myth that anything but a for-profit corporation is the right way to organize a company.

### OpenAI’s Non-Profit Model

OpenAI was founded in 2015 as a “non-profit intelligence research company.” From the initial blog post :

> OpenAI is a non-profit artificial intelligence research company. Our goal is to advance digital intelligence in the way that is most likely to benefit humanity as a whole, unconstrained by a need to generate financial return. Since our research is free from financial obligations, we can better focus on a positive human impact. We believe AI should be an extension of individual human wills and, in the spirit of liberty, as broadly and evenly distributed as possible. The outcome of this venture is uncertain and the work is difficult, but we believe the goal and the structure are right. We hope this is what matters most to the best in the field.

I was pretty cynical about the mo[tivations of ](https://stratechery.com/2015/openai-artificial-intelligence-and-data-data-and-recruiting/)OpenAI’s founders, at least Altman and Elon Musk; I wrote in a Daily Update :

> Elon Musk and Sam Altman, who head organizations (Tesla and YCombinator, respectively) that look a lot like the two examples I just described of companies threatened by Google and Facebook’s data advantage, have done exactly that with OpenAI, with the added incentive of making the entire thing a non-profit; I say “incentive” because being a non-profit is almost certainly a lot less about being altruistic and a lot more about the line I highlighted at the beginning: “We hope this is what matters most to the best in the field.” In other words, OpenAI may not have the best data, but at least it has a mission structure that may help idealist researchers sleep better at night. That OpenAI may help balance the playing field for Tesla and YCombinator is, I guess we’re supposed to believe, a happy coincidence.

Whatever [Altma](https://projects.propublica.org/nonprofits/organizations/810861541)n and Musk’s motivations, the decision to make OpenAI a non-profit wasn’t just talk: the company is a 501(c)3; you can view their annual IRS filings here . The first question on Form 990 asks the organization to “Briefly describe the organization’s mission or most significant activities”; the first filing in 2016 stated:

> OpenAIs goal is to advance digital intelligence in the way that is most likely to benefit humanity as a whole, unconstrained by a need to generate financial return. We think that artificial intelligence technology will help shape the 21st century, and we want to help the world build safe AI technology and ensure that AI’s benefits are as widely and evenly distributed as possible. Were trying to build AI as part of a larger community, and we want to openly share our plans and capabilities along the way.

Two years later, and the commitment to “openly share our plans and capabilities along the way” was gone; three years after that and the goal of “advanc[ing] digital intelligence” was replaced by “build[ing] general-purpose artificial intelligence”.

In 2018 Musk, [according to a Semafor repor](https://www.semafor.com/article/03/24/2023/the-secret-history-of-elon-musk-sam-altman-and-openai)t earlier this year , attempted to takeoverthe company, but was rebuffed; he left the board and, more critically, stopped paying for OpenAI’s operations. That led to the second critical piece of background: faced with the need to pay for massive amounts of compute power, Altman, now firmly in charge of OpenAI, created OpenAI Global, LLC, a capped profit company with Microsoft as minority owner. This image of OpenAI’s current structure is from their website :

![](img/094b510fe5e7132a4df827d0d3d43912.png)

OpenAI Global could raise money and, critically to its investors, make it, but it still operated under the auspices of the non-profit and its mission; OpenAI Global’s operating agreement states:

> The Company exists to advance OpenAI, Inc.’s mission of ensuring that safe artificial general intelligence is developed and benefits all of humanity. The Company’s duty to this mission and the principles advanced in the OpenAI, Inc. Charter take precedenceoverany obligation to generate a profit. The Company may never make a profit, and the Company is under no obligation to do so. The Company is free to re-invest any or all of the Company’s cash flow into research and development activities and/or related expenses without any obligation to the Members.

Microsoft, despite this constraint on OpenAI Global, was not only an investor, but also a customer, incorporating OpenAI into all of its products.

### ChatGPT Tribes

The third critical piece of background is the most well-known, and what has driven those ambitions to new heights: ChatGPT was released at the end of November 2022, and it has taken the world by storm. Today ChatGPT hasover100 million weekly users andover$1 billion in revenue; it has also fundamentally altered the conversation about AI for nearly every major company and government.
