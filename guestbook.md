---
layout: page
title: GuestBook
permalink: /guestbook/
main_nav: true
---

<!doctype html>
<html>
<head>
    <meta charest="utf-8">
    <title>GestBook</title>
    <link href="style2.css" rel="stylesheet" type="text/css">
    <style>
        #me:hover{
            transform-origin:right top;
            -webkit-transform-origin:right top;
            -moz-transform-origin:right top;
            -o-transform-origin:right top;
            -ms-transform-origin:right top;
            transform:rotate(15deg);
            -webkit-transform:rotate(15deg);
            -moz-transform:rotate(15deg);
            -o-transform:rotate(15deg);
            -ms-transform:rotate(15deg);
        }
    </style>
</head>

<body>

<h3>Actively Protect Security</h3>
<ul>
    <li>
        방명록 입니다.
    </li>
</ul>

<FORM ACTION="insert.php" METHOD="POST">
    <TABLE BORDER=1 WIDTH=600>
        <TR>
            <TD>이름</TD><TD><INPUT TYPE="TEXT" NAME="name"></TD>
            <TD>비밀번호</TD><TD><INPUT TYPE="PASSWORD" NAME="pass"></TD>
        </TR>
        <TR>
            <TD COLSPAN=4>
                <TEXTAREA NAME="content" COLS=80 ROWS=5></TEXTAREA>
            </TD>
        </TR>
        <TR>
            <TD COLSPAN=4 align=right><INPUT TYPE="SUBMIT" VALUE="확인"></TD>
        </TR>
    </TABLE>

<?
include "db_info.php";

    if (!isset($_GET['no']))
        $no = 0;
    else $no = $_GET['no'];

$pagesize = 5;

$query1 = "SELECT count(*) FROM guestbook";
$result1 = mysqli_query($conn, $query1);
$row1 = mysqli_fetch_array($result1);
$total = $row1[0];

$query = "SELECT * FROM guestbook ORDER BY id DESC LIMIT $no, $pagesize;";
//$query = "SELECT * FROM guestbook ORDER BY id DESC";
$result = mysqli_query($conn, $query);
//$total = mysqli_affected_rows($conn);

    for($i=$no; $i<$no+$pagesize; $i++){
        if ($i < $total)
        {
            //if(!mysqli_data_seek($result,$i)) die(mysqli_error($conn));
            $row = mysqli_fetch_array($result) or die(mysqli_error($conn));

?>
            <TABLE WIDTH=500 BORDER=1>
                <TR>
                    <TD>No. <?=$row['id']?></TD>
                    <TD><?=$row['name']?></TD>
                    <TD><?=$row['wdate']?></TD>
                    <TD><a href="delete.php?id=<?=$row['id']?>">del</a></TD>
                </TR>
                <TR>
                    <TD COLSPAN=4><?=$row['content']?></TD>
                </TR>
            </TABLE>

<?
        }
    }
        $prev = $no - $pagesize; // 이전 페이지는 시작 글에서 $scale을 뺀 값부터
        $next = $no + $pagesize; // 다음 페이지는 시작 글에서 $scale을 더한 값부터

       if ($prev >= 0) {
                   echo("<a href='{$_SERVER['PHP_SELF']}?no=$prev'>이전</a>");
       }
       if ($next < $total) {
                   echo("<a href='{$_SERVER['PHP_SELF']}?no=$next'>다음</a></center>");
       }
?>

</body>
</html>

