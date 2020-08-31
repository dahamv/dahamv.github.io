* XML GET
```bash
curl -H "Accept: application/xml" -H "Content-Type: application/xml" -X GET "http://hostname/resource"
```
* JSON GET
```bash
curl -i -H "Accept: application/json" -H "Content-Type: application/json" -X GET "http://hostname/resource"
```
* JSON PUT
```bash
curl -i -H 'Content-Type: application/json' -H 'Accept: application/json' -X PUT -d '{"updated_field1":"updated_value1"}' "http://hostname/resourcex"
```
* JSON POST uploading a file
```bash
curl -i -H 'Accept: application/json' -X POST -F "filename=@/file/path" -F "other_field=its_value"   "http://hostname/resource"
```
* JSON DELETE
```bash
curl -i -H 'Content-Type: application/json' -H 'Accept: application/json' -X DELETE -d '{"key1":"value1"}' "http://hostname/resource"
```
* POST
```bash
curl -i -X POST -H 'Content-Type: application/x-www-form-urlencoded' --data 'key1=value1&key2=value2' url
```
* "Debugging mode" (without actual content output):
```bash
curl -XGET -vvv http://hostname/resource > dev\null
```

### Useful arguments
* ```-k```: not check SSL certificates
* ```-L```: follow redirects
* ```-v```: get verbose output
* ```-V```: get headers at output
