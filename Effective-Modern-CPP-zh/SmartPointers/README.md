#Charpter4 智能指针
诗人和作曲家喜欢写一些关于love的作品，也有可能写一些关于计数(counting)的作品,很少有两者兼顾的。总有些例外，如Elizabeth Barrett Browning:"How do I love thee? Let me count the ways",又如Paul Simon:"There must be 50 ways to leave your lover.",被这些诗句启发，我们来尝试列举下为什么原生指针(raw pointer)不那么讨人喜欢(love)的理由:

1.从它的声明看不出它指向的是一个单个的对象还是一个数组

2.当你使用完它的时候，从它的声明看不出来你是否应该把它销毁，例如，当指针拥有(owns)它当前指向的对象时

3.当你确定要销毁它指向的内容的时候，又要犯难了，因为你不知道要使用delete，还是要使用另外一个不同的销毁机制(如将该指针传递到一个指定的析构函数里)

4.当你终于要使用delete决定要销毁它了,因为第1条，你又不知道该使用delete还是delete[],因为一旦使用错误，结果会是不确定的

5.最后，你终于确定了指针指向的内容是啥了，也确定了改用什么样的方式来销毁；问题又来了，因为你不能保证在你的程序的每条路径中，你的销毁代码只执行一次，不执行的话会造成内存泄露，多执行哪怕一次会产生不确定的行为

6.目前没有方法来确定一个指针是悬挂指针,即确定一个指针不再拥有它指向的对象。当一个指针指向的对象被销毁了，该指针就变成了悬挂指针。

原生指针是一款很强大的工具，但是依据进数十年的经验，可以确定的一点是:稍有不慎，这个工具就会反噬它的使用者。

终于，来解决上述难题的智能指针出现了，智能指针表现起来很像原生指针，它相当于是原生指针的一层再包装(wrapper)，但是规避了许多使用原生指针带来的陷阱。你应该尽量使用智能指针，它几乎能做到原生指针能做到的所有功能，却很少给你犯错的机会。

在C++11标准中规定了四个智能指针:std::auto_ptr, std::unique_ptr, std::shared_ptr, 以及std::weak_ptr.它们都用来设计辅助管理动态分配对象的生命周期，即，确保这些对象在正确的时间(包括发生异常时)用正确的方式进行回收，以确保不会产生内存泄露.

C++98尝试用std::auto_ptr来标准化后来成为C++11中的std::unique_ptr的行为，为了达到目标，move语法是不可少的，但是，C++98当时还没有move语法，所以做了个妥协方案:利用拷贝操作来模拟move.这导致了一些很让人吃惊的代码(如拷贝一个std::auto_ptr会将它设置为null!)和一些让使用者觉得沮丧的使用限制(不能在容器中使用std::auto_ptr)

std::unique_ptr做到了std::auto_ptr所能做到的所有事情，而且它的实现还更高效。

智能指针的API有着显著的区别，他们之间唯一共同的一点功能就是默认的构造方法。因为这种API详细的介绍满大街都是啊，所以我把重点放到了这些API介绍所没有的知识，如:值得注意的使用场景，运行性能分析等等。掌握这些信息你就不只会可以单单的使用它们，更是学会了如何有效的运用它们。