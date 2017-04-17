<!-- page_number: true -->
<img src="images/importgong4py.png" alt="Import Gong4py" style="display: block; margin-right: auto; margin-left: auto; "/>

# feedparser: 
### Universal Feed Parser
#### using feedparser 5.2.1, Python 3.5.2
##### Slide by JuneTech
##### last edit: 2017. 3. 27
---

## feedparser

Universal Feed Parser is a Python module for downloading and parsing syndicated feeds.

1. Type of feed it can handle:
    * RSS 0.90
    * Netscape RSS 0.91
    * Userland RSS 0.91
    * RSS 0.92, 0.93, 0.94, 1.0, 2.0
    * Atom 0.3, 1.0
    * CDF
    * Dublin Core
    * Apple iTunes extension
1. Requirement
    * Python 2.4 or later
    * All python 3

---
## Quickstart

To-do:
* Acquire information about update of webtoons
  * Naver comics
  * Daum cartoons
  * Olleh webtoons: 
  * Marumaru illegal translations
* Get feed as a response

***Note:*** feedparser has *only one* primary public function: `parse`

---
## RSS
Rich Site Summary
* XML formatted plain text
* RSS uses a family of standard web feed formats to publish *frequently updated* information
* filename extension: .rss, .xml

## Atom
Atom Syndication Format
* XML language used for web feeds, paired with Atom Publishing Protocol
* Similar to RSS but with richer metadata

---
## Type of RSS for each webtoon sites
* Naver comics
  * No official RSS/Atom feed
  * Private feeds exist
    * https://ncf.suod.kr/ : **Atom 1.0**
* Daum webtoons
  * No RSS feed button, but simply changing view to rss in the URL works
  * http://webtoon.daum.net/webtoon/view/treasurehunter3
  -> http://webtoon.daum.net/webtoon/rss/treasurehunter3 : **RSS 2.0**
  * Feed exists for each cartoon series
* Olleh webtoon
  * No official RSS/Atom feed
  * No private feed works
* Marumaru illegal translations
  * No RSS feed button
  * http://marumaru.in/?m=bbs&bid=mangaup&mod=rss : **RSS 2.0**
  * All updates are consolidated

---
## Code: Marumaru illegals
```python
import feedparser as fp

feed2parse = fp.parse('http://marumaru.in/?m=bbs&bid=mangaup&mod=rss') # URL을 string으로 집어넣어야함

print(type(feed2parse))
print(feed2parse)
```

---
## Result: Marumaru illegals
```sh
PS D:\projects> python .\feedparser_test.py
<class 'feedparser.FeedParserDict'>
{'feed': {'link': 'http://marumaru.in/?r=home&m=bbs&bid=mangaup', 'title': '만화 업데이트 알림', 'language': 'ko', 'titl
e_detail': {'type': 'text/plain', 'language': None, 'base': 'http://marumaru.in/?m=bbs&bid=mangaup&mod=rss', 'value': '
만화 업데이트 알림'}, 'links': [{'rel': 'alternate', 'type': 'text/html', 'href': 'http://marumaru.in/?r=home&m=bbs&bid=
mangaup'}]}, 'headers': {'Set-Cookie': '__cfduid=d2d8f8a9d7bc64654ff88bbd7433c33aa1490608458; expires=Tue, 27-Mar-18 09:
54:18 GMT; path=/; domain=.marumaru.in; HttpOnly', 'X-Turbo-Charged-By': 'LiteSpeed', 'Date': 'Mon, 27 Mar 2017 09:54:18
 GMT', 'Transfer-Encoding': 'chunked', 'Connection': 'close', 'CF-RAY': '346177af510a3a12-ICN', 'Content-Encoding': 'gzi
p', 'Server': 'cloudflare-nginx', 'Expires': 'Thu, 19 Nov 1981 08:52:00 GMT', 'Cache-Control': 'no-cache, must-revalidat
e', 'Pragma': 'no-cache', 'Content-Type': 'text/xml', 'X-Powered-By': 'PHP/5.5.36'}, 'namespaces': {'dc': 'http://purl.o
rg/dc/elements/1.1/'}, 'bozo_exception': CharacterEncodingOverride('document declared as us-ascii, but parsed as utf-8',
), 'href': 'http://marumaru.in/?m=bbs&bid=mangaup&mod=rss', 'version': 'rss20', 'status': 200, 'entries': [{'summary_det
ail': {'type': 'text/html', 'language': None, 'base': 'http://marumaru.in/?m=bbs&bid=mangaup&mod=rss', 'value': '[단편]
천년의 무심 후편'}, 'tags': [], 'updated': 'Mon, 27 Mar 2017 18:20:01 +0000', 'link': 'http://marumaru.in/?r=home&m=bbs&
bid=mangaup&uid=201824', 'summary': '[단편] 천년의 무심 후편', 'links': [{'rel': 'alternate', 'type': 'text/html', 'href
': 'http://marumaru.in/?r=home&m=bbs&bid=mangaup&uid=201824'}], 'id': 'http://marumaru.in/?r=home&m=bbs&bid=mangaup&uid=
201824', 'title': '[단편] 천년의 무심 후편', 'author': '', 'authors': [{}], 'guidislink': False, 'updated_parsed': 
...
```
So so many response parameters...

