## CORS request using cURL command

Testing or debugging CORS requests can be simply done with curl. To do so, `Origin` HTTP header must be passed to curl command.

```
curl -H "Origin: http://example.com" --verbose https://www.my-api-server.com
```
