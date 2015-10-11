Module: main
main
Source:
main.js, line 3
Methods
<static> createHttpClient(options) → {module:request~Request}
Constructs a Request object for wrapping your own HTTP method / functionality
Parameters:
Name	Type	Description
options	module:request~options	The options Object taken by the module:request~Request constructor
Source:
main.js, line 94
Returns:
A new instance of the module:request~Request class
Type
module:request~Request
<static> delete(options, file, callback)
HTTP DELETE method wrapper
Parameters:
Name	Type	Description
options	module:request~options | String	The options Object taken by the Request constructor, filtered by module:tools.shortHand. options.method = 'DELETE' and it can not be overridden. If a String is passed, it is handled as options = {url: options}
file	String | null	Optional; specify a filesystem path to save the response body as file instead of Buffer. Passing null turns off the data listeners
callback	module:main~callback	Completion callback
Source:
main.js, line 216
Example
// shorthand syntax, buffered response
http.delete('http://localhost/delete', function (err, res) {
	if (err) {
		console.error(err);
		return;
	}
	
	console.log(res.code, res.headers, res.buffer.toString());
});

// with explicit options object
http.delete({
	url: 'http://localhost/delete'
}, function (err, res) {
	if (err) {
		console.error(err);
		return;
	}
	
	console.log(res.code, res.headers, res.file);
});
<static> get(options, file, callback)
HTTP GET method wrapper
Parameters:
Name	Type	Description
options	module:request~options | String	The options Object taken by the Request constructor, filtered by module:tools.shortHand. options.method = 'GET' and it can not be overridden. If a String is passed, it is handled as options = {url: options}
file	String | null	Optional; specify a filesystem path to save the response body as file instead of Buffer. Passing null turns off the data listeners
callback	module:main~callback	Completion callback
Source:
main.js, line 148
Example
// shorthand syntax, buffered response
http.get('http://localhost/get', function (err, res) {
	if (err) {
		console.error(err);
		return;
	}
	
	console.log(res.code, res.headers, res.buffer.toString());
});

// save the response to file with a progress callback
http.get({
	url: 'http://localhost/get',
	progress: function (current, total) {
		console.log('downloaded %d bytes from %d', current, total);
	}
}, 'get.bin', function (err, res) {
	if (err) {
		console.error(err);
		return;
	}
	
	console.log(res.code, res.headers, res.file);
});

// with explicit options, use a HTTP proxy, and limit the number of redirects
http.get({
	url: 'http://localhost/get',
	proxy: {
		host: 'localhost',
		port: 3128
	},
	maxRedirects: 2
}, function (err, res) {
	if (err) {
		console.error(err);
		return;
	}
	
	console.log(res.code, res.headers, res.buffer.toString());
});
<static> head(options, callback)
HTTP HEAD method wrapper
Parameters:
Name	Type	Description
options	module:request~options | String	The options Object taken by the Request constructor, filtered by module:tools.shortHand. options.method = 'HEAD' and it can not be overridden. If a String is passed, it is handled as options = {url: options}
callback	module:main~callback	Completion callback
Source:
main.js, line 170
Example
// shorthand syntax, default options
http.head('http://localhost/head', function (err, res) {
	if (err) {
		console.error(err);
		return;
	}
	
	console.log(res.code, res.headers);
});
<static> mimeSniff(url, callback)
Reads the first chunk from the target URL. Tries to pass back the proper MIME type
Parameters:
Name	Type	Description
url	String	The target URL
callback	function	Completion callback with either module:request~stdError or the MIME type as string
Source:
main.js, line 477
Example
http.mimeSniff('http://localhost/foo.pdf', function (err, mime) {
	if (err) {
		console.error(err);
		return;
	}
	
	console.log(mime); // application/pdf
});
<static> post(options, file, callback)
HTTP POST method wrapper
Parameters:
Name	Type	Description
options	module:request~options | String	The options Object taken by the Request constructor, filtered by module:tools.shortHand. options.method = 'POST' and it can not be overridden. If a String is passed, it is handled as options = {url: options}
file	String | null	Optional; specify a filesystem path to save the response body as file instead of Buffer. Passing null turns off the data listeners
callback	module:main~callback	Completion callback
Source:
main.js, line 291
Example
// shorthand syntax, buffered response, content-type = application/x-www-form-urlencoded
// param1=value1&param2=value2 are stripped from the URL and sent via the request body

