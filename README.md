# bashee
Some handy bash based linux scripts

jabs (just another backup script)
---------------------------------
Files:
- jabs
- jabs.cfg
- jabs.exclude

Usage:        jabs SWITCH [OPTIONS] [PARMS]
Description:  SWITCH = -D|-P|-H|-C|-E|-V|-F
              -----------------------------
              
              -C prints the configuration file content
              -D runs rsync in dry run mode (no changes)
              -E prints the exclude file content
              -F Checks the existence of configuration file parameter
              -H prints this help information
              -P runs rsync in production mode (changes will apply)
              -V prints jabs version number
              
              Note: Switches can not be combined.
              
              OPTIONS = [-l|-n] [-b|-s|-r] [-a|-z] [-v|-o|-i] [-d|-k] [-e|-x]
                  ---------------------------------------------------------------
               -a archiving incrementals on
               -b incremental backup mode
               -d delete objects on target, if deleted from source
               -e delete objects on target which matches the exclude file pattern
               -i STDERR and STDOUT write to logfile
               -k keep objects on target, if deleted from source
               -l logging on (logfile see $MYNAME.cfg)
               -n logging off
               -o STDERR and STDOUT are silent
               -r archiving incrementals only
               -s syncronization mode
               -v Standard output behavior
               -x keep objects on target which matches the exclude file pattern
               -z archiving incrementals off

               Note: Options may be combined. But some combinations are not allowed
                     Invalid option combinations are:
                     "-b -s -r", "-s -a", "-a -z", "-r -z", "-l -n", "-v -o -i",

                     Switch/option concatenations are possible. i.e "-Dlbao" etc.

               PARMS
               -----
               --cfg      Name of an alternative configuration file.
                          i.e --cfg="/dest/alternative.cfg"
                          Default is 'jabs.cfg'
               --ctype    [inet|wan] or [lan]. How to connect remote server.
                          [lan] = Remote server accessible over LAN.
                          [inet|wan] = remote server accessible over WAN (internet)
                          Default is 'lan'
               --delay    Time in seconds to wait until backup server is ready for connection. This can
                          speed up the jabs if the backup server is already running. In this case
                          it is recommended to set the delay as low as possible (i.e --delay=0)
                          Default is 0
               --domain   Domain name of remote server, f --ctype=inet|wan
                          No Default!
               --ip       IP address of remote server, if --ctype=lan
                          No Default!
               --excl     Provides the exclude file name. i.e --excl="/dest/jabs.exclude"
                          Default is 'jabs.exclude'
               --flow     [push|pull] defines the source->target direction. Implies 'REMOTE=y'
                          Default is 'pull'
               --logname  Name of the logfile
                          Default is '$HOME"/"$MYNAME"_"$(date \%Y-\%m-\%d_\%H-\%M)'
                          i.e '/home/username/jabs_2017-01-21_00-07'
               --mac      Is the MAC address of the remote server NIC.
                          No Default!
               --poff     If set to [yes|y|1], then the remote server has to be powered off after rsync.
                          If set to [no|n|0], then the remote server is keep running after rsync
                          Default is 'no'
               --panic    Panic treshold. If reached, process stops
                          Default is 0
               --target   Provides the backup destination. i.e --target="/dest/mybkupdest"
                          No Default!
               --retent   Retention periode of incrementals. If archiving is on, then all incrementals
                          older than the retention periode will be archived. If archiving is off, all
                          incrementals older than the retention periode will be deleted!
                          Default is 0
               --remote   [y|n]. Source or target resides on a remote server. Pls. see also '--flow'
                          Default is 'yes'
               --server   Name or IP address of the target. i.e --server=mybackupserver
                          No Default!
               --source   Provides the backup source.  i.e --source="/src/mybkupsource"
                          No Default!
               --ssh      [no|n|0] = no ssh, [yes|y|1] = ssh
                          Default is 'yes'
               --sshport  ssh port to use
                          Default is 22
               --timeout  Max timeout in seconds to wait for remote server
                          Default is 0
               --trace    [yes|y|1] = Switches the trace function on. i.e --trace=y
                          [no|n|0] = Switches the trace function off. i.e --trace=no
                          Default is 'no'
               --treshold Treshold. If reached, space calculation on target is necessary
                          Default is 0
               --user     User name on remote server. i.e --user=remoteshelluser
                          [lan] = Device accessible over LAN. [inet|wan] = device accessible over WAN (internet)
                          Default is '$USER'
               --version  prints jabs version number. Same as -V and is executed like OPTIONS.
                          i.e 'jabs --version'
               --wol      If set to [yes|y|1], then the remote server has to be waked up first. [no|n|0]
                          means do not wakeonlan remote server
                          Default is 'no'
               --wolport  wakeonlan port to use
                          Default is 9

               Note: For further infromation on source and destination, please refer to the rsync
                     man pages. In particular make sure to understand the meaning of using or not
                     using a trailing slash on sources. Trailing slashe on destination has no effect
                     at all. And it is always good practice to put source and destination between
                     quotes (double or single quotes) to prevent unexpected results when object names
                     containg special charaters (i.e blanks)

               Example: With the following command, jabs will backup your source to the target
                        on the given backup server by using a remote shell user. Incremental backup mode
                        as well as archive all incrementals which have reached the end of the retention period.
                        Logging is switched off. Updates are applied. But be carful.

                        ./jabs -banP --server="/mysource/" --target="/mydest" --server=server --user=username
                         
               Remarks: The command line arguments overwrites the configuration file parameters.
                        If configuration file parameters and the associated command line arguments are missing,
                        this may lead into a misleading behavior. The '-F' SWITCH will do a simple check of the
                        configuration file and its parameters. But the check is not conditional. It provides
                        information about missing, missing mandatory and missing optional parameters only. And
                        does not validate values and their context


wold (wake-on-lan proxy)
------------------------
Files:
- wold
- wold.cfg
- mac

 Usage:       jabs [OPTIONS] [PARMS]

              OPTIONS:
              --------
              -h             Prints short help information
              -v             Prints version information
              --help         Prints short help information (same as -h)
              --version      Prints version information (same as -v)

              PARMS:
              ------
              --cfg          Daemon configuration file
              --interface    Network interface to listen
              --log          Logfile name
              --mactable     MAC table to use
              --port         Port to listen

