1. To disable the ssh authenticity prompts , add the following lines to the beginning of /etc/ssh/ssh_config (just behind " Host ...." )

   StrictHostKeyChecking=no
   UserKnownHostsFile=/dev/null
# SAC
