<!--ConnectPHPMyadmin-->
<?php
$con= mysqli_connect("localhost", "root","","projectroom") or die("ไม่สามารถเชื่อข้อมูลได้");
echo "";
?>
<!--ShowReservationStatus-->
<html>
    <head>
        <meta charset="UTF-8">
        <title></title>
    </head>
    <body>
        <form method="post" action='ShowReservationStatus.php'>
        <h1 style="text-align: center" >ตรวจสอบสถานะการจองห้องพัก</h1>
        <p style="text-align: center">จากวันที่
        <input type="date" name="startDate" max="2018-1-1">
        ถึง
        <input type="date" name="endDate" max="2060-12-31">
        <br><br>
        <input style="text-align:center "type="submit" value="ตรวจสอบ">
        </p>
        </form>
    </body>
</html>
<!--ShowReservationStatus-->
<?php
require './dbConnect.php';
$booking=array();
$date=array();
$rooms=array(1,2,3);
$book_room=$_POST['startDate'];
$out_room=$_POST['endDate'];
$sql="SELECT * FROM description_room WHERE book_room >= '{$book_room}' and book_room <= '{$out_room}'";
$result= mysqli_query($con,$sql);
while($row= mysqli_fetch_assoc($result))
{
    $booking[$row['num_room']][]=$row['book_room'];
}
$objstartDate= date_create($book_room);
$objendDate = date_create($out_room);
$diff = date_diff($objstartDate,$objendDate);
$diffdate = $diff->days+1;

$table='<table align="center" border="1" bgcolor="#FFF8DC" cellspacing="1" cellpadding="2"><tr><th>ห้อง/วันที่</th>';
for($i=1;$i<=$diffdate;$i++)
{
    $date[$i]= date_format($objstartDate, 'Y-m-d');
    $table.='<th>'.$date[$i].'</th>';
    date_add($objstartDate,date_interval_create_from_date_string('1 days'));
}
$table.='</tr>';
foreach ($rooms as $r) 
{
    $table.='<tr><th>'.$r.'</th>';
    foreach($date as $d)
    {
        if(isset($booking[$r])&&in_array($d,$booking[$r]))
        {
            $table.='<td align="center" bgcolor="#FF0000">จอง</td>';
        }
        else {$table.='<td align="center" bgcolor="#7FFF00">ว่าง</td>';}
    }
    $table.='</tr>';
}
$table.='</table>';
if($result)
{
    echo "";
}
 else 
{
     echo mysqli_erro($con);
}
?>

<html>
    <head>
        <meta charset="UTF-8">
        <title></title>
    </head>
    <body>
            <h1 style="text-align:center">สถานะการจอง </h1> 
            <h1 style="text-align:center">วันที<?php echo $book_room ?>ถึง<?php echo $out_room?></h1>
            <table border="1" bgcolor="#666666" cellspacing="1" cellpadding="2">
            
            
            <?php echo $table;?>     
            
    </body>
</html>
    <!--member-->
<html>
    <head>
        <meta charset="UTF-8">
        <title>Member</title>
    </head>
    <body>
        <form method="post" action="insertmember.php">
            ชื่อ (Name)   :
            <select name="title" style="width:100px;">
                <option value="Mr">นาย/Mr.</option>
                <option value="Mrs">นางสาว/Mrs.</option>
            </select>
            <input type="text" name="fname">
            &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
            นามสกุล(Surname) : <input type="text" name="lname"> 
            <br><br>
            มือถือ(Moblie Phone) :
            &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
            <input type="text" name="phone">
            &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
            ที่อยู่ (Address) : <input size="31"type="text" name="address">
            <br>
            <sub>(กรุณากรอกให้ครบจำนวน10หลัก)</sub>
            <br><br>
            เลขบัตรประชาชน (ID Card): <input type="text" name="idcard">
            &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
            E-mail : <input type="text" name="mail">
            <br><sub>(กรุณากรอกให้ครบ13หลัก)</sub>
            <br><br>
            กรุณาเลือกห้อง (Please select a room) :
            <select name="room" style="width:100px;">    
                <option value="suit">Suit room</option>
                <option value="vip">VIP  room</option>
                <option value="plain">Ordinary room</option>
            </select>
            &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
            ราคา (Price) : <input type="text" name="price">
            <br><br>
            ผู้ใหญ่ (Adult) : <input type="text" name="adult" style="width:30px;">
            ผู้ใหญ่ (Child) : <input type="text" name="child" style="width:30px;">
            <br><br>
            <input type="submit" name="save" value="ตกลง">
            &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
            <input type="submit" name="cancel" value="ยกเลิก">
            <form>       
    </body>
</html>
//PHPinsertmember
<?php
require './dbConnect.php';
$num_room=$_POST['room'];
$title=$_POST['title'];
$fname=$_POST['fname'];
$lname=$_POST['lname'];
$address=$_POST['address'];
$id_card=$_POST['idcard'];
$email=$_POST['mail'];
$phone=$_POST['phone'];
$price=$_POST['price'];
$adult=$_POST['adult'];
$child=$_POST['child'];

$sql="insert into member_room(room,title,fname,lname,address,idcard,mail,phone,price,adult,child)value('$num_room','$title','$fname','$lname','$address','$id_card','$email','$phone','$price','$adult','$child')";
$result= mysqli_query($con,$sql);
if($result)
{
    echo "<a href='member.php'>บันทึกข้อมูลเรียบร้อย</a>";
}
else
{
    echo mysqli_error($con);
}


