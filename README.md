# error_repo
A place to log the various things I've learned so I don't have to keep googling them.


Just like my opencv repo, while learning how to poorly used Xcode, I've come across various errors, and these are the successful ways I've fixed them, and possibily the link where I managed to find the source!

To start with, how to fix this error:


    using boost library xcode 14 No member named 'memset' in namespace 'std::__1'; did you mean simply 'memset'?

https://stackoverflow.com/questions/75930323/boost-library-compiler-errors-in-xcode
https://stackoverflow.com/questions/11600782/what-does-it-mean-when-you-check-on-recursive-in-header-search-paths/53088237#53088237


Simply remove the 'recursive' from the Header Search Paths for the boost library. It appears that -I is set for the compilier in this case for every folder, which isn't required!


=======================================================================================================================================================================================================================================

```
boost/bind/detail/result_traits.hpp:46:22 Type 'void (UDPServer::*)(const boost::system::error_code &, unsigned long, bool)' cannot be used prior to '::' because it has no members
```
This error came from not adding a passed variable **bool batman** correctly within the following code:

```
   UDPServer::UDPServer(boost::asio::io_context& io_context, short port,bool batman)
    : socket_(io_context, udp::endpoint(udp::v4(), port)) {
        start_receive();
    }
    

    void UDPServer::start_receive(bool batman) {
        socket_.async_receive_from(boost::asio::buffer(recv_buffer_),
                                   remote_endpoint_,boost::bind(&UDPServer::handle_receive, this,boost::asio::placeholders::error,boost::asio::placeholders::bytes_transferred,batman));
    }
```
The issue is due to a mismatch between the signature of the handle_receive function and the arguments being passed to it using boost::bind. Specifically, the handle_receive function takes an additional bool parameter that boost::asio::placeholders does not provide.

To fix this, you need to correctly bind the bool parameter when calling boost::bind. Here’s how you can modify your code to properly handle this:

This should be added to the end of the line of **socket_.async_receive_from(...,**batman**)

=======================================================================================================================================================================================================================================

```
Expected '(' for function-style cast or type construction
```
This error comes from adding the type of a variable to your function when it doesn't need to be declared:
```
ln 1 main (){
ln 2 bool batman = false;
ln 3 func(bool batman){
ln 4 blah;
ln 5 }
ln 6 std::cout<< "whatever"
ln 7 func(bool batman);
ln 8 return 0;
ln 9 }
```
line 7 would give the error. Fixed by not declaring the passed varible.`func(batman)`
