---
layout: post
title: Firefox, Xdebug, Notepad++, and You!
date: 2015-02-16 15:21:12.000000000 -05:00
categories:
- PHP
tags:
- Debugging
- HowTo
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
<p>Picture this: It's a dark and stormy Friday afternoon when all of the sudden, the PHP code that you've worked tirelessly on no longer works correctly. For some reason, the program is taking correct values and outputting junk.  Unfortunately, due to complex requirements, and tight schedule, you didn't have time to put a verbose logging system into the program, and you're stuck var_dumping to the webpage.<br />
Has anyone experienced this or something like it?  I have, and I swore to put an end to the echoing madness!<!--more--></p>
<p>After scouring the interwebz for answers, I found this PHP extension called <a href="http://xdebug.org/download.php">Xdebug</a>. Xdebug is a PHP extension designed to assist in debugging and profiling PHP code.  It comes packaged with breakpoints, variable peeking and watching.  It even supports debugging remotely.  "This is perfect!", I thought, "I can install this and debug server code on my desktop!"  Those of you who know Xdebug have probably already tried this, and are thinking "No, Wass.  Don't bother. We've all tried it, and it's just too much of a pain to get around the port forwarding and the firewall."  However, I tried anyway.  And guess what??</p>
<p><a href="https://werdsofthewass.files.wordpress.com/2015/03/nppoutput.png"><img src="assets/nppoutput.png?w=287" alt="nppoutput" width="287" height="300" class="alignnone size-medium wp-image-36" /></a><br />
<strong><em>I SUCCEEDED!</em></strong></p>
<p>And now, I shall share my knowledge and my setup:<br />
So, basically, I've connected Xdebug on the server to Notepad++ on the desktop using ssh remote port forwarding.  <a href="http://csce.uark.edu/~kal/info/private/ssh/ch09_02.htm">This article</a> goes in depth about port forwarding, and the difference between the local and remote variants.  Using remote forwarding, I've linked port 9000 on the server with port 9000 on the client tunneling through the ssh port 22; this entirely bypasses the RIS Firewall and its address translation.  I use PuTTY to tunnel the port, but any SSH client with remote port forwarding will do the trick.<br />
Here is the ssh command:<br />
<code>ssh -R 9000:localhost:9000 ServerName</code><br />
Here is a screenshot of the tunnel configuration:<br />
<a href="https://werdsofthewass.files.wordpress.com/2015/03/puttycfg.png"><img src="assets/puttycfg.png?w=300" alt="puttycfg" width="300" height="266" class="alignnone size-medium wp-image-37" /></a><br />
In essence, what this does is it listens on the server's port 9000 and redirects any traffic on that port to localhost:9000.  It's a little confusing, but in this context localhost refers to the <em>client</em>, not the server.</p>
<hr />
<p>###Server Side###<br />
Now, let's talk server requirements:  Gotta get Xdebug.  <code>yum install php-pecl-xdebug</code> should do the trick, or you can just pecl it.  Go <a href="http://xdebug.org/docs/install">here</a> for more info.  Since Xdebug is a PHP Module, you can verify that it is correctly installed by running <code>php -m</code> on your server terminal.  If Xdebug appears in <em>both</em> &#091;PHP Modules&#093; and &#091;Zend Modules&#093; You're in good shape!  Next step is to verify that Xdebug is set up and working in your php.ini file.<br />
Make sure these lines are in there:</p>
<p>xdebug.remote_enable=1<br />
xdebug.remote_handler="dbgp"<br />
xdebug.remote_host=127.0.0.1<br />
xdebug.remote_port=9000<br />
xdebug.remote_mode=req<br />
xdebug.idekey=default</p>
<p>There are more settings that can enable and configure more features of Xdebug, but these are the required ones to get the remote debugging working.  Notice that the remote host is the loopback address. This is intentional, since we are tunneling the connection through SSH, and not actually connecting to the remote host directly.</p>
<hr />
<p>###Client Side###<br />
I use Notepad++ as the debugging client, but other text editors/IDEs will work too.  Click <a href="http://xdebug.org/docs/remote">here</a> to see more compatible editors.  So long as the editor has DBGP capability, it should work.</p>
<p>First, the client needs to have access to the files that you will be debugging.  This can be accomplished in a few ways: Samba share, WinSCP, FTP, are all viable methods.  Even straight copying the files from the server to the client will work.  The main requirement is having both a remote path (path that the server understands) and a local path (path that the client understands) to the files you wish to debug.  If your local and remote files are copied rather than linked, make sure they are synchronized when you debug.<br />
To accomplish this, I use <a href="http://ashkulz.github.io/NppFTP/">NppFTP</a>.  This is a Notepad++ plugin that offers similar functionality to WinSCP, just built right into Notepad++.  I have configured NppFTP to cache server files in a local directory, then use that cache as the required local path.  This keeps the two sets of files synched with each other.</p>
<p>Secondly, the client needs to have DBGP capability.  Click <a href="http://xdebug.org/docs/remote">here</a> to see more compatible editors.  This is Xdebug's remote communication protocol.  Notepad++ has a <a href="http://sourceforge.net/projects/npp-plugins/files/DBGP%20Plugin/">DBGp plugin</a> which understands the protocol, and allows communication between it and Xdebug on the server.  Once you have installed your supported editor/plugin, go to the configuration and set these properties:<br />
*    Set the Remote Server IP to 127.0.0.1<br />
*    Set the IDE Key to match what you set on the server (in this case: default)<br />
*    Set the remote path and local paths to match whatever your synchronization software is configured.<br />
Remember, we are using SSH to tunnel the connection.  That's why we are pointing the client at itself.</p>
<hr />
<p>###All Together Now!###<br />
In order to start the debugging session, make sure the DBGP client in the editor is listening, and open a web browser to the page you want to debug.  Next add this parameter: <code>XDEBUG_SESSION_START=idekey</code> to the webpage URL.  Remember to change idekey to whatever you set the IDEKey on the server to.  Alternatively, you can install <a href="https://addons.mozilla.org/en-US/firefox/addon/the-easiest-xdebug/">The Easiest Xdebug</a> extension for Firefox, and it'll handle everything for you.</p>
<p>Once everything is said and done, Here's how it all works:<br />
1.    SSH -R sets up remote forwarding from server:9000 to client:9000.<br />
2.    Browser sends the <code>XDEBUG_SESSION_START</code> parameter to server:80.<br />
3.    The webserver detects the parameter and starts the Xdebug server.<br />
4.    Xdebug server sends a message to server:9000.<br />
5.    SSH captures the message on server:9000 and forwards it to client:9000.<br />
6.    DBGP client picks up the message on client:9000 and processes it.<br />
7.    DBGP client sends a response back to client:9000.<br />
8.    SSH captures the message on client:9000 and forwards it to server:9000.<br />
9.    Xdebug server picks up the message on server:9000 and processes it.</p>
<hr />
<p>###Conclusion###<br />
So Xdebug is pretty awesome!  It can turn Notepad++ into a fairly powerful debugging engine, complete with breakpoints, and variable peeking and watching.  Unfortunately, so far, I've only figured out how to configure it with one developer per server.  I bet if you manipulate the ports, you can get multiple developers on one server or multiple servers for one developer.  Once I get through that, I'll write a follow up.  Stay Tuned!</p>
