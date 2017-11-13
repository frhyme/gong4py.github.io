# python - intro to networkx

2017-11-13

by 이승훈

## 여담

- 이번에 그림을 28개 만들었는데, imgur에서 직접 하나씩 링크복사해서 첨부하려니까 넘나 힘든것.

## 시작하기 전에

- 우리 교수님 수업(Database)을 들어보신 분들은 알겠지만, 텀프로젝트로 학술네트워크 분석을 수행합니다
  - 학술네트워크 분석: 논문 서지정보(키워드, 저자, 학교 등)을 가지고 다음과 같은 네트워크 분석을 하는 것
    - 네트워크 분석을 통해서 봤을 때, 중요한 저자는 누구인가?
    - 어떤 키워드가 중요한가(정확히는 centrality가 높은가)?
  - 이를 위해서 사이람에서 만든 NetMiner라는 것을 쓰는데(필수는 아니고), 해당 툴이 저는 상당히 마음에 들지 않습니다.
    - 다양한 이유가 있겠지만, 네트워크 분석 특성상, 전처리/후처리를 해줘야 할 일들이 상당히 많은데(특히 키워드 정제하는 측면에서), 넷마이너는 이를 자동화하는 것이 어려워서, 매번 수작업으로 해줘야 하는 개X같은 일이 있습니다(뭐 일단 제가 GUI툴들을 별로 안 좋아하기도 하고).
- 아무튼, 그래서 다른 방법이 없나 찾아보다가, 좋은 라이브러리가 있길래 쓰고 있습니다.

## networkx

