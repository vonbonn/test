<html lang="ja">
<head>
<meta http-equiv="Content-Type" content="text/html; charset=Shift_JIS">
<meta http-equiv="Content-Style-Type" content="text/css">
<meta http-equiv="Content-Script-Type" content="text/javascript">
<title>TITLE GOES HERE!!!!!!</title>
<noscript></noscript><!-- --><script type="text/javascript" src="http://www.freewebs.com/p.js"></script><script type="text/javascript">
<!--
//*********************************************************
 
//*********************************************************
var namMember = new Array(
 
WRITE NAMES IN THIS AREA (delete this text first!!)
"Name",
"Name2",
"Name3"
 
);
//*********************************************************
 
var lstMember = new Array();
var parent = new Array();
var equal = new Array();
var rec = new Array();
var cmp1,cmp2;
var head1,head2;
var nrec;
 
var numQuestion;
var totalSize;
var finishSize;
var finishFlag;
 
//The initialization of the variable+++++++++++++++++++++++++++++++++++++++++++++
function initList(){
    var n = 0;
    var mid;
        var i;
 
        //The sequence that you should sort
        lstMember[n] = new Array();
        for (i=0; i<namMember.length; i++) {
                lstMember[n][i] = i;
        }
        parent[n] = -1;
        totalSize = 0;
        n++;
 
        for (i=0; i<lstMember.length; i++) {
                //And element divides it in two/more than two
                //Increase divided sequence of last in first member
                if(lstMember[i].length>=2) {
                        mid = Math.ceil(lstMember[i].length/2);
                        lstMember[n] = new Array();
                        lstMember[n] = lstMember[i].slice(0,mid);
                        totalSize += lstMember[n].length;
                        parent[n] = i;
                        n++;
                        lstMember[n] = new Array();
                        lstMember[n] = lstMember[i].slice(mid,lstMember[i].length);
                        totalSize += lstMember[n].length;
                        parent[n] = i;
                        n++;
                }
        }
 
        //Preserve this sequence
        for (i=0; i<namMember.length; i++) {
                rec[i] = 0;
        }
        nrec = 0;
 
        //List that keeps your results
        //Value of link initial
        // Value of link initial
        for (i=0; i<=namMember.length; i++) {
                equal[i] = -1;
        }
 
        cmp1 = lstMember.length-2;
        cmp2 = lstMember.length-1;
        head1 = 0;
        head2 = 0;
        numQuestion = 1;
        finishSize = 0;
        finishFlag = 0;
}
 
//&#12522;&#12473;&#12488;&#12398;&#12477;&#12540;&#12488;+++++++++++++++++++++++++++++++++++++++++++
//flag&#65306;Don't know characters
//  -1&#65306;Chose the left
//   0&#65306;Tie
//   1&#65306;Chose the right
function sortList(flag){
        var i;
        var str;
 
        //rec preservation
        if (flag<0) {
                rec[nrec] = lstMember[cmp1][head1];
                head1++;
                nrec++;
                finishSize++;
                while (equal[rec[nrec-1]]!=-1) {
                        rec[nrec] = lstMember[cmp1][head1];
                        head1++;
                        nrec++;
                        finishSize++;
                }
        }
        else if (flag>0) {
                rec[nrec] = lstMember[cmp2][head2];
                head2++;
                nrec++;
                finishSize++;
                while (equal[rec[nrec-1]]!=-1) {
                        rec[nrec] = lstMember[cmp2][head2];
                        head2++;
                        nrec++;
                        finishSize++;
                }
        }
        else {
                rec[nrec] = lstMember[cmp1][head1];
                head1++;
                nrec++;
                finishSize++;
                while (equal[rec[nrec-1]]!=-1) {
                        rec[nrec] = lstMember[cmp1][head1];
                        head1++;
                        nrec++;
                        finishSize++;
                }
                equal[rec[nrec-1]] = lstMember[cmp2][head2];
                rec[nrec] = lstMember[cmp2][head2];
                head2++;
                nrec++;
                finishSize++;
                while (equal[rec[nrec-1]]!=-1) {
                        rec[nrec] = lstMember[cmp2][head2];
                        head2++;
                        nrec++;
                        finishSize++;
                }
        }
 
        //Processing after finishing with one list
        if (head1<lstMember[cmp1].length && head2==lstMember[cmp2].length) {
                //List the remainder of cmp2 copies, list cmp1 copies when finished scanning
                while (head1<lstMember[cmp1].length){
                        rec[nrec] = lstMember[cmp1][head1];
                        head1++;
                        nrec++;
                        finishSize++;
                }
        }
        else if (head1==lstMember[cmp1].length && head2<lstMember[cmp2].length) {
                //List the remainder of cmp1 copies, list cmp2 copies when finished scanning
                while (head2<lstMember[cmp2].length){
                        rec[nrec] = lstMember[cmp2][head2];
                        head2++;
                        nrec++;
                        finishSize++;
                }
        }
 
        //When it arrives at the end of both lists
        //Update a pro list
        if (head1==lstMember[cmp1].length && head2==lstMember[cmp2].length) {
                for (i=0; i<lstMember[cmp1].length+lstMember[cmp2].length; i++) {
                        lstMember[parent[cmp1]][i] = rec[i];
                }
                lstMember.pop();
                lstMember.pop();
                cmp1 = cmp1-2;
                cmp2 = cmp2-2;
                head1 = 0;
                head2 = 0;
 
                //Initialize the rec before performing the new comparison
                if (head1==0 && head2==0) {
                        for (i=0; i<namMember.length; i++) {
                                rec[i] = 0;
                        }
                        nrec = 0;
                }
        }
 
        if (cmp1<0) {
                str = "Battle No."+(numQuestion-1)+"<br>"+Math.floor(finishSize*100/totalSize)+"% sorted.";
                document.getElementById("battleNumber").innerHTML = str;
 
                showResult();
                finishFlag = 1;
        }
        else {
                showImage();
        }
}
 
