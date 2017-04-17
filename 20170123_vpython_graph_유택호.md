# Vpython Tutorial - Graph
## Taekho You
## 2017.01.23

---
# About Vpython
- Window, Macintosh, Linux
- Latest version: 6.11
  - Compatible with Python 2.7.9
  - Older version (5.74) compatible with Python 3.2
- Download at http://vpython.org

---
# Why Vpython?
- 3D object를 이용한 모델링 툴
- 모델링 결과를 표현하기 위한 방법으로 그래프 기능이 있는 것으로 추측
- 그래프 기능은 사실 기본 기능 정도만 있지 않나 싶음
- matplotlib 등 여러 라이브러리를 활용할 수 있지만, vpython 내부에서 한번에 처리하고 싶을때는 유용할 듯
![center](/Users/TaekhoYou/Desktop/vpython/vpython_3d.png)

---
## Import Vpython
```python
# import vpython features, including 3d objects
from visual import *
# import graphing features
from visual.graph import * 
```

---
## Type of graphs
- gcurve
- gvbars
- gdots
- ghbars
- ghistogram // 위의 네 개와 약간 다르기 때문에, 따로 정리함

---
## Basic Structure
```python
# Create new window with gdisplay
oscillation = gdisplay(xtitle='Time', ytitle='Response')

# Define graphs
funct1 = gcurve(color=color.cyan, dots = true)
funct2 = gvbars(delta=0.5, color=color.red)
funct3 = gdots(color=color.yellow)

# Plot graphs
for t in arange(-30, 74, 1):
    funct1.plot( pos=(t, 5.0+5.0*cos(-0.2*t)*exp(0.015*t)) )
    funct2.plot( pos=(t, 2.0+5.0*cos(-0.1*t)*exp(0.015*t)) )
    funct3.plot( pos=(t, 5.0*cos(-0.03*t)*exp(0.015*t)) )
```
![60% center](/Users/TaekhoYou/Desktop/vpython/vpython_graphs.png)

---
## Options of gdisplay - properties
```python
gd = gdisplay(x=0, y=0, width=600, height=150, 
      title='N vs. t', xtitle='t', ytitle='N', 
      foreground=color.black, background=color.white, 
      xmax=50, xmin=-20, ymax=5E3, ymin=-2E3, 
      logx = True, logy = True)
```
- 결과창의 위치 (x,y)
  - 기본: (0,0)
- 그래프의 크기 (width, height)
- 타이틀 (title, xtitle, ytitle)
- 글자색, 배경색 (foreground, background)
  - 기본: 흰 글씨, 검은 바탕 -> 흰색 배경이면 좋아보이지 않음
- 축의 크기 (xmax, xmin, ymax, ymin)
- 로그 축 변경 (logx, logy)

---
## Options of gdisplays
```python
graph1 = gdisplay()

# Add label
label(display=graph1.display, pos=(32,15), text="A")

# Control visibility
graph1.display.visible = false
```
- gdisplay()을 정의해서 여러 개의 창에 그래프를 그릴 수 있음
- label() 함수를 이용해 글자를 추가할 수 있음
  - 글자박스를 지울수 있는지는 확인이 필요함
  - 위치는 축을 기준으로 추가하기 때문에, 노가다는 아님

![60% center](/Users/TaekhoYou/Desktop/vpython/vpython_label.png)

---
## Options of graphs - Plot
```python
# point as a single or a list
f1.plot(pos=(100,-30))
f1.plot(pos=[(100,-30),(20,50),(0,-10)])

# equation with range 
for x in range (0., 8.05, 0.1):
	f1.plot(pos=(x,5*cos(2*x)*exp(-0.2*x)))
```

---
## Options of graphs - special
```python
gcurve(color=color.cyan,
       dot=True, shape="round", size=5, dot_color=color.green)
gdots(shape="square",size=5)
gvbars(color=color.yellow, delta=0.5)
ghbars(delta=1)
```
- gcurve()
  - 데이터포인트 점을 함께 찍어줌, shape, size, color
- gdots() 
  - shape(default="square", "round"), size(default=5), color
- gvbars(), ghbars()
  - delta (바의 넓이, default = 1), color

---
## Ghistogram
```python
# set ghistogram with bin size
data = ghistogram(bins=arange(-20, 80, 10), color=color.red)

# plot data to histogram
datalist1 = [5, 37, 12, 21, 25, 28, 8, 63, 52, 75, 7]
data.plot(data=datalist1)

# add datapoints to existing one
datalist2 = [7, 23, 25, 72, -15]
data.plot(data=datalist2, accumulate=1)
```

![60% center](/Users/TaekhoYou/Desktop/vpython/vpython_histogram.png)

---
## 결론 or 아쉬운점
- 그래프 툴은 상당히 간단함
- 그래서 정말 단순히 계산한 결과값을 바로 프린트해서 보는 용도로는 적절할 것 같은데, 그 이상의 용도가 있을지 아쉬움
- 안예쁨 (특히 배경이 흰 색일때는 매우 안예쁨)