Info: *Doesn't exist anywhere*

---
## Dictionary parsing: Marumaru illegals

Information wanted: title, URL
```python
import feedparser as fp

feed2parse = fp.parse('http://marumaru.in/?m=bbs&bid=mangaup&mod=rss') # URL을 string으로 집어넣어야함
for cartoon_info in feed2parse['entries']:
    print(cartoon_info['title']+': '+cartoon_info['link'])
```

---
## Result 2: Marumaru illegals
```sh
PS D:\projects> python .\feedparser_test.py
프리즈마 이리야 드라이 46화: http://marumaru.in/?r=home&m=bbs&bid=mangaup&uid=201883
[단편] 천년의 무심 후편: http://marumaru.in/?r=home&m=bbs&bid=mangaup&uid=201824
일곱개의 대죄 211.5화 - 인형은 사랑을 갈망한다: http://marumaru.in/?r=home&m=bbs&bid=mangaup&uid=201822
지정폭력소녀 시오미 짱 11화: http://marumaru.in/?r=home&m=bbs&bid=mangaup&uid=201760
사유리네 동생은 천사 15화: http://marumaru.in/?r=home&m=bbs&bid=mangaup&uid=201759
```

---
## Code: Daum webtoon 'Treasure Hunter 3'
```python
import feedparser as fp

treasurehunter_feed = fp.parse('http://webtoon.daum.net/webtoon/rss/treasurehunter3')
print(type(treasurehunter_feed))
print(treasurehunter_feed) 
```

---
## Result 1: Daum webtoon
```sh
PS D:\projects> python .\feedparser_test.py
<class 'feedparser.FeedParserDict'>
{'encoding': 'UTF-8', 'href': 'http://webtoon.daum.net/webtoon/rss/treasurehunter3', 'headers': {'Content-Type': 'applic
ation/xml;charset=UTF-8', 'Date': 'Mon, 27 Mar 2017 10:29:49 GMT', 'Content-Language': 'ko', 'Server': 'Apache', 'Connec
tion': 'close', 'Transfer-Encoding': 'chunked'}, 'feed': {'link': 'http://webtoon.daum.net', 'rights': 'Copyright (c) Da
um Communications. All rights reserved.', 'links': [{'href': 'http://webtoon.daum.net', 'rel': 'alternate', 'type': 'tex
t/html'}], 'docs': 'http://webtoon.daum.net/webtoon/rss/treasurehunter3', 'published': '2017-03-23 13:57:58', 'subtitle'
: '미디어 다음 만화속 세상 트레져헌터3', 'author_detail': {'email': 'media-cs@daum.net'}, 'generator': 'cartoon admin',
'author': 'media-cs@daum.net', 'subtitle_detail': {'value': '미디어 다음 만화속 세상 트레져헌터3', 'base': 'http://webto
on.daum.net/webtoon/rss/treasurehunter3', 'type': 'text/html', 'language': 'ko'}, 'generator_detail': {'name': 'cartoon
admin'}, 'rights_detail': {'value': 'Copyright (c) Daum Communications. All rights reserved.', 'base': 'http://webtoon.d
aum.net/webtoon/rss/treasurehunter3', 'type': 'text/plain', 'language': 'ko'}, 'publisher_detail': {'email': 'media-cs@d
aum.net'}, 'published_parsed': time.struct_time(tm_year=2017, tm_mon=3, tm_mday=23, tm_hour=13, tm_min=57, tm_sec=58, tm
_wday=3, tm_yday=82, tm_isdst=0), 'title_detail': {'value': '트레져헌터3', 'base': 'http://webtoon.daum.net/webtoon/rss/
treasurehunter3', 'type': 'text/plain', 'language': 'ko'}, 'title': '트레져헌터3', 'publisher': 'media-cs@daum.net', 'au
thors': [{'email': 'media-cs@daum.net'}], 'language': 'ko'}, 'bozo': 0, 'status': 200, 'namespaces': {}, 'version': 'rss
20', 'entries': [{'link': 'http://webtoon.daum.net/webtoon/viewer/41710', 'published': '2017-03-23 13:57:58', 'guidislin
k': False, 'links': [{'href': 'http://webtoon.daum.net/webtoon/viewer/41710', 'rel': 'alternate', 'type': 'text/html'}],
 'published_parsed': time.struct_time(tm_year=2017, tm_mon=3, tm_mday=23, tm_hour=13, tm_min=57, tm_sec=58, tm_wday=3, t
m_yday=82, tm_isdst=0), 'title_detail': {'value': '3부 50화', 'base': 'http://webtoon.daum.net/webtoon/rss/treasurehunte
r3', 'type': 'text/plain', 'language': 'ko'}, 'title': '3부 50화', 'id': 'http://webtoon.daum.net/webtoon/viewer/41710',
 'summary': "<img src='http://t1.daumcdn.net/cartoon/58D35586066ECD0001' >3부 50화", 'summary_detail': {'value': "<img s
rc='http://t1.daumcdn.net/cartoon/58D35586066ECD0001' >3부 50화", 'base': 'http://webtoon.daum.net/webtoon/rss/treasureh
unter3', 'type': 'text/html', 'language': 'ko'}}, {'link': 'http://webtoon.daum.net/webtoon/viewer/41623', 'published':
...
```