// do not pass the query values by using URL encoding
// the URL encoding is done automatically
http.post('http://localhost/post?param1=value1&param2=value2', function (err, res) {
	if (err) {
		console.error(err);
		return;
	}
	
	console.log(res.code, res.headers, res.buffer.toString());
});

// explicit POST request, saves the response body to file

// for Buffer or String reqBody, the content-length header is reqBody.length
var reqBody = {
	param1: 'value1',
	param2: 'value2'
};

// this serialization also does URL encoding so you won't have to
reqBody = querystring.stringify(reqBody);

http.post({
	url: 'http://localhost/post',
	reqBody: new Buffer(reqBody),
	headers: {
		// specify how to handle the request, http-request makes no assumptions
		'content-type': 'application/x-www-form-urlencoded;charset=utf-8'
	}
}, 'post.bin', function (err, res) {
	if (err) {
		console.error(err);
		return;
	}
	
	console.log(res.code, res.headers, res.file);
});

// multipart/form-data request built with form-data (3rd party module)
// you have to use the http export of FormData
// the instanceof operator fails otherwise
// implementing a manual check is not very elegant
var form = new http.FormData();

form.append('param1', 'value1');
form.append('param2', 'value2');

http.post({
	url: 'http://localhost/post',
	reqBody: form
}, function (err, res) {
	if (err) {
		console.error(err);
		return;
	}
	
	console.log(res.code, res.headers, res.buffer.toString());
});
<static> put(options, file, callback)
HTTP PUT method wrapper
Parameters:
Name	Type	Description
options	module:request~options | String	The options Object taken by the Request constructor, filtered by module:tools.shortHand. options.method = 'PUT' and it can not be overridden. If a String is passed, it is handled as options = {url: options}
file	String | null	Optional; specify a filesystem path to save the response body as file instead of Buffer. Passing null turns off the data listeners
callback	module:main~callback	Completion callback
Source:
main.js, line 403
Example
// put a buffer
http.put({
	url: 'http://localhost/put',
	reqBody: new Buffer('data to put'),
	headers: {
		// if you don't define a type, then mmmagic does it for you
		'content-type': 'text/plain'
	}
}, function (err, res) {
	if (err) {
		console.error(err);
		return;
	}
	
	console.log(res.code, res.headers, res.buffer.toString());
});

// put a ReadStream created with fs.createReadStream
// http-request knows how to handle a ReadStream
// content-length is taken from the file itself by making a fs.stat call
// content-type is detected automatically by using mmmagic if unspecified
http.put({
	url: 'http://localhost/put',
	reqBody: fs.createReadStream('/path/to/file')
}, function (err, res) {
	if (err) {
		console.error(err);
		return;
	}
	
	console.log(res.code, res.headers);
});

// pipe a HTTP response (a http.IncomingMessage Object) to a PUT request
// http-request knows how to handle an IncomingMessage
// content-length is taken from the response if it exists
require('http').get('http://example.org/file.ext', function (im) {
	http.put({
		url: 'http://localhost/put',
		reqBody: im,
		headers: {
			// if you don't define content-type
			// it is taken from the response if exists
			'content-type': 'application/octet-stream'
		}
	}, function (err, res) {
		if (err) {
			console.error(err);
			return;
		}
		
		console.log(res.code, res.headers);
	});
});

// put a readable stream with known size and type, save the response body to file
http.put({
	url: 'http://localhost/put',
	reqBody: readableStream,
	headers: {
		'content-length': 1337,
		'content-type': 'text/plain'
	}
}, 'put.bin', function (err, res) {
	if (err) {
		console.error(err);
		return;
	}
	
	console.log(res.code, res.headers, res.buffer.toString());
});
<static> setMaxSockets(value)
Sets the http.Agent.defaultMaxSockets value
Parameters:
Name	Type	Description
value	Number	
Source:
main.js, line 526
Example
// use up to 128 concurrent sockets
http.setMaxSockets(128);
Type Definitions
callback(error, result)
Describes the callback passed to each HTTP method wrapper
Parameters:
Name	Type	Description
error	module:request~stdError	The standard error or *null* on success
result	module:request~stdResult	The standard result object if error is null
Source:
main.js, line 536
Index
Modules
main
request
tools
Classes
Request

Documentation generated by JSDoc 3.2.2 on Fri May 15 2015 16:50:19 GMT+0100 (BST)
