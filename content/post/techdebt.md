+++
title = "Technical Debt: Origin, Impact, and Management"
date = 2022-01-19
author = "Carlos R.F."
+++

# Introduction

Ward Cunningham coined the term "Technical Debt" (TD) in 1992 to
describe the deterioration of software quality and its impact on the
company (Cunningham 1992). The metaphor enables technical people to
communicate with business people about software quality issues. The
debt’s principal is the amount of effort required to solve the problem.
The interest is reflected in a variety of ways, including increased
maintenance costs, decreased system performance, increased operating
costs, and decreased usability (Ciolkowski, Lenarduzzi, and Martini
2021). When TD is high, people see a system that ostensibly meets the
functional requirements, but is internally misaligned with them and
fails to meet many non-functional requirements. A business with high
debt may “function,” but it has little cash flow left to expand the
portfolio or increase capacity; similarly, a system with high TD may
“work,” but any attempt to modify functionality or increase data/traffic
volume will reveal its fragility.

Since 1992, there has been an increase in industry and academic interest
in TD. This interest is reflected in the TechDebt conference/workshop
series, which was founded more than a decade ago. The series provides a
forum for academic and industry researchers to share their findings and
experiences in defining, measuring, and managing TD.

What is it about this topic that is gaining so much traction? Let the
numbers speak by themselves: it is estimated that 30% of development
resources are wasted because of TD, with peaks of 80%, leading to
project crises(Ciolkowski, Lenarduzzi, and Martini 2021)(Besker,
Martini, and Bosch 2019). As the Software Industry matures, and Software
systems age, TD has become an important topic because of the tremendous
risk it presents to companies.

We explain how this software degradation enters the source code, as well
as why and how it affects the company, in the sections that follow.
Then, we present well-established Software Engineering practices for
dealing with it in a methodical manner.

# Technical Debt Origination and Impact

TD is the result of iterative software development (Fairbanks
2020b)(Ciolkowski, Lenarduzzi, and Martini 2021). Iterative development
allows us to begin developing and releasing software to users even when
we only have a partial understanding of the requirements. As a result,
iterative development, as opposed to waterfall development, allows us to
deliver features to users faster and receive feedback sooner rather than
later. It is a useful method for software development. It does, however,
come with the creation of TD.

As a project progresses through an iterative incremental process, the
team gains a better and broader understanding of the requirements.
Suddenly, the existing data structure or module structure does not
correspond to the requirements. As a result, new code is forced to work
alongside existing code, resulting in layers of “translations” or
less than ideal designs. As time passes, the code becomes more difficult
to understand, particularly the theory underlying all of the
structuring; because it is a mix. The accumulation of unsuitable designs
reaches a point where the code actually misleads developers and is
difficult to follow. New developers join the maintenance team, and they
begin sending patches with only a hazy understanding of the design,
exacerbating the situation. The source code deteriorates to the point
where it is nearly impossible to accurately predict the short and long
term impact of any change. This is well documented in Software
Engineering (Bourque and Fairley 2014)(Ciolkowski, Lenarduzzi, and
Martini 2021). Some refer to it as the *Loss of Intellectual Control* ,
a term coined by Dijkstra (Fairbanks 2020b)(Fairbanks 2020a).

This degradation is the misalignment of the code with the functional
requirements. The code that has high level of TD could implement the
functional requirements, but its design is not in alignment with such
specification: i.e., mismatching of concepts naming and relationship,
unfit structuring, and misleading execution models are abundant. As
result, changing functionality becomes increasingly costly.

Efficiency and latency are non-functional requirements also affected by
TD. The TD appears when the component that was not designed for
efficiency begins to increase the operational cost or show unacceptable
latencies. It was not designed for efficiency because the data volume
was originally low and designing for efficiency required greater effort.

Following guidelines and standards contributes
to keeping maintainability by enforcing code testability, technology
adequacy, acceptable efficiency (in most cases), robustness, and readability.
Following such guidelines and standards is a non-functional requirement.
As a result, any code that deviates from them fails to meet non-functional
requirements as well, and it is classified as TD (Ilkiewicz and Letouzey 2012).

In addition to the degradation caused by iterative incremental
development processes, there are challenges caused by technological
inadequacy. Technologies that were beneficial in the past may now be detrimental.
For example, some markup language that in the past were
used for rapid development like Coldfusion or ASP, have lost favor in
the industry. As result, the browsability and debuggability is hampered
by a lack of good modern Integrated Development Environment (IDE)
support, necessitating additional effort for understanding and
troubleshooting. The lack of good IDE support, as well as the
difficulties in creating clean module interfaces, discourages
modularization, resulting in large files with a large number of
execution paths that are difficult for a human brain to follow
(Antinyan, Sandberg, and Staron 2019). This is exacerbated by the
constant stream of patches. As a result, code written in those languages
degrades faster than other technologies with better support.

