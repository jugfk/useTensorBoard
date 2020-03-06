# 젯슨 나노에서 텐서플로우의 텐서보드 사용하기
***

* 다운로드하기
```
git clone https://github.com/jetsonworld/useTensorBoard.git
```

### 터미널에서 텐서보드
* 텐서보드 실행하기
```
cd useTensorBoard/01_Terminal
cat add_tensorboard.py
python3 add_tensorboard.py
tensorboard --logdir=/tmp/1
```

 * 웹브라져를 열어서 아래와 같이 실행합니다.
```
localhost:6006
```

![01_tensorboard.png](https://raw.githubusercontent.com/jetsonworld/useTensorBoard/master/00_images/01_tensorboard.png)


### 주피터노트북에서 텐서보드
```
cd useTensorBoard/02_Jupyter
jupyter notebook
```
* 주피터노트북 맨뒤에 2개를 추가합니다.
```
# Functions to show the Graphs
import numpy as np
from IPython.display import clear_output, Image, display, HTML

def strip_consts(graph_def, max_const_size=32):
    """Strip large constant values from graph_def."""
    strip_def = tf.GraphDef()
    for n0 in graph_def.node:
        n = strip_def.node.add() 
        n.MergeFrom(n0)
        if n.op == 'Const':
            tensor = n.attr['value'].tensor
            size = len(tensor.tensor_content)
            if size > max_const_size:
                tensor.tensor_content = b"<stripped %d bytes>"%size
    return strip_def

def show_graph(graph_def, max_const_size=32):
    """Visualize TensorFlow graph."""
    if hasattr(graph_def, 'as_graph_def'):
        graph_def = graph_def.as_graph_def()
    strip_def = strip_consts(graph_def, max_const_size=max_const_size)
    code = """
        <script>
          function load() {{
            document.getElementById("{id}").pbtxt = {data};
          }}
        </script>
        <link rel="import" href="https://tensorboard.appspot.com/tf-graph-basic.build.html" onload=load()>
        <div style="height:600px">
          <tf-graph-basic id="{id}"></tf-graph-basic>
        </div>
    """.format(data=repr(str(strip_def)), id='graph'+str(np.random.rand()))

    iframe = """
        <iframe seamless style="width:1200px;height:620px;border:0" srcdoc="{}"></iframe>
    """.format(code.replace('"', '&quot;'))
    display(HTML(iframe))
```

```
show_graph(tf.get_default_graph())
```

![02_tensorboard_in_jupyter.png](https://raw.githubusercontent.com/jetsonworld/useTensorBoard/master/00_images/02_tensorboard_in_jupyter.png)

![03_tensorboard_in_jupyter.png](https://raw.githubusercontent.com/jetsonworld/useTensorBoard/master/00_images/03_tensorboard_in_jupyter.png)

![04_tensorboard_in_jupyter.png](https://raw.githubusercontent.com/jetsonworld/useTensorBoard/master/00_images/04_tensorboard_in_jupyter.png)
