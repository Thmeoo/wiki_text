# wiki_text
1.初步处理：手动清洗维基语法


观察原始的XML数据，尝试清理维基百科特有的模板语法，图片和媒体标签，包括删除维基模板（{{}}）和图片/图库标签<gallery>/,</gallery>；处理维基链接，将{{...}}格式链接转换格式，确保正确解析；清除无关的列表和格式符号。


这样处理的文件效果并不好，由于不熟悉wiki语法，漏掉了很多应该清理的内容，随后尝试引入工具提升效率及准确率


2.引入wikiextractor自动化清洗


wikiextractor是官方提取数据中标题和正文的工具，采用wikiextractor提取数据的代码见wiki_extractor.ipynb，运行时需下载WikiExtractor.py文件。


这样提取到的文件中，每一条数据仅包含文章正文与标题，但存在大量繁体字，不利于nlp任务，因此再通过opcc进行繁简体转换，并修改及统一符号的格式，最终得到满足示例要求的jsonl格式。


wikiextractor的缺陷是会将原数据集中的参考文献，注释及外部链接等等内容删除，只保留最精炼的文本内容，考虑到nlp任务可能会用到这些信息，因此又采用了另一种清洗策略。

3.采用genism+正则匹配的方式清洗

代码见wiki_genism.ipynb

gensim的作用主要有：a.利用extract_pages()从wikipedia.bz2文件中逐条提取（标题，正文，ID）b.利用filter_wiki去除wikipedia文章中的维基标记语法

正则匹配方法（re.sub）主要用于进一步删除非正文信息及清理文本格式

通过gensim及正则匹配，再利用opcc进行繁简体转换，得到了带有参考文献等内容的文本，并将其保存为要求的jsonl格式。
