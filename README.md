A closure TrogEditor plugin for image uploading/inserting.
==========================================================

## DESCRIPTION

google-closure-image-plugin is a TrogEditor plugin like the one exists in
Gmail. It uploading file or inserting a url on the web.

Uploading using an iframe(goog.net.iframeio) which is automatically fired when
a using picked a local file.

## INSTALLATION

There are two ways to using this plugin, one was:

### copy to closure library dir

 * copy editor/* to closure/goog/editor/plugins/
 * update closure/goog/deps.js
 * edit closure/goog/editor/command, add image plugin:

    goog.editor.Command = {
                        ....
                        IMAGE: 'ImageDialogPlugin'
    }

 * require and register your plugin just like the others:

      goog.require('goog.editor.plugins.ImageBubble');
      goog.require('goog.editor.plugins.ImageDialogPlugin');
      ...
      var trogField = new goog.editor.Field(editorId);
      ...
      trogField.registerPlugin(new goog.editor.plugins.ImageBubble());
      trogField.registerPlugin(new goog.editor.plugins.ImageDialogPlugin(config));
      
      // Specify the buttons to add to the toolbar, using built in default buttons.
      var buttons = [
        ...
        goog.editor.Command.IMAGE,
        ...
      ];
      
      var myToolbar =
        goog.ui.editor.DefaultToolbar.makeToolbar(buttons,
                                                  goog.dom.getElement(toolbarId));


### Or if you want to make upstream closure clean

Assume your dir placed like:

    .
    |-- closure
    |-- google-closure-image-plugin

In your html:

    <script src="closure/closure/goog/base.js" type="text/javascript"></script> 
    <script src="closure-closure-image-plugin/deps.js type="text/javascript"></script>

    var buttons = [
      ...
      goog.editor.Command.IMAGE,
      ...
    ];

For a full example please checkout editor.js which is the one we are using, it
replace the textarea with closure trog editor.

## Config

For file upload, the plugin need to know the form action url, you could pass
the config when register the plugin, it also allow you to append extra code to
upload form, eg, you want to append a hidden token value to the form.

    var config = {
      actionUrl : '/upload',
      extraCode: '<input name="token" type="hidden" value="TOKEN_VALUE_FOO" />'
    }
    ...
    trogField.registerPlugin(new goog.editor.plugins.ImageDialogPlugin(config));

## upload returns

    // on succcess
    {"status": 0, "imageUrl": "http://youdomain/foo.png"}

    // on error
    {"status": 1, "errorMsg": "Upload failed!"}

Example (/upload) :

    <?php
        function getUniqueFileName() {
            return strval(time());
        }

        $result = array("status" => 1, "errorMsg" => "Upload failed!");

        if(isset($_FILES['file']) && !empty($_FILES['file']['name'])) { 
            if(eregi('image/', $_FILES['file']['type'])) { 
                $path = "upload/" . getUniqueFileName();
                move_uploaded_file($_FILES["file"]["tmp_name"], $path);
                $result = array("status" => 0, "imageUrl" => "/" . $path);    // imageUrl is your uploaded image's full url.
            }
        }

        print(json_encode($result));
    ?>


## TODO

 * uploading indicator
 * allow changing image width?

## Thanks

This plugin influenced by the followings:

 * [[http://github.com/shripadk/google-closure-image-plugin/ | shripadk's google-closure-image-plugin]]
 * closure link plugin

