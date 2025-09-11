在SDK中把车道线的点存下来：
```cpp
for (auto &p : laneline_result->sampled_points_bv_world) {
    std::cout << p.x << ", " << p.y << std::endl;
}
```

用python可视化
```python
import matplotlib.pyplot as plt
import sys
# 读取文件
file_path = sys.argv[1]
with open(file_path, 'r') as file:
    lines = file.readlines()

# 提取数据
x_values = []
y_values = []
for line in lines:
    parts = line.strip().split(' ')
    x_values.append(float(parts[0]))
    y_values.append(float(parts[1]))

# 绘制图表
plt.scatter(y_values, x_values)
plt.xlabel('Y')
plt.ylabel('X')
plt.title('Scatter Plot of Points')
```