Rapid development technologies are also built around the ability to
combine rendering, business logic, and data access in the same file.
This approach, which was popular at one point in the past, allows the
developer to create a simple web portal in a relatively short period of
time. However, it has a significant negative impact on its upkeep.
Modernizing the system is difficult when there is no separation of
concerns. This and other technologies that encourage poor design are
also the originators of TD.

One more cause of TD is simply the lack of experience and proper
education background of developers making code and system design
decisions which find their way into production. This is usually the result
of the lack of a development process that includes reviewing and
auditing source code changes.

What is the specific impact on productivity? What follows is a summary
of some of the findings that Besker *et al.* presented in their
outstanding study about this topic in (Besker, Martini, and Bosch 2019).
This study aims to measure the impact on productivity of TD as the
wasted software development time that produces no value for the customer
or user. They did a study and then replicated it. The first study
included 43 developers. 56% had more than 10 years of experience, all
had university-level education, and 82% of them had a master’s degree.
44% of the developers worked on systems that were between 5-10 years
old, and 30% of them on systems that were 10 or older. Besker *et al.*
collected data twice a week for seven weeks from the developers. The
study does not really take into account if there was any kind of
systematic TD management in the systems the developers were working on.
Therefore, the numbers presented are lower bounds for systems that do
not have any systematic TD management. The second study included 47
developers. Since the second study simply validated the first study’s
findings, the first study will be the focus of this post.

The first study discovered that 23.1% of time is wasted on average. The
average wasted time for a system 5-10 years old was 15.3%, and 55.3% for
a system older than 20 years old. Modeling and Simulation Systems,
Real-Time Systems, and Embedded Systems wasted less time than the
average, whereas Data Management Systems, System Integration, and Web
SaaS Systems wasted more than twice as much. *Additional testing*,
meaning extra testing during development (local testing) beyond what is
normally done, accounted for 43.1% of the mean wasted time; *additional
refactoring*, the extra refactoring to prepare a software for change
beyond what is reasonable, 42.8%; *additional code analysis*, additional
study of the code for comprehension beyond what is normally expected
effort, 37.9%. In terms of the level of impact different TD types had on
wasted time, 36.9% reported that code issues had a *somewhat* or *to a great extent*
impact on wasted time, followed by 21.9% for testing
issues and 21.7% for architectural issues.

The second study was a replication of the first, and it confirmed the
original findings. It is worth noting that in the second study, they
discovered a slightly higher average: 28.51% of wasted time on TD.

# Managing Tech Debt

How can we get rid of TD? TD is inextricably linked to iterative
incremental development. Iterative incremental development is the best
way to create software. It enables us to deliver value faster and adapt
to changing market demands, but it still brings us TD. On a system that
is constantly evolving, it is impossible to eliminate TD. As a result,
the goal is to keep it low so that it does not have a significant negative impact on
productivity and quality.

TD management requires a systematic approach to remediate it (Ozkaya,
Nord, and Kruchten 2012). The management process consists of
identification, and remediation activities. The identification consists
of the examining of existing source code components, whether
proactively, or reactively as we visit the code for other reasons, or
the evaluation of static analysis tools reports. The remediation
consists of re-engineering tasks, which could comprise partial
rewriting, or refactors to bring the source code to a better alignment
with functional and non-functional requirements.

The systematic approach to address TD may feel like a new invention, but
in reality, it has been part of Software Engineering for decades, it is
part of what has been called Software Maintenance. It was referred to in
the late 70s as *perfective* maintenance and it is currently part of the
IEEE Software Engineering Body of Knowledge (Bourque and Fairley 2014)
(Dorfman and Thayer 2000). It is defined as “modification of a software
product after delivery to provide enhancements for users, improvement of
program documentation, and recoding to improve software performance,
maintainability, or other software attributes.” TD management, along
with defect and vulnerability remediation, should be included in the
Software Maintenance processes. Their tasks necessitate consistent,
planned effort.

