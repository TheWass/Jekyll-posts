---
layout: post
title: Semi-Formal Diagrams pt2
date: 2014-10-22 14:58:39.000000000 -04:00
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
  _wp_old_slug: '95'
author:
  login: d35ruct0
  email: lord.d35ruct0@gmail.com
  display_name: The Wass
  first_name: ''
  last_name: ''
excerpt: !ruby/object:Hpricot::Doc
  options: {}
---
<p>Ohkay, where was I?...  Ah, yes.  Two more diagrams:  <strong>Sequence</strong> and <strong>Collaboaration</strong>.  The goal of these two diagrams is to fill in the missing information that use case diagrams and data flow diagrams do not have: A specific order to events. Sequence and collaboration diagrams both show the same information, but in two different ways.  Sequence diagrams emphasize a temporal element and collaboration diagrams emphasize the relations between events.  Remember: Sequence and collaboration diagrams show the <em>control</em> flow.  Data flow diagrams show the <em>data</em> flow.  Although the data flow could imply the control flow, it does not necessarily have to.<br />
<!--more--></p>
<h2>Sequence diagram</h2>
<p>A sequence diagram is an interaction diagram modeling a single scenario as it executes in the system.  There are two elements in this diagram: A <strong>participant</strong> is an object or entity participating in the scenario.  A <strong>message</strong> is a communication among participants. The participants are arranged on one axis and time is the other axis.</p>
<p>Here's an example:<br />
<a href="https://werdsofthewass.files.wordpress.com/2015/03/sequence.png"><img src="assets/sequence.png?w=300" alt="sequence" width="300" height="220" class="alignnone size-medium wp-image-60" /></a></p>
<p>Quick rundown of the notation:  The blocks at the top are the participants, which can be an actor or an object.  Time runs vertically downward.  The blocks along the timelines represent how long that particular participant is active. The solid arrows represent transfer of control, and the dashed arrows represent a return statement.  Using this diagram, it is quite easy to see the chronological ordering of events and how long each process is being kept open, relatively speaking.  The downside to this diagram is it can get cluttered pretty quickly as more complexity is added.</p>
<h2>Collaboration diagram</h2>
<p>The collaboration diagram fixes this issue by reorganizing the diagram into connected blocks similar to a UML class diagram.  This puts focus on the relationships rather than the time.  The ordering is still present, but in a different form.</p>
<p>Here's an example:<br />
<a href="https://werdsofthewass.files.wordpress.com/2015/03/collaboration.png"><img src="assets/collaboration.png?w=254" alt="collaboration" width="254" height="300" class="alignnone size-medium wp-image-39" /></a></p>
<p>As you can see, the numbering determines the order of the operations.  The information presented in both the sequence and collaboration diagrams is the same.  Given one, you can easily create the other.</p>
<hr />
<h1>Summary</h1>
<p>Here's the rundown:</p>
<ul>
<li>Use case diagrams show user-program interactions.</li>
<li>Data flow diagrams show the flow of data throughout the system.</li>
<li>Sequence and collaboration diagrams show the flow of control between processes.</li>
</ul>
<p>All three of these diagrams show uniquely different information, and no single diagram can contain all the information of the other two.  Ideally, these three diagrams combined contain all of the information that is in the written requirements specification, just in picture form.  Looking at and referring back to these diagrams is integral for transforming these semi-formal models into the formal, provable, mathematical models which software engineers need to show that the code they are constructing meets the requirements.  Stay Tuned!</p>
