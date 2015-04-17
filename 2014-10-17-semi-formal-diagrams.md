---
layout: post
title: Semi-Formal Diagrams
date: 2014-10-17 14:53:13.000000000 -04:00
categories:
- Software Engineering Series
tags:
- Diagrams
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
<p>Once the written specification is hammered out, the process of formalizing the requirements begins.  The jump from a written description to a formal model is quite large.  Semi-formal models initializes this translation by creating diagrams that are more code-like in nature, but are abstract enough to be read and understood by non-technical personnel.  These diagrams put the written specification into picture form.  There are four different types of diagrams:  <strong>Use-Case</strong>, <strong>Data Flow</strong>, <strong>Sequence</strong>, and <strong>Collaboration</strong>.<br />
<!--more--></p>
<hr />
<h2>Use-Cases</h2>
<p>Use-cases represent who the users of the system are, and how they interact with it.  Each use-case describes a set of related scenarios that could happen in the system given an initial state, and some trigger. Each one of these scenarios features a single actor interacting with a single feature.  Essentially, use-cases describe how actors and events are related.  Use-cases help developers define the scope of the system by generalizing the ways that users will interact with it.  The best way to identify use-cases is to sit down with the user and tell them to <em>show</em> what they do in each scenario that they are involved with.  The goal is to understand how they interact with the system.  In nearly every case, showing someone else a procedure is more informational than describing that same procedure.  In other words, go by what they do, not by what they say they do.  This will yield far more accurate information.</p>
<h2>Use-Case Diagram</h2>
<p>There are two entities in a use-case diagram (UCD); actors, and events.  An actor is an outside entity that is acting on your program.  Each event represents some functionality that the actors interact with.  The events and actors are related by using different types of relations.  Actors can initiate or participate in an event, and events can include or extend other events.</p>
<p>Take a look at this UCD of a bar: <br />
<a href="https://werdsofthewass.files.wordpress.com/2015/03/usecase.png"><img src="assets/usecase.png?w=213" alt="usecase" width="213" height="300" class="alignnone size-medium wp-image-61" /></a></p>
<p>Here's some semantics: The <strong>actors</strong> are stick figures, and the <strong>events</strong> are ovals.  The events inside the box belong to the system.  Events that appear on the outside of the box indicate events that aren't controlled by the system, but the system relies on them.  The actor to event relationships come in two flavors: <strong>initiate</strong>, and <strong>participate</strong>. An actor cannot participate in an event until it has been initiated.  For instance, the client cannot receive the beer until the bartender serves it.  The event to event relationships indicate dependencies. The <strong>include</strong> association indicates that one event requires behavior in another event.  The <strong>extend</strong> association indicates that one event can modify or add to the capabilities of another event.</p>
<p>The include and extend are rather similar in concept, but there is one main difference: <strong>extend is optional</strong>, <strong>include is not</strong>.  For example, since "Serve Beer" includes "Fill Glass", you <em>must</em> complete the "Fill Glass" event before the "Serve Beer" event can be completed.  In another example, "Put on Tab" extends "Pay for Beer", the program <em>can choose</em> whether to leave the "Pay for Beer" event unmodified, or choose to extend it with the "Put on Tab" event, which would change the functionality of "Pay for Beer".</p>
<p>Remember, a use-case diagram shows a collection of scenarios; basically, who does what.  This is useful for the developers because it shows who is involved with certain parts of the system.  It does have a couple of downfalls: This diagram does not show relationships between main events.  Just looking at this diagram, we cannot determine whether to restock or serve beer first, nor can we see what determines which beer to serve.  This is where the other diagrams come in.</p>
<hr />
<h2>Data Flow Diagram</h2>
<p>The data flow diagram (DFD) shows right what it says on the tin, the flow of data throughout the system.  It also shows the processes that transform the data, and the data stores to hold the data.</p>
<p>There are four main elements each with their own notation:</p>
<ul>
<li><strong>External Entity</strong> - A data source or sink</li>
<li><strong>Process</strong> - Models of data transformations</li>
<li><strong>Data Store</strong> - Objects that permanently store data used by the system</li>
<li><strong>Data Flow</strong> -  Links the other three elements together and indicates the type of data that flows between the two elements. </li>
</ul>
<p>The DFD has three different types based on the level of abstraction.  The <strong>top level</strong> shows the data flow between the system and external entities.  The <strong>mid level</strong> breaks the system apart into distinct processes, and shows the data flow between the processes and the external entities.  The <strong>bottom level</strong> breaks each process down into several sub-processes.  As the levels get lower, the level of detail increases.  Similar to zooming in and out on a map or a picture.</p>
<p>Here's an example of a top level DFD: <br />
<a href="https://werdsofthewass.files.wordpress.com/2015/03/dataflow0.png"><img src="assets/dataflow0.png?w=239" alt="dataflow0" width="239" height="300" class="alignnone size-medium wp-image-40" /></a></p>
<p>The top level is designed to show an overview of data (beer) flowing in and out of the system to each of the external entities.  In this diagram, External entities are shown as boxes, processes are shown as boxes with rounded edges, and the data flow is shown by arrows.  The kind of data is indicated by a label on the arrow. <br />
You might have noticed that the bartender is missing entirely from this diagram.  This is because the bartender is <em>not</em> external to the system.  The bartender acts as the data transformation agent that, for instance, takes a beer order and transforms it into beer.  Basically, the bartender controls the bar, in the same way a programer controls a computer.</p>
<p><a href="https://werdsofthewass.files.wordpress.com/2015/03/dataflow1.png"><img src="assets/dataflow1.png?w=300" alt="dataflow1" width="300" height="287" class="alignnone size-medium wp-image-41" /></a></p>
<p>I like this diagram.  Not only is it useful to both developers and non-technical personnel, but also the word "beer" appears 15 different times!<br />
The mid level DFD breaks the top level down into sub-components, one for each major process within the system. You'll notice that the external entities are still there, and they are still sinking and sourcing the same data.  This time, we can see where in the bar process each piece of data is being handled, and how it is being transformed.</p>
<p>Now we'll take the Tap Beer process down to the bottom level:<br />
<a href="https://werdsofthewass.files.wordpress.com/2015/03/dataflow2.png"><img src="assets/dataflow2.png?w=300" alt="dataflow2" width="300" height="188" class="alignnone size-medium wp-image-42" /></a></p>
<p>Notice that the other processes become external data sources and sinks to process 2.  Since this diagram is only concerned with tapping beer, the other processes become data providers.  At this level, sub processes can only generate one output, However, multiple different data sinks are able to use that one output.  This feature makes them very compatible to programming functions or methods.  In fact, these sub processes are likely functions that would call other helping functions in order to complete the task; similar to how a main() function works in procedural programming.  Therefore, the bottom level DFD is quite useful for developers and programmers working on the project, but it is too unnecessarily detailed for the non-technical stakeholder.  They would not find this particular diagram as useful as the top level.<br />
Each mid level process should have one bottom level diagram attached to it.  This could mean that there is 5-10 bottom level diagrams for a single software system.</p>
<p>It is certainly possible to combine all of these into a single data flow diagram, showing all of the bottom level processes at once.  However, this would end up being a <em>massive</em> diagram, far too much information for anyone to parse through effectively.  Each DFD level has a unique intent and a unique audience; they should be treated as separate, but connected diagrams.</p>
<hr />
<p>Alright, this post has gone on long enough.  I still have two more diagrams to go through.  But, I'll save them for the next post. Stay Tuned!</p>
<hr />
<p>All the diagrams shown in this post are generated using <a href="https://www.draw.io/">draw.io</a>. This is a great, free, online resource to use for constructing any sort of diagram.  It also saves documents to the browser, which is pretty slick!</p>
