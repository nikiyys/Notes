---
layout: default
---

## [](header-2) 网格数据解析

### [](header-3) rcMeshLoaderObj

**Obj格式**网格数据的解析类

```cpp
class rcMeshLoaderObj
{
private:
    void addVertex(float x, float y, float z, int& cap);
    void addTriangle(int a, int b, int c, int& cap);
    
    std::string m_filename;
    float m_scale;  
    float* m_verts;             // vb数组，size: m_vertCount*3
    int* m_tris;                // ib数组，size: m_triCount*3
    float* m_normals;
    int m_vertCount;            // 顶点数量
    int m_triCount;             // 三角形数量
};
```

```cpp
struct rcChunkyTriMeshNode
{
    float bmin[2];
    float bmax[2];
    int i;
    int n;
};
```