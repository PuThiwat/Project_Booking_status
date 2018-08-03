<!--ConnectPHPMyadmin-->
<?php
$con= mysqli_connect("localhost", "root","","projectroom") or die("ไม่สามารถเชื่อข้อมูลได้");
echo "";
?>
<!--Home page-->
<html>
    <head>
        <meta charset="UTF-8">
        <title>Home Page</title>
    </head>
    <body>
    <style>
    input[type=submit] 
    {
    width: 10%;
    background-color: #4CAF50;
    color: white;
    padding: 18px 10px;
    margin: 8px 0;
    border: none;
    border-radius: 5px;
    cursor: pointer;
    }
    </style>
    
    <style>
    .header{width:800px; height:100px; background-color:blanchedalmond; color:scrollbar } 
    .Article{float:right; width:80%; height:500px; background-color:bisque }
    .aside{width:20%; background-color:burlywood; float:left; height:500px; text-align:left}
    .footer{background-color:peru; height:200px; clear:both}
    </style>    
    
    <div style="width:800px; margin:0 auto;">
        <div class="header"> 
            <h1 style="text-align:center">HomePage</h1>    
        </div>
        
        <div class="Article">
        
        </div>
        
        <div class="aside">
        <form  method="post" action="member.php">
            <input type="submit" name="mbtn" value="สมัครสมาชิก" style="width:100px">
        </form>
        <form method="post" action="index.php">
            <input type="submit" name="ibtn" value="สถานะจอง" style="width:100px">
        </form>
        </div>
        
        <div class="footer">
        
        </div>
    </div>    
        
    </body>
</html>

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
    <style>
    input[type=submit] {
    width: 100%;
    background-color: #4CAF50;
    color: white;
    padding: 18px 10px;
    margin: 8px 0;
    border: none;
    border-radius: 4px;
    cursor: pointer;
    }
    </style>
    <style>
        .tname{background-color:blanchedalmond; width:1000px; height:10%}
        .lname{float:left; background-color:blanchedalmond; height:300px; width:40%}
        .rname{float:left; background-color:blanchedalmond; height:300px; width:60%}
        .lname{}
    </style>
    <form method="post" action="insertmember.php">
        <div>
        <div style="width:1000px; margin:0 auto">
            <div class="tname">
                <h1 style="text-align:center">กรุณากรอกข้อมูล</h1>
            </div>
            
            <div class="lname">
               ชื่อ (Name)   :
            <select name="title" style="width:100px;">
                <option value="Mr">นาย/Mr.</option>
                <option value="Mrs">นางสาว/Mrs.</option>
            </select>
            <input type="text" name="fname">
            <br><br>
            มือถือ(Moblie Phone) : <input type="text" name="phone"><br>
            <sub>(กรุณากรอกให้ครบจำนวน10หลัก)</sub>
            <br>
            เลขบัตรประชาชน (ID Card): <input type="text" name="idcard"><br>
            <sub>(กรุณากรอกให้ครบ13หลัก)</sub>
            <br>
            กรุณาเลือกห้อง (Please select a room) :
            <select name="numroom" style="width:100px;">    
                <option value="suit">Suit room</option>
                <option value="vip">VIP  room</option>
                <option value="plain">Ordinary room</option>
            </select>
            <br><br>
            ผู้ใหญ่ (Adult) : <input type="text" name="adult" style="width:30px;">
            ผู้ใหญ่ (Child) : <input type="text" name="child" style="width:30px;">
            </div>
            
            <div class="rname">
                นามสกุล(Surname) : <input type="text" name="lname">
                <br><br>
                ที่อยู่ (Address) : <input size="31"type="text" name="address">
                <br><br>
                E-mail : <input type="text" name="email">
                <br><br>
                ราคา (Price) : <input type="text" name="price">
                <br><br>
            </div>
            
            <div class="lowname">
                <input type="submit" name="save" value="ตกลง" style="width:60px" onclick="alert('ทำการบันทึกข้อมูลเรียบร้อย')">
            </div>
        </div>
        
    </form>
    
      
    </body>
</html>

//PHPinsertmember
<?php
require './dbConnect.php';
$numroom=$_POST['room'];
$title=$_POST['title'];
$fname=$_POST['fname'];
$lname=$_POST['lname'];
$address=$_POST['address'];
$idcard=$_POST['idcard'];
$email=$_POST['email'];
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


