---
layout: post
title: Uploading local images using AngularJS
date: 2016-02-09 00:00:00

---

This post will show you how to upload local images using Angular and ng-upload. A few notes before we dive into the technical details: the code snippets below were added to my existing code, this was part of a MEAN stack app (Angular, Express/Node, MongoDB).
This is recommended for users who have some minimal experience with AngularJS controllers.

Ng-upload will need to be installed: <a href="https://www.npmjs.com/package/ng-upload">ng-upload</a>
<br>
<br>
HTML code that allows a user to upload a local image from their computer:  

{% highlight HTML %}


  <h4>Upload on file select</h4>
  <div>          
     <button type="file" ngf-select="uploadFiles($file, $invalidFiles)"
        accept="image/*" style="color:black; border-radius: 5px;" ngf-max-height="10000" ngf-max-size="20MB">
        Select File
    </button>
  </div>
{% endhighlight %}
Note: The attributes "ngf-max-height" "ngf-max-size" allow you to set limits on image height and size. Once the pic is selected, "ngf-select" calls the function "uploadFiles" with the arguments (the user selected file and a potential invalid file message).

<br>
<br>
To display the results of the file upload, include the following in your HTML file:
<br>
{% highlight HTML %}

 Uploaded File:
  <div style="font:smaller">{{f.name}} {{errFile.name}} {{errFile.$error}} {{errFile.$errorParam}}
    <span class="progress" ng-show="f.progress >= 0">
      <div style="width:{{f.progress}}%"
      ng-bind="f.progress + '%'"></div>
    </span>
  </div>
  {{errorMsg}}<br><br>

  <!-- Display the the image returned from the server -->
  <img ng-src="data:image/jpg;base64,{{image}}">
  <br>

{% endhighlight %}

<br>
Angular controller code for the above HTML:


{% highlight javascript %}


.controller('someControllerName', ['$scope', 'Upload', '$timeout', function($scope, Upload, $timeout) {

// Use to convert an arraybuffer to a base 64 string 

function _arrayBufferToBase64( buffer ) {
    var binary = '';
    var bytes = new Uint8Array( buffer );
    var len = bytes.byteLength;
    for (var i = 0; i < len; i++) {
        binary += String.fromCharCode( bytes[ i ] );
    }
  return window.btoa( binary );
}

// Function that calls ng-upload's Upload function
$scope.uploadFiles = function(file, errFiles) {
  $scope.f = file;
  $scope.errFile = errFiles && errFiles[0];
   if (file) {
      file.upload = Upload.upload({
        url: '/images',
        data: { file: file },
      });
     file.upload.then(function(response) {
  
        // response is an array buffer that needs to be converted to a base64 encoded string in
        // order to used by the img element ex:<img ng-src=“data:image/jpg;base64,{{image}}">

        var dataNow64bit = _arrayBufferToBase64(response.data.img.data.data);

       $scope.image = dataNow64bit;

        $timeout(function() {
          file.result = response.data;
        });
      },
  
      function(response) {
        if (response.status > 0)
          $scope.errorMsg = response.status + ': ' + response.data;
      },
  
      function(evt) {
        file.progress = Math.min(100, parseInt(100.0 * evt.loaded / evt.total));
      });
    }
  };

}
{% endhighlight %}
   

References: 
<br>
<a href="http://jsfiddle.net/danialfarid/0mz6ff9o/135/">http://jsfiddle.net/danialfarid/0mz6ff9o/135</a>
<br>
<a href="http://stackoverflow.com/questions/9267899/arraybuffer-to-base64-encoded-string">http://stackoverflow.com/questions/9267899/arraybuffer-to-base64-encoded-string</a>
