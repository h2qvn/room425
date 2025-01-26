以下是一个包含Markdown所有常见样式的测试文件内容，用于测试Markdown渲染效果.
# 这是一级标题

## 这是二级标题

### 这是三级标题

#### 这是四级标题

##### 这是五级标题

###### 这是六级标题

---

### 文本样式

**这是加粗文本**  
*这是斜体文本*  
~~这是删除线文本~~  
```python
from XEdu.hub import Workflow as wf
import cv2
from pinpong.board import Board, Servo, Pin

Board("uno").begin()
# 初始化舵机，D9引脚
servo = Servo(Pin(Pin.D9))
servo.write_angle(0)

cap = cv2.VideoCapture(0)  # 序号0表示本电脑的默认摄像头
det = wf(task='det_body')  # 实例化detect模型

# 定义人体大小框的阈值
max_box_area_far = 80000   # 人远离时的最大人体框面积

while cap.isOpened():
    ret, frame = cap.read()
    if not ret:
        break
    bboxs = det.inference(data=frame, thr=0.3)
    print('找到了', len(bboxs), '个人')
    img = frame
    
    # 检测人体框的大小，判断人的远近
    for [x1, y1, x2, y2] in bboxs:
        width = x2 - x1
        height = y2 - y1
        area = width * height
        
        # 如果人体框面积大于远离阈值，则认为人远离
        if area > max_box_area_far:
            
            print("靠近")
            servo.write_angle(0)
        else:
            print("远离")
            servo.write_angle(180)   
        
        # 画检测框
        cv2.rectangle(img, (int(x1), int(y1)), (int(x2), int(y2)), (0, 255, 0), 2)
    
    cv2.imshow('video', img)
    if cv2.waitKey(1) & 0xFF == ord('q'):
        break

cap.release()
cv2.destroyAllWindows()
```

---

### 列表

#### 无序列表
- 项目一
- 项目二
  - 子项目一
  - 子项目二
- 项目三

#### 有序列表
1. 第一项
2. 第二项
   1. 子项一
   2. 子项二
3. 第三项

#### 任务列表
- [x] 已完成任务
- [ ] 待完成任务

---

### 链接与图片

#### 链接
[这是一个链接](https://www.example.com)

#### 图片
![示例图片](https://pic2.zhimg.com/v2-c74cf86a68d6e761b72df6c3966013a4_r.jpg)

---

### 引用

> 这是一段引用文本。
> 
> 引用可以包含多行文本。

---

### 代码块

#### 行内代码
`console.log('Hello, world!');`

#### 块级代码
```javascript
function helloWorld() {
    console.log('Hello, world!');
}
helloWorld();
```
表格
表头一表头二表头三单元格一单元格二单元格三单元格四单元格五单元格六单元格七单元格八单元格九
分割线
HTML 混合内容
<p style="color: red;">这是直接嵌入的HTML内容</p>
```