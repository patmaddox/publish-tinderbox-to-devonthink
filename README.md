<h1>Publish to DEVONthink</h1>
<p><b>Warning: This is beta software!</b></p>
<p>It does the job fine for me, but is still a bit rough around the edges. If you don’t understand how it’s built, you might get confused. <a href="http://forum.eastgate.com/t/publish-from-tinderbox-to-devonthink/1123">Ask for help on the forum</a> and I’ll do my best to help you out.</p>
<hr/>
<p>I love taking notes with <a href="http://www.eastgate.com/Tinderbox/">Tinderbox</a>, don’t you? It gives me a nice structured way of looking at information, and then I can export it to take it with me or share it with others.</p>
<p>Ever since <a href="http://www.devontechnologies.com/products/devonthink/devonthink-to-go.html">DEVONthink To Go</a> was released, I’ve wanted to use it to take my Tinderbox notes with me. So I run <code>File->Export->as HTML</code>, index the folder in <a href="http://www.devontechnologies.com/products/devonthink/overview.html">DEVONthink</a>, and off I go, right? Not quite…</p>
<p>There’s one key problem that prevents me from effectively taking my Tinderbox notes on the road:</p>
<p><b>Indexed folders in DEVONthink kind of suck, unless that’s what you really want.</b></p>
<p>You’ve got a giant <a href="http://www.sonnysoftware.com">Bookends</a> library that you want to search with DEVONthink? Great, index the Bookends folder. Just don’t bother using DEVONthink’s <i>Copy item link</i> feature. As soon as you move or rename a file on disk, the <code>x-devonthink-item://</code> link breaks. And I love those links! You can add them to your task manager and calendar, and they <i>always</i> take you to the record you want – <i>if</i> it’s an imported record. So how do you make that work with Tinderbox?</p>
<p><b>Take notes in Tinderbox. Publish to DEVONthink. Maintain the link.</b></p>
<p>Publishing from Tinderbox to DEVONthink is easy, with the code provided in this document. With it, you can:</p>
<ul><li> press one button to publish a note from Tinderbox to DEVONthink</li>
<li> automatically link the Tinderbox note to the created DEVONthink record</li>
<li> update your DEVONthink records with the press of a button</li>
<li> file the record wherever you like in your DEVONthink database</li>
<li> use the <code>x-devonthink-item://</code> link anywhere you want, confident that it will always work</li></ul>
<h2>Two ways to publish</h2>

<h3>1. The quick-start method (just edit this document!)</h3>
<p>The quickest way to publish from Tinderbox to DEVONthink is to simply use this document. Add a note and run <code>Stamps->Publish to DEVONthink</code>. If everything goes well, DEVONthink will have a new HTML record and your note will now have its <code>$SourceURL</code> attribute set to the <code>x-devonthink-item://</code> URL. Click it to open the record in DEVONthink.</p>

<h3>2. Publish from existing documents</h3>
<p>Already have a document with a bunch of notes in it? No problem. It’s a little bit more involved than the quick-start method, but it should still only take a few minutes. Here’s what you do:</p>

<ol><li> Make sure you have an <a href="http://acrobatfaq.com/atbref7/index/MiscUserInterfaceAspects/Built-inexportTemplates.html">HTML template</a> defined (you can use <code>File->Built-In Templates->HTML</code> if you don’t have any)</li>
<li> Add the <code>Code</code> prototype: <code>File->Built-In Prototypes->Code</code></li>
<li> Copy the <code>Stamps</code> container into your document</li>
<li> Copy the <code>Applescripts</code> container into your document</li>
<li> Create a stamp with the following action code: <code>action($Text("/Stamps/Publish to DEVONthink"))</code></li></ol>

<p>Run the stamp on your note!</p>


<h2>What gets published</h2>
<ul><li> <code>$Name</code></li>
<li> <code>$URL</code></li>
<li> HTML representation of the note, as defined by <code>$HTMLExportTemplate</code> property</li></ul>
<p>If you make changes to your note, you can update the record in DEVONthink simply by publishing the note again.</p>

<h2>Links between notes become DEVONthink links</h2>
<p>One of my main goals with this project is to build a personal <a href="http://wiki.c2.com/?WelcomeVisitors">wiki</a> with Tinderbox – and then take it with me wherever I go. To do that, the pages need to be able to link to one another… which is precisely what this document does.</p>
<p>Assuming that you have published your notes to DEVONthink, any links will automatically be converted to <code>x-devonthink-item://</code> links.</p>
<p><b>For this to work, you may need to publish your notes twice.</b></p>
<p>The first pass imports the note into DEVONthink. This sets <code>$SourceURL</code> on the note. Before a note has a <code>$SourceURL</code>, any incoming links will not work in DEVONthink. Here’s an example:</p>
<ul><li> <b>Note A</b> links to <b>Note B</b>. Neither has been published to DEVONthink.</li>
<li> Publish <b>Note A</b>. The link to <b>Note B</b> will not be converted to a DEVONthink link, because <b>Note B</b> has not been published and does not have a <code>$SourceURL</code></li>
<li> Publish <b>Note B</b>. It now has a <code>$SourceURL</code>. The published DEVONthink record for <b>Note A</b> still has a broken link, because it was published before <b>Note B</b>.</li>
<li> Publish <b>Note A</b> again. It picks up the <code>$SourceURL</code> from <b>Note B</b>, and the published DEVONthink record has a working <code>x-devonthink-item://</code> link</li></ul>
<p><b>The simplest solution is to publish all your notes twice each time you add or link new notes.</b></p>
<p>I hope to improve this functionality in the future to make it faster and more automatic.</p>
<h3>Possible issues</h3>
<p>There are probably some edge cases, and for now it’s best to only publish top-level notes with distinct names. But it’s working well for me so far…</p>

<h3>Implementation note</h3>
<p>To get this working quickly, and to minimize the number of steps required to make this work for existing documents, the <code>x-devonthink-item://</code> linking functionality uses <code>$MyString</code> as a temporary storage area. If you have agents that suddenly go out of whack, it may be because they’re relying on <code>$MyString</code>. I’ll see what I can do to avoid using it in the future.</p>

<h3>Performance concerns</h3>
<p>I use <code><a href="http://acrobatfaq.com/atbref7/index/ActionsRules/Operators/FullOperatorList/findquery.html">find()</a></code> to look up all the notes that have been published to DEVONthink. Then I iterate through them and replace the exported paths with the <code>x-devonthink-item://</code> URLs. This is a lot slower than just looking for the links that are in the note itself. But I think that’s the only way to support links that are provided by child notes.</p>


<h2>Version history</h2>

<h3>0.3.0</h3>
<ul><li> Replace all exported path links with DEVONthink links when possible – this supports links generated by agents, <code><a href="http://acrobatfaq.com/atbref7/index/ExportCodes/ExportCodes-FullListing/includeitemgrouptemplate.html">&Hat;include&Hat;</a></code>, <code><a href="http://acrobatfaq.com/atbref7/index/ExportCodes/ExportCodes-FullListing/linkToitemdatacssclass.html">&Hat;linkTo&Hat;</a></code>, etc</li></ul>

<h3>0.2.0</h3>
<ul><li> Replace path-based text links with DEVONthink links – e.g. <code>&lt;a href="Publish_to_DEVONthink.html"&gt;Check it out!&lt;/a&gt;</code> becomes <code>&lt;a href="x-devonthink-item://919FE3FC-88CD-4E6A-804A-F4DDA4F2C114"&gt;Check it out!&lt;/a&gt;</code></li></ul>

<h3>0.1.0</h3>
<ul><li> Basic publish to DEVONthink functionality</li></ul>
