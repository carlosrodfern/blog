+++
title = "On Assertions"
date = 2022-01-04
author = "Carlos R.F."
+++

# Findings on Benefits and Practical Use
Assertions are formal constraints on software systems placed in the source code and are checked at runtime [^3]. They have been around in computer science for decades, dating back to 1975 in an ACM article called "Design of self-checking software" [^6]. Assertions have been highly associated with the formal verification introduced by Floyd[^7] and Hoare[^8], However, some of the expressions that are highly important in the Floyd-Hoare approach may not be practical to write as assertions in programs. For example, some loop invariants coded as assertions inside the loop may impact the time complexity of the loop significantly, e.g., from O(n) to O(n^2), making it a non-practical thing to do. Therefore, we can see assertions as a limited application of the Floyd-Hoare logic as runtime checks[^2][^3].
The following are the benefits of incorporating assertions into the code:

* Reduces the fault density in the code significantly and, as a result, increases software quality [^1][^2]
* Promotes readability and, as a result, helps programmers better understand the code [^2]
* Improves diagnosability and, as a result, reduces the troubleshooting effort to find the cause of faults [^4][^5]

These are some of the interesting findings from the few articles referenced here that can help gain a better understanding of the benefits, enabling us to adopt and apply assertions efficiently.

1) Assertions have a greater impact on reducing faults in methods with a larger number of contributors. Since assertions help with readability and also check that the expectations of the previous programmer's code are still met, it makes sense to see why this is true. This was also verified empirically in the study by Casalnuovo et al. [^2]. As a logical consequence, adding assertions to methods that change the most helps to maintain quality.

2) Programmers who have experience with a method and a sense of ownership of it tend to make more assertions. Asserts can be difficult to craft, necessitating knowledge of what to check and where to place them. It is then logical to see that the programmers with more experience in a method and a sense of ownership are the ones adding more assertions because they have a better algorithmic or conceptual understanding of the code. This is also corroborated by the study from Casalnuovo et al. [^2].

3) There is an upper limit to the improvements in diagnosability, beyond which adding more assertions does not yield a greater improvement. The quality of assertions is more important than the quantity. This is corroborated by the study by Baudry et al. [^4].

The article from Rosenblum [^3] presents his conclusion based on his extensive experience using an assertion system developed by the same author in Unix programming. The author delineates various types of assertions utilized during development, offering a more comprehensive categorization compared to the more straightforward "precondition," "postcondition," and "invariant" found in Floyd-Hoare logic. The classification can be used to help even identify general places where you want to make assertions; however, the simpler classification found in Floyd-Hoare logic is still valuable in that it is a good abstraction that is much easier to understand and remember. As a result, it may be beneficial to start with the Floyd-Hoare classification for comprehension, followed by the Rosenblum classification for specific areas of application.

Rosenblum divides his classification into _function interfaces_ and _function bodies_. The former contains preconditions and postconditions specifying function expectations, while the latter contains invariants specifying things that must hold true in between steps of the computation.

The _function interface_ assertions are:

* consistency between arguments,
* requirements of range and enumeration membership of arguments,
* non-null pointers,
* requirements of the arguments with respect to the global state before a function can be called,
* dependency of the return values on arguments,
* expected effects on global state,
* and expected immutability of arguments and global state after a function is called.

The _function bodies_ assertions are:

* condition of the else parts of if statements,
* conndition of default branch of the switch statement,
* consistency between related data within the execution,
* condition of the intermediate state of computation.

The article is not clear regarding the difference in practice between the last two.

Daikon, a tool mentioned in [^2], detects invariants that can be added to the code. The tool appears to generate more valid assertions than the ones added by developers, as well as more potential postconditions than developers do. However, the tool cannot generate all assertions written by developers, and one-third of assertions generated are not correct or are not relevant [^9][^10].

It is worth noting an anecdotal observation described in Kudrjavets et al. [^1]. They believe that enforcing assertions with metrics like the minimum number per kilo lines of code (KLOC) will not go well in teams. They received feedback from programmers and managers at Microsoft that, unless there is a culture of using assertions, such mandates will not produce the desired results. Therefore, the best approach to increasing assertions in the source code would be to educate engineers of its benefits and encourage its use during code reviews by pointing out places where a good-quality assertion would be highly beneficial.

# Reference
[^1]: G. Kudrjavets, N. Nagappan and T. Ball, "Assessing the Relationship  between Software Assertions and Faults: An Empirical Investigation," 2006 17th International Symposium on Software Reliability Engineering, 2006, pp. 204-212, doi: 10.1109/ISSRE.2006.14.
[^2]: C. Casalnuovo, P. Devanbu, A. Oliveira, V. Filkov and B. Ray, "Assert Use in GitHub Projects," 2015 IEEE/ACM 37th IEEE International Conference on Software Engineering, 2015, pp. 755-766, doi: 10.1109/ICSE.2015.88.
[^3]: D. S. Rosenblum, "A practical approach to programming with assertions", IEEE Transactions on Software Engineering, vol. 21, no. 1, pp. 19-31, Jan. 1995, doi: 10.1109/32.341844.
[^4]: B. Baudry, Y. Le Traon and J. . -M. Jezequel, "Robustness and diagnosability of OO systems designed by contracts," Proceedings Seventh International Software Metrics Symposium, 2001, pp. 272-284, doi: 10.1109/METRIC.2001.915535.
[^5]: L. C. Briand, Y. Labiche, and H. Sun, "Investigating the use of analysis contracts to support fault isolation in object oriented code." In Proceedings of the 2002 ACM SIGSOFT international symposium on Software testing and analysis (ISSTA '02). Association for Computing Machinery, New York, NY, USA, 70–80. DOI:https://doi.org/10.1145/566172.566183
[^6]: S. Yau and R. Cheung, "Design of self-checking software", in ACM SIGPLAN Notices, volume 10, pages 450-455, ACM, 1975
[^7]: R. W. Floyd. "Assigning meanings to programs". Mathematical aspects of computer science, 19(19-32):1, 1967.
[^8]: T. Hoare. Assertions in modern software engineering practice. In 2013 IEEE 37th Annual Computer Software and Applications Conference, pages 459–459. IEEE Computer Society, 2002.
[^9]: N. Polikarpova, I. Ciupa, and B. Meyer. "A comparative study of programmer-written and automatically inferred contracts." In Proceedings of the Eighteenth International Symposium on Software Testing and Analysis, ISSTA ’09, pages 93–104, New York, NY, USA, 2009. ACM.
[^10]: T. W. Schiller, K. Donohue, F. Coward, and M. D. Ernst. "Case studies and tools for contract specifications." In Proceedings of the 36th International Conference on Software Engineering, ICSE 2014, pages 596–607, New York, NY, USA, 2014. ACM.