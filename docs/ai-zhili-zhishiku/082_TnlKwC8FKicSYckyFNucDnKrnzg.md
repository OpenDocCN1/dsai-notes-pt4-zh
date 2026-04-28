# 数据收集时的合法性

> 来源：[https://docs.feishu.cn/docx/Z6Aydp5wmoFl5xx6moncW2jfnth](https://docs.feishu.cn/docx/Z6Aydp5wmoFl5xx6moncW2jfnth)

### **BookCorpus**

+   数据量：9.85亿字，包含11000册书籍

+   数据内容：BookCorpus主要是图书语料，该数据集用于训练 RoBERTA、XLNET 和 T5 等

+   下载地址：https://huggingface.co/datasets/bookcorpus

+   国内访问：https://hf-mirror.com/datasets/bookcorpus

收集时的合法性存疑

Per Bandy[ a](https://www.theguardian.com/books/2016/sep/28/google-swallows-11000-novels-to-improve-ais-conversation)nd Vi[nc](https://www.theguardian.com/books/2016/sep/28/google-swallows-11000-novels-to-improve-ais-conversation)ent (2021), it is unlikely that authors were notified about data collection from their works. Discussing BookCorpus in 2016, Richard Lea wrote in The Guardian t[ha](https://www.theguardian.com/books/2016/sep/28/google-swallows-11000-novels-to-improve-ais-conversation)t “The only problem is that [researchers] didn’t ask” (https://www.theguardian.com/books/2016/sep/28/google-swallows-11000-novels-to-improve-ais-conversation). When notified about BookCorpus and its uses, one author from Smashwords said “it didn’t even occur to me that a machine could read my book” (https://www.theguardian.com/books/2016/sep/28/google-swallows-11000-novels-to-improve-ais-conversation). Authors did not consent to the collection and use of their books. While authors on smashwords.com published their books for free, they did not consent to including their work in BookCorpus, and many books contain copyright restrictions intended to prevent redistribution. As described by Richard Lea in The Guardian (https://www.theguardian.com/books/2016/sep/28/google-swallows-11000-novels-to-improve-ais-conversation), many books in BookCorpus include: "a copyright declaration that reserves “all rights”, specifies that the ebook is “licensed for your personal enjoyment only”, and offers the reader thanks for “respecting the hard work of this author.”' Considering these copyright declarations, authors did not explicitly consent to include their work in BookCorpus or related datasets. Using the framework of consentful tech (https://www.consentfultech.io), a consent- ful version of BookCorpus would ideally involve author consent that is Freely given, Reversible, Informed, Enthusiastic, and Specific (FRIES). It is unlikely that authors were provided with a mechanism to revoke their consent in the future or for certain uses. For example, if an author released a book for free before BookCorpus was collected, then changed the price and/or copyright after BookCorpus was collected, the book likely remained in BookCorpus. In fact, preliminary analysis suggests that this is the case for at least 438 books in BookCorpus which are no longer free to download from Smashwords, and would cost $1,182.21 to purchase as of April 2021.