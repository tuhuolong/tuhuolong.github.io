---
layout: post
title: Html5 File Upload with Progress
date: 2012-06-05 16:09:00
categories: [WebKit, 浏览器]
tags: [upload, file, html5, xmlhttprequest, javascript, server]
---
Html5 finally solves an age old problem of being able to upload files while also showing the upload progress. Today most websites
 use Flash Player to achieve this functionality. Some websites continue to use the Html `<form> with enctype=multipart/form-data,` but
 with modification on the server side to enable showing users the upload progress. Essentially, what you need to do is hook into the server’s byte stream while it is receiving a file so you know how many bytes you’ve received and somehow convey that information
 back to the client browser, while it is still in the process of uploading the file. This solution works extremely well and is not fraught with the issues Flash Player causes (especially for large files). However it is fairly complicated and not for the faint
 of heart because you are essentially taking over the entire server side processing (when you tap into the byte stream) and that includes implementing the `multipart/form-data` protocol
 on the server side, along with a bunch of other things.##Uploading files using Html5
The XMLHttpRequest object has gotten a facelift in the Html5 specifications. Specifically the[XMLHttpRequest
 Level 2](http://www.w3.org/TR/XMLHttpRequest2/) specification (currently the latest version) that has included the following new features:1. Handling of byte streams such as File, Blob and FormData objects for uploading and downloading
2. Progress events during uploading and downloading
3. Cross-origin requests
4. Allow making anonymous request - that is not send HTTP Referer
5. The ability to set a Timeout for the Request

In this post we’ll look more closely at #1 and #2. In particular, uploading of files using XMLHttpRequest and providing upload progress information to the end-user. Note that this solution **does not require any change to the server side**, at
 least insofar as handling the multipart/form-data protocol. So existing server side logic should remain unchanged, which makes adapting this technology that much easier.[]()
![Html5FileUpload1](http://www.matlus.com/content/uploads/2011/01/Html5FileUpload1_6023e7c1-a2a8-43ee-bb13-c2439888adf9.jpg "Html5FileUpload1") 
###Figure 1: Showing the File Upload screen -Upload just started
 
[]()![Html5FileUpload2](http://www.matlus.com/content/uploads/2011/01/Html5FileUpload2_f8ce435b-691a-4102-97c4-47cf159436e3.jpg "Html5FileUpload2")###Figure 2: Showing the File Upload screen with upload completed.
Notice in the images above the following pieces of information made available to the user:1. Information on the file that has been selected such as- File Name
- File Size
- Mime Type


2. A Progress bar with percent complete
3. The upload speed or upload bandwidth
4. The approximate time remaining
5. The bytes uploaded thus far
6. A response from the server side (the organge box)

Item #6 above may not sound important, but in fact is quite important in this context because, remember that the user is not
 navigating away from this page as she normally would after submitting an html form. Because we're using XMLHttpRequest, the uploading is happening in the background. The page the user is on remains intact. Which is a nice feature to have if your business process
 can work with it.##Html5 Progress Event
As per the Html5 [Progress
 Events](http://www.w3.org/TR/progress-events/) spec, the Html5 progress event provides the following information relevant to this conversation1. `total` - Total bytes being transferred
2. `loaded` - Bytes uploaded thus far
3. `lengthComputable` - Specifies if the total size of the data/file being uploaded is known

You should notice that we need to use these two pieces of information to calculate (figure out) all of the other information
 we're displaying to the user. It is fairly simple to figure out all of the other information given the above two pieces of information but it does involve quite a bit of extra coding and setting up a timer.##Html5 Progress Event - What it should have been
Considering that those who go down the route of providing upload progress information to the end user will require to show all
 of the other information to the end user as well, the Html5 Progress Event specifications should have accounted for this need as well, as it would be fairly simple for browser vendors to provide these additional pieces of information each time the progress
 event is raised. So I propose that the progress event should be modified to the following:1. `total` - Total bytes being transferred
2. `loaded` - Bytes uploaded thus far
3. `lengthComputable` - Specifies if the total size of the data/file being uploaded is known
4. `transferSpeed` as a `long`
5. `timeRemaining` as a JavaScript `Date` object

##Html5 Upload using XMLHttpRequest
I've provided a [Uploading
 files using Html5 with Progress indication](http://exposureroom.biz/upload.aspx) demo for those who want to jump right to it. All of the JavaScript code required
 is in the page. However, because the demo is a real world demo there are a number of CSS styles in use, and the layout of the page (the Html) is fairly complex. In this article we work with a minimalistic version of the Html and JavaScript so as to keep things
 simple and understandable.###Browser Support for this Feature
At the time of this writing only the following browsers support this feature1. Firefox 4.0 beta 6
2. Chrome 6
3. Safari 5.02

IE 9 Beta and Opera 10.62 do not support this feature##Let's get started
The entire code listing for the minimalistic but functional implementation can be found in [Code
 Listing 6](http://www.matlus.com/html5-file-upload-with-progress/#codeListing6). Each aspect of the minimalistic Html/JavaScript is explained in detail in the rest of this article. Below, is
 a very simple Html form. This Html is no different then the Html you'd use to do a regular Html/HTTP file upload you'd use today (Html4 and pretty much any previous version of Html).[]()###The Html Portion
	<!DOCTYPE html>
	<html>
	<head>
	    <title>Upload Files using XMLHttpRequest - Minimal</title>
	</head>
	<body>
	  <form id="form1" enctype="multipart/form-data" method="post" action="Upload.aspx">
	    <div class="row">
	      <label for="fileToUpload">Select a File to Upload</label><br />
	      <input type="file" name="fileToUpload" id="fileToUpload" onchange="fileSelected();"/>
	    </div>
	    <div id="fileName"></div>
	    <div id="fileSize"></div>
	    <div id="fileType"></div>
	    <div class="row">
	      <input type="button" onclick="uploadFile()" value="Upload" />
	    </div>
	    <div id="progressNumber"></div>
	  </form>
	</body>
	</html>
###Code Listing 1: The bare minimum Html Page
Notice that the `<input
 type="file"/>` element has the `**onchange**` event
 assigned to a JavaScript method called `**fileSelected()**`.
 So essentially, each time someone selects a file by browsing to a file on their local system, this event is raised. The `**fileSelected()**`JavaScript
 method looks like the following.[]()###The fileSelected() JavaScript method
	function fileSelected() {
	  var file = document.getElementById('fileToUpload').files[0];
	  if (file) {
	    var fileSize = 0;
	    if (file.size > 1024 * 1024)
	      fileSize = (Math.round(file.size * 100 / (1024 * 1024)) / 100).toString() + 'MB';
	    else
	      fileSize = (Math.round(file.size * 100 / 1024) / 100).toString() + 'KB';
	          
	    document.getElementById('fileName').innerHTML = 'Name: ' + file.name;
	    document.getElementById('fileSize').innerHTML = 'Size: ' + fileSize;
	    document.getElementById('fileType').innerHTML = 'Type: ' + file.type;
	  }
	}
###Code Listing 2: The fileSelected() JavaScript method
The first line of code in the method is doing something you've not seen or done before. Essentially, once you have a reference
 to the `<input type="file"/>` element,
 you have access to what is called a `**FileList**` object
 which is new in Html5 and is part of the [File
 API](http://www.w3.org/TR/FileAPI/)specifications of Html 5. A `FileList` object
 is a collection of files. More specifically, it is a collection of `**File**` objects.
 A `File` object
 has the following properties.1. `name` - The name of the file (excluding any path)
2. `type` - The MIME type of the file (in lower case)
3. `size` - The size of the file in bytes

This is already getting interesting isn't it? We have access to this information on the client side! In fact the File API gives
 us access to the contents of the file (the file stream or bytes) on the client side too, using the `**FileReader**` object.
 We won't be dealing with the `FileReader` in
 this example, so I'm not going to overwhelm you with yet another new object. We have a couple of new objects to discuss yet. You could use the information the `File` object
 provides you to say, prevent users from uploading files larger than the certain size. Or you could use the`type` property
 (MIME type) to figure out the type of file the user is attempting to upload and change the behavior of your form (on the client side of course). The above JavaScript method populates the grey box with grey text (in [Figures
 1 & 2](http://www.matlus.com/html5-file-upload-with-progress/#figure2) above) with the information the `File`object
 provides us about the selected file. The file size is in bytes, so there is logic in the method to convert the size into a human readable form, such as 15.65MB instead of 15795748864 bytes. After the user has selected a file she'll want to upload the file
 and so she's going to click on the Upload button.
 Notice in the code listing for the Html form ([Code
 Listing 1](http://www.matlus.com/html5-file-upload-with-progress/#codeListing1)) that the Upload button has its `onclick` event
 assigned to the `**uploadFile()**` JavaScript
 method.[]()###uploadFile() JavaScript method
	function uploadFile() {
	  var xhr = new XMLHttpRequest();
	  var fd = document.getElementById('form1').getFormData();
	
	  /* event listners */
	  xhr.upload.addEventListener("progress", uploadProgress, false);
	  xhr.addEventListener("load", uploadComplete, false);
	  xhr.addEventListener("error", uploadFailed, false);
	  xhr.addEventListener("abort", uploadCanceled, false);
	  /* Be sure to change the url below to the url of your upload server side script */
	  xhr.open("POST", "UploadMinimal.aspx");
	  xhr.send(fd);
	}
###Code Listing 3: The uploadFile() JavaScript method
In the second line of this method you'll see another object (`[FormData](http://www.w3.org/TR/2010/WD-XMLHttpRequest2-20100907/#the-formdata-interface)`)
 that we've never seen nor used before. You're not really seeing it, but we do get a reference to it using the`**getFormData()**` method
 of the `<form>` element.
 A `FormData` object
 is similar to a dictionary, a collection of name/value pairs. Where the name part is the name of a form field (as defined in the Html page) and the value is the value of this field. The value part can be a string, number or even a `File` object,
 as you can see in [Code
 listing 4](http://www.matlus.com/html5-file-upload-with-progress/#codeListing4). So when you call the`getFormData()` method
 on the form you get a reference to a FormData object that holds a collection of name/value pairs of that form. You can use this reference as the information you want to send to the server side script. This makes it really simple to submit the entire form.
 Now you could create an instance of a `FormData` object
 manually and send that to the server or you could add additional information to the `FormData` before
 sending it to the server side script.###A note about FormData
Note that as if this writing Chrome 6 supports uploading files using the new API but does not support the `getFormDate()` method. It does however, support the `FormData` object.
 So the full code listing of the bare minimal code in [Code Listing 6](http://www.matlus.com/html5-file-upload-with-progress/#codeListing6) manually creates a `FormData`instance
 as discussed below. Another way to instantiate an instance of FormData is to pass in a reference to a form element. like so: `var fd = new FormData(document.getElementById('form1')); `This constructor has
 the advantage of populating the `FormData` instance with all the form's fields, instead of having to do it manually (similar to the `getFormData` method
 of the form element discussed earlier. At the time of this writing, on Firefox supports this method.[Code listing 4](http://www.matlus.com/html5-file-upload-with-progress/#codeListing4),
 below shows you an example of how you would create an instance of a`FormData` object,
 assign some arbitrary fields and their values, including a reference to a`File` object
 (the file selected by the user using the `<&tl;file type="file">` element.[]()###Manually Creating a FormData object
	var fd = new FormData();
	fd.append("author", "Shiv Kumar");
	fd.append("name", "Html 5 File API/FormData");
	fd.append("fileToUpload", document.getElementById('fileToUpload').files[0]);
###Code Listing 4: Manually creating a FormData instance
Going back to [Code
 Listing 3](http://www.matlus.com/html5-file-upload-with-progress/#codeListing3), you'll notice we've subscribed to a few events of the XMLHttpRequest object. In particular, pay attention to
 the first line after the event listners comment in the code. `xhr.**upload**.addEventListener("progress", uploadProgress, false);` The
 progress event we subscribe to is not that of the XMLHttpRequest instance, but rather the `**upload**` property
 of the XMLHttpRequest instance, which is an [XMLHttpRequestUpload](http://dev.w3.org/2006/webapi/XMLHttpRequest-2/#xmlhttprequestupload) type
 that is really an event target that has a `**progress**`event
 we can subscribe to in order to get progress information on the upload that is taking place. We don't have to worry about this object. Just remember, when using XMLHttpRequest for uploading data to the server, you must subscribe to its `**upload**` property's `**progress**`event.###A note about Progress Events
The XMLHttpRequest object does have a progress event of it's own and you'll subscribe to that event when you download data from the server (which we are not covering here).[]()###The Event handler implementations
	function uploadProgress(evt) {
	  if (evt.lengthComputable) {
	    var percentComplete = Math.round(evt.loaded * 100 / evt.total);
	    document.getElementById('progressNumber').innerHTML = percentComplete.toString() + '%';
	  }
	  else {
	    document.getElementById('progressNumber').innerHTML = 'unable to compute';
	  }
	}
	
	function uploadComplete(evt) {
	  /* This event is raised when the server send back a response */
	  alert(evt.target.responseText);
	}
	
	function uploadFailed(evt) {
	  alert("There was an error attempting to upload the file.");
	}
	
	function uploadCanceled(evt) {
	  alert("The upload has been canceled by the user or the browser dropped the connection.");
	}  
	
###Code Listing 5: The implementation of the various event handlers
The code listing above is pretty self explanatory so there is nothing much to say here.##The Minimalistic Solution
The Code listing below is the entire Html page including the JavaScript and Html required to get a bear minimum File upload
 with progress indicator working. I've intentionally kept it simple so if you want to do your own layout and information display you can start with this and expand it. Html5 also introduces a `**progress**` element
 that can be used to show progress. The `progress`element
 has `max` and `value` and
 so it makes it really simple to show progress. However, at the time of this writing, only Chrome 6 supports this element so I've not used it in the minimalistic solution.###Change the Url to the server side script
Be sure to change the url to point to the url of your server side script that handles file uploads. In the code listing below, it is **UploadMinimal.aspx** in the `uploadFile()`method: `xhr.open("POST",
 "UploadMinimal.aspx");`[]()###Minimalistic Html and JavaScript
	<!DOCTYPE html>
	<html>
	<head>
	    <title>Upload Files using XMLHttpRequest - Minimal</title>
	
	    <script type="text/javascript">
	      function fileSelected() {
	        var file = document.getElementById('fileToUpload').files[0];
	        if (file) {
	          var fileSize = 0;
	          if (file.size > 1024 * 1024)
	            fileSize = (Math.round(file.size * 100 / (1024 * 1024)) / 100).toString() + 'MB';
	          else
	            fileSize = (Math.round(file.size * 100 / 1024) / 100).toString() + 'KB';
	
	          document.getElementById('fileName').innerHTML = 'Name: ' + file.name;
	          document.getElementById('fileSize').innerHTML = 'Size: ' + fileSize;
	          document.getElementById('fileType').innerHTML = 'Type: ' + file.type;
	        }
	      }
	
	      function uploadFile() {
	        var fd = new FormData();
	        fd.append("fileToUpload", document.getElementById('fileToUpload').files[0]);
	        var xhr = new XMLHttpRequest();
	        xhr.upload.addEventListener("progress", uploadProgress, false);
	        xhr.addEventListener("load", uploadComplete, false);
	        xhr.addEventListener("error", uploadFailed, false);
	        xhr.addEventListener("abort", uploadCanceled, false);
	        xhr.open("POST", "UploadMinimal.aspx");
	        xhr.send(fd);
	      }
	
	      function uploadProgress(evt) {
	        if (evt.lengthComputable) {
	          var percentComplete = Math.round(evt.loaded * 100 / evt.total);
	          document.getElementById('progressNumber').innerHTML = percentComplete.toString() + '%';
	        }
	        else {
	          document.getElementById('progressNumber').innerHTML = 'unable to compute';
	        }
	      }
	
	      function uploadComplete(evt) {
	        /* This event is raised when the server send back a response */
	        alert(evt.target.responseText);
	      }
	
	      function uploadFailed(evt) {
	        alert("There was an error attempting to upload the file.");
	      }
	
	      function uploadCanceled(evt) {
	        alert("The upload has been canceled by the user or the browser dropped the connection.");
	      }
	    </script>
	</head>
	<body>
	  <form id="form1" enctype="multipart/form-data" method="post" action="Upload.aspx">
	    <div class="row">
	      <label for="fileToUpload">Select a File to Upload</label><br />
	      <input type="file" name="fileToUpload" id="fileToUpload" onchange="fileSelected();"/>
	    </div>
	    <div id="fileName"></div>
	    <div id="fileSize"></div>
	    <div id="fileType"></div>
	    <div class="row">
	      <input type="button" onclick="uploadFile()" value="Upload" />
	    </div>
	    <div id="progressNumber"></div>
	  </form>
	</body>
	</html>
###Code Listing 6: The Complete but minimalistic code listing
Well, that pretty much covers the minimalistic version of the new Html5 feature. Getting at the other information that you see
 in [Figure2](http://www.matlus.com/html5-file-upload-with-progress/#figure2) is
 mathematical really. It is quite a bit of extra work to not only get at the information, but to display and animate etc. For instance to get the rate of upload (the upload speed). We do the following: 1. in the `uploadProgress(evt)` event,
 should store the `evt.loaded` and `evt.total` in
 global variables. 2. We set up a timer event to fire every second. 3. In the timer callback we get the difference of the number of bytes transfered between now and the last time the callback was called (so 1 second ago). 4. That gives us the number of bytes
 per second. Which is the upload speed. Note that in the demo I set up the timer to fire every 500 milliseconds to get more granularity. As a result the difference between the number of bytes is doubled since the number of bytes is really the number of bytes
 transferred in half a second. I won't go into how to find the time remaining, but it's all there in the demo. Do a "view source" to see the code in the demo's html page. I hope you found this article helpful.