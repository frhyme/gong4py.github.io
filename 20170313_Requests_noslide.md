<!-- page_number: true -->
<img src="images/importgong4py.png" alt="Import Gong4py" style="display: block; margin-right: auto; margin-left: auto; "/>

# Requests: 
### HTTP library for python
#### using Requests 2.13.0, Python 3.5.2
##### Slide by JuneTech
##### last edit: 2017. 3. 13
---

## Requests

<img src="images/requests-sidebar.png" alt="Requests_mark" style="width: 40%; display: block; margin-right: auto; margin-left: auto; "/>

Requests is the only *Non-GMO* HTTP library for Python, safe for human consumption.

* HTTP/1.1 library for Python
* Keep-Alive & Connection Pooling
* International Domains and URLs
* Sessions with Cookie Persistence
* Browser-style SSL Verification
* Automatic Content Decoding
* Basic/Digest Authentication
* Elegant Key/Value Cookies
* Automatic Decompression
* Unicode Response Bodies
* HTTP(S) Proxy Support
* ...

---
## Quickstart

To-do:
* Send query to Weather Planet API
  * with address
  * with latitude, longitude
* Get response

---
## Code
```python
import requests

app_key = "2b97abb1-7ede-35e2-b0b5-3de576ded4bd"
weather_url="http://apis.skplanetx.com/weather/current/hourly"
parameters1 = {"version":"1", "city":"경북", "county":
	"포항시 남구", "village":"효자동"}
parameters2 = {"version":"1", "lat":"36.01293", 
	"lon":"129.32182"}

r = requests.get(weather_url, params = parameters1, 
	headers = {"appKey":app_key})
print(r.json())
r = requests.get(weather_url, params = parameters2, 
	headers = {"appKey":app_key})
print(r.json())
```

---

## Result 1
last statement: print(r.json())
```sh
H:\rpi> python .\weatherplanet_test.py
{'weather': {'hourly': [{'grid': {'city': '경북', 'village': '상도동', 'latitude': '36.0088400000', 
'longitude': '129.3514300000', 'county': '포항시 남구'}, 'precipitation': {'sinceOntime': '0.00', 'type': '0'}, 
'wind': {'wdir': '219.00', 'wspd': '1.30'}, 'humidity': '53.00', 'sky': {'code': 'SKY_O03', 'name': '구름많음'}, 
'timeRelease': '2017-03-13 07:00:00', 'lightning': '0', 'temperature': {'tmax': '13.00', 'tc': '7.40', 'tmin': '7.00'}}]}, 
'common': {'stormYn': 'N', 'alertYn': 'Y'}, 'result': {'requestUrl': 
'/weather/current/hourly?village=효자동&county=포항시 남구&version=1&city=경북', 'code': 9200, 'message': '성공'}}
{one more python-long dictionary}
```
So many response parameters...

Info: https://developers.skplanetx.com/apidoc/kor/weather/information/#doc1169

## Result 0
use `r.text` instead of `r.json()`: one big text chunk
```
{"weather":{"hourly":[{"grid":{"longitude":"129.3514300000","latitude":"36.0088400000","village":"상도동","city":"경북","county":"포항시 남구"},"wind":{"wdir":"270.00","wspd":"1.70"},"precipitation":
{"sinceOntime":"0.00","type":"0"},"sky":{"code":"SKY_O02","name":"구름조금"},"temperature":{"tc":"10.50","tmax":"13.00","tmin":"7.00"},"humidity":"52.00","lightning":"0","timeRelease":"2017-03-13 09:
00:00"}]},"common":{"alertYn":"Y","stormYn":"N"},"result":{"code":9200,"requestUrl":"/weather/current/hourly?village=효자동&county=포항시 남구&version=1&city=경북","message":"성공"}}
```

---
## Dictionary parsing

표시할 정보: 최고/최저 온도(섭씨), 날씨요약, 위치 정보
```python
def print_current_weather(response):
    temp_dict = response['weather']['hourly'][0]['temperature']
    korean_summary = response['weather']['hourly'][0]['sky']['name']
    location_dict = response['weather']['hourly'][0]['grid']

    print("최고 온도:", temp_dict['tmax'], "| 최소 온도:", temp_dict['tmin'])
    print(korean_summary)
    print(location_dict['city'], location_dict['county'], location_dict['village'])
```

---
## Result 2
```sh
최고 온도: 13.00 | 최소 온도: 7.00
구름많음
경북 포항시 남구 상도동
최고 온도: 13.00 | 최소 온도: 5.00
구름많음
경북 포항시 남구 지곡동
```

---
## Reference
* Requests web page:
http://docs.python-requests.org/en/master/
* Weather Planet web page:
https://developers.skplanetx.com/apidoc/kor/weather/?leftAppId=15054296
* Weather Planet guide(한글):
http://rhammer.tistory.com/124
