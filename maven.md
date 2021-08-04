# maven

## 依赖树的解析

### 依赖树的结构

在pom.xml的dependencies元素中声明项目所需的依赖，Maven将引入直接依赖，并通过解析直接依赖的pom.xml将传递性依赖导入到当前项目，这样层层递进最终构成了一个树状结构的依赖关系。

依赖树的构建：深度优先遍历并通过缓存节点剪枝

### 依赖调解

出现多个版本构建依赖时，Maven采用了“短路径优先”的原则

### 循环依赖

循环依赖指两个或多个构件形成环状依赖关系。一般在工程实践上推荐使用**单向依赖关系**组织代码和模块，形成简单松耦合的层级关系。

一般我们可以使用 mvn dependency:tree -Dverbose | grep cycle 来判断项目中是否存在循环依赖。

### 依赖排除

Exclusion 会降低Maven依赖解析的效率。

1. Exclusion会造成依赖重复扫描和缓存。对于相同的节点，不同的Exclusion集合会重复扫描和缓存多次。
2. 在距离根节点越远的地方Exclusion，影响的子树范围越小。
3. **依赖树高度越高，引入Exclusion的代价越大。**

作为开发者我们该如何应对这个问题？

1. 尽可能要求SDK的提供方暴露出轻量级SDK，避免引入巨型依赖，保证依赖树的高度在适当范围内。
2. 适当调大IDEA和Maven的Xmx，检查当前项目是否存在复杂的Exclusion声明。
3. 根本解决方案还是**需要联系上下游项目，划分清楚模块范围，控制好代码边界，避免循环依赖；只引入必须的依赖，避免源码依赖和聚合SDK，减少传递性依赖，降低依赖树高度。**

### 依赖可视化

1. 通过执行 mvn dependency:tree -Dverbose 可以得到文本输出的依赖树关系；
2. 执行 mvn dependency:tree -DoutputType=dot 得到dot定义的输出，通过Graphviz（需要brew install graphviz安装）将digraph渲染成图片。
3. IDEA在pom.xml右键执行Diagrams→Show dependencies也可以得到可视化的依赖树，但当依赖关系复杂时效果较差。