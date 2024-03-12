# error_repo
A place to log the various things I've learned so I don't have to keep googling them.


Just like my opencv repo, while learning how to poorly used Xcode, I've come across various errors, and these are the successful ways I've fixed them, and possibily the link where I managed to find the source!

To start with, how to fix this error:


    using boost library xcode 14 No member named 'memset' in namespace 'std::__1'; did you mean simply 'memset'?

https://stackoverflow.com/questions/75930323/boost-library-compiler-errors-in-xcode
https://stackoverflow.com/questions/11600782/what-does-it-mean-when-you-check-on-recursive-in-header-search-paths/53088237#53088237


Simply remove the 'recursive' from the Header Search Paths for the boost library. It appears that -I is set for the compilier in this case for every folder, which isn't required!
