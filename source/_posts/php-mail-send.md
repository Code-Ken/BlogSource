title: PHP邮件的发送
date: 2016-01-08 11:58:20
categories: PHP
tags: [PHP,PHPMailer]
---

php虽然提供了mail()函数，但并不好用，而PHPMailer是一个不错的邮件发送工具，使用起来也是非常简单！

<!--more-->

```
require_once ('class.phpmailer.php');

/**
 * 发送邮件
 * @param $array $sendto_email 发件人
 * @param $subject  主题
 * @param $msg 内容
 * @throws phpmailerException
 */
function smtp_mail($sendto_email = $array(), $subject, $msg)
{ 
   global $g_email_host;
   global $g_email_user_name;
   global $g_email_password;
   global $g_email_sender_account;
   global $g_email_sender_name;
   $mail = new PHPMailer();    
   $mail->IsSMTP();                  
   $mail->Host = $g_email_host;     
   $mail->SMTPAuth = true;          
   $mail->Username = $g_email_user_name;     //发件人邮箱 
   $mail->Password = $g_email_password; // 发件人邮箱密码   
   $mail->From = $g_email_sender_account;      // 发件人邮箱    
   $mail->FromName =  $g_email_sender_name;  
  
   $mail->CharSet = "UTF-8";      
   $mail->setLanguage('zh_cn');   
   $mail->Encoding = "base64";    
   foreach($sendto_email as $val)
   {
      $mail->AddAddress($val,""); 
   }
   
  
   $mail->Subject = $subject;      
   $mail->Body =$msg;                                                                  
   $date = date('Y-m-d',time());  
   if(!$mail->Send())    
   {       
      echo "邮件错误信息: " . $mail->ErrorInfo;
   }
   else
   {
      file_put_contents("{$date}.log", date('Y-m-d H:i:s', time()) . "邮件发送成功 \n", FILE_APPEND);
   }

}
```
