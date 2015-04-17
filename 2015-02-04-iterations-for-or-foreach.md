---
layout: post
title: 'Iterations: ''for'' or ''foreach''?'
date: 2015-02-04 16:42:56.000000000 -05:00
categories:
- PHP
tags:
- Code Tricks
- PHP
status: publish
type: post
published: true
meta:
  _wpcom_is_markdown: '1'
  _edit_last: '82057723'
  _publicize_pending: '1'
  geo_public: '0'
  _wpas_skip_facebook: '1'
  _wpas_skip_google_plus: '1'
  _wpas_skip_twitter: '1'
  _wpas_skip_linkedin: '1'
  _wpas_skip_tumblr: '1'
  _wpas_skip_path: '1'
author:
  login: d35ruct0
  email: lord.d35ruct0@gmail.com
  display_name: The Wass
  first_name: ''
  last_name: ''
excerpt: !ruby/object:Hpricot::Doc
  options: {}
---
<p>Those of you who first learned programming (me included) probably started learning with C++ or Java. And with these languages, iteration is typically done with <code>while</code> or <code>for</code> loops. The <code>foreach</code> construct is used for specific object classifications. As you might expect, these constructs may not operate in the same manner. Let me share with you the similarities and differences between <code>while</code>, <code>for</code>, and <code>foreach</code>.<!--more--></p>
<h2>While Loops</h2>
<p>a <code>while</code> loop is the basic iteration structure. It contains a block of code that is repeatedly executed while some condition remains true. Consider this code:<br />
<code><br />
$count = 10;<br />
while ($count &gt; 0) {<br />
echo $count . "\n";<br />
$count--;<br />
}<br />
echo "Blastoff!!"<br />
</code></p>
<p>This is a very trivial example. This code outputs a countdown from 10 to 1 and outputs "Blastoff!!" at the end. As you can see, once the end brace is reached, it loops back to the top of the loop and then checks the condition again. Once the condition becomes false, the code block is skipped entirely.<br />
Now, this is probably second nature to all of you, but bear with me, you might learn something!</p>
<h2>For loops</h2>
<p>Consider this code:<br />
<code><br />
for ($count = 10; $count &gt; 0; $count--) {<br />
echo $count . "\n";<br />
}<br />
echo "Blastoff!";<br />
</code><br />
Compare this code to the while loop version. Not only do they do the exact same thing; they are interpreted the <em>exact same way</em>. The for loop is syntactic sugar for the while loop. Let me explain:<br />
The compiler takes the components of the for loop and executes them in a specific order:</p>
<ol>
<li>assignment</li>
<li>condition: if false, goto end; if true, continue</li>
<li>body</li>
<li>increment</li>
<li>goto condition</li>
<li>end</li>
</ol>
<p>This:<br />
<code><br />
for (assignment; condition; increment) {<br />
body<br />
}<br />
</code><br />
Changes to this:<br />
<code><br />
assignment;<br />
while (condition) {<br />
body<br />
increment<br />
}<br />
unassign //the variable that was assigned.<br />
</code><br />
The main difference between a for loop and a while loop is this: In a while loop, the assignment happens before the code block is executed. This means the counter is accessible <em>after</em> the while loop has completed. The for loop automatically gets rid of the counter variable.</p>
<p>For loops were designed to simplify iterating over an array:<br />
<code><br />
$data = array('Bob', 'Tom', 'Joe', 'Sam', 'Ann');<br />
for ($i = 0; $i &lt; sizeOf($data); $i++) {<br />
echo $data[$i];<br />
}<br />
</code><br />
This code (as much as I <a href="https://werdsofthewass.wordpress.com/2014/07/31/php-arrays/" title="PHP Arrays">hate</a> php arrays) is easy to read and understand. It just simply echos out each name in the data array. The while version is a little more difficult to parse through, but still readable:<br />
<code><br />
$data = array('Bob', 'Tom', 'Joe', 'Sam', 'Ann');<br />
$i = 0;<br />
while ($i &lt; sizeOf($data)) {<br />
echo $data[$i];<br />
$i++;<br />
}<br />
unset($i);<br />
</code><br />
This, once again, is a trivial example. What if that array contained more arrays or objects that you had to loop through. Now you have to deal with nested for loops, and you have to keep track of multiple count variables. What if I told you there is a better way? A way which brings object oriented programming into the mix:</p>
<h2>Foreach loop</h2>
<p>The foreach loop operates entirely different than the procedural loops. Foreach leverages object oriented principles to carry out iteration. This makes it ideal for iterating on arrays of objects, or containers. For this to work, the container object must implement the iterable interface. For most programming languages, this means the container class must have these methods:</p>
<ul>
<li>mixed current() - returns the current value/object.</li>
<li>scalar key() - returns the current index.</li>
<li>void next() - advances the iterator.</li>
<li>void rewind() - resets the iterator to the beginning.</li>
<li>boolean valid() - returns boolean true if the iterator is on a valid object.</li>
</ul>
<p>All five of these methods are invisible to the programmer using the object. Foreach leverages them to perform iteration. In some languages, the array construct has these functions built in. Consider this code:<br />
<code><br />
$data = array('Bob', 'Tom', 'Joe', 'Sam', 'Ann');<br />
foreach ($data as $name) {<br />
echo $name;<br />
}<br />
</code><br />
Notice that it is even simpler than a for loop. No mucking about with a count variable. Also, for loops <em>require</em> the array's index to be in order. Foreach loop doesn't care.<br />
$data = array(10=&gt;'Bob', 3=&gt;'Tom', 247=&gt;'Joe', 74=&gt;'Sam', 26=&gt;'Ann');<br />
This will work just as well in a foreach loop; it is extraordinarily difficult for a for loop to iterate over this (xtra credit!).<br />
Here are the function calls a foreach loop makes:</p>
<ol>
<li>rewind</li>
<li>valid (break if false)</li>
<li>current</li>
<li>key (optional, only called if used in the body)</li>
<li>next</li>
<li>goto valid</li>
</ol>
<p>The fundamental distinguishing feature of a foreach loop is that valid returns true or false if the pointer is on a valid object. No counting needed; just keep going until there is no more to go through.</p>
<p>The foreach construct looks cleaner and runs quicker than a for loop, especially when dealing with arrays. In all loops, a comparison is run at every single iteration. For arrays, this comparison is most often comparing the size of the array with the current index. More often than not, this is a relatively expensive operation which tends to grow linearly with size, and it is <em>calculated every single time</em>. Whereas the foreach loop checks validity, a very simple, fast operation.<br />
Foreach does fall short when modifying the array it is iterating over. This involves a lot of functional overhead to keep track of the dynamically changing values.</p>
<p>See <a href="http://www.phpbench.com">phpbench.com</a> for more epic php benchmarks!</p>
<h2>tl;dr</h2>
<p>When looping through php arrays:<br />
If you are modifying the array that you are looping through, use a for loop.<br />
Otherwise, use a foreach loop.</p>
<p>The more you know...</p>
<hr />
