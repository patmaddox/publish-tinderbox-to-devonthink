<h1>Publish to DEVONthink</h1>
<p><b>Warning: This is beta software!</b></p>
<p>It does the job fine for me, but is still a bit rough around the edges. If you don’t understand how it’s built, you might get confused. <a href="http://forum.eastgate.com/t/publish-from-tinderbox-to-devonthink/1123">Ask for help on the forum</a> and I’ll do my best to help you out.</p>

<p>You can always find the most up-to-date version on <a href="https://github.com/patmaddox/publish-tinderbox-to-devonthink">GitHub</a>.</p>
<h4>Take notes in Tinderbox. Publish to DEVONthink. Maintain the link.</h4>
<p>Publishing from Tinderbox to DEVONthink is easy, with the code provided in this document. With it, you can:</p>
<ul><li> press one button to publish your notes from Tinderbox to DEVONthink</li>
<li> automatically link the Tinderbox note to the created DEVONthink record</li>
<li> update your DEVONthink records with the press of a button</li>
<li> file the record wherever you like in your DEVONthink database</li>
<li> use the <code>x-devonthink-item://</code> link anywhere you want, confident that it will always work</li></ul>

<h4>Two ways to publish</h4>

<h5>1. The quick-start method (just edit this document!)</h5>
<p>The quickest way to publish from Tinderbox to DEVONthink is to simply use this document.</p>

<ol><li> Add a note and assign it the <code>p_DEVONitem</code> prototype
<li> Run <code>Stamps->Publish note to DEVONthink</code></ol>

<p>If everything goes well, DEVONthink will have a new HTML record and your note will now have its <code>$SourceURL</code> attribute set to the <code>x-devonthink-item://</code> URL. Click it to open the record in DEVONthink.</p>

<h5>2. Publish from existing documents</h5>
<p>Already have a document with a bunch of notes in it? No problem. It’s a little bit more involved than the quick-start method, but it should still only take a few minutes. I have created a Doctor helper to check your document for necessary infrastructure, and to provide instructions on how to set up your document.</p>

<p>Once the doctor check comes back good, run the publish stamp!</p>


<h4>What gets published</h4>
<ul><li> <code>$Name</code></li>
<li> <code>$Tags</code></li>
<li> <code>tinderbox://</code> callback URL</li>
<li> HTML representation of the note, as defined by <code>$HTMLExportTemplate</code> property</li></ul>
<p>If you make changes to your note, you can update the record in DEVONthink simply by publishing the note again.</p>

<h4>Links between notes become DEVONthink links</h4>
<p>One of my main goals with this project is to build a personal <a href="http://wiki.c2.com/?WelcomeVisitors">wiki</a> with Tinderbox – and then take it with me wherever I go. To do that, the pages need to be able to link to one another… which is precisely what this document does.</p>
<p>Assuming that you have published your notes to DEVONthink, any links between notes will automatically be converted to <code>x-devonthink-item://</code> links.</p>
<h5>Performance concerns</h5>
<p>I use <code><a href="http://acrobatfaq.com/atbref7/index/ActionsRules/Operators/FullOperatorList/findquery.html">find()</a></code> to look up all the notes that have been published to DEVONthink. Then I iterate through them and replace the exported paths with the <code>x-devonthink-item://</code> URLs. This is a lot slower than just looking for the links that are in the note itself. But I think that’s the only way to support links that are provided by child notes.</p>


<h4>Upgrading from earlier versions</h4>
<p>Starting with version 0.5.1, upgrading from earlier versions of this tool is a matter of following the Doctor’s instructions.</p>

<h4>Version history</h4>

<h5>0.5.1</h5>
<ul><li> <b>NEW </b>Add Doctor to check a document for necessary infrastructure.</li></ul>

