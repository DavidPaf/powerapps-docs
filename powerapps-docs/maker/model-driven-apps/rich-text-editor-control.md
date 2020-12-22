---
title: Use the rich text editor control in Power Apps | MicrosoftDocs
description: "The rich text editor control provides the app user a WYSIWYG editing area for formatting text"
ms.custom: ""
ms.date: 12/18/2020
ms.reviewer: "matp"
ms.service: powerapps
ms.suite: ""
ms.tgt_pltfrm: ""
ms.topic: "article"
author: "Mattp123"
ms.author: "craigm"
manager: "kvivek"
tags: 
search.audienceType: 
  - maker
search.app: 
  - PowerApps
  - D365CE
---
# Add the rich text editor control to a model-driven app

[!INCLUDE [cc-data-platform-banner](../../includes/cc-data-platform-banner.md)]

The rich text editor control provides the app user a WYSIWYG editing area for formatting text. The control's input and output format is HTML. The control allows copied rich text, such as from a web browser or Word, to be pasted into the control. 

Some of the format options available are:

- Bold, italic, underline, and strikethrough.
- Text color, highlight color.
- Font type and size.
- Numbered lists and bulleted lists.
- Hyperlinks.
- Tables.
- Images.

<img src="media/rich-text-control.png" alt="Rich text control editor in a model-driven app" height="500" width="520"> 

## Add or replace a text column for rich text editing

You can create a new text column and configure the control, or replace an existing text column. The rich text editor control can be used with single or multi-line text columns.

