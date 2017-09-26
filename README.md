
基于summernote进行的自定义, 主要改变是: setting文件直接写的是中文,图片添加直接选择图片,并且写定了id值,所以一个页面只能有一个编辑器,并且可以连续上传同一个文件名的文件，并且去掉了图片的三个浮动选项.
如果还想自定义的话, 需要按要求修改后,直接运行grunt --force, 然后将dist文件中需要的js和css拷贝到相应的地方即可.

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