---
## Dictionary parsing: Daum webtoon

Information wanted: title, URL
```python
import feedparser as fp

treasurehunter_feed = fp.parse('http://webtoon.daum.net/webtoon/rss/treasurehunter3')
for th_info in treasurehunter_feed['entries']:
    print(th_info['title']+': '+th_info['id'])
```

---
# Result 2: Daum webtoon
```sh
PS D:\projects> python .\feedparser_test.py
3부 50화: http://webtoon.daum.net/webtoon/viewer/41710
3부 49화: http://webtoon.daum.net/webtoon/viewer/41623
3부 48화: http://webtoon.daum.net/webtoon/viewer/41512
3부 47화: http://webtoon.daum.net/webtoon/viewer/41406
3부 46화: http://webtoon.daum.net/webtoon/viewer/41292
3부 45화: http://webtoon.daum.net/webtoon/viewer/41168
3부 44화: http://webtoon.daum.net/webtoon/viewer/41049
3부 43화: http://webtoon.daum.net/webtoon/viewer/40864
3부 42화: http://webtoon.daum.net/webtoon/viewer/40761
3부 41화: http://webtoon.daum.net/webtoon/viewer/40658
3부 40화: http://webtoon.daum.net/webtoon/viewer/40551
3부 39화: http://webtoon.daum.net/webtoon/viewer/40438
3부 38화: http://webtoon.daum.net/webtoon/viewer/40330
3부 37화: http://webtoon.daum.net/webtoon/viewer/40224
3부 36화: http://webtoon.daum.net/webtoon/viewer/40114
3부 35화: http://webtoon.daum.net/webtoon/viewer/40017
3부 34화: http://webtoon.daum.net/webtoon/viewer/39923
3부 33화: http://webtoon.daum.net/webtoon/viewer/39826
휴재공지: http://webtoon.daum.net/webtoon/viewer/39735
```

---
## Code: Naver comics from private RSS
Search for RSS of your favorite Naver comics in: https://ncf.suod.kr/webtoon
```python
import feedparser as fp

denma_feed = fp.parse('https://ncf.suod.kr/webtoon/119874.xml?limit=15')
for denma_info in denma_feed['entries']:
    print(denma_info['title']+': '+denma_info['link'])
```

---
## Result: Naver comics from private RSS
```sh
PS D:\projects> python .\feedparser_test.py
2-663화 3.A.E.(6): http://comic.naver.com/webtoon/detail.nhn?titleId=119874&no=988&weekday=tue
2-662화 3.A.E.(5): http://comic.naver.com/webtoon/detail.nhn?titleId=119874&no=987&weekday=tue
2-661화 3.A.E.(4): http://comic.naver.com/webtoon/detail.nhn?titleId=119874&no=986&weekday=tue
2-660화 3.A.E.(3): http://comic.naver.com/webtoon/detail.nhn?titleId=119874&no=985&weekday=tue
2-659화 3.A.E.(2): http://comic.naver.com/webtoon/detail.nhn?titleId=119874&no=984&weekday=tue
2-658화 3.A.E.(1): http://comic.naver.com/webtoon/detail.nhn?titleId=119874&no=983&weekday=tue
2-657화 3.The knight(185): http://comic.naver.com/webtoon/detail.nhn?titleId=119874&no=982&weekday=tue
...
5화 해적선장 하독(1): http://comic.naver.com/webtoon/detail.nhn?titleId=119874&no=5&weekday=tue
4화 파마나의 개(4): http://comic.naver.com/webtoon/detail.nhn?titleId=119874&no=4&weekday=tue
3화 파마나의 개(3): http://comic.naver.com/webtoon/detail.nhn?titleId=119874&no=3&weekday=tue
2화 파마나의 개(2): http://comic.naver.com/webtoon/detail.nhn?titleId=119874&no=2&weekday=tue
1화 파마나의 개(1): http://comic.naver.com/webtoon/detail.nhn?titleId=119874&no=1&weekday=tue
```

---
## Conclusion
* feedparser is not designed as a standalone module
* It grabs every information from xml file, similar to REST API
* Decision of updates in data & refreshment rate should be done somewhere else
  * If statement with past data
  * after method in tkinter

---
## Reference
* Universal Feed Parser web page:
http://pythonhosted.org/feedparser/
* Daum webtoon RSS address:
http://m.blog.naver.com/mirzurfeier_s/220693433715
