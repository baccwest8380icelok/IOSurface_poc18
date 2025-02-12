    # CVE-2024-44285: IOSurface UaF

    This bug was introduced in iOS 18 and it is relatively shallow as it occurs in the second function after the base `s_method` call.  
    The vulnerable function lies in `s_set_corevideo_bridged_keys`, which is method #54 on iOS and #57 on macOS.  

    A pre-patch version of the vulnerable function can be seen below:

    ![Pre-Patch Function](images/function.png)

    Diffing the two versions shows the patch very clearly:

    ![Diffing the two functions](images/diff.png)

    A single lock was missing from the pre-patch version. Calling this method from multiple threads it is possible to over-release an OSArray. (Funnily enough, it seems that that while in the KEXT the locking mechanism was missed, in the userland library this method is correctly called via locks)1. [linux](https://github.com/torvalds/linux) [26591, 10475]: *Linux kernel source tree*
    2. [redis](https://github.com/antirez/redis) [15162, 5153]: *Redis is an in-memory database that persists on disk. The data model is key-value, but many different kind of values are supported: Strings, Lists, Sets, Sorted Sets, Hashes, HyperLogLogs, Bitmaps.*
    3. [git](https://github.com/git/git) [10618, 6172]: *Git Source Code Mirror - This is a publish-only repository and all pull requests are ignored. Please follow Documentation/SubmittingPatches procedure for any of your improvements.*
    4. [How-to-Make-a-Computer-Operating-System](https://github.com/SamyPesse/How-to-Make-a-Computer-Operating-System) [9104, 1605]: *How to Make a Computer Operating System in C++*
    5. [emscripten](https://github.com/kripken/emscripten) [8976, 1072]: *Emscripten: An LLVM-to-JavaScript Compiler*
    6. [toxcore](https://github.com/irungentoo/toxcore) [7371, 907]: *The future of online communications.*
    7. [The-Art-Of-Programming-By-July](https://github.com/julycoding/The-Art-Of-Programming-By-July) [7179, 3646]: *本github已于14年6月基本停止更新，完整精致的纸质版《编程之法：面试和算法心得》已在异步/互动上销售！*
    8. [the_silver_searcher](https://github.com/ggreer/the_silver_searcher) [7141, 508]: *A code-searching tool similar to ack, but faster.*
    9. [php-src](https://github.com/php/php-src) [7008, 2549]: *The PHP Interpreter*
    10. [wrk](https://github.com/wg/wrk) [6625, 422]: *Modern HTTP benchmarking tool*
    11. [jq](https://github.com/stedolan/jq) [5489, 283]: *Command-line JSON processor*
    12. [libgit2](https://github.com/libgit2/libgit2) [5192, 1240]: *The Library*
    13. [macvim](https://github.com/b4winckler/macvim) [5070, 580]: *Vim - the text editor - for Mac OS X*
    14. [h2o](https://github.com/h2o/h2o) [4934, 350]: *H2O - the optimized HTTP/1, HTTP/2 server*
    15. [ccv](https://github.com/liuliu/ccv) [4802, 1094]: *C-based/Cached/Core Computer Vision Library, A Modern Computer Vision Library*
    16. [fish-shell](https://github.com/fish-shell/fish-shell) [4779, 465]: *The user-friendly command line shell.*
    17. [godot](https://github.com/okamstudio/godot) [4708, 1256]: *Godot Game Engine*
    18. [mjolnir](https://github.com/sdegutis/mjolnir) [4706, 129]: *Lightweight automation and productivity app for OS X*
    19. [TextSecure](https://github.com/WhisperSystems/TextSecure) [4552, 1353]: *A private messenger for Android.*
    20. [twemproxy](https://github.com/twitter/twemproxy) [4539, 909]: *A fast, light-weight proxy for memcached and redis*
    21. [WinObjC](https://github.com/Microsoft/WinObjC) [4464, 488]: *Objective-C for Windows*
    22. [robotjs](https://github.com/octalmage/robotjs) [4144, 140]: *Node.js Desktop Automation. *
    23. [memcached](https://github.com/memcached/memcached) [3947, 1341]: *memcached development tree*
    24. [disque](https://github.com/antirez/disque) [3875, 223]: *Disque is a distributed message broker*
    25. [masscan](https://github.com/robertdavidgraham/masscan) [3825, 561]: *TCP port scanner, spews SYN packets asynchronously, scanning entire Internet in under 5 minutes.*
    26. [firefox-ios](https://github.com/mozilla/firefox-ios) [3799, 678]: *Firefox for iOS*
    27. [Telegram](https://github.com/DrKLO/Telegram) [3661, 1783]: *Telegram for Android source*
    28. [streem](https://github.com/matz/streem) [3632, 218]: *prototype of stream based programming language*
    29. [vim.js](https://github.com/coolwanglu/vim.js) [3535, 208]: *JavaScript port of Vim*
    30. [libuv](https://github.com/joyent/libuv) [3444, 782]: *Go to *
    31. [tengine](https://github.com/alibaba/tengine) [3421, 1014]: *A distribution of Nginx with some advanced features*
    32. [grpc](https://github.com/grpc/grpc) [3413, 514]: *The C based gRPC (C++, Node.js, Python, Ruby, Objective-C, PHP, C#)*
    33. [Craft](https://github.com/fogleman/Craft) [3332, 438]: *A simple Minecraft clone written in C using modern OpenGL (shaders).*
    34. [seafile](https://github.com/haiwen/seafile) [3280, 581]: *Open source cloud storage with file encryption and group sharing, and emphasis on reliability and high performance. *
    35. [watchman](https://github.com/facebook/watchman) [3261, 249]: *Watches files and records, or triggers actions, when they change. *
    36. [pyenv](https://github.com/yyuu/pyenv) [3230, 236]: *Simple Python version management*
    37. [mruby](https://github.com/mruby/mruby) [3184, 486]: *Lightweight Ruby*
    38. [tmux](https://github.com/tmux/tmux) [3133, 175]: *tmux source code*
    39. [redcarpet](https://github.com/vmg/redcarpet) [3091, 346]: *The safe Markdown parser, reloaded.*
    40. [css-layout](https://github.com/facebook/css-layout) [3014, 147]: *Reimplementation of CSS layout using pure JavaScript*
    41. [flinux](https://github.com/wishstudio/flinux) [2997, 159]: *Foreign LINUX - Run unmodified Linux applications inside Windows.*
    42. [lwan](https://github.com/lpereira/lwan) [2904, 393]: *Experimental, scalable, high performance HTTP server*
    43. [fastsocket](https://github.com/fastos/fastsocket) [2896, 505]: *Fastsocket is a highly scalable socket and its underlying networking implementation of Linux kernel. With the straight linear scalability, Fastsocket can provide extremely good performance in multicore machines. In addition, it is very easy to use and maintain. As a result, it has been deployed in the production environment of SINA.*
    44. [skynet](https://github.com/cloudwu/skynet) [2877, 1412]: *A lightweight online game framework*
    45. [torch7](https://github.com/torch/torch7) [2847, 646]: **
    46. [beanstalkd](https://github.com/kr/beanstalkd) [2784, 371]: *Beanstalk is a simple, fast work queue.*
    47. [progress](https://github.com/Xfennec/progress) [2640, 146]: *Linux tool to show progress for cp, rm, dd, ...*
    48. [FFmpeg](https://github.com/FFmpeg/FFmpeg) [2631, 1641]: *mirror of git://source.ffmpeg.org/ffmpeg.git*
    49. [Cello](https://github.com/orangeduck/Cello) [2611, 188]: *Higher level programming in C*
    50. [tig](https://github.com/jonas/tig) [2501, 196]: *Text-mode interface for git*
    51. [libuv](https://github.com/libuv/libuv) [2474, 360]: *Cross-platform asychronous I/O*
    52. [swoole-src](https://github.com/swoole/swoole-src) [2494, 825]: *Asynchronous & concurrent & distributed networking framework for PHP.*
    53. [p2pvc](https://github.com/mofarrell/p2pvc) [2392, 157]: *A point to point color terminal video chat.*
    54. [linux](https://github.com/raspberrypi/linux) [2389, 1261]: **
    55. [mpv](https://github.com/mpv-player/mpv) [2381, 314]: *Video player based on MPlayer/mplayer2*
    56. [pifs](https://github.com/philipl/pifs) [2369, 132]: *πfs - the data-free filesystem!*
    57. [micropython](https://github.com/micropython/micropython) [2360, 459]: *MicroPython - a lean and efficient Python implementation for microcontrollers and constrained systems*
    58. [c4](https://github.com/rswier/c4) [2322, 393]: *C in four functions*
    59. [http-parser](https://github.com/nodejs/http-parser) [2311, 588]: *http request/response parser for c*
    60. [nanomsg](https://github.com/nanomsg/nanomsg) [2304, 333]: *nanomsg library*
    61. [stb](https://github.com/nothings/stb) [2291, 264]: *stb single-file public domain libraries for C/C++*
    62. [nginx-rtmp-module](https://github.com/arut/nginx-rtmp-module) [2285, 655]: *NGINX-based Media Streaming Server*
    63. [numpy](https://github.com/numpy/numpy) [2256, 1169]: *Numpy main repository*
    64. [shadowsocks-android](https://github.com/shadowsocks/shadowsocks-android) [2241, 1964]: *A Shadowsocks client for Android*
    65. [SoftEtherVPN](https://github.com/SoftEtherVPN/SoftEtherVPN) [2227, 670]: *A Free Cross-platform Multi-protocol VPN Software, developed by SoftEther VPN Project at University of Tsukuba, Japan.*
    66. [Cinder](https://github.com/cinder/Cinder) [2194, 544]: *Cinder is a community-developed, free and open source library for professional-quality creative coding in C++.*
    67. [curl](https://github.com/bagder/curl) [2181, 776]: *Curl is a tool and libcurl is a library for transferring data with URL syntax, supporting FTP, FTPS, HTTP, HTTPS, GOPHER, TFTP, SCP, SFTP, TELNET, DICT, LDAP, LDAPS, FILE, IMAP, SMTP, POP3, RTSP and RTMP. libcurl offers a myriad of powerful features*
    68. [ijkplayer](https://github.com/Bilibili/ijkplayer) [2172, 794]: *Android/iOS video player based on FFmpeg n2.8, with MediaCodec, VideoToolbox support.*
    69. [vim](https://github.com/vim/vim) [2165, 135]: *The official Vim repository*
    70. [libfreenect](https://github.com/OpenKinect/libfreenect) [2160, 806]: *Drivers and libraries for the Xbox Kinect device on WIndows, Linux, and OS X*
    71. [s2n](https://github.com/awslabs/s2n) [2159, 176]: *s2n : an implementation of the TLS/SSL protocols*
    72. [mdp](https://github.com/visit1985/mdp) [2141, 114]: *A command-line based markdown presentation tool.*
    73. [synergy](https://github.com/synergy/synergy) [2134, 550]: *Share one mouse and keyboard between multiple computers on your desk.*
    74. [libsodium](https://github.com/jedisct1/libsodium) [2103, 259]: *A modern and easy-to-use crypto library.*
    75. [mongoose](https://github.com/cesanta/mongoose) [2095, 708]: *Embedded web server for C/C++*
    76. [shairport](https://github.com/abrasive/shairport) [2061, 494]: *Airtunes emulator! Shairport is no longer maintained.*
    77. [OBS](https://github.com/jp9000/OBS) [2032, 556]: *Open Broadcaster Software*
    78. [node-fibers](https://github.com/laverdet/node-fibers) [2016, 127]: *Fiber/coroutine support for v8 and node.*
    79. [goaccess](https://github.com/allinurl/goaccess) [1960, 150]: *GoAccess is a real-time web log analyzer and interactive viewer that runs in a terminal in *nix systems.*
    80. [wax](https://github.com/probablycorey/wax) [1845, 401]: *Wax is now being maintained by alibaba*
    81. [osv](https://github.com/cloudius-systems/osv) [1845, 342]: *OSv, a new operating system for the cloud.*
    82. [FLIF](https://github.com/FLIF-hub/FLIF) [1843, 95]: *Free Lossless Image Format*
    83. [yaf](https://github.com/laruence/yaf) [1813, 806]: *A fast php framework written in c, built in php-ext*
    84. [mozjpeg](https://github.com/mozilla/mozjpeg) [1795, 167]: *Improved JPEG encoder.*
    85. [radare2](https://github.com/radare/radare2) [1789, 378]: *unix-like reverse engineering framework and commandline tools*
    86. [kore](https://github.com/jorisvink/kore) [1786, 127]: *An easy to use, scalable and secure web application framework for writing web APIs in C.*
    87. [xhyve](https://github.com/mist64/xhyve) [1775, 73]: *xhyve, a lightweight OS X virtualization solution*
    88. [zimg](https://github.com/buaazp/zimg) [1743, 196]: *A lightweight and high performance image storage and processing system.*
    89. [reptyr](https://github.com/nelhage/reptyr) [1726, 85]: *Reparent a running program to a new terminal*
    90. [tg](https://github.com/vysheng/tg) [1711, 407]: *telegram-cli*
    91. [postgres](https://github.com/postgres/postgres) [1692, 558]: *Mirror of the official PostgreSQL GIT repository. Note that this is just a *mirror* - we don't work with pull requests on github. To contribute, please see http://wiki.postgresql.org/wiki/Submitting_a_Patch*
    92. [nginx-releases](https://github.com/nginx/nginx-releases) [1690, 963]: ** NOTE: This repository has been retired *. Complete (unofficial) history of nginx releases.  Please note that this repository is unofficial and pull requests have no chance of being merged. The proper way to submit changes to nginx is via the nginx development mailing list, see http://nginx.org/en/docs/contributing_changes.html.*
    93. [ios-webkit-debug-proxy](https://github.com/google/ios-webkit-debug-proxy) [1674, 137]: *A DevTools proxy (Chrome Remote Debugging Protocol) for iOS devices (Safari Remote Web Inspector).*
    94. [obs-studio](https://github.com/jp9000/obs-studio) [1642, 382]: *OBS*
    95. [pthreads](https://github.com/krakjoe/pthreads) [1637, 275]: *Threading for PHP - Share Nothing, Do Everything :)*
    96. [msysgit](https://github.com/msysgit/msysgit) [1630, 530]: *msysGit has been superseded by Git for Windows 2.x*
    97. [clib](https://github.com/clibs/clib) [1624, 74]: *C package manager-ish*
    98. [RedPhone](https://github.com/WhisperSystems/RedPhone) [1593, 416]: *A secure calling app for Android.*
    99. [keen](https://github.com/keendreams/keen) [1590, 159]: *Keen Dreams on Greenlight!*
    100. [hiredis](https://github.com/redis/hiredis) [1578, 531]: *Minimalistic C client for Redis \>= 1.2*
