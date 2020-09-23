﻿<div align="center">

## Advanced way of displaying a queryresult


</div>

### Description

This code shows a certain number of records per page, if more records found it will print

First ....4,5,6,7,8... Last

or

1,2,3,4,5...Last

or

...15,16,17,18,19

Look at the screenshot!
 
### More Info
 


<span>             |<span>
---                |---
**Submitted On**   |
**By**             |[Jor](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByAuthor/jor.md)
**Level**          |Advanced
**User Rating**    |3.9 (39 globes from 10 users)
**Compatibility**  |PHP 3\.0, PHP 4\.0
**Category**       |[Databases](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByCategory/databases__8-5.md)
**World**          |[PHP](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByWorld/php.md)
**Archive File**   |[](https://github.com/Planet-Source-Code/jor-advanced-way-of-displaying-a-queryresult__8-787/archive/master.zip)





### Source Code

```
<?php
/* READ THIS FIRST!!
In order to get this code to work the following steps have to be done!
* Change the username, password and dbname in order to connect to your database
* Change both of the query's($query and $query2), you have to define the right fields and the right table
 You can also expand the query's if you want
 Example : $query = "SELECT $Fields FROM $TABLE WHERE $Conditions ORDER BY $Field DESC/ASC"
Variables you can change if you want :
- $display : number of records displayed per page
- $max_pages_to_print : number of pages that will be printed(1,2,3, etc..)
- $middlenumber : only change this one when you change $max_pages_to_print
 For example : If you increase $max_pages_to_print with 2, increase $middlenumber with 1
 				If you decrease $max_pages_to_print with 2, decrease $middlenumber with 1
OK, thats all you need to know, you can contact me at : dorst@ddwebdesign.nl
*/
// CODE STARTS HERE!!!
// connect to database
$global_db = mysql_connect('localhost', $username, $password);
mysql_select_db($dbname, $global_db);
// query to get the number of records
$query = "SELECT Field1, Field2 FROM $TABLE";
$result = mysql_query($query) or die("ERROR");
$num_record = mysql_num_rows($result);
if($num_record > $display) { // Only show 1,2,3,etc. when there are more records found that fit on 1 page
// when the page is loaded first...
if(empty($pagenr)) {
$pagenr = 1;
}
// some variables
$display = 10; // number of records to display per page
$max_pages_to_print = 7; // number of pages to show, if you change this you also have to change the variable 'middlenumber', for example:increase this one with two, increase middlenumber with one
$startrecord = $pagenr * $display; // first record to show from the queryresult
$num_pages = intval($num_record / $display) + 1; // total number of pages
$loopcounter = 0; // counter for whileloop
$currentpage = $pagenr; // Page where we are at the moment
$middlenumber = 3; // Number will be decreased from variable currentpage in order to get the currentpage always in the middle
$colourcounter = 0; // Variable to change the background-color of the <td>
$i = 1; // variable that will print 1,2,3,etc..
$x = 0; // variable i use to put always the current, marked page in the middle
// actual stuff starts here
print("<table border=0 align=center><tr>");
if($currentpage >= $max_pages_to_print) {
print("<td><a href=\"$PHP_SELF?pagenr=1\">First</a>...</td>");
}
//BEGIN LOOP
while($loopcounter < $max_pages_to_print) {
if($currentpage >= $max_pages_to_print) { // If user clicks om page higher than $max_pages_to_print
// Mark current page
if($currentpage == $i) {
print("<td>$i &nbsp</td>"); // print pagenumbers
$i += 1; //increase pagenr
$loopcounter += 1; // increase loopcounter
}
// End marking
if($i > $num_pages) { // if last page has been printed, exit loop
break;
}
if($x == 0) {
$i = $currentpage - $middlenumber; // current page will always be printed in the middle
}
print("<td><a href=\"$PHP_SELF?pagenr=$i\">$i</a> &nbsp</td>"); // print pagenumbers
$x = $x + 1;
$i += 1; //increase pagenr
$loopcounter += 1; // increase loopcounter
}
else { // Else user clicks on a pagenumber lower $max_pages_to_print
// Mark current page
if($currentpage == $i) {
print("<td>$i &nbsp</td>"); // print pagenumbers
$i += 1; //increase pagenr
$loopcounter += 1; // increase loopcounter
}
// End marking
if($i > $num_pages) { // if less than $max_mages_to_print, exit loop
break;
}
print("<td><a href=\"$PHP_SELF?pagenr=$i\">$i</a> &nbsp</td>"); // print pagenumbers
$i += 1; //increase pagenr
$loopcounter += 1; // increase loopcounter
} // End if
}
// END LOOP
if(($num_pages > $max_pages_to_print AND // notice the user that there are more pages
	$i <= $num_pages)) {
print("<td>...<a href=\"$PHP_SELF?pagenr=$num_pages\">Last</a></td>");
}
print("</tr></table>");
$startrecord = $startrecord - $display; // Set startrecord to the right position
// Some calculation for the lastrecord
if($currentpage == $num_pages) { // Last page...
$lastrecord = $num_record; // so $lastrecord = $num_record
}
else {
$lastrecord = ($currentpage * $display);
}
} // End of the first if-statement
// Some info
print("<table align=center>
<tr><td>You are now on page $currentpage</td></tr>
<tr><td>There are $num_pages pages in total</td></tr>
<tr><td>$num_record records are spread over $num_pages pages</td></tr>
<tr><td>Current display : $startrecord - $lastrecord</td></tr>
</table>");
// End info
// actual query, watch the end($startrecord, $display)
$query2 = "SELECT Field1, Field2 FROM $TABLE LIMIT $startrecord, $display";
$result2 = mysql_query($query2) or die("ERROR");
// print results on screen
print("<table border=0 align=center width=300 bgcolor=#0E711D cellpadding=\"2\" cellspacing=\"1\"><tr><td><font color=#FFFFFF><b>Fieldname1</b></font></td><td align=right><font color=#FFFFFF><b>Fieldname2</b></font></td></tr>");
while(list($Field1, $Field2) = mysql_fetch_row($result2)) {
if ($colorcounter == 0) {
$colorbg = "#ECE28B";
}
else {
$colorbg = "#F4EFC1";
$colorcounter = $colorcounter - 2;
}
print("<tr><td bgcolor=$colorbg>$Field1</td><td align=right bgcolor=$colorbg>$Field2</td></tr>");
$colorcounter = $colorcounter + 1;
}
print("</table>");
?>
```