1. Sign in to Power Apps. Go to **Solutions**, open the solution that you want, open the table that you want, and then select the **Forms** tab. 
2. Select the form, and then select **Edit form**.
3. In the form designer on the command bar, select **Switch to classic**.
4. On the legacy form designer canvas, add or create a text column or select an existing text column, such as the account table **Description** column. On the **Home** tab, select **Change Properties**.
5. On the **Column Properties** page, select the **Controls** tab, and then select **Add control**.
6. Select **Rich Text Editor Control**, and then select **Add**.
7. Select **Web**, **Phone**, and **Tablet** if you want all client apps to have the ability to use rich text in the column. Then select **OK** to use the default rich text editor control configuration. If you want to change the rich text editor control configuration, see [Rich text editor control configuration options](#rich-text-editor-control-configuration-options).

    <img src="media/rich-text-control2.png" alt="Rich text control editor configuration" height="497" width="485">
8. Save and then publish the form.

## Rich text editor control configuration options

The rich text editor control comes with a rich set of configuration options that make it possible to customize its appearance, features, and behavior. To configure the rich text editor control, follow these steps:

1. Create a JSON file that includes the defaultSupportedProps structure and configuration with the changes you want. More information: [Sample rich text editor configurations](#sample-rich-text-editor-configurations) and [Rich text editor properties](#rich-text-editor-properties)
2. In Power Apps, create a JavaScript web resource using the JSON file created in step 1. More information: [Create or edit model-driven app web resources to extend an app](create-edit-web-resources.md)
3. Open the **Column Properties** page for the text column with the rich text editor control, and then next to **RichTextEditorControl_URL** select **Edit**.
   > [!div class="mx-imgBorder"] 
   > ![Rich text editor control URL](media/richtexteditorcontrol-url.png)
4. Select **Bind to static value**, enter the relative URL to the JavaScript web resource in the box next to **SingleLine.URL**, and then select **OK**. The relative URL is located on the web resource definition.
5. Select **OK** to close the **Column Properties** page.
6. On the form editor command bar, select **Publish**.

## Rich text editor properties

### defaultSupportedProps

You can configure all the ckEditor supported properties under this property. A few of the commonly used configurations are described here. For complete documentation about CKEditor configurations, see [CKEditor.config](https://ckeditor.com/docs/ckeditor4/latest/api/CKEDITOR_config.html).


|Attribute  |Description  |
|---------|---------|
|height     | Sets the height of the content editor. The default is 185 px.     |
|font_defaultLabel     | Sets the default font style. The default is Segoe UI.    |
|fontSize_defaultLabel     | Sets the default font size. The default is 14.        |
|toolbarLocation     |  The location of the user interface where the toolbar will be rendered. Supported values are *top* and *bottom*. Default is bottom.     |
|plugins     | Comma-separated list of plug-ins to be used in an editor instance. Note that the actual plug-ins that are loaded might still be affected by two other settings: *extraPlugins* and *removePlugins*. <br /> Updating this setting might remove the plug-ins from the toolbar. If you set this property to an empty string, the editor will load without the toolbar. <br /> If you want to add one or more plug-ins to the toolbar, we recommend that you use *extraPlugins*. If you want to remove one or more from the default list, use *removePlugins*.     |
|extraPlugins     |  A comma-separated list of additional plug-ins to be loaded. This setting makes it easier to add new plug-ins without touching the plugins setting. <br /> There are many plug-ins that are required for other plug-ins to work. For example, the dialog plug-in is required for the link plug-in. The rich text editor automatically adds those, and you can't override them by updating this property. This setting will simply append new plug-ins to the previous list. <br /> If you want to remove any of the presets, we recommend that you use the *removePlugins* property.     |
| removePlugins   | A list of plug-ins that must not be loaded. This setting makes it possible to avoid loading some plug-ins defined in the plugins/extraPlugins setting without having to touch them.   | 

### externalPlugins

By using this property, you can write your own plug-ins and use them in the rich text editor control. Below is the schema for externalPlugins property.

```xml
"externalPlugins": [
    {
      "name": "<<Plugin Name>>",
      "path": "<<Plugin’s folder path>>”
    }
  ]

Example:
"externalPlugins": [
    {
      "name": "EmbedMedia",
      "path": "http://aurorav32308.aurorav32308dom.extest.microsoft.com/CITTest/%7B637230928490017310%7D/WebResources/msdyncrm_/AssistEditControl/KBEditor/libs/ckeditor/plugins/embedmedia/"
    }
  ]
```

### imageEntity

By setting this property, you can avoid using the default table for images so that you can enforce additional security if needed.


|Attribute  |Description  |
|---------|---------|
|ImageEntityName      |  The name of the image table.    |
|ImageFileAttributeName      | The attribute name of the blob reference.      |

### disableImages

Setting this property to true will disable images. This property will have highest priority. This means that when this property is set to true, irrespective of the [imageEntity](#imageentity) property value, images will be disabled. By default, images are enabled.

### disableDefaultImageProcessing

By default, images will be uploaded using the client API. As soon as an image gets added to the editor, it will be uploaded to the platform. To process images, set this property to true.

## Sample rich text editor configurations

The following sample rich text editor configuration code sample data can be used to enable specific types of rich text experiences. For each sample, you create a JSON web resource. More information: [Rich text editor control configuration options](#rich-text-editor-control-configuration-options)

### Add the full screen expander

`{ "showAsTabControl": true, "showFullScreenExpander": true }`

:::image type="content" source="media/cke-screen-expander.png" alt-text="Screen expander control":::

### Add the HTML source view tab

`{ "showAsTabControl": true, "showHtml": true }`

:::image type="content" source="media/cke-html-source.png" alt-text="HTML tab control":::

### Add a simple toolbar with font size, bold, italic, underline, and highlight

`{ "defaultSupportedProps": {"toolbar":[{ "items": ["FontSize", "Bold", "Italic", "Underline", "BGColor"]}]  }}`

:::image type="content" source="media/cke-simple-editor.png" alt-text="Controls for a simple editor":::

### Remove the toolbar to make a rich text rendering surface

`{ "defaultSupportedProps": {"toolbar":[]  }}`

:::image type="content" source="media/cke-no-toolbar.png" alt-text="No toolbar":::

### Add a new font list and set Brush Script MT as the default font with a default size of 20 px

`{ "defaultSupportedProps": {"font_names":"Brush Script MT/'Brush Script MT', cursive;Calibri/Calibri, Helvetica, sans-serif;Calibri Light/'Calibri Light', 'Helvetica Light', sans-serif;", "font_defaultLabel":"Brush Script MT", "fontSize_sizes":"8/8px;12/12px;20/20px;32/32px", "fontSize_defaultLabel":"20", "stickyStyle":{"font-size":"20px", "font-family":"'Brush Script MT', cursive"}  }}`

:::image type="content" source="media/cke-default-font.png" alt-text="Set a new default font":::

### Position the toolbar at the top of the rich text editor

`{ "defaultSupportedProps": {"toolbarLocation":"top"  }}`

:::image type="content" source="media/cke-toolbar-top.png" alt-text="Toolbar positioned at the top of the rich text editor":::

### Start the editor at 30 px height and then auto-grow to fit content

`{ "defaultSupportedProps": { "autoGrow_onStartup": false , "autoGrow_maxHeight": 0 , "autoGrow_minHeight": 30 , "height": 30  }}`

:::image type="content" source="media/cke-autogrow.png" alt-text="Typing into the rich text area will increase it to fit the content":::

### Fix the height of the editor at 500 px

`{ "defaultSupportedProps": { "removePlugins":["autogrow"], "height": 500   }}`

:::image type="content" source="media/cke-fixed-height.png" alt-text="With a fixed height, the editor remains at the same height. When enough content is added, a scroll bar appears.":::

## Find the current setting for a rich text editor configuration

1. In a Microsoft Edge or Google Chrome web browser, run your model-driven app and open a form that has the rich text editor control, such as an account row.
1. Hold down **Ctrl** while clicking the rich text editor control area, and then select **Inspect**.
1. In the inspection pane, select the **Console** tab, and then select the parent **Main.aspx** page in the drop-down list box on the command bar.

   :::image type="content" source="media/cke-select-parent-main.png" alt-text="Select the Console tab and then select the parent main.aspx page from the drop-down list box":::

1. Select **Clear console** on the inspection pane command bar.

   :::image type="content" source="media/cke-clear-console.png" alt-text="Clear console command":::

1. In the inspection pane console, enter **CKEDITOR.config.** to display the different configurations.

   :::image type="content" source="media/cke-configs.png" alt-text="List of CK Editor configurations":::

1. Select a configuration, such as **autoGrow_minHeight**, to display the current setting.

## Sample rich text editor configuration file

The following sample sets several of the options in the rich text editor&mdash;such as the height, location, and default font type&mdash;and uses plug-in logic. For more information about plug-ins, see [Use plug-ins to extend business processes](../../developer/common-data-service/plug-ins.md).

```json
{
  "defaultSupportedProps": {
    "height": 200,
    "toolbarLocation": "bottom",
	"font_defaultLabel": "Arial",
    "fontSize_defaultLabel": "20",
    "removePlugins": "iframe",
    "extraPlugins": "uploadimage",
    "plugins": "Image"
  },
  "externalPlugins": [
    {
      "name": "EmbedMedia",
      "path": "/WebResources/msdyncrm_/AssistEditControl/KBEditor/libs/ckeditor/plugins/embedmedia/"
    }
  ],
  "disableImages": true,
  "imageEntity" : {
     "imageEntityName": "msdyn_richtextfiles",
     "imageFileAttributeName": "msdyn_fileblob"
 }
}
```

## Create plain text surface that makes the strips html (except for <br /> tags)

`{ "defaultSupportedProps": {     "enterMode": 2 ,     "shiftEnterMode": 2 ,     "allowedContent":"*",     "disallowedContent":"*",     "forcePasteAsPlainText": true ,     "toolbar":[],     "removePlugins":"contextmenu,liststyle,openlink,tableresize,tableselection,tabletools"  },  "disableImages": true}}`

:::image type="content" source="media/rte-plain-text-surface.png" alt-text="Creating a plain text surface makes the strips html":::

## Remove the context menu so right-clicking will work with the default browser spell check

Enabling this functionality removes the contextual right-click editing capability.

`{  "defaultSupportedProps": {     "removePlugins":"contextmenu,liststyle,openlink,tableresize,tableselection,tabletools"  }}`

:::image type="content" source="media/rte-right-click-config.png" alt-text="Remove the context menu so right-clicking will work with the default browser spell check":::

## Use the webresource for organization-wide changes

The default RTE webresource is available with the display name RTEGlobalConfiguration.json. This configuration is used for all instances of the RTE control and can be used to make organization wide changes. This includes RTE used in timeline rich-text notes, knowledge management, and single and multi-line fields that are configured to use the RTE control.

```json
{
  "defaultSupportedProps": {
 
        "autoGrow_onStartup": true,
 
        "basicEntities": true,
 
        "browserContextMenuOnCtrl": true,
 
        "copyFormatting_allowRules": true,
 
        "customConfig": "",
 
        "dialog_backgroundCoverColor": "black",
 
        "disableNativeSpellChecker": false,
 
        "enterMode": 3,
 
        "extraPlugins": "accessibilityhelp,autogrow,autolink,basicstyles,bidi,blockquote,button,collapser,colorbutton,colordialog,confighelper,contextmenu,copyformatting,dialog,find,floatpanel,font,indentblock,justify,panel,panelbutton,pastefromword,quicktable,selectall,stickystyles,superimage,tableresize,tableselection,tabletools",
 
        "fillEmptyBlocks": true,
 
        "font_defaultLabel": "Segoe UI",
 
        "font_names": "Angsana New/'Angsana New', 'Leelawadee UI', Sathu, serif;Arial/Arial, Helvetica, sans-serif;Arial Black/'Arial Black', Arial, sans-serif;Calibri Light/'Calibri Light', 'Helvetica Light', sans-serif;Calibri/Calibri, Helvetica, sans-serif;Cambria/Cambria, Georgia, serif;Candara/Candara, Optima, sans-serif;Century Gothic/'Century Gothic', sans-serif;Comic Sans MS/'Comic Sans MS';Consolas/Consolas, Courier, monospace;Constantia/Constantia, 'Hoefler Text', serif;Corbel/Corbel, Skia, sans-serif;Cordia New/'Cordia New', 'Leelawadee UI', Silom, sans-serif;Courier New/'Courier New';DaunPenh/DaunPenh, 'Leelawadee UI', 'Khmer MN', sans-serif;Franklin Gothic Book/'Franklin Gothic Book', 'Avenir Next Condensed', sans-serif;Franklin Gothic Demi/'Franklin Gothic Demi', 'Avenir Next Condensed Demi Bold', sans-serif;Franklin Gothic Medium/'Franklin Gothic Medium', 'Avenir Next Condensed Medium', sans-serif;Garamond/Garamond, Georgia, serif;Gautami/Gautami, 'Nirmala UI', 'Telugu MN', sans-serif;Georgia/Georgia, serif;Impact/Impact, Charcoal, sans-serif;Iskoola Pota/'Iskoola Pota', 'Nirmala UI', 'Sinhala MN', sans-serif;Kalinga/Kalinga, 'Nirmala UI', 'Oriya MN', sans-serif;Kartika/Kartika, 'Nirmala UI', 'Malayalam MN', sans-serif;Latha/Latha, 'Nirmala UI', 'Tamil MN', sans-serif;Leelawadee UI/'Leelawadee UI', Thonburi, sans-serif;Lucida Console/'Lucida Console', Monaco, monospace;Lucida Handwriting/'Lucida Handwriting', 'Apple Chancery', cursive;Lucida Sans Unicode/'Lucida Sans Unicode';Mangal/Mangal, 'Nirmala UI', 'Devanagari Sangam MN', sans-serif;Nirmala UI/'Nirmala UI', sans-serif;Nyala/Nyala, Kefa, sans-serif;Palatino Linotype/'Palatino Linotype', 'Book Antiqua', Palatino, serif;Raavi/Raavi, 'Nirmala UI', 'Gurmukhi MN', sans-serif;Segoe UI/'Segoe UI', 'Helvetica Neue', sans-serif;Shruti/Shruti, 'Nirmala UI', 'Gujarati Sangam MN', sans-serif;Sitka Heading/'Sitka Heading', Cochin, serif;Sitka Text/'Sitka Text', Cochin, serif;Sylfaen/Sylfaen, Mshtakan, Menlo, serif;TW Cen MT/'TW Cen MT', 'Century Gothic', sans-serif;Tahoma/Tahoma, Geneva, sans-serif;Times New Roman/'Times New Roman', Times, serif;Times/Times, 'Times New Roman', serif;Trebuchet MS/'Trebuchet MS';Tunga/Tunga, 'Nirmala UI', 'Kannada MN', sans-serif;Verdana/Verdana, Geneva, sans-serif;Vrinda/Vrinda, 'Nirmala UI', 'Bangla MN', sans-serif;メイリオ/Meiryo, メイリオ, 'Hiragino Sans', sans-serif;仿宋/FangSong, 仿宋, STFangsong, serif;微軟正黑體/'Microsoft JhengHei', 微軟正黑體, 'Apple LiGothic', sans-serif;微软雅黑/'Microsoft YaHei', 微软雅黑, STHeiti, sans-serif;新宋体/NSimSun, 新宋体, SimSun, 宋体, SimSun-ExtB, 宋体-ExtB, STSong, serif;新細明體/PMingLiU, 新細明體, PMingLiU-ExtB, 新細明體-ExtB, 'Apple LiSung', serif;楷体/KaiTi, 楷体, STKaiti, serif;標楷體/DFKai-SB, 標楷體, BiauKai, serif;游ゴシック/'Yu Gothic', 游ゴシック, YuGothic, sans-serif;游明朝/'Yu Mincho', 游明朝, YuMincho, serif;隶书/SimLi, 隶书, 'Baoli SC', serif;黑体/SimHei, 黑体, STHeiti, sans-serif;굴림/Gulim, 굴림, 'Nanum Gothic', sans-serif;궁서/Gungsuh, 궁서, GungSeo, serif;돋움/Dotum, 돋움, AppleGothic, sans-serif;맑은 고딕/'Malgun Gothic', '맑은 고딕', AppleGothic, sans-serif;바탕/Batang, 바탕, AppleMyungjo, serif;바탕체/BatangChe, 바탕체, AppleMyungjo, serif;ＭＳ Ｐゴシック/'MS PGothic', 'ＭＳ Ｐゴシック', 'MS Gothic', 'ＭＳ ゴシック', 'Hiragino Kaku Gothic ProN', sans-serif;ＭＳ Ｐ明朝/'MS PMincho', 'ＭＳ Ｐ明朝', 'MS Mincho', 'ＭＳ 明朝', 'Hiragino Mincho ProN', serif",
 
        "fontSize_defaultLabel": "9",
 
        "fontSize_sizes": "8/8pt;9/9pt;10/10pt;11/11pt;12/12pt;14/14pt;16/16pt;18/18pt;20/20pt;22/22pt;24/24pt;26/26pt;28/28pt;36/36pt;48/48pt;72/72pt;",
 
        "height": 185,
 
        "keystrokes": [],
 
        "qtCellBorderColor": "rgb(171, 171, 171)",
 
        "qtCellBorderStyle": "solid",
 
        "qtCellBorderWidth": "1px",
 
        "qtCellPadding": "1",
 
        "qtCellSpacing": "0",
 
        "qtCellWith": "120px",
 
        "qtColumns": 8,
 
        "qtRows": 6,
 
        "qtStyle": {
 
            "border-collapse": "collapse",
 
            "font-size": "9pt"
 
        },
 
        "removeDialogTabs": "flash:Upload;link:upload",
 
        "removePlugins": "a11yhelp,codemirror,liststyle,magicline,scayt,showborders",
 
        "skin": "superowa",
 
        "stickyStyle": {
 
            "font-size": "9pt",
 
            "font-family": "'Segoe UI','Helvetica Neue',sans-serif"
 
        },
 
        "stickyStyles_defaultTag": "div",
 
        "superimageImageMaxSize": 5,
 
        "toolbarcollapser_enableResizer": true,
 
        "toolbarLocation": "bottom",
 
        "uploadRecordId": []
 
    },
 
    "disableContentSanitization": false,
 
    "disableDefaultImageProcessing": false,
 
    "disableImages": false,
 
    "imageEntity": {
 
        "imageEntityName": "msdyn_richtextfiles",
 
        "imageFileAttributeName": "msdyn_imageblob"
 
    },
 
    "showAsTabControl": false,
 
    "showFullScreenExpander": false,
 
    "showHtml": false,
 
    "showPreview": false,
 
    "showPreviewHeaderWarning": false}
}
``` 

## Known issues

- HTML markup is displayed for columns configured to use the rich text editor control that are displayed in components other than a column on a form. For example, this occurs in views, subgrids, paginated reports, and portals.
> [!div class="mx-imgBorder"] 
> ![HTML markup displayed in a column on a subgrid.](media/html-markup-issue.png)

- Don't replace the email activity control with the rich text editor control because it impacts the way images need to be saved and converted. We are working to fully enable using RTE as a replacement for the email activity editor in a future release.

### See also

[Create and edit columns for Microsoft Dataverse using Power Apps portal](../common-data-service/create-edit-field-portal.md)