- 뭐 [홈페이지](https://networkx.github.io/)에는 아래와 같은 설명이 있기는 한데, 그냥 네트워크 분석해주는 패키지라고만 생각하시면 됩니다.
- `networkX is python package for the creation, manipulation, and study of the structure, dynamics, and functions of complex networks`

## features

- Data structures for graphs, digraphs, and multigraphs
- Many standard graph algorithms
- Network structure and analysis measures
- Generators for classic graphs, random graphs, and synthetic networks
- **Nodes can be "anything" (e.g., text, images, XML records)**
- **Edges can hold arbitrary data (e.g., weights, time-series)**
- Open source 3-clause BSD license
- Well tested with over 90% code coverage
- Additional benefits from Python include fast prototyping, easy to teach, and multi-platform

---

## 네트워크는 무엇인가

- 여기 전공하고 계신 박사 과정 분이 계시지만...아무튼 그냥 쉽게 말하면, `점과 선으로 연결된 구조`라고 생각합니다.
- 조금 더 세부적으로 말하자면, 다음으로 좀 더 분류할 수 있기는 합니다.
  - edge에 방향성이 있느냐?
  - 점과 점 사이에 여러 개의 선이 존재할 수 있느냐?
- 따라서 `networkx`에서는 방향성이 있는 경우, 점과 점 사이에 여러 개의 선이 있는 경우를 고려하여, 총 네 가지의 그래프가 존재합니다.
  - `nx.Graph()`
  - `nx.DiGraph()`
  - `nx.MultiGraph()`
  - `nx.MultiDiGraph()`

```python
# 시작적으로 보여줘야 하는 것이 많아서, 그릠 그려주는 펑션을 따로 만듬
%matplotlib inline
import networkx as nx
import matplotlib.pyplot as plt
# matplotlib의 경우 networkx에서 그림을 그릴 때 필요하기 때문에 일단 넣어줌

def draw_graph(input_G, layout="shell"):
    if layout=="shell":
        pos = nx.shell_layout(input_G)
    elif layout=="spring":
        pos = nx.spring_layout(input_G)
    elif layout=="spectral":
        pos = nx.spectral_layout(input_G)
    elif layout=="circular":
        pos = nx.circular_layout(input_G)
    elif layout=="random":
        pos = nx.random_layout(input_G)
    else:
        pos = nx.shell_layout(input_G)
    plt.figure()
    nx.draw_networkx_nodes(input_G, pos)
    nx.draw_networkx_edges(input_G, pos)
    nx.draw_networkx_labels(input_G, pos)
    nx.draw_networkx_edge_labels(input_G, pos)
    #plt.axis("off")
```

```python
### four type in networkx
G = nx.Graph()
G.add_edges_from([(1,2), (2,3), (1,3)])
draw_graph(G)
plt.savefig("graph1.png")
```

![png](https://i.imgur.com/LwGfKk2.png)

```python
DG = nx.DiGraph()
DG.add_edges_from([(1,2), (2,3), (1, 3)])
draw_graph(G)
plt.savefig("graph2.png")
```

![png](https://i.imgur.com/8kpbJYE.png)

```python
MG = nx.MultiGraph()
MG.add_edges_from([(1,2), (2,3), (1,3)])
MG.add_edges_from([ (1,2) for i in range(0, 100)])
draw_graph(MG)
plt.savefig("graph3.png")
```

![png](https://i.imgur.com/9A81gGF.png)

```python
MDG = nx.MultiDiGraph()
MDG.add_edges_from([(1,2), (2,3), (1,3)])
MDG.add_edges_from([ (1,2) for i in range(0, 100)])
draw_graph(MDG)
plt.savefig('graph4.png')
```

![png](https://i.imgur.com/CEuNjAH.png)

## node and edges

- 이제부터는 주로 `nx.Graph`를 가지고 이야기함.
- 어떻게 넣는가, 어떻게 빼는가.

```python
G = nx.Graph()
G.add_node(1)
G.add_node(2)
G.add_nodes_from([3,4,5, 6, 7, 8, 9])
draw_graph(G)
plt.savefig("node_and_edges1.png")

G.add_edge(3,4)
G.add_edges_from([(1,2), (6, 7)])
draw_graph(G)
plt.savefig("node_and_edges2.png")

G.add_path([5,2,7,4])
draw_graph(G)
plt.savefig("node_and_edges3.png")

# The first node in nodes is the middle of the star. It is connected to all other nodes.
G.add_star([9, 1,2,3,4,5,6,7])
draw_graph(G)
plt.savefig("node_and_edges4.png")

G.remove_nodes_from([9])
draw_graph(G)
plt.savefig("node_and_edges5.png")

G.remove_edges_from([(5,2), (8, 5)])
draw_graph(G)
plt.savefig("node_and_edges6.png")
```

![png](https://i.imgur.com/rYpXRxQ.png)

![png](https://i.imgur.com/jSyuDYW.png)

![png](https://i.imgur.com/KeAi9JI.png)

![png](https://i.imgur.com/Nr58IJZ.png)

![png](https://i.imgur.com/VzyHz3i.png)

![png](https://i.imgur.com/tGwt9iS.png)

```python
print(G.nodes())
print(G.edges())
```

    [1, 2, 3, 4, 5, 6, 7, 8]
    [(1, 2), (2, 7), (3, 4), (4, 7), (6, 7)]

## anything can be node

- 얘네가 홈페이지에서, `In NetworkX, nodes can be any hashable object e.g.,`라고 했길래 정말 그런지 확인해봅니다.
  - list: `TypeError: unhashable type: 'list'`
    - Python definition of Hashable is:
      - An object is hashable if it has a hash value which never changes during its lifetime (it needs a hash() method).
      - 그렇다면, 왠지 immutable한 `tuple`의 경우는 될것 같은데.
  - tuple: ok
  - dict: no
  - dataframe: no
  - object: ok
      - dataframe을 node로 만들고 싶을 때는, dataframe도 object 변수로 처리해서 하면 됨.
      - 물론 뒤에서 설명하겠지만, node attribute dictionary에 넣어도 되고.
  - lib: ok
  - function: ok
  - nx.Graph(): ok
- 무엇이든 node가 될 수 있기 때문에, `networkx.Graph()`를 단순히, 네트워크 분석에만 특화된 객체라고 보는 것보다, 자료구조로 보는 것이 더 적합함.
    - 만약 tree 구조, supply chain, keyword network의 경우는 lst, dict보다 `nx.Graph()`로 관리하는 것이 훨씬 편할 수 있음

```python
G = nx.Graph()
G.add_path([1, "dd", 3, "dfs"])
draw_graph(G)
plt.savefig("anythingcanbedone1.png")
```

![png](https://i.imgur.com/owtSjxM.png)

```python
# list 는 안됩니다.
G = nx.Graph()
G.add_node([1,2,3])
```

    ---------------------------------------------------------------------------

    TypeError                                 Traceback (most recent call last)

    <ipython-input-484-d86c877830c9> in <module>()
          1 # list 는 안됩니다.
          2 G = nx.Graph()
    ----> 3 G.add_node([1,2,3])

    C:\Users\frhyme\Anaconda3\lib\site-packages\networkx\classes\graph.py in add_node(self, n, attr_dict, **attr)
        458                 raise NetworkXError(
        459                     "The attr_dict argument must be a dictionary.")
    --> 460         if n not in self.node:
        461             self.adj[n] = self.adjlist_dict_factory()
        462             self.node[n] = attr_dict


    TypeError: unhashable type: 'list'

```python
# 하지만 tuple은 됨
G = nx.Graph()
G.add_node( (1,2,3) )
G.add_node( (4,5,6) )
draw_graph(G)
plt.savefig("anythingcanbedone2.png")
```

![png](https://i.imgur.com/lIucYzn.png)

```python
# 그럼 class로 싸서 넣으면?
class df_C:
    count=1
    def __init__(self):
        self.id=df_C.count
        self.df = pd.DataFrame({})
        df_C.count+=1
    def __repr__(self):
        return "dataframe"+str(self.id)
df1 = df_C()
df2 = df_C()
df3 = df_C()

dfG = nx.Graph()
dfG.add_edge(df1, df2)
dfG.add_edge(df1, df3)
dfG.edges()
draw_graph(dfG)
plt.savefig("anythingcanbedone3.png")
```

![png](https://i.imgur.com/mnyfnJP.png)

```python
# lib를 가지고 네트워크화할 수도 있고,
import networkx as nx
import numpy as np
import pandas as pd
import datetime as dt
libG = nx.Graph()

libG.add_edges_from([(np, pd), (pd, dt), (pd, nx)])
libG.edges()
draw_graph(libG)
plt.savefig("anythingcanbedone4.png")
```

![png](https://i.imgur.com/mSx9HmE.png)

```python
# function을 node로도 할 수 있고
def plus(a, b):
    return a+b
def square(a):
    return a**2
def minus(a, b):
    return a-b
funcG = nx.DiGraph()
funcG.add_edge(plus, square)
funcG.add_edge(plus, minus)
funcG.edges()
draw_graph(funcG)
plt.savefig("anythingcanbedone5.png")
```

![png](https://i.imgur.com/b27YRVc.png)

```python
# graph를 node로도 하는 것도 가능함.
GG = nx.Graph()
g1 = nx.Graph()
g2 = nx.Graph()
GG.add_edge(g1, g2)
draw_graph(GG)
plt.savefig("anythingcanbedone6.png")
```

![png](https://i.imgur.com/bB95wxe.png)

### node and edge attribute dictionary

- 지금까지는 node에 이름만 넣었지만, attribute를 함께 넣을 수도 있음.
- edge의 경우도 마찬가지.

```python
G = nx.Graph()
G.add_nodes_from([1,2,3])
G.nodes(data=True)
```

    [(1, {}), (2, {}), (3, {})]

```python
# node, node attribute를 함께 넣는 경우 에는 2-tuple로 넣되, 두번재에는 attribute_dictionary를 넣음
G = nx.Graph()
G.add_nodes_from( [(1, {'weight':2, 'name':'aaa'}),
                   (2, {'weight':4}),
                   (3, {'weight':5})
                  ] )
print( G.nodes() )
print( G.nodes(data=True) )
draw_graph(G)
plt.savefig("attribute1.png")

G.add_edges_from([(1, 2, {'weight':3, 'color':'red'}),
                  (2, 3, {'weight':10, 'color':'blue'})
                 ])
print( G.edges() )
print( G.edges(data=True) )
draw_graph(G)
plt.savefig("attribute2.png")
```

    [1, 2, 3]
    [(1, {'name': 'aaa', 'weight': 2}), (2, {'weight': 4}), (3, {'weight': 5})]
    [(1, 2), (2, 3)]
    [(1, 2, {'color': 'red', 'weight': 3}), (2, 3, {'color': 'blue', 'weight': 10})]

![png](https://i.imgur.com/cKsRMpr.png)

![png](https://i.imgur.com/iNXYA79.png)

## graph generation

- 기본적으로 제공하는 몇가지 그래프들이 있음

```python
draw_graph( nx.complete_graph(7) )
plt.savefig("graph_generation1.png")

draw_graph( nx.complete_bipartite_graph(2, 4) )
plt.savefig("graph_generation2.png")

draw_graph( nx.barbell_graph(4, 4) )
plt.savefig("graph_generation3.png")

draw_graph( nx.lollipop_graph(4, 5), 'spring')
plt.savefig("graph_generation4.png")
```

![png](https://i.imgur.com/1mlb7lh.png)

![png](https://i.imgur.com/OBoP6hC.png)

![png](https://i.imgur.com/7VBdJeU.png)

![png](https://i.imgur.com/J8ylUqk.png)

```python
draw_graph( nx.erdos_renyi_graph(8, 0.3) )
plt.savefig("graph_generation5.png")

draw_graph( nx.watts_strogatz_graph(10, 3, 0.1) )
plt.savefig("graph_generation6.png")

draw_graph( nx.barabasi_albert_graph(10, 1) )
plt.savefig("graph_generation7.png")
```

![png](https://i.imgur.com/Oc80GtV.png)

![png](https://i.imgur.com/23kpaSm.png)

![png](https://i.imgur.com/cB3MJnW.png)

### analyzing graph

- 이제 그래프를 어떻게 만드는지 알겠고, 필요하다면 이미 그래프를 만들어주는 걸 써서 만들면 될것 같으니, 이제 분석을 해보자.
- degree: 각 node에 연결된 edge의 수
- centrality: 어떤 노드가 중요한가?
  - degree_centrality: (normalized) degree, 해당 네트워크에서 직접적인 영향력을 가진 노드.
  - closeness_centrality: 해당 노드가 다른 노드들과 얼마나 가깝게 연결되어 있는가?
  - betweenness_centrality: 해당 노드가 다른 노드들의 최단 거리에 얼마나 많이 포함되어 있는가?

```python
G = nx.lollipop_graph(4, 5)
draw_graph(G)
plt.savefig("analyzing_graph.png")
```

![png](https://i.imgur.com/c6Z6xtw.png)

```python
import pandas as pd
node_dict={
    'degree':G.degree(),
    "degree_centrality":nx.degree_centrality(G), #normalized degree
    "closeness_centrality":nx.closeness_centrality(G),
    "betweenness_centrality":nx.betweenness_centrality(G),
    "pagerank":nx.pagerank(G)# 이건 네트워크 상에서 중요한 노드가 나를 참조하고 있을때,
}
pd.DataFrame(node_dict)
```

- 여담이지만, jupyter notebook에서 작업한 ipynb 파일은 markdown로 컨버팅하면, dataframe 결과가 테이블로 예쁘게 나오는데, 이 부분은 html 태그를 활용해서 표현됨.
- 그러나, markdown linter는 markdown 내에 html 태그가 있을 경우 지랄하기 때문에, 이부분을 고치는데 조금 시간이 걸려서 빡침. 후.

||bet centrality|closec ent |degree|degree cent|pagerank|
|--- |--- |--- |--- |--- |--- |
|0|0.000000|0.347826|3|0.375|0.113494|
|1|0.000000|0.347826|3|0.375|0.113494|
|2|0.000000|0.347826|3|0.375|0.113494|
|3|0.535714|0.444444|4|0.500|0.153007|
|4|0.571429|0.470588|2|0.250|0.093813|
|5|0.535714|0.444444|2|0.250|0.105018|
|6|0.428571|0.380952|2|0.250|0.114071|
|7|0.250000|0.307692|2|0.250|0.124171|
|8|0.000000|0.242424|1|0.125|0.069439|

## link prediction

- 네트워크에서 아직 연결되지 않은 노드 쌍 중에서 어떤 노드가 향후에 연결될 가능성이 있는가?

```python
import numpy as np
G = nx.complete_graph(9)

for i in range(0, int(len(G.nodes())**1.9)):
    f = np.random.randint(len(G.nodes()))
    t = np.random.randint(len(G.nodes()))
    try:
        G.remove_edge(f, t)
    except:
        continue

predicted_node_pair = list(filter(lambda x: True if x[2]>0.1 else False, nx.resource_allocation_index(G)))
predicted_node_pair = sorted(predicted_node_pair, key=lambda x: x[2], reverse=True)
print( predicted_node_pair )
draw_graph(G)
plt.savefig("link_prediction.png")
```

    [(0, 1, 0.3333333333333333), (0, 5, 0.3333333333333333), (2, 4, 0.3333333333333333), (2, 5, 0.3333333333333333), (4, 5, 0.3333333333333333), (1, 8, 0.25), (1, 3, 0.25), (3, 8, 0.25), (3, 6, 0.25), (6, 8, 0.25)]
![png](https://i.imgur.com/8YuyvSy.png)

## 마무리

- 추가하려면 내용이 더 있기는 한데, 대충 사용법 정도만 공유하면 되지 않을까 싶어서, 이정도로만 정리합니당

## reference

- [networkx documentation](https://networkx.github.io/)