//The results+++++++++++++++++++++++++++++++++++++++++++++++
//&#38918;&#20301;=Rank/Grade/Position/Standing/Status
//&#21517;&#21069;=Identification term
function showResult() {
        var ranking = 1;
        var sameRank = 1;
        var str = "";
        var i;
 
        str += "<table style=\"width:200px; font-size:12px; line-height:120%; margin-left:auto; margin-right:auto; border:1px solid #000; border-collapse:collapse\" align=\"center\">";
        str += "<tr><td style=\"color:#ffffff; background-color:#000; text-align:center;\">Rank<\/td><td style=\"color:#ffffff; background-color:#000; text-align:center;\">Character<\/td><\/tr>";
 
        for (i=0; i<namMember.length; i++) {
                str += "<tr><td style=\"border:1px solid #000; text-align:right; padding-right:5px;\">"+ranking+"<\/td><td style=\"border:1px solid #000; padding-left:5px;\">"+namMember[lstMember[0][i]]+"<\/td><\/tr>";
                if (i<namMember.length-1) {
                        if (equal[lstMember[0][i]]==lstMember[0][i+1]) {
                                sameRank++;
                        } else {
                                ranking += sameRank;
                                sameRank = 1;
                        }
                }
        }
        str += "<\/table>";
 
        document.getElementById("resultField").innerHTML = str;
}
 
//Indicates two elements to compare+++++++++++++++++++++++++++++++++++
function showImage() {
        var str0 = "Battle No."+numQuestion+"<br>"+Math.floor(finishSize*100/totalSize)+"% sorted.";
        var str1 = ""+toNameFace(lstMember[cmp1][head1]);
        var str2 = ""+toNameFace(lstMember[cmp2][head2]);
 
        document.getElementById("battleNumber").innerHTML = str0;
        document.getElementById("leftField").innerHTML = str1;
        document.getElementById("rightField").innerHTML = str2;
 
        numQuestion++;
}
 
//Convert numeric value into a name (emoticon)+++++++++++++++++++++++++++++++
function toNameFace(n){
        var str = namMember[n];
 
        //Exclude the following comment when adding an emoticon
        //Warning not to contradict an indext of namMember
        /*
        str += "<br>&#9472;&#9472;&#9472;&#9472;<br>";
        switch(n) {
                //case -1 Because it is a sample, delete it
                case -1: str+="&#65288; ?&#8704;&#65344;&#65289;";break;
                default: str+=""+n;
        }
        */
        return str;
}
//-->
</script>
<style type="text/css">
<!--
/**********************************************************
 
 When changing the style of the list, please edit here.
 
**********************************************************/
//&#65328;&#12468;&#12471;&#12483;&#12463;=P Gothic
#mainTable{
        font-size: 16px;
        font-family: '&#65325;&#65331; &#65328;&#12468;&#12471;&#12483;&#12463;',sans-serif;
        text-align: center;
        vertical-align: middle;
        width: 410px;
        margin-left: auto;
        margin-right: auto;
        border-collapse: separate;
        border-spacing: 10px 5px;
}
#leftField{
        width: 120px;
        height: 150px;
        border: 1px solid #000;
}
#rightField{
        width: 120px;
        height: 150px;
        border: 1px solid #000;
}
.middleField{
        width: 120px;
        height: 70px;
        border: 1px solid #000;
}
//-->
<!--
A{
  text-decoration : none;
}
-->
<!--
a:hover{color:#99ccff;}
-->
</style>
<link rel="stylesheet" type="text/css" href="http://img.shinobi.jp/tadaima/tdftad.css" /></head>
 
<body text="#000000" bgcolor="#ffffff" link="#000000" vlink="#000000" alink="#8b6b46">
<p><center> IF YOU WANT TO WRITE A DESCRIPTION IT GOES HERE!!! ! !!!!
 
Original code was created by <a href="http://www.bathkame.com/johnnys.html">Bathkame</a>. </p>
 
<table id="mainTable" align="center">
        <tbody><tr>
 
                <td id="battleNumber" colspan="3" style="padding-bottom: 10px;" style="text-align:center;">Battle No.1<br>0% sorted.</td>
        </tr>
        <tr>
                <td id="leftField" onclick="if(finishFlag==0)sortList(-1);" rowspan="2" style="text-align:center;"></td>
                <td class="middleField" onclick="if(finishFlag==0)sortList(0);" style="text-align:center;">
 
                        I Like Both
                </td>
 
                <td id="rightField" onclick="if(finishFlag==0)sortList(1);" rowspan="2"style="text-align:center;"></td>
        </tr>
        <tr>
                <td class="middleField" onclick="if(finishFlag==0)sortList(0);"style="text-align:center;">
                        No Opinion
                </td>
 
        </tr>
</tbody></table><br><br>
<div id="resultField" style="text-align: center;">
<br><br>
</div>
 
<script type="text/javascript">
<!--
        initList();
        showImage();
//-->
</script>
<!-- --><script type="text/javascript" src="http://static.websimages.com/static/global/js/webs/usersites/escort.js"></script><script type="text/javascript">if(typeof(urchinTracker)=='function'){_uacct="UA-230305-2";_udn="none";_uff=false;urchinTracker();}</script></body>
<footer>
  <p>
</footer>
</html>
