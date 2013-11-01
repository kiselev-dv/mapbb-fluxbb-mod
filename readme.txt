##
##
##        Mod title:  MapBB Code Mod
##
##      Mod version:  1.0
##  Works on FluxBB:  1.4, 1.4-rc3
##     Release date:  YYYY-MM-DD
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
##            Notes:  This is just a template. Don't try to install it! Rows
##                    in this header should be no longer than 78 characters
##                    wide. Edit this file and save it as readme.txt. Include
##                    the file in the archive of your mod. The mod disclaimer
##                    below this paragraph must be left intact. This space
##                    would otherwise be a good space to brag about your mad
##                    modding skills :)
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

install_mod.php to /

files/leaflet.draw.css to include/mapbb/
files/mapbbcode-window.html to include/mapbb/
files/leaflet.draw.js to include/mapbb/
files/mapbbcode-config.js to include/mapbb/
files/leaflet.ie.css to include/mapbb/
files/mapbbcode-config-src.js to include/mapbb/
files/leaflet.js to include/mapbb/
files/mapbbcode.js to include/mapbb/
files/mapbbcode-src.js to include/mapbb/
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
#---------[ 2. RUN ]----------------------------------------------------------
#

install_mod.php


#
#---------[ 3. DELETE ]-------------------------------------------------------
#

install_mod.php


#
#---------[ 4. OPEN ]---------------------------------------------------------
#

header.php


#
#---------[ 5. FIND (line: 87) ]---------------------------------------------
#

<link rel="stylesheet" type="text/css" href="style/<?php echo $pun_user['style'].'.css' ?>" />

#
#---------[ 6. AFTER, ADD ]-------------------------------------------------
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
    libPath: 'include/mapbb/',
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
#---------[ 7. OPEN ]---------------------------------------------------------
#

include/parser.php


#
#---------[ 8. FIND (line: 887) ]---------------------------------------------
#

	return clean_paragraphs($text);


#
#---------[ 9. BEFORE, ADD ]---------------------------------------------------
#

	$text = parse_mapbb($text);

#
#---------[ 10. FIND (line: 891) ]--------------------------------------------
#

//
// Clean up paragraphs and line breaks
//
function clean_paragraphs($text)


#
#---------[ 11. BEFORE, ADD ]-------------------------------------------------
#

function parse_mapbb($text)
{
	$text = preg_replace_callback('#\[map](.*?)\[\/map]#', function($mc){
		$rid = uniqid(rand(), true);
		return '<div id="map'.$rid.'">[map]'.$mc[1].'[/map]</div><script language="javascript">'.
		'if(mapBBcode) mapBBcode.show(\'map'.$rid.'\');</script>';},.
		$text);

	return $text;
}


#
#---------[ 12. SAVE/UPLOAD ]-------------------------------------------------
#
