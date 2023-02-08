Choose the Licence Management pear.

Create new licence pool.
<img width="961" alt="Screenshot 2023-02-08 at 11 09 08" src="https://user-images.githubusercontent.com/119075926/217491589-11ad1d30-5d5e-49c9-9417-4d0ce047e73a.png">


Add pears to this pool.

 /opt/splunk/etc/system/local/server.conf
 [lmpool:<POOL_NAME>]
 peers = <pears_guid>,<pears_guid>
 quota = MAX
 stack_id = enterprise
<img width="664" alt="Screenshot 2023-02-08 at 11 36 38" src="https://user-images.githubusercontent.com/119075926/217491755-ff9ec09a-5938-4961-a304-9aabbcf3daf4.png">


At the license pears.
<img width="932" alt="Screenshot 2023-02-08 at 11 37 19" src="https://user-images.githubusercontent.com/119075926/217491931-b65ddc86-ec1d-4dee-a854-43f77159cf37.png">

<img width="353" alt="Screenshot 2023-02-08 at 11 36 54" src="https://user-images.githubusercontent.com/119075926/217491815-c6888232-2ab3-426c-b07a-a56f207574c4.png">
