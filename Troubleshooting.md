Following are general trouble shooting tips. The platform specific tips are [[Linux|Troubleshooting#linux]], [[windows|Troubleshooting#windows]], [[Mac OS|Troubleshooting#mac-os]] and [[Vagrant|Troubleshooting#vagrant]]:
  * If you get an error that ends with:

    ```
      fancy_urllib.InvalidCertificateException?: Host appengine.google.com returned
      an invalid certificate (ssl.c:507: error:14090086:SSL
      routines:SSL3_GET_SERVER_CERTIFICATE:certificate verify failed):
    ```

    try removing the `cacerts.txt` and `urlfetch_cacerts.txt` files as described [here](http://stackoverflow.com/questions/13899530/gae-sdk-1-7-4-and-invalidcertificateexception) and [here](http://stackoverflow.com/questions/17777994/why-cant-i-launch-my-app-from-the-shell).


  * If you get an error that ends with:

    ```
      File "/usr/lib/python2.7/ssl.py", line 405, in do_handshake
    self._sslobj.do_handshake()
      IOError: [Errno socket error] [Errno 1] _ssl.c:510: error:14077410:SSL routines:SSL23_GET_SERVER_HELLO:sslv3 alert handshake failure
    ```

    try upgrading your python, follow these steps:
    - `sudo apt-get update`
    - `sudo apt-get install --only-upgrade python2.7`

    **Note:** This issue will only raise if your python version is < 2.7.9.
 
 * If you get 403 error while serving oppia locally, this can be because you are working behind a proxy.

    go to `oppia_tools/google_app_engine_1.9.XX/google_appengine/google/appengine/tools` and open the `appengine_rpc.py` file. Comment the following line in it. `opener.add_handler(fancy_urllib.FancyProxyHandler())` . Run the server again.
 [Resource](https://stackoverflow.com/questions/16698621/google-app-engine-error-httperror/17522082)
 

### Linux

  * If you get an error that ends with:

    ```
    ssl.PROTOCOL_SSLv3: OpenSSL.SSL.SSLv3_METHOD,
    AttributeError: 'module' object has no attribute 'PROTOCOL_SSLv3'
    ```

    try commenting out the line

    ```
      '../oppia_tools/google_appengine_1.9.19/google_appengine/lib/requests/requests/packages/urllib3/contrib/pyopenssl.py:70': (ssl.PROTOCOL_SSLv3: OpenSSL.SSL.SSLv3_METHOD,)
    ```
    as described [here](https://code.google.com/p/googleappengine/issues/detail?id=11539#c2).

  * If you get an error that ends with:

    ```
      File "numpy/setup.py", line 5, in configuration
        config = Configuration('numpy',parent_package,top_path)
      File "/tmp/pip-build-4rsvws0b/numpy/build/py3k/numpy/distutils/misc_util.py", line 743, in __init__
        raise ValueError("%r is not a directory" % (package_path,))
    ValueError: 'build/py3k/numpy' is not a directory
    Converting to Python3 via 2to3...
    ```
    make sure that your default `pip` is using Python 2, not Python 3. See [#1742](https://github.com/oppia/oppia/issues/1742).

  * If you get an error that looks something like this:

    ```
    error: can't combine user with prefix, exec_prefix/home, or install_(plat)base

    ----------------------------------------
    Cleaning up...
    Command /usr/bin/python -c "import setuptools, tokenize;__file__='/tmp/pip-build-xiOWdq/numpy/setup.py';exec(compile(getattr(tokenize, 'open', open)(__file__).read().replace('\r\n', '\n'), __file__, 'exec'))" install --record /tmp/pip-t7c_y1-record/install-record.txt --single-version-externally-managed --compile --user --home=/tmp/tmpP9jGIf failed with error code 1 in /tmp/pip-build-xiOWdq/numpy
    Traceback (most recent call last):
      File "/usr/bin/pip", line 9, in <module>
        load_entry_point('pip==1.5.6', 'console_scripts', 'pip')()
      File "/usr/lib/python2.7/dist-packages/pip/__init__.py", line 248, in main
        return command.main(cmd_args)
      File "/usr/lib/python2.7/dist-packages/pip/basecommand.py", line 161, in main
        text = '\n'.join(complete_log)
    UnicodeDecodeError: 'ascii' codec can't decode byte 0xe2 in position 72: ordinal not in range(128)
    ```
    try updating the version of pip to v8.1.2 (as described in [this comment](https://github.com/oppia/oppia/issues/1580#issuecomment-218423065)).

  * If you get an error that ends with either:

    ```
    File "/usr/local/Cellar/python/2.7.11/Frameworks/Python.framework/Versions/2.7/lib/python2.7/distutils/command/install.py", line 264, in finalize_options
    "must supply either home or prefix/exec-prefix -- not both"
    DistutilsOptionError: must supply either home or prefix/exec-prefix -- not both
    ```

    or

    ```
    ImportError: No module named _ctypes
    ```

    please ensure that you are using Python 2. Also, if your system already has numpy installed, please ensure that its version is 1.6.1 (since that is the only one compatible with Google App Engine). For more details, see [this issue](https://github.com/oppia/oppia/issues/1545).

  * If you get an error that ends with:

    ```
    File "/oppia_tools/google_appengine_1.9.50/google_appengine/google/appengine/dist27/socket.py", line 73, in 
    from _ssl import RAND_add, RAND_egd, RAND_status, SSL_ERROR_ZERO_RETURN, SSL_ERROR_WANT_READ, SSL_ERROR_WANT_WRITE, SSL_ERROR_WANT_X509_LOOKUP, SSL_ERROR_SYSCALL, SSL_ERROR_SSL, SSL_ERROR_WANT_CONNECT, SSL_ERROR_EOF, SSL_ERROR_INVALID_ERROR_CODE
    ImportError: cannot import name RAND_egd
    ```
    go to `oppia_tools/google_appengine_1.9.50/google_appengine/google/appengine/dist27` and open the `socket.py` file. In this file go to the line 73 (or, alternatively, search for ‘RAND_egd’) and remove import of ‘RAND_egd’ from that line.

### Mac OS
  * If you get an error that includes:

    ```
    clang: error: invalid argument '-faltivec' only allowed with 'ppc/ppc64/ppc64le'
    clang: error: invalid argument '-faltivec' only allowed with 'ppc/ppc64/ppc64le'
    clang: error: invalid argument '-faltivec' only allowed with 'ppc/ppc64/ppc64le'
    clang: error: invalid argument '-faltivec' only allowed with 'ppc/ppc64/ppc64le'
    error: Command "gcc -fno-strict-aliasing -fno-common -dynamic -arch x86_64 -arch i386 -g -Os -pipe -fno-common -fno-strict-aliasing -fwrapv -DENABLE_DTRACE -DMACOSX -DNDEBUG -Wall -Wstrict-prototypes -Wshorten-64-to-32 -DNDEBUG -g -fwrapv -Os -Wall -Wstrict-prototypes -DENABLE_DTRACE -arch x86_64 -arch i386 -pipe -DNO_ATLAS_INFO=3 -Inumpy/core/blasdot -Inumpy/core/include -Ibuild/src.macosx-10.10-intel-2.7/numpy/core/include/numpy -Inumpy/core/src/private -Inumpy/core/src -Inumpy/core -Inumpy/core/src/npymath -Inumpy/core/src/multiarray -Inumpy/core/src/umath -Inumpy/core/include -I/System/Library/Frameworks/Python.framework/Versions/2.7/include/python2.7 -Ibuild/src.macosx-10.10-intel-2.7/numpy/core/src/multiarray -Ibuild/src.macosx-10.10-intel-2.7/numpy/core/src/umath -c numpy/core/blasdot/_dotblas.c -o build/temp.macosx-10.10-intel-2.7/numpy/core/blasdot/_dotblas.o -faltivec -I/System/Library/Frameworks/vecLib.framework/Headers" failed with exit status 1
    ```

  please see the instructions in [issue #1179](https://github.com/oppia/oppia/issues/1179) for a fix.

  * If you get an error that includes:

   ```
   GitPython is not installed for Python 2.x
   The 'dist' command will not work without it. Get it using pip or easy_install
   ```
   please install [GitPython](http://gitpython.readthedocs.io/en/stable/intro.html#installing-gitpython) before proceeding further.

  * If you get an error that includes:

   ```
   No Java runtime present, requesting install.
   closure-compiler failed.
   ```
   please download [Java](https://support.apple.com/kb/DL1572?locale=en_US) and install it.

  * if you get an error that includes:
   
   ```
   Checking whether Skulpt is installed in third_party
   cp: /Users/sdawson/opensource/oppia_tools/skulpt-0.10.0/skulpt/dist/*: No such file or directory
   ```
   please remove the below mentioned directories and try running `start.sh` again:
   ```
   ../oppia_tools/
   ../node_modules/
   third_party/
   core/templates/prod/
   ```
   
  * if you run into issues while installing numpy, and the error message looks something like this:

   ```
   Collecting numpy==1.6.1
  Downloading numpy-1.6.1-cp27-none-macosx_10_6_intel.macosx_10_9_intel.macosx_10_9_x86_64.whl (11.6MB)
    100% |████████████████████████████████| 11.6MB 104kB/s 
Installing collected packages: numpy
Exception:
Traceback (most recent call last):
  File "/usr/local/lib/python2.7/site-packages/pip/basecommand.py", line 215, in main
    status = self.run(options, args)
  File "/usr/local/lib/python2.7/site-packages/pip/commands/install.py", line 317, in run
    prefix=options.prefix_path,
  File "/usr/local/lib/python2.7/site-packages/pip/req/req_set.py", line 742, in install
    **kwargs
  File "/usr/local/lib/python2.7/site-packages/pip/req/req_install.py", line 831, in install
    self.move_wheel_files(self.source_dir, root=root, prefix=prefix)
  File "/usr/local/lib/python2.7/site-packages/pip/req/req_install.py", line 1032, in move_wheel_files
    isolated=self.isolated,
  File "/usr/local/lib/python2.7/site-packages/pip/wheel.py", line 247, in move_wheel_files
    prefix=prefix,
  File "/usr/local/lib/python2.7/site-packages/pip/locations.py", line 153, in distutils_scheme
    i.finalize_options()
  File "/usr/local/Cellar/python/2.7.11/Frameworks/Python.framework/Versions/2.7/lib/python2.7/distutils/command/install.py", line 264, in finalize_options
    "must supply either home or prefix/exec-prefix -- not both"
DistutilsOptionError: must supply either home or prefix/exec-prefix -- not both
   ```
   this StackOverflow [answer](http://stackoverflow.com/a/24357384) provides a possible fix.

  * if you run into an error that looks like this when starting App Engine:

   ```
     File "/System/Library/Frameworks/Python.framework/Versions/2.7/lib/python2.7/urllib2.py", line 558, in http_error_default
       raise HTTPError(req.get_full_url(), code, msg, hdrs, fp)
   HTTPError: HTTP Error 503: Service Unavailable
   ```
   please take a look at this StackOverflow [answer](https://stackoverflow.com/a/19460147) and see if it helps.

### Windows

It's possible that windows firewall might be preventing localhost:8181 to launch. In such a case, you should re-config the firewall by adding new inbound rule so that ports 8181 and 8000 are allowed. (Instruction about how to add inbound rules can be found [here](https://msdn.microsoft.com/en-us/library/hh168549(v=nav.90).aspx))

### Vagrant

- If you run `git commit` from the host machine, you will likely have your commit rejected because you have not installed the pre-commit hooks. The hooks only install after you have run Oppia for the first time on a machine. Since you are actually installing and running Oppia on a VM, those hooks do not exist on the host. There are several ways to overcome this:
  - (Recommended) Do `git commit` and `git push` from the guest. This is actually not as difficult or burdensome as it may sound: All directories are mapped into the Vagrant VM, including `.git`, so configurations such as your username and e-mail will carry over as well.
  - Try to build Oppia natively on Windows (this is difficult, and is neither recommended nor supported).
  - Note that doing a `git push` using SSH will not work, since the guest machine cannot see your host's private key. If you want to use SSH, you can add the Vagrant VM's public key to your account, but *this is NOT RECOMMENDED*! Vagrant uses the same SSH key for all machines, so anyone could write to any of your repos. 
-  If Vagrant prints an error involving `\r not found`, the recommended fix is to ensure you have the [appropriate line endings set up](#prerequisites) and then clone your repo down again after copying out or saving any work.
-  If the service reports that it starts, but then terminates and your vagrant install doesn't respond to port 8181 or 8000. Then try to delete the oppia_tools directory and re-run the scripts/start.sh to reinstall.


### If the above doesn't work...

If you run into any issues with the installation process, please let us know by [filing an issue](https://github.com/oppia/oppia/issues/new?title=Describe%20your%20feature%20request%20or%20bug%20report%20succinctly&body=If%20you%27d%20like%20to%20propose%20a%20feature,%20describe%20what%20you%27d%20like%20to%20see.%20Mock%20ups%20would%20be%20great!%0A%0AIf%20you%27re%20reporting%20a%20bug,%20please%20be%20sure%20to%20include%20the%20expected%20behaviour,%20the%20observed%20behaviour,%20and%20steps%20to%20reproduce%20the%20problem.%20Console%20copy-pastes%20and%20any%20background%20on%20the%20environment%20would%20also%20be%20helpful.%0A%0AThanks!). Thanks!