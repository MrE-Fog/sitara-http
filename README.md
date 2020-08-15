# sitara-http

A wrapper to simplify using the libcurl for making REST API calls over a network.

## Requirements
* jsoncpp -- this is included by default in Cinder.  For openFrameworks you'll need to add it manually or include an addon.

### Windows

#### openFrameworks
To use this add-on, use the projector generator.  Once you've created a project, use the `ofxCurl.props` property sheet in `config/` to set up your compiler properly.

You'll also need to copy `libcurl.dll` from the `libcurl\dll` folder to the same directory as your executable.

This library has been tested with Visual Studio 2015 and openFrameworks v0.9.1

#### cinder
This block can be used with cinder simply by using Tinderbox.

You'll need to copy `libcurl.dll` from the `libcurl\dll` folder to the same directory as your executable.

### OSX
This project is currently not configured for Xcode in OSX.  If anyone wants to take this on, I'd be glad to help.  Otherwise you'll have to wait until I have a reason to need it on my Mac :)

## Usage
For all use cases, you'll want to create an instance of ofxCurl by calling

```
std::shared_ptr<Curl::HTTPClient> curl_instance = Curl::HTTPClient::make();
```

This creates an instance of cURL that will manage all network calls.

All requests are performed by starting with an `HTTPRequest` struct:

```
HTTPRequest new_request;
new_request.mMethod = HTTP_GET;
new_request.mUrl = "http://www.httpbin.org/get";
new_request.mParameterString = "";
```

For a simple blocking request, you would call `makeRequest`:

```
HTTPResponse response = curl_instance->makeRequest(new_request);
```

This will cause your program to wait for the call to be completed before storing the result in the `HTTPResponse` object.

Alternatively, you an utilize cURL's multithreaded multi-curl functionality by calling `addRequest`.  To do this, you'll need to add a callback function to your request of the type `std::function<void(HTTPResponse*)>`:

```
Curl::HTTPRequest Get;
Get.mUrl = "http://www.httpbin.org/get";
Get.mMethod = Curl::HTTP_GET;
Get.mParameterString = mCurl->mapToString(requestParameters);
Get.mCallback = [=](Curl::HTTPResponse* response, Curl::HTTPClient* client) { std::printf("Request complete with response code %d; response was %s.\n", response->mResponseCode, response->mBody.c_str()); };
mCurl->addHTTPRequest(Get);
```

Your instance of `ofxCurl` will automatically handle loading the call when it has bandwidth available, making the request, and calling your callback function when it has completed the request.


## Examples

## Contributing
This project uses the [Git Flow](http://nvie.com/posts/a-successful-git-branching-model/) paradigm.  Before contributing, please make your own feature branch with your changes.

## More Information
* [libcurl Homepage](https://curl.haxx.se/)