<h5>0.5.0</h5>
<ul><li> <b>NEW</b> Add “Publish all to DEVONthink” action</li>
<li> <b>NEW</b> Add <code>$DTExportTBView</code> to select which view is used in the return link from DEVONthink.</li>
<li> <b>NEW</b> Publish <code>$Tags</code> (<a href="http://forum.eastgate.com/t/publish-from-tinderbox-to-devonthink/1123/30">thank you @jmm</a>!)</li>
<li> <b>FIX</b> First-time published notes get DEVONthink links (you don’t need to publish new notes twice anymore)</li>
<li> <b>FIX</b> Updated “Sectional HTML page” template to determine relative header level based on outline depth</li>
<li> <b>FIX</b> Use <code>$DTExportSource</code> to track exported source rather than <code>$MyString</code></li>
<li> <i>Upgrading from 0.4.x to 0.5.0
</i><ul><li> If you’ve only added notes and haven’t changed the document structure (e.g. templates, prototypes) it may be easiest to copy your existing notes into this updated document.</li>
<li> Add a new String user attribute called <code>$DTExportSource</code>.</li>
<li> Add a new String user attribute called <code>$DTExportTBView</code>.
<ul><li> Suggested: <code>chart;outline;map;timeline</code></li>
<li> Default: Your choice for default view</li></ul></li>
<li> Add “TB Return URL” template</li>
<li> Replace v0.4.0 “HTML page” template with v0.5.0 “Sectional HTML page” template</li>
<li> Update “p_DEVONitem” prototype:
<ul><li> Change template to “Templates/Sectional HTML page”</li>
<li> Add <code>$DTExportTBView</code> to key attributes</li></ul></li>
<li> Replace v0.4.0 “Stamps” container with v0.5.0 “Actions” container</li>
<li> Replace v0.4.0 “Applescripts” container with v0.5.0 “Applescripts” container </li>
<li> Update “Publish to DEVONthink” stamp:
<ul><li> <code>action($Text("/Actions/Publish all to DEVONthink"));</code></li></ul></li>
<li> Optional: Add a stamp to publish a single note at a time:
<ul><li> <code>action($Text("/Actions/Publish note to DEVONthink"));</code></li></ul></li></ul></li></ul>

<h5>0.4.0</h5>
<ul><li> <b>NEW</b> Add <code>$PublishToDEVONthink</code> attribute for explicit publish selection</li>
<li> <b>NEW</b> Set DEVONthink’s URL field to Tinderbox note link (<a href="http://forum.eastgate.com/t/publish-from-tinderbox-to-devonthink/1123/21">forum request</a>)</li>
<li> <b>FIX</b> Linking to and from notes in containers</li>
<li> <b>FIX</b> Link issue when publishing from aliases</li>
<li> <b>FIX</b> Respect DEVONthink’s import destination configuration (<a href="http://forum.eastgate.com/t/publish-from-tinderbox-to-devonthink/1123/12">forum report</a>)</li></ul>

<h5>0.3.0</h5>
<ul><li> Replace all exported path links with DEVONthink links when possible – this supports links generated by agents, <code><a href="http://acrobatfaq.com/atbref7/index/ExportCodes/ExportCodes-FullListing/includeitemgrouptemplate.html">&Hat;include&Hat;</a></code>, <code><a href="http://acrobatfaq.com/atbref7/index/ExportCodes/ExportCodes-FullListing/linkToitemdatacssclass.html">&Hat;linkTo&Hat;</a></code>, etc</li></ul>

<h5>0.2.0</h5>
<ul><li> Replace path-based text links with DEVONthink links – e.g. <code>&lt;a href="Publish_to_DEVONthink.html"&gt;Check it out!&lt;/a&gt;</code> becomes <code>&lt;a href="x-devonthink-item://919FE3FC-88CD-4E6A-804A-F4DDA4F2C114"&gt;Check it out!&lt;/a&gt;</code></li></ul>

<h5>0.1.0</h5>
<ul><li> Basic publish to DEVONthink functionality</li></ul>


<h4>Upcoming Development</h4>
<ul><li> Delete from DEVONthink</li>
<li> Create placeholder records for linked notes</li>
<li> Tooling to update an existing document
<ul><li> Stamp to update components (actions, applescripts, prototypes)</li></ul></li></ul>
