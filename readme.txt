##
##
##        Mod title:  MapBB Code Mod
##
##      Mod version:  1.1.1
##  Works on FluxBB:  1.4
##     Release date:  2013-11-01
##      Review date:  YYYY-MM-DD (Leave unedited)
##           Author:  Your name (dmitry.v.kiselev@gmail.com)
##
##      Description:  This mod adds support for [map] bbencode
##
##   Repository URL:  http://fluxbb.org/resources/mods/xxx (Leave unedited)
##
##   Affected files:  header.php
##                    include/parser.php
##
##       Affects DB:  No
##
##            Notes:  Install and try to add post with folowing bbcode
##                    [map]59.9342,30.3367(S); 59.4366,24.7529(F); 
##		      59.377,28.204(Border control); 59.934,30.337 
##                    59.718,30.037 59.39,28.532 59.372,27.444 59.428,26.807 
##                    59.367,26.433 59.469,26.016 59.437,24.753[/map]
##                    Enjoy.
##
##       DISCLAIMER:  Please note that "mods" are not officially supported by
##                    FluxBB. Installation of this modification is done at 
##                    your own risk. Backup your forum database and any and
##                    all applicable files before proceeding.
##
##


#
#---------[ 1. UPLOAD ]-------------------------------------------------------
#

files/leaflet.draw.css to include/mapbb/
files/mapbbcode-window.html to include/mapbb/
files/leaflet.draw.js to include/mapbb/
files/mapbbcode-config.js to include/mapbb/
files/leaflet.ie.css to include/mapbb/
files/leaflet.js to include/mapbb/
files/mapbbcode.js to include/mapbb/
files/leaflet.draw.ie.css to include/mapbb/
files/Bing.js to include/mapbb/
files/leaflet.css to include/mapbb/

files/images/layers.png to include/mapbb/images
files/images/marker-icon-2x.png to include/mapbb/images
files/images/marker-shadow.png to include/mapbb/images
files/images/layers-2x.png to include/mapbb/images
files/images/spritesheet-2x.png to include/mapbb/images
files/images/marker-icon.png to include/mapbb/images
files/images/spritesheet.png to include/mapbb/images

#
#---------[ 2. OPEN ]---------------------------------------------------------
#

header.php


#
#---------[ 3. FIND (line: 87) ]---------------------------------------------
#

<link rel="stylesheet" type="text/css" href="style/<?php echo $pun_user['style'].'.css' ?>" />

#
#---------[ 4. AFTER, ADD ]-------------------------------------------------
#

<link rel="stylesheet" href="include/mapbb/leaflet.css" />
<link rel="stylesheet" href="include/mapbb/leaflet.draw.css" />
<!--[if lte IE 8]>
    <link rel="stylesheet" href="include/mapbb/leaflet.ie.css" />
    <link rel="stylesheet" href="include/mapbb/leaflet.draw.ie.css" />
<![endif]-->
<script src="include/mapbb/leaflet.js"></script>
<script src="include/mapbb/leaflet.draw.js"></script>
<script src="include/mapbb/Bing.js"></script>
<script src="include/mapbb/mapbbcode.js"></script>
<script src="include/mapbb/mapbbcode-config.js"></script>
<script language="Javascript" type="text/javascript">
<!--
var mapBBcode = new MapBBCode({
    windowPath: 'include/mapbb/',
    layers: 'OpenStreetMap',
    defaultZoom: 2,
    defaultPosition: [22, 11],
    viewWidth: 600,
    viewHeight: 300,
    fullViewHeight: 600,
    editorHeight: 400,
    windowWidth: 800,
    windowHeight: 500,
    fullFromStart: false,
    preferStandardLayerSwitcher: true
});
//-->
</script>


#
#---------[ 5. OPEN ]---------------------------------------------------------
#

include/parser.php


#
#---------[ 6. FIND (line: 887) ]---------------------------------------------
#

	return clean_paragraphs($text);


#
#---------[ 7. BEFORE, ADD ]---------------------------------------------------
#

	$text = parse_mapbb($text);

#
#---------[ 8. FIND (line: 891) ]--------------------------------------------
#

//
// Clean up paragraphs and line breaks
//
function clean_paragraphs($text)


#
#---------[ 9. BEFORE, ADD ]-------------------------------------------------
#

function parse_mapbb($text)
{
	$text = preg_replace_callback('#\[map(.*?)\[\/map]#', function($mc){
		$rid = uniqid(rand(), true);
		return '<div id="map'.$rid.'">[map'.$mc[1].'[/map]</div><script language="javascript">'.
		'if(mapBBcode) mapBBcode.show(\'map'.$rid.'\');</script>';},
		$text);

	return $text;
}


#
#---------[ 10. IF EasyBBCode mode Installed ]-----------------------------
#

#
#---------[ 11. OPEN ]---------------------------------------------------------
#

mod_easy_bbcode.php

#
#---------[ 12. FIND (line: 12) ]--------------------------------------------
#

							function insert_text(open, close)
							{
								if (document.forms['<?php echo $bbcode_form ?>'])
									msgfield = document.forms['<?php echo $bbcode_form ?>']['<?php echo $bbcode_field ?>'];
								else if (document.getElementsByName('<?php echo $bbcode_field ?>'))
									msgfield = document.getElementsByName('<?php echo $bbcode_field ?>')[0];
								else
									document.all.req_message;
							
								//IE Support
#
#---------[ 13. REPLACE WITH ]-------------------------------------------------
#

														function getEditField()
							{
								if (document.forms['<?php echo $bbcode_form ?>'])
									return document.forms['<?php echo $bbcode_form ?>']['<?php echo $bbcode_field ?>'];
								else if (document.getElementsByName('<?php echo $bbcode_field ?>'))
									return document.getElementsByName('<?php echo $bbcode_field ?>')[0];
								else
									return document.all.req_message;
							}
							function insert_text(open, close)
							{
								msgfield = getEditField();
								
								// IE support

#
#---------[ 14. FIND (line: 12) ]--------------------------------------------
#

							<input type="button" value="QUOTE" name="QUOTE" onclick="insert_text('[quote]','[/quote]')" />

#
#---------[ 15. AFTER, ADD ]--------------------------------------------
#

							<input type="button" value="MAP" name="MAP" onclick="mapBBcode.editorWindow(getEditField());" />

#
#---------[ 16. FI EasyBBCode mode Installed ]-----------------------------
#

#
#---------[ 17. SAVE/UPLOAD ]-------------------------------------------------
#
