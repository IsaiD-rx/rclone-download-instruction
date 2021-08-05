  

1. Create a remote called ‘gdrive (or any name you like)’. 

   * Open a terminal window (terminal 1). After installing rclone on the server, type `rclone config`, you will see something like this:

     <img src="/Users/rli12/Desktop/Picture1.png" alt="Picture1" style="zoom:50%;" />

     Choose `n`. If you do not have a remote before, you will not see those ‘current remotes’ things.

   * Then you need to input the remote name. Here we use ‘drive’. After inputting the name, and you will see something like this: 

     <img src="/Users/rli12/Desktop/Screen Shot 2021-08-05 at 12.02.13 AM.png" alt="Screen Shot 2021-08-05 at 12.02.13 AM" style="zoom:30%;" />

     We are using Google drive, so input `15`. 

   * Then you will see the request for client_id and client_secret. 

     <img src="/Users/rli12/Desktop/Screen Shot 2021-08-05 at 12.06.19 AM.png" alt="Screen Shot 2021-08-05 at 12.06.19 AM" style="zoom:40%;" />

     The client_id and client_secret should be created following the instructions: https://rclone.org/drive/#making-your-own-client-id
     Please make sure that the Google account used to create the client_id and client_secret is a generic account.

   * After you input the client_id and client_secret, you should see the option to set the scope that rclone uses. 

     <img src="/Users/rli12/Desktop/Picture11.png" alt="Picture11" style="zoom:40%;" />

   * Then you should see the request for ‘root_folder_id’. Just press enter and leave this blank.

   * Then you should see the request for ‘service_account_file’. Also press enter and leave this blank. 

   * Then you should see this. Type `n`.

     <img src="/Users/rli12/Desktop/Screen Shot 2021-08-05 at 12.17.54 AM.png" alt="Screen Shot 2021-08-05 at 12.17.54 AM" style="zoom:40%;" />

   * Then you should see the question `Use auto config?`. Choose `n`. 

     <img src="/Users/rli12/Desktop/Picture12.png" alt="Picture12" style="zoom:50%;" />

   * Then a link would be given to get a verification code. After inputting the verification code, it will ask `configure this as a Shared Drive (Team Drive)?`. We use default (`n`) here. Then it will give you the information of the remote. After confirming the information, the remote called ‘gdrive’ would be created successfully. 

   * Then type `q` to exit the config. 

2. Create a directory by `mkdir drive` (or any name you like) on the server. Then mount the directory using command `rclone mount gdrive: drive`. **Keep current terminal window (terminal 1) open.** 

3. Open another terminal window (terminal 2) and enter the folder 'drive' you created in step2. You should see all the files in your Google account. 
   Keep using the current terminal window( terminal 2).  Create a storage directory by `mkdir local` (or any name you like) to download the files. You could download the data using command `rsync -auv drive/ local`.  

**You always need to mount the ‘drive’ folder when you are downloading.** If the connection is interrupted, you could re-mount the 'drive' folder. First type `fusermount -uz ~/gdrive` and then re-mount using command`rclone mount gdrive: drive`. 

 

 

