---
layout: post
title: Petri Net Analysis
date: 2015-02-02 15:11:56.000000000 -05:00
categories:
- Software Engineering Series
tags:
- Diagrams
- Petri nets
- Requirements
- Software Engineering
status: publish
type: post
published: true
meta:
  _wpcom_is_markdown: '1'
  _edit_last: '82057723'
  _publicize_pending: '1'
  geo_public: '0'
author:
  login: d35ruct0
  email: lord.d35ruct0@gmail.com
  display_name: The Wass
  first_name: ''
  last_name: ''
excerpt: !ruby/object:Hpricot::Doc
  options: {}
---
<p>Since petri nets are largely mathematical in nature, they are very accessible to analyze.  People are still discovering new and innovative techniques to tap more potential from petri nets.  I should warn you, this article is going to be quite math-heavy, delving further into graph and set theory than previous posts. For that reason, I have created an accompanying article on the basics of <a href="https://werdsofthewass.wordpress.com/2015/01/22/introduction-to-set-theory/" title="Introduction to Set Theory">set theory</a>.<!--more--></p>
<hr />
<h2>Definitions</h2>
<p>First on the to do list is to nail down some definitions that will be prevalent throughout this article.</p>
<h3>Petri net</h3>
<p>A petri net is a collection of directed arcs connecting places and transitions. Places may hold marks. The state or marking of a net is its assignment of marks to places.  A petri net (&#x1A4;) can be defined by a 6-tuple:</p>
<blockquote><p>
&#x1A4; = (P, T, F, W, K, M<sub>0</sub>)
</p></blockquote>
<p>Where:</p>
<ul>
<li>P is a finite set of places.</li>
<li>T is a finite set of transitions.</li>
<li>F is the set of firing relationships: F = P x T &cup; T x P</li>
<li>W is the set of weights or costs for each firing relationship: F &rarr; &#x2115;<br />
The default is 1.</li>
<li>K is the set of marker capacities for each place: P &rarr; &#x2115;<br />
The default is &infin;.</li>
<li>M<sub>0</sub> is the set of initial markers for each place: P &rarr; &#x2115;</li>
</ul>
<h3>Marking</h3>
<p>A marking (M) is simply an assignment of tokens to places.  M is denoted as a set of integers whose i<sup>th</sup> component is the number of tokens at place i.  M = [0, 0, 3, 0, 1]  In this marking, place 3 has three tokens and the place 5 has one token.  Each marking represents a 'state' of the machine.  M is always ordered so that the indexes correspond to places in the net.</p>
<h3>Enabled</h3>
<p>A transition is enabled if every place pointing to the transition has at least as many marks as the weight of the firing relationship connecting them, and if we do not exceed the capacity of any place that gets a token after t fires.</p>
<h3>Firing Transitions</h3>
<p>Given some marking M, we may fire an enabled transition to yield a new marking. We denote this new marking M'.  We say that t fires from M to M' and write this as M [t&gt; M'.  If there is a sequence of transition firings M [t&gt; ... [t&gt; M*,we say M* is reachable from M.</p>
<hr />
<h2>Reachability Analysis</h2>
<p>The big question regarding petri nets is whether or not a marking is reachable from another marking.  In terms of a program, reachability can show us if a particular state can ever be attained, and if it can, the path(s) it took to get there.  We can also use reachability to investigate the possible conditions of a system failure, or an end state.  In essence, reachability is able to debug the program before it hits the design board. <br />
Let's analyze the bar scenario.  Here's the petri net with the initial markings in place.</p>
<p><a href="https://werdsofthewass.files.wordpress.com/2015/03/pnbeer.png"><img src="assets/pnbeer.png?w=300" alt="pnbeer" width="300" height="99" class="alignnone size-medium wp-image-50" /></a></p>
<p>Mathematical sets are not designed to be in any specific order, so let's put the places in a specific order.  All the markings will correspond to that order.</p>
<blockquote><p>
P = [Order, Beer, Bill]<br />
T = [Order Beer, Serve Beer, Pay Bill]<br />
M<sub>0</sub> = [0, 0, 0]
</p></blockquote>
<p>There are many ways to analyze reachability, but the easiest is to construct a tree of all possible markings interconnected with the transitions. This tree continues until no transitions can fire, or until no more unique markings are created.</p>
<p><a href="https://werdsofthewass.files.wordpress.com/2015/03/pnabartree.png"><img src="assets/pnabartree.png?w=300" alt="pnabartree" width="300" height="251" class="alignnone size-medium wp-image-49" /></a></p>
<p>In this case, the tree is infinite.  It will continue going on forever.  Despite this, we can still draw conclusions.  Notice that the first two numbers are entirely independent of each other, and the third number cannot be any larger than the second number.  From this we can derive a reachability set from M<sub>0</sub>: R = {[p<sub>0</sub>, p<sub>1</sub>, &forall;p<sub>2</sub> : p<sub>2</sub>&le;p<sub>1</sub>]}. In this case, R is the set of all valid markings of the petri net.  Now, this may not be exact since it is derived from a partial tree.</p>
<p>The ultimate in reachability analysis is to derive a function that calculates reachability from any marking to any other marking.  Typically, this will end up to be rather complex as the petri net grows, and there is no set method on actually accomplishing this; it depends on the petri net itself.  For this one, we can create 'rules' that define each transition and relate arbitrary markings together.  From there, we can derive a function or series of functions that capture those rules:</p>
<p>Rules: M = {[p<sub>0</sub>, p<sub>1</sub>, p<sub>2</sub>]} &rarr; M' ={[p'<sub>0</sub>, p'<sub>1</sub>, p'<sub>2</sub>]}</p>
<ul>
<li>t<sub>0</sub>: M' = {[p<sub>0</sub>+1, p<sub>1</sub>, p<sub>2</sub>]}</li>
<li>t<sub>1</sub>: M' = {[p<sub>0</sub>-1, p<sub>1</sub>+1, p<sub>2</sub>+1]}</li>
<li>t<sub>2</sub>: M' = {[p<sub>0</sub>, p<sub>1</sub>, p<sub>2</sub>-1]}</li>
</ul>
<p>Given these rules, we can ascertain that p'<sub>1</sub> must be greater than or equal to p<sub>1</sub>.  This is because t<sub>1</sub> is the only modifier for p<sub>1</sub>.  We can also ascertain that p'<sub>0</sub> + p'<sub>1</sub> must be greater than or equal to p<sub>0</sub> + p<sub>1</sub>.  This is because t<sub>1</sub> keeps the sum the same, but the numbers change. t<sub>0</sub> is the only transition that increases the sum.  By the same token, p'<sub>1</sub> - p'<sub>2</sub> must be greater than p<sub>1</sub> - p<sub>2</sub> since t<sub>2</sub> is the only transition that increases that difference.</p>
<blockquote><p>
&forall; m'(p<sub>0</sub>, p<sub>1</sub>, p<sub>2</sub>) &isin; R(m(p<sub>0</sub>, p<sub>1</sub>, p<sub>2</sub>)&isin;M) : m'(p<sub>1</sub>) &ge; m(p<sub>1</sub>) AND m'(p<sub>0</sub>)+m'(p<sub>1</sub>) &ge; m(p<sub>0</sub>)+m(p<sub>1</sub>) AND m'(p<sub>1</sub>)-m'(p<sub>2</sub>) &ge; m(p<sub>1</sub>)-m(p<sub>2</sub>)
</p></blockquote>
<p>Translating from Math to English this reads:</p>
<blockquote><p>
Let m be a marking with three places (p<sub>0</sub>, p<sub>1</sub>, p<sub>2</sub>) and let m' be a reachable marking.<br />
Let M be the set of all markings and let R(m) be the set of Reachable markings from a specific marking.<br />
For all markings in the reachability set, the following conditions must all be true:</p>
<ul>
<li>The second place in the reachable marking must be greater or equal to the second place in the initial marking.</li>
<li>The sum of the first and second places in the reachable marking must be greater or equal to the sum of the first and second places in the initial marking.</li>
<li>The difference of the second and third places in the reachable marking must be greater or equal to the difference of the second and third places in the initial marking.</li>
</ul>
</blockquote>
<p>This function effectively describes every reachable marking given some initial marking for that petri net.  This is a simple petri net, so as you can see, the function can get quite hairy.</p>
<p></p>
<p>So, by now, you all are probably asking...</p>
<h1>What's the point?</h1>
<p>Petri net reachability is epically awesome when it comes to defining software requirements.  By constructing a petri net based on a written description of some software system, you can effectively execute the program in order to debug flaws that are inherent in the requirements of the software <em>before</em> the software is even designed!  Reachability analysis is powerful enough to detect when the system might stop and under what conditions.  It can also tell you the precise conditions that could cause a software system to fail.</p>
<p>The petri net shines best when describing communication between two or more systems in terms of sharing a central resource, such as a file or database.  Using the reachability tree, It can detect collisions and concurrency issues before a single line of code is written.  I'll wrap it up here and save more of my thoughts for another page.  Stay Tuned!</p>
