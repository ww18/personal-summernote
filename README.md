
基于summernote进行的自定义, 主要改变是: setting文件直接写的是中文,图片添加直接选择图片,并且写定了id值,所以一个页面只能有一个编辑器,并且可以连续上传同一个文件名的文件，并且去掉了图片的三个浮动选项

# Summernote
Super Simple WYSIWYG Editor on Bootstrap(3.0 and 2.x).

[![Build Status](https://secure.travis-ci.org/HackerWins/summernote.png)](http://travis-ci.org/HackerWins/summernote)
[![Built with Grunt](https://cdn.gruntjs.com/builtwith.png)](http://gruntjs.com/)

### Summernote
Summernote is a javascript program that helps you to create WYSIWYG Editor on web.

### Why Summernote?

Summernote has something specials no like others.
* Simple UI
* Interative WYSIWYG editing
* Handy integration with server

#### Inspired by
* Gmail WYSIWYG Editor (http://www.gmail.com)
* Redactor (http://imperavi.com/redactor/)

### Easy to install

Summernote uses opensouce libraries(jQuery, bootstrap, fontAwesome) 

#### 01. include js/css

Include Following code into `<head>` tag of your HTML:

```html
<!-- include libraries(jQuery, bootstrap, fontawesome) -->
<script type="text/javascript" src="//code.jquery.com/jquery-1.9.1.min.js"></script> 
<link rel="stylesheet" href="//netdna.bootstrapcdn.com/bootstrap/3.1.1/css/bootstrap.min.css" />
<script type="text/javascript" src="//netdna.bootstrapcdn.com/bootstrap/3.1.1/js/bootstrap.min.js"></script>
<link rel="stylesheet" href="//netdna.bootstrapcdn.com/font-awesome/4.0.3/css/font-awesome.min.css" />

<!-- include summernote css/js-->
<link href="//oss.maxcdn.com/summernote/0.5.1/summernote.css" rel="stylesheet">
<script src="//oss.maxcdn.com/summernote/0.5.1/summernote.min.js"></script>
```

If your summernote download is placed in a different folder, don't forget to change file's paths.

#### 02. target elements
And place `div` tag to somewhere in the `body` tag. This element will be placed by the visual representation of the summernote.
```html
<div id="summernote">Hello Summernote</div>
```

#### 03. summernote
Finally, run script after document ready.
```javascript
$(document).ready(function() {
  $('#summernote').summernote();
});
```

### API
Get HTML `code` if you need.

```javascript
var sHTML = $('#summernote').code();
```

`Destroy` summernote.

```javascript
$('#summernote').destroy();
```

#### Dependencies
* jQuery: http://jquery.com/
* Bootstrap: http://getbootstrap.com (both 2.x and 3.x)
* fontAwesome: https://github.com/FortAwesome/Font-Awesome (both 3.x and 4.x)

### Supported platform
* Modern Browser (Safari, Chrome, Firefox, Opera, Internet Explorer 9+)
* OS (Windows, Mac, Linux)

### Upcoming Features
* Responsive Toolbar
* Table: Handles(Sizing, Selection) and Popover
* support IE8
* Clipboard
* Media Object Selection


### for Hacker

#### structure of summernote.js

```
summernote.js - Renderer.js (Generate markup) - Locale.js (Locale object)
              ㄴEventHandler.js - Editor.js  (Abstract editor)
                                ㄴStyle.js   (Style Getter and Setter)
                                ㄴHistory.js (Store on jQuery.data)
                                ㄴToolbar.js (Toolbar module)
                                ㄴPopover.js (Popover module)
                                ㄴHandle.js  (Handle module)
                                ㄴDialog.js  (Dialog module)
-----------------------------Core Script-----------------------------
  agent.js  (agent information)
  async.js  (aysnc utility)
  key.js    (keycode object)
  dom.js    (dom functions)
  list.js   (list functions)
  range.js  (W3CRange extention)
---------------------------------------------------------------------
```

#### build summernote
```bash
# grunt-cli is need by grunt; you might have this installed already
npm install -g grunt-cli
npm install

# build full version of summernote: dist/summernote.js
grunt build

# generate minified copy: dist/summernote.min.js, dist/summernote.css
grunt dist
```
At this point, you should now have a `build/` directory populated with everything you need to use summernote.

#### test summernote
run tests with PhantomJS
```bash
grunt test
```

#### start local server for developing summernote.
run local server with connect and watch.
```bash
# this will open a browser on http://localhost:3000.
grunt server
# If you change source code, automatically reload your page.
```

#### Coding convention
* JSHint: http://www.jshint.com/about/
* JSHint rule: https://github.com/HackerWins/summernote/blob/master/.jshintrc

### Contacts
* Email: susukang98@gmail.com
* Twitter: http://twitter.com/hackerwins

example upload image
```html
$.sendFile = function(file, editor, $editable) {

    var filename = false;
    try {
        filename = file['name'];
    } catch(e) {
        filename = false;
    }

    //以上防止在图片在编辑器内拖拽引发第二次上传导致的提示错误
    var ext = filename.substr(filename.lastIndexOf("."));
    ext = ext.toUpperCase();
    var timestamp = new Date().getTime();

    const tempCommon = commonParam.substr(1).split('&');
    var data = new FormData();
    data.append("report_img", file);
    for(let i = 0; i < tempCommon.length; i++){
        const temp = tempCommon[i].split('=');
        data.append(temp[0], temp[1]);
    }
    $.ajax({
        data: data,
        type: "POST",
        url: url,
        cache: false,
        contentType: false,
        processData: false,
        success: function(data) {
            console.log('html: ', $editable)
            editor.insertImage($editable, data.data['img_src']);
        }
    });
}

const summerOption = {
    onImageUpload: function(files, editor, $editable) {
        $.sendFile(files[0],editor,$editable);
    },
    width: 852.5,
    fontSizes: ['12', '14', '16','18','20','22', '24', '36'],
    toolbar: [["fontsize",["fontsize"]], ["font", ["bold", "italic", "underline", "clear"]],  ["color", ["color"]], ["para", ["ul", "ol", "paragraph"]], ["insert", [ "hr"]],["table", ["table"]], ["insert", [ "picture"]]]
}

$('#summernote').summernote(summerOption);
$('#summernote').code('请添加内容...');
```

### License
summernote may be freely distributed under the MIT license.
