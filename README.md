# Setup the shared folder

1. Request the access to the shared file using **a generic Google account**. 

2. After obtaining access, go to your Google Drive using your browser.
In the left panel of the web page, click on "Shared with me", and
you should see the shared folder show up.

3. Right-click on the shared folder, then select "Add shortcut to Drive" 
and choose a suitable location under "My Drive". *Remember this location.*


# Install rclone

If your system does not come with `rclone` already, and you do not
have permission to install packages globally, then you can install `rclone`
locally to your home directory.

To do so, you can download a pre-compiled binary (x86_64) of `rclone` by

```
curl -O https://downloads.rclone.org/rclone-current-linux-amd64.zip
unzip rclone-current-linux-amd64.zip
cd rclone-*-linux-amd64
```

After this, rclone can be invoked using its full or relative path,
such as `./rclone`.


# Configuring rclone for gdrive  

1. On the machine where you wish to download the files, create a remote called `gdrive`
   (or any name you like) as follows:

   * Open a terminal window (**terminal 1**). After installing rclone on the server, type `rclone config`, you will see something like this:

      ```
      Current remotes:

      Name                 Type
      ====                 ====

      e) Edit existing remote
      n) New remote
      d) Delete remote
      r) Rename remote
      c) Copy remote
      s) Set configuration password
      q) Quit config
      e/n/d/r/c/s/q>
      ```

     Choose `n`. If you do not have a remote before, you will not see those ‘current remotes’ things.

   * Then you need to input the remote name. Here we use ‘drive’. After inputting the name, and you will see something like this: 

      ```
      13 / FTP Connection
         \ "ftp"
      14 / Google Cloud Storage (this is not Google Drive)
         \ "google cloud storage"
      15 / Google Drive
         \ "drive"
      16 / Google Photos
         \ "google photos"
      17 / Hadoop distributed file system
         \ "hdfs"
      18 / Hubic
         \ "hubic"
      19 / In memory object storage system.
         \ "memory"
      20 / Jottacloud
         \ "jottacloud"
      21 / Koofr
         \ "koofr"
      22 / Local Disk
         \ "local"
      23 / Mail.ru Cloud
         \ "mailru"
      24 / Mega
         \ "mega"
      25 / Microsoft Azure Blob Storage
         \ "azureblob"
      26 / Microsoft OneDrive
       \ "onedrive"
      27 / OpenDrive
         \ "opendrive"
      28 / OpenStack Swift (Rackspace Cloud Files, Memset Memstore, OVH)
         \ "swift"
      29 / Pcloud
         \ "pcloud"
      30 / Put.io
         \ "putio"
      31 / QingCloud Object Storage
         \ "qingstor"
      32 / SSH/SFTP Connection
         \ "sftp"
      33 / Sugarsync
         \ "sugarsync"
      34 / Tardigrade Decentralized Cloud Storage
         \ "tardigrade"
      35 / Transparently chunk/split large files
         \ "chunker"
      36 / Union merges the contents of several upstream fs
         \ "union"
      37 / Webdav
         \ "webdav"
      38 / Yandex Disk
         \ "yandex"
      39 / Zoho
         \ "zoho"
      40 / http Connection
         \ "http"
      41 / premiumize.me
         \ "premiumizeme"
      42 / seafile
         \ "seafile"
      ```
     We are using Google drive, so input `15`. 

   * Then you will see the request for client_id and client_secret. 

     ```
     ** See help for drive backend at: https://rclone.org/drive/ **

      Google Application Client Id
      Setting your own is recommended.
      See https://rclone.org/drive/#making-your-own-client-id for how to create your own.
      If you leave this blank, it will use an internal key which is low performance.
      Enter a string value. Press Enter for the default ("").
      client_id>
     ```

     The client_id and client_secret should be created following the instructions: https://rclone.org/drive/#making-your-own-client-id

     Please make sure that the Google account used to create the client_id and client_secret is a generic account.

   * After you input the client_id and client_secret, you should see the option to set the scope that rclone uses. 

     ```
     Choose a number from below, or type in your own value
      1 / Full access all files, excluding Application Data Folder.
         \ "drive"
      2 / Read-only access to file metadata and file contents.
         \ "drive.readonly"
         / Access to files created by rclone only.
      3 | These are visible in the drive website.
         | File authorization is revoked when the user deauthorizes the app.
         \ "drive.file"
         / Allows read and write access to the Application Data folder.
      4 | This is not visible in the drive website.
         \ "drive.appfolder"
         / Allows read-only access to file metadata but
      5 | does not allow any access to read or download file content.
         \ "drive.metadata.readonly"
      scope> 1
     ```

   * You should see the request for ‘root_folder_id’. Just press enter and leave this blank.

   * You should see the request for ‘service_account_file’. Also press enter and leave this blank. 

   * You should see this. Type `n`.

     ```
     Edit advanced config? (y/n)
      y) Yes
      n) No (default)
      y/n> 
     ```

   * You should see the question `Use auto config?`. Choose `n`. 

     ```
     Use auto config?
      * Say Y if not sure
      * Say N if you are working on a remote or headless machine
      y) Yes (default)
      n) No
      y/n> 
     ```

   * Then, a link would be given to get a verification code.

   * You will be asked: it will ask whether to configure this as a Shared Drive.

     ```
     Configure this as a Shared Drive (Team Drive)?
      y) Yes
      n) No (default)
      y/n>
     ```

     We say no (`n`) here.

   * Then, it will give you the information of the remote. After confirming the information, the remote called `gdrive` would be created successfully. 

   * Finally, type `q` to exit the config. 

2. Mount the remote Google drive on the machine:

   * Request the access to the shared file using your generic account. 
   
   * After having the access, go to the Google account,  and you should find the shared file under ''shared with me''. 
   
   * Right-click on the shared file, then select "Add shortcut to Drive" and choose a suitable location.
  
   * Create a directory by `mkdir drive` (or any name you like).

   * Then, mount the directory using command `rclone mount gdrive: drive`.

   * Keep this terminal window (**terminal 1**) in order to keep the 
     remote drive mounted.

   * Open a new terminal window (**terminal 2**) and `cd drive`
     to navigate to the remote drive.

   * You should be able to see all your Google Drive files,
   including the shortcut to the shared folder at the location you had 
   chosen above, in the remote drive by `ls`.

3. Download the files from the remote Google drive using `rsync`:

   * Open another temrinal window (**terminal 3**). 
     Create a directory to store the data by `mkdir local`
     (or any name you like).

   * Suppose you had created the shortcut to the shared folder in your Google
   Drive at `shared` and the foler that we shared with you is named `project`,
   then you can now download the data using the command
   `rsync -auv drive/shared/project local`.
   Please be sure to replace `drive/shared/project` with the actual path to
   your shared folder.


## Remarks

You keep to keep the remote `drive` mounted while you are downloading.
If you terminate `rclone` or if your connection is interrupted,
you would need to re-mount the `drive` folder:

* First, cleanly unmount the remote drive by `fusermount -uz gdrive`.

* Then, re-mount the drive by `rclone mount gdrive: drive`. 

