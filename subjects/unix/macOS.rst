=====
macOS
=====

Time Machine
============

.. code-block:: bash
  
  # https://www.reddit.com/r/apple/comments/5vb66w/speed_up_your_time_machine_backups_tremendously/
  # Speed up backups at the expense of CPU utilisation:
  sudo sysctl debug.lowpri_throttle_enabled=0
  # Go back to default after backup is done:
  sudo sysctl debug.lowpri_throttle_enabled=1


  # Mount disk via AFP
  # https://apple.stackexchange.com/questions/162544/how-to-restore-system-from-network-drive
  mount -t afp afp://adminname:password@ServerIPAddress/ShareName /Volumes/TimeMachine
  hdid /Volumes/TimeMachine/NameOfYourSparseBundle






Useful SW
=========

- sshfs
- snipping-tool(Latex reader)