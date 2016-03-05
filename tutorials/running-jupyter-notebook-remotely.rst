Run Jupyter Notebook On a Remote Server
=========================================

If you have big data on a remote server, you might want to open a jupyter notebook directly on that server, rather than copying data down to your own computer. You can run a jupyter notebook on a linux server with no GUI interface and work on it from a browser window on your own computer. Here's how.

On the Remote server
-------------------

::

    jupyter notebook --no-browser --port=8889

On your Local Mac
----------------

**in terminal**::

  ssh -N -L localhost:8888:localhost:8889 <ssh-login@your-remote-machine>

  #  -N = tells server that no remote commands will be executed
  #  -L = where you list the port forwarding configuration (remote port 8889 to local port 8888)
  # optional:
  # -f = puts SSH into the background, so you can use that terminal window

**in a browser window**::

   localhost:8888       # displays the remotely running notebook


To Close the Notebook
----------------------

On Remote Server::

   ctrl-c, ctrl-c    #in the window where the notebook is running

On  Local Mac::

   ctrl-c, ctrl-c    #in the window where the notebook is running



**NOTE: if you put the process in the background (-f), you'll need to kill the process manually:**

Run ``top`` in your terminal window, and look for the PID # associated with the notebook task. Type ``q`` to exit out of ``top``, then ``kill <PID #>`` to kill the process.

