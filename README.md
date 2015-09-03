# Client logic Python(3.4.3) example:

### config
```
app_id = '{app_id}'
secret_key = '{secret_key}'
```


### generate token

**url parameters:**

- api: api_route
- aid: app_id
- ts: time_stamp
- t: token

**Target url:**

```http:host/{api_route}?aid={app_id}&ts={time_stamp}&t={token}```

**Token:**

```token_url = {api_route}?aid={app_id}&ts={time_stamp}```

```token = urlsafe_b64encode(md5(token_url + secret_key))```

python code:

```
api = 'external-api/v1/orgs/{}/courses/{}/scores'.format({org_id}, {course_code})
postfix = '?aid={}&ts={}'.format(app_id, int(time.time()))
api += postfix

token = '&t=' + base64.urlsafe_b64encode(hashlib.md5((api + secret_key).encode()).digest()).decode()
url = 'http://host/{}{}'.format(api, token)
```

### test get api
```
request = Request(url)
request.add_header('Accept', 'application/json')
request.get_method = lambda: 'GET'
page = urlopen(request)

json_text = page.readall()
print(json_text)
```


# Client logic Ruby(2.0.0p481) example:

### config
```
$app_id = '{app_id}'
$secret_key = '{secret_key}'
```

### generate token
```
api = 'external-api/v1/orgs/%s/courses/%s/scores' % [{org_id}, {course_code}]
postfix = "?aid=#{$app_id}&ts=%s" % Time.now.to_i
api += postfix

token = '&t=' + Base64.urlsafe_encode64(Digest::MD5.digest((api + $secret_key)))
url = "http://host/#{api}#{token}"
```

### test get api
```
Net::HTTP.version_1_2
	Net::HTTP.start('host', port) {|http|
	  response = http.get("/#{api}#{token}")
	  puts response.body
	}
```

# Client logic Java(1.8.0_25) example:

### config
```
String APP_ID = "{app_id}";
String SECRET_KEY = "{secret_key}";
```

### generate token
```	
String api = String.format("external-api/v1/orgs/%s/courses/%s/scores", {org_id}, {course_code});
String postfix = String.format("?aid=%s&ts=%s", APP_ID, (int)(System.currentTimeMillis()/1000));
api += postfix;

String token = "&t=";
try
{
	MessageDigest mdInst = MessageDigest.getInstance("MD5");
	mdInst.update((api + SECRET_KEY).getBytes());
	// import java.util.Base64; JAVA8 feature, or use custom base64encode, replace '+/' to '-_' for url safe
 	token += Base64.getUrlEncoder().encodeToString(mdInst.digest());
}
catch(Exception e)
{
	e.printStackTrace();
}
String url = String.format("http://host/%s%s", api, token);
```

### test get api
```
BufferedReader in = null;
try
{
	URL realUrl = new URL(url);
	URLConnection connection = realUrl.openConnection();
	connection.setRequestProperty("accept", "*/*");
    connection.setRequestProperty("connection", "Keep-Alive");
    connection.setRequestProperty("user-agent", "Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.1;SV1)");
    connection.connect();
    in = new BufferedReader(new InputStreamReader(connection.getInputStream()));
	String line;
	while ((line = in.readLine()) != null)
	{
    	System.out.println(line);
   	}
}
catch (Exception e)
{
	e.printStackTrace();
}
finally
{
	try
	{
        if (in != null) 
        {
            in.close();
        }
	}
    catch (Exception e2)
    {
        e2.printStackTrace();
	}
}
```

# Client logic PHP(5.5.27) example:

### config
```
static $app_id = '{app_id}';
static $secret_key = '{secret_key}';
```

### generate token
```
$api = sprintf('external-api/v1/orgs/%d/courses/%s/scores', {org_id}, {course_code});
$postfix = "?aid={$app_id}&ts=".time();
$api .= $postfix;

$token = '&t='.strtr(base64_encode(md5($api.$secret_key, true)), '+/', '-_');
$url = "http://host/{$api}{$token}";
```

### test get api
```
$result = file_get_contents($url);
echo $result;
```