There is a very popular method for identification and quantification
called SQALE, which is supported by several vendor tools (Ilkiewicz and
Letouzey 2012) (e.g. Sonar, and Squore). SQALE stands for “software
quality assessment based on life-cycle expectations.” The method uses
tools to identify issues in code and then uses definitions to determine
the amount of effort necessary to fix them. For example, the team
defines one trivial rule like “there is no commented-out block of
instruction,” and the remediation is to remove it, and the effort is 2
minutes per occurrence; and another rule like “there is no method with
Cyclomatic Complexity[^1] over 12,” and the remediation is to refactor
with the IDE, and the effort is 1h per occurrence if it is less than 24,
or 2h if it is more than 24. Then the tool identifies these situations
and calculates the total effort necessary to remediate them.

Another simple and effective method of identifying TDs is by the team
self-admitting areas of concern. This method depends on regularly reviewing
existing and new code by experienced engineers.

Regardless of methods or tools used, identified TD should be collected,
and then a regular effort should be scheduled (meetings or tickets) to
analyze them. The result of the analysis should be a consolidation and
prioritization of the TD issues, and then the submission of
re-engineering tasks (tickets) with sufficient specification of what the
remediation is (Ozkaya, Nord, and Kruchten 2012) (Ilkiewicz and Letouzey
2012) (Ciolkowski, Lenarduzzi, and Martini 2021).

Ideally, we should monitor the progress of the elimination of TD as
well. SQALE can show us that progress in hours saved. Metrics like the
distribution of Cyclomatic Complexity can show us progress in reducing
complexity.

In conclusion, a systematic approach is necessary for effective TD
management. The identification, quantification, and analysis of TD on a
regular basis, plus the re-engineering tasks to remediate them, should
be part of the software maintenance process.

# Conclusion

TD is a good metaphor to explain the necessity of its creation to move
fast, but also its long-term devastating effect on productivity and
quality if not managed properly. The importance of systematically
addressing it as part of our software maintenance process cannot be
stated enough. The implicit cost the company pays regularly in wasted
hours of productivity and also quality issues can be very high, as can
the negative impact on business agility as a result of it. A company can
benefit from a dedicated process for TD identification and analysis, as
well as allocating resources to TD reduction in addition to new
development, defect remediation, security vulnerability remediation, and
implementations.

# References

Antinyan, V., A. B. Sandberg, and M. Staron. 2019. “A Pragmatic View on
Code Complexity Management.” *Computer* 52 (2): 14–22.
<https://doi.org/10.1109/MC.2018.2888761>.

Besker, Terese, Antonio Martini, and Jan Bosch. 2019. “Software
Developer Productivity Loss Due to Technical Debt—a Replication and
Extension Study Examining Developers’ Development Work.” *Journal of
Systems and Software* 156: 41–61.
https://doi.org/<https://doi.org/10.1016/j.jss.2019.06.004>.

Bourque, Pierre, and Richard E. (Dick) Fairley, eds. 2014. “Guide to the
Software Engineering Body of Knowledge.” In, (5-1) - (5-14). IEE
Computer Society.

Ciolkowski, M., V. Lenarduzzi, and A. Martini. 2021. “10 Years of
Technical Debt Research and Practice: Past, Present, and Future.” *IEEE
Software* 38 (06): 24–29. <https://doi.org/10.1109/MS.2021.3105625>.

Cunningham, Ward. 1992. “The WyCash Portfolio Management System.”
*SIGPLAN OOPS Mess.* 4 (2): 29–30.
<https://doi.org/10.1145/157710.157715>.

Dorfman, Merlin, and Richard H. Thayer, eds. 2000. “Software
Engineering.” In, 287–303. IEEE Computer Society.

Fairbanks, G. 2020a. “Testing Numbs Us to Our Loss of Intellectual
Control.” *IEEE Software* 37 (03): 93–96.
<https://doi.org/10.1109/MS.2020.2974636>.

———. 2020b. “The Rituals of Iterations and Tests.” *IEEE Software* 37
(06): 105–8. <https://doi.org/10.1109/MS.2020.3017445>.

Ilkiewicz, M., and J. Letouzey. 2012. “Managing Technical Debt with the
SQALE Method.” *IEEE Software* 29 (06): 44–51.
<https://doi.org/10.1109/MS.2012.129>.

Ozkaya, I., R. L. Nord, and P. Kruchten. 2012. “Technical Debt: From
Metaphor to Theory and Practice.” *IEEE Software* 29 (06): 18–21.
<https://doi.org/10.1109/MS.2012.167>.

# Footnotes

[^1]: This metric measures the complexity of a method. The higher the
number, the harder is to comprehend the method implementation. The more
difficult it is to comprehend, the more time you will spend attempting
to implement the necessary change. A number between 10 and 12 it is
usually the threshold reported by literature as an indication that the
method complexity is high.